### Elfutils 0.173
- Bug type: double-free
- CVE ID: [CVE-2018-16402](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16402)
- Download: ftp://sourceware.org/pub/elfutils/0.173/
- Reproduce: `./eu-nm $POC`
- ASAN dumps the backtrace:\
```
=================================================================
==17023==ERROR: AddressSanitizer: attempting double-free on 0x60400000de90 in thread T0:
    #0 0x7f72c87202ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x7f72c802c5df in elf_end /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/libelf/elf_end.c:171
    #2 0x7f72c83ab3a0 in free_file /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/libdwfl/dwfl_module.c:57
    #3 0x7f72c83ab3a0 in __libdwfl_module_free /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/libdwfl/dwfl_module.c:113
    #4 0x7f72c83a9610 in dwfl_end /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/libdwfl/dwfl_end.c:54
    #5 0x4098cc in show_symbols /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/src/nm.c:1495
    #6 0x40cce4 in handle_elf /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/src/nm.c:1579
    #7 0x403599 in process_file /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/src/nm.c:375
    #8 0x403599 in main /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/src/nm.c:250
    #9 0x7f72c76e682f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #10 0x4041e8 in _start (/home/hjwang/UAF_Objects/elfutils-0.173_afl_asan/build/bin/eu-nm+0x4041e8)

0x60400000de90 is located 0 bytes inside of 36-byte region [0x60400000de90,0x60400000deb4)
freed by thread T0 here:
    #0 0x7f72c87202ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x7f72c80aa729 in __libelf_reset_rawdata /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/libelf/elf_compress.c:325

previously allocated by thread T0 here:
    #0 0x7f72c8720602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x7f72c80a99c5 in __libelf_decompress /home/hjwang/UAF_Objects/elfutils-0.173_Fuzzing/libelf/elf_compress.c:223
    #2 0x7f72c80b0d90  (/home/hjwang/UAF_Objects/elfutils-0.173_afl_asan/build/lib/libelf.so.1+0x9ad90)

SUMMARY: AddressSanitizer: double-free ??:0 __interceptor_free
==17023==ABORTING
```