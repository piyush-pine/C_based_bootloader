
# C-Based Bootloader Project

## Overview

This bootloader project demonstrates the basics of writing a floppy disk bootloader, transitioning from real mode to protected mode, and building a minimal kernel in C++. It starts with a 16-bit assembly boot sector that prints "Hello world!" and eventually moves to 32-bit or 64-bit protected mode execution.

## Features

- Bootloader written in assembly (NASM) targeting x86 architecture
- Boots under BIOS and jumps to protected mode
- Freestanding C++ kernel compiled with custom linker scripts and cross-compilation toolchains
- Minimal printing functionality to screen for early output debugging
- Uses linker scripts to layout binary sections appropriately
- QEMU-compatible for easy emulation and testing

## Prerequisites

- NASM assembler  
- Cross-compilation toolchain (`i386-elf-g++` or `x86_64-elf-g++`) for freestanding builds  
- QEMU emulator for running and testing the bootloader and kernel  
- Linux environment preferred (Kali Linux, Ubuntu, WSL, etc.)

Install basics on Debian-based Linux:
```bash
sudo apt-get install nasm qemu gcc-multilib g++-multilib
```

## Building the Project

1. Assemble your bootloader assembly files to object format:
```bash
nasm -f elf32 boot4.asm -o boot4.o
```

2. Compile and link the kernel C++ code with the bootloader object and linker script:
- For 32-bit:
```bash
i386-elf-g++ -m32 kmain.cpp boot4.o -o kernel.bin -nostdlib -ffreestanding -std=c++11 -fno-exceptions -fno-rtti -Wall -Wextra -Werror -T linker.ld
```
- For 64-bit:
```bash
x86_64-elf-g++ -m64 kmain.cpp boot4.o -o kernel.bin -nostdlib -ffreestanding -std=c++11 -mno-red-zone -fno-exceptions -fno-rtti -Wall -Wextra -Werror -T linker.ld
```

3. Create a bootable image if needed (depending on your workflow).

## Running

Test your bootloader in QEMU:
```bash
qemu-system-x86_64 -fda kernel.bin
```
or for 32-bit QEMU:
```bash
qemu-system-i386 -fda kernel.bin
```

## Notes

- The `linker.ld` linker script controls memory layout of the final binary.
- The project is kept minimal for educational purposes and can be extended with more features such as file system drivers, multitasking, etc.

## References
- Original project repo: [piyush-pine/C_based_bootloader](https://github.com/piyush-pine/C_based_bootloader)


[1](http://3zanders.co.uk/2017/10/13/writing-a-bootloader/)
[2](https://github.com/piyush-pine/C_based_bootloader)
