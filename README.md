# Compiling Rust to a flat RV64GC binary

- Put the following in `.cargo/config.toml`:
  ```toml
  [build]
  target = "riscv64gc-unknown-none-elf"
  rustflags = ["-C", "link-arg=-Tlinker-script.xc", "-C", "link-arg=--oformat=binary"]
  ```
- Copy `/usr/riscv64-linux-gnu/ldscripts/elf64lriscv.xc` to the project root and name it `linker-script.xc`.
- Make the following changes to the linker script:
  - Move the `.text` section to just below the `PROVIDE` line.
  - Add `*(.text._start)` as its first entry.
- Annotate the `_start` function with `#[no_mangle]` and `#[link_section = ".text._start"]`.
