# Compiling Rust to a flat RV64G binary

- Put the following in `.cargo/config.toml`:
  ```toml
  [build]
  target = "riscv64gc-unknown-none-elf"
  rustflags = ["-C", "link-args=-Tlinker-script.xc", "-C", "target-feature=-c"]
  ```
  - `-c` disables compressed instructions.
- Copy `/usr/riscv64-linux-gnu/ldscripts/elf64lriscv.xc` to the project root and name it `linker-script.xc`.
- Make the following changes to the linker script:
  - Move the `.text` section to just below the `PROVIDE` line.
  - Add `*(.text._start)` as its first entry.
- Annotate the `_start` function with `#[no_mangle]` and `#[link_section = ".text._start"]`.
- Makefile to convert the ELF file to a flat binary:
  ```makefile
  target/riscv64gc-unknown-none-elf/debug/PROJECT-NAME.com: target/riscv64gc-unknown-none-elf/debug/PROJECT-NAME
  	riscv64-linux-gnu-objcopy --output-target=binary $< $@
  ```
