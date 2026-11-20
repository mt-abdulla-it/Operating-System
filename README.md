# 🖥️ x86_64 Bare-Metal Operating System Kernel

A lightweight 64-bit operating system kernel implemented in C and Assembly, targeting the x86_64 architecture with Multiboot2 specification compliance.

---

## 🌟 Architectural Overview

- **Architecture:** x86_64 (Intel / AMD 64-bit Long Mode)
- **Boot Protocol:** Multiboot2 compliant via GRUB bootloader
- **Memory Architecture:** 4-level identity paging (PML4, PDPT, Page Directory with 2MB huge pages)
- **Display Driver:** Direct memory-mapped VGA text-mode buffer (`0xb8000`) with color and scrolling support
- **Toolchain:** `x86_64-elf-gcc` freestanding cross-compiler, NASM assembler, and GNU ld linker script

---

## ⚙️ Boot & Execution Sequence

```text
1. Firmware (BIOS / UEFI)
   └── GRUB Bootloader (Multiboot2)
       └── 32-bit Protected Mode Entry (_start in header.asm / main.asm)
           ├── CPUID & Long Mode Hardware Verification
           ├── 4-Level Paging Table Setup (Identity Mapped)
           ├── Global Descriptor Table (GDT) Loading
           └── Jump to 64-bit Subroutine (main64.asm)
               └── C Kernel Entry Point (kernel_main)
                   └── VGA Text Console Output
```

---

## 🛠️ Build and Emulation

### Prerequisites
- Docker (for cross-compiler toolchain)
- QEMU (`qemu-system-x86_64`)

### 1. Build Kernel ISO via Containerized Toolchain
```bash
# Build toolchain container image
docker build buildenv -t x86_64-kernel-env

# Compile assembly, C sources, and link ISO inside container
docker run --rm -v "$(pwd)":/root/env x86_64-kernel-env make build-x86_64
```

### 2. Run in QEMU Emulator
```bash
qemu-system-x86_64 -cdrom dist/x86_64/kernel.iso
```

---

## 📁 Source Directory Layout

- `src/impl/x86_64/boot/`: Assembly startup routines (multiboot header, long mode transition)
- `src/impl/kernel/`: High-level kernel entry point
- `src/impl/x86_64/`: Low-level architectural drivers (VGA text console)
- `src/intf/`: Internal C header interfaces
- `targets/x86_64/`: Linker script and GRUB ISO configuration
- `buildenv/`: Containerized cross-compiler definitions

---

## 📜 License

Licensed under the MIT License.
