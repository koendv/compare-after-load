# Compare sections after load

This is a small patch to [gdb](https://www.sourceware.org/gdb/), that automatically checks a program was flashed correctly.

## Use
After applying this patch to gdb, switch on checking flash with:
```
set remote compare_sections_after_load on
```

With  `remote compare_sections_after_load` set, if `load` ends with the message
```
sections matched.
```
then all went well.

If `load` ends with a message like:
```
Section .text, range 0x800010c -- 0x8002160: MIS-MATCHED!
warning: One or more sections of the target image does not match the loaded file
```
then you need to investigate.

Put `set remote compare_sections_after_load on` in your .gdbinit, and after every `load` command, `compare_sections` is run automatically.

```
$ cat ~/.gdbinit
set remote compare_sections_after_load on
```

### Example

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
sections matched.
(gdb)
```

### Note

`compare-sections` asks the debugger probe to calculate crc checksums of target flash memory, and gdb compares these checksums with the same crc checksums, calculated over the elf binary file.
