# Go property-testing harness (`rapid` and `testing`)

Apply these Go-specific patterns alongside the specification, generator, and effect-reification principles in [`SKILL.md`](../SKILL.md).

**Zero-stall runner selection and sub-second package loops.** Run tests on the single leaf package (`go test -run TestLaw ./pkg/leaf > /tmp/gotest.log 2>&1`), which compiles and executes in `0.3–1.5s`. Check `go.mod` and `vendor/` first:
- **`pgregory.net/rapid` available (or fetchable in the ephemeral clone):** Use `rapid.Check(t, func(t *rapid.T) { ... })` and `t.Repeat(map[string]func(*rapid.T){ ... })` state machines (`-rapid.checks=500`), drawing from small domains via `rapid.SampledFrom`, `rapid.IntRange(0, 4)`, and `rapid.SliceOfN(..., 0, 8)`.
- **Offline workspace without `rapid`:** Never stall on blocked network fetches; write an in-package property driver over `testing/quick` or deterministic `math/rand/v2` (`rand.New(rand.NewPCG(seed, 0))`) generating `[]Op` slices of length `1..16` with an inline delta-debugging shrinker (`ddmin` deleting slices/ops while the predicate still fails) so shrinking stays instant with zero external module edits.

**Per-trial `FsMap` purity (avoiding outer `t.TempDir()` sharing).** Calling `t.TempDir()` or `t.Setenv()` *outside* `rapid.Check` / `quick.Check` shares one directory or process environment across hundreds of trials, breaking referential transparency and shrink replay. Create a fresh `root, err := os.MkdirTemp("", "fsmap-*")` with `defer os.RemoveAll(root)` *inside* each individual trial invocation, materialize `root/d0/d1/work` plus sibling entries (`root/a`, `root/d0/a`), compare `snapshotTree(root)` against `modelTree` (`reflect.DeepEqual` on canonical structs), and pass explicit `cmd.Dir = work` and `cmd.Env = cleanEnv` when running `exec.Command`.

**Go-specific operational semantics and concurrency checks.**
- **Slice backing-array aliasing:** Probe `append` over sub-slices (`s[:k]` where `cap(s) > k`) mutating shared backing arrays across cached views or caller-retained buffers (`force capacity cliffs with s[:k:k] vs. slack capacity`).
- **`nil` vs. empty slices/maps and interface-nil traps:** Vary `[]T(nil)` vs. `[]T{}` across JSON/protobuf roundtrips and equality checks, typed-`nil` pointers stored inside non-`nil` interfaces (`var p *MyErr = nil; var err error = p`), and map-iteration randomization order leaking into supposedly deterministic outputs.
- **Schedule linearizability (`-race`) and `context` cancellation:** Run concurrent caches, pools, and pipelines under `go test -race -count=10 ./pkg/leaf`, pairing `2–4` goroutines with cancelled `context.Context` deadlines mid-flight to verify `StateT` rollback and absence of data races or leaked goroutines.

**Clean `func Test...` extraction.** Once shrunk to the minimal `2–5` step counterexample, express the final regression as an idiomatic, dependency-free `func TestComponentInvariant(t *testing.T)` in the target package's `_test.go` style (restoring `go.mod`/`go.sum` to `target_sha` if `rapid` was added temporarily).
