# Ecosystem PBT discovery and external Rust `proptest` driver fallback

Apply these harness-discovery and cross-language driver patterns alongside [`SKILL.md`](../SKILL.md) whenever targeting languages beyond Rust/Python/Go, working in C/C++, testing CLI/daemon boundaries, or operating in an environment without an installed property-testing library.

## 1. Check native property-testing libraries first

Inspect the repository's manifests, lockfiles, and local test dependencies before building an external driver:
- **TypeScript / JavaScript (`package.json`):** `fast-check` (`fc.assert`, `fc.property`, `fc.commands` / `fc.modelRun` over `vitest`, `jest`, or `node --test`).
- **JVM (`pom.xml`, `build.gradle`):** `jqwik` (`@Property`, `ActionChain`), `QuickTheories`, or ScalaCheck (`Commands`).
- **BEAM / FP / Scripting:** `StreamData` (`mix.exs`), `PropEr` (`rebar.config`), `QuickCheck` / `Hedgehog` (`.cabal`), `FsCheck` (`.csproj`), `eris` (`composer.json`), `rantly` (`Gemfile`).

When a native shrinking library is already present (or cleanly installable inside the ephemeral workspace), write the state-machine and `alpha(c) == model` properties directly in that library—enforcing per-trial `TempDir` creation *inside* each trial body rather than outer test-suite hooks.

## 2. External Rust `proptest` driver (`/tmp/<repo>-driver/`)

Whenever the target language has no property-testing library available (C, C++, Lua, SQL, shell, offline Node/Ruby/PHP/JVM checkouts) or the unit under test is a compiled library, binary, or local service, create a small standalone Cargo crate *outside* the target repository (`/tmp/<repo>-driver/Cargo.toml` with `proptest = "1"` and `serde_json = "1"`). Let Rust `proptest` own `Op` generation, `sample::Index` selection, Groce swarm masks, reference model `step_A`, and shrinking across the fastest applicable transport:

1. **In-process C / C++ ABI (`cc` crate or linked `.a` / `.so`, `100,000+` trials/s).**
   Compile the target leaf `.c`/`.cc` sources (or link the repo's built static archive) into the Rust driver via `build.rs` (`cc::Build`) and `extern "C"` declarations. Build two profiles:
   - **Operational-semantics profile:** `-O1 -g -fno-omit-frame-pointer -fsanitize=address,undefined` (`ASAN_OPTIONS=detect_leaks=0:abort_on_error=1`, `UBSAN_OPTIONS=print_stacktrace=1:halt_on_error=1`) to verify definedness side-conditions (`idx < slice.len()`, `live(id) in dom(alpha)`, alignment, initialized prefix, signed `Z_2^w` bounds) at `0–2` byte and threshold boundaries. Redirect runner stderr to `/tmp/driver.log` and extract only compact `(file:line, seed, violated_side_condition)` tuples.
   - **Release parity profile:** `-O2 -DNDEBUG` to catch behavior changes when `assert(...)` side effects or debug guards are elided.

2. **Persistent line-delimited JSON `stdin`/`stdout` worker (`2,000–10,000` trials/s for interpreted/VM runtimes).**
   Avoid spawning a fresh interpreter per trial when testing Python, Node, Ruby, PHP, Lua, or JVM modules without native PBT libraries. Spawn *one* warm child process (`Command::new(...)` with `Stdio::piped()` and `env_clear()`) running a `25`-line loop that reads one JSON-encoded `Vec<Op>` trace per line from `stdin`, constructs a fresh concrete `sut` (and fresh per-trial temporary directory when stateful on disk), executes the operations, and writes one JSON line of `(steps_out, alpha_sut)` to `stdout`. Wrap the handle in a small Rust struct that respawns the worker cleanly if an unhandled exception or exit closes the pipe mid-shrink.

3. **Per-trial subprocess or loopback socket driver (`CLI binaries, daemons, multi-binary pipelines`).**
   Materialize the initial `BTreeMap<RelPath, Entry>` in a fresh per-trial `TempDir` (`root/d0/d1/work` with sibling `root/a`, `root/d0/a`), invoke the target binary with `env_clear()` and `.current_dir(&work)` (or send generated protocol frames over `127.0.0.1:<ephemeral_port>`), and compare `(status, parsed_output, snapshot_tree(root))` directly against `model`.

## 3. Zero-footprint native witness projection

Treat the external Rust crate strictly as a discovery and shrinking engine. Once `proptest` shrinks a divergence to its minimal `2–5` step fixed point, translate that exact deterministic case into a small, dependency-free test in the target repository's own native language and test runner (`15–30` line C `assert()` test wired into `ctest`/`Makefile`, `node:test`, `RSpec`/`Minitest`, `PHPUnit`, `JUnit`, `pytest`, `go test`) and verify baseline failure and post-fix pass directly inside the target repository with zero Rust artifacts added to the tree.
