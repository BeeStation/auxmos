Rust-based atmospherics for Space Station 13 using [auxtools](https://github.com/willox/auxtools).

The compiled binary on Citadel is compiled for Citadel's CPU, which therefore means that it uses [AVX2 fused-multiply-accumulate](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#Advanced_Vector_Extensions_2). Yes, really. If you have issues, compile it yourself, via `cargo rustc --package auxmos --target=i686-pc-windows-msvc --release --features "all_reaction_hooks katmos" -- -C target-cpu=native`. It has to be 32-bit, mind.

---

**IMPORTANT**: For beecode, `--features "all_reaction_hooks"` will cause problems such as crashing, due to Bee having different fusion code. Replacing that argument with `--features trit_fire_hook,plasma_fire_hook,generic_fire_hook` will prevent auxtools from attempting to hook into fusion procs.

The `master` branch is to be considered unstable; use the releases if you want to make sure it actually works. [The latest release is here](https://github.com/BeeStation/auxmos/releases/latest).
