# Ecosystem PBT discovery and external Rust `proptest` driver fallback

Apply these harness-discovery and cross-language driver patterns alongside [`SKILL.md`](../SKILL.md) whenever targeting languages beyond Rust/Python/Go, working in C/C++, testing CLI/daemon boundaries, or operating in an environment without an installed property-testing library.

## 1. Check native property-testing libraries first

Inspect the repository's manifests, lockfiles, and local test dependencies before building an external driver:
- **TypeScript / JavaScript (`package.json`):** `fast-check` (`fc.assert`, `fc.property`, `fc.commands` / `fc.modelRun` over `vitest`, `jest`, or `node --test`).
- **JVM (`pom.xml`, `build.gradle`):** `jqwik` (`@Property`, `ActionChain`), `QuickTheories`, or ScalaCheck (`Commands`).
- **BEAM / FP / Scripting:** `StreamData` (`mix.exs`), `PropEr` (`rebar.config`), `QuickCheck` / `Hedgehog` (`.cabal`), `FsCheck` (`.csproj`), `eris` (`composer.json`), `rantly` (`Gemfile`).

When a native shrinking library is already present (or cleanly installable inside the ephemeral workspace), write the state-machine and `alpha(c) == model` properties directly in that library—enforcing per-trial `TempDir` creation *inside* each trial body rather than outer test-suite hooks. Whenever provisioning a compiler, package, or reference library artifact requires Nix/package-manager resolution, network downloads, or archive extraction, file a plain infrastructure request ticket for a dedicated **environment-setup subagent** working strictly under `/tmp/<repo>-env/` (never reading or reaching into the target workspace or test traces) so it hands back declarative Nix files (`env.nix`, `nix-build --no-out-link`) or ready artifact paths and commands by reference.

## 2. Fast C/C++ shared-library and external Rust `proptest` drivers

Whenever the target language has no property-testing library available (C, C++, Lua, SQL, shell, offline Node/Ruby/PHP/JVM checkouts) or the unit under test is a compiled library, binary, or local service, drive the target from outside the repository using the fastest transport that fits the domain:

1. **Sub-second C / C++ shared library + Python `ctypes` / `Hypothesis` (`0.3s` compile, ideal for shortlex and stateless/tree APIs).**
   Compile target leaf `.c`/`.cc` files directly into a disposable shared object (`cc -std=c99 -O2 -fPIC -shared leaf.c -o /tmp/<repo>-driver/lib.so`) and bind functions via Python `ctypes` (`setting explicit argtypes and restype`). Exhaust tiny input/AST domains in shortlex order (`itertools.product` over `0..=3` steps in `< 0.1s`) and run `Hypothesis` or seeded state traces against Python reference models (`using list-of-pairs rather than dict when duplicate keys are legal`). Always verify extracted standalone C witnesses (`check.c`) under **both** `-O1 -g -fno-omit-frame-pointer -fsanitize=address,undefined` and `-O2 -DNDEBUG`!

2. **In-process Rust `proptest` ABI (`cc::Build` / `extern "C"`, `100,000+` stateful transitions/s).**
   Create `/tmp/<repo>-driver/Cargo.toml` (`proptest = "1"`, `serde_json = "1"`) and export a shared `CARGO_TARGET_DIR=/tmp/<repo>-cargo-target` across all parallel worker lanes so `proptest` compiles once and subsequent module drivers link in `< 1s`. Build two C/C++ profiles:
   - **Operational-semantics profile:** `-O1 -g -fno-omit-frame-pointer -fsanitize=address,undefined` (`ASAN_OPTIONS=detect_leaks=0:abort_on_error=1`, `UBSAN_OPTIONS=print_stacktrace=1:halt_on_error=1`) to verify definedness side-conditions (`idx < slice.len()`, `live(id) in dom(alpha)`, alignment, initialized prefix, signed `Z_2^w` bounds) at `0–2` byte and threshold boundaries. Redirect runner stderr to `/tmp/driver.log` and extract only compact `(file:line, seed, violated_side_condition)` tuples.
   - **Release parity profile:** `-O2 -DNDEBUG` to catch behavior changes when `assert(...)` side effects or debug guards are elided.

3. **Persistent line-delimited JSON `stdin`/`stdout` worker (`2,000–10,000` trials/s for interpreted/VM runtimes).**
   Avoid spawning a fresh interpreter per trial when testing Python, Node, Ruby, PHP, Lua, or JVM modules without native PBT libraries. Spawn *one* warm child process (`Command::new(...)` with `Stdio::piped()` and `env_clear()`) running a `25`-line loop that reads one JSON-encoded `Vec<Op>` trace per line from `stdin`, constructs a fresh concrete `sut` (and fresh per-trial temporary directory when stateful on disk), executes the operations, and writes one JSON line of `(steps_out, alpha_sut)` to `stdout`. Wrap the handle in a small Rust struct that respawns the worker cleanly if an unhandled exception or exit closes the pipe mid-shrink.

4. **Per-trial subprocess or loopback socket driver (`CLI binaries, daemons, multi-binary pipelines`).**
   Materialize the initial `BTreeMap<RelPath, Entry>` in a fresh per-trial `TempDir` (`root/d0/d1/work` with sibling `root/a`, `root/d0/a`), invoke the target binary with `env_clear()` and `.current_dir(&work)` (or send generated protocol frames over `127.0.0.1:<ephemeral_port>`), and compare `(status, parsed_output, snapshot_tree(root))` directly against `model`.

## 3. Zero-footprint native witness projection

Treat the external Rust crate strictly as a discovery and shrinking engine. Once `proptest` shrinks a divergence to its minimal `2–5` step fixed point, translate that exact deterministic case into a small, dependency-free test in the target repository's own native language and test runner (`15–30` line C `assert()` test wired into `ctest`/`Makefile`, `node:test`, `RSpec`/`Minitest`, `PHPUnit`, `JUnit`, `pytest`, `go test`) and verify baseline failure and post-fix pass directly inside the target repository with zero Rust artifacts added to the tree.
