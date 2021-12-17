# Compare sections after load

This is a small patch to [gdb](https://www.sourceware.org/gdb/), that automatically checks a program was flashed correctly.

An example:

```
(gdb) set remote compare_sections_after_load on
(gdb) file HelloWorld.ino.elf 
A program is being debugged already.
Are you sure you want to change the file? (y or n) y
Reading symbols from HelloWorld.ino.elf...
(No debugging symbols found in HelloWorld.ino.elf)
(gdb) load 
Loading section .isr_vector, size 0x10c lma 0x8000000
Loading section .text, size 0x1b44 lma 0x800010c
Loading section .rodata, size 0x310 lma 0x8001c50
Loading section .init_array, size 0x14 lma 0x8001f60
Loading section .fini_array, size 0xc lma 0x8001f74
Loading section .data, size 0x70 lma 0x8001f80
Start address 0x080015e8, load size 8176
Transfer rate: 17 KB/sec, 628 bytes/write.
Section .isr_vector, range 0x8000000 -- 0x800010c: matched.
Section .text, range 0x800010c -- 0x8001c50: matched.
Section .rodata, range 0x8001c50 -- 0x8001f60: matched.
Section .init_array, range 0x8001f60 -- 0x8001f74: matched.
Section .fini_array, range 0x8001f74 -- 0x8001f80: matched.
Section .data, range 0x8001f80 -- 0x8001ff0: matched.
(gdb) 
```

Simply apply the patch, put `set remote compare_sections_after_load on` in your .gdbinit, and after every `load` command, `compare_sections` is run automatically.