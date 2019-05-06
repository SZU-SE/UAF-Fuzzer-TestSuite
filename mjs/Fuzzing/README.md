### mjs
- Bug type: use-after-free
- Reference 1: https://github.com/cesanta/mjs/issues/73
- Reference 2: https://github.com/cesanta/mjs/issues/78
- Install：`clang mjs.c -DMJS_MAIN -fsanitize=address -g -o mjs`
- Reproduce: `mjs_exe file`
- ASAN dumps the backtrace: 
```
==29087==ERROR: AddressSanitizer: heap-use-after-free on address 0x6180000002b5 at pc 0x0000004bbc55 bp 0x7ffdeb955820 sp 0x7ffdeb954fd0
READ of size 152 at 0x6180000002b5 thread T0
    #0 0x4bbc54  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x4bbc54)
    #1 0x55374b  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x55374b)
    #2 0x54f99b  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54f99b)
    #3 0x593024  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x593024)
    #4 0x547e0f  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x547e0f)
    #5 0x54357c  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54357c)
    #6 0x5439a8  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x5439a8)
    #7 0x54c981  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54c981)
    #8 0x7f063b5c982f  (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #9 0x419e88  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x419e88)

0x6180000002b5 is located 565 bytes inside of 815-byte region [0x618000000080,0x6180000003af)
freed by thread T0 here:
    #0 0x4d2918  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x4d2918)
    #1 0x50d7b9  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x50d7b9)
    #2 0x54f8e6  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54f8e6)
    #3 0x593024  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x593024)
    #4 0x547e0f  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x547e0f)
    #5 0x54357c  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54357c)
    #6 0x5439a8  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x5439a8)
    #7 0x54c981  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54c981)
    #8 0x7f063b5c982f  (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

previously allocated by thread T0 here:
    #0 0x4d2918  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x4d2918)
    #1 0x50d7b9  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x50d7b9)
    #2 0x54f8e6  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54f8e6)
    #3 0x546de1  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x546de1)
    #4 0x54357c  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54357c)
    #5 0x5439a8  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x5439a8)
    #6 0x54c981  (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x54c981)
    #7 0x7f063b5c982f  (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-use-after-free (/home/hongxu/FUSE/tests/mjs-ins/mjs_debug_main_FOT-new.out+0x4bbc54) 
Shadow bytes around the buggy address:
  0x0c307fff8000: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c307fff8010: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c307fff8020: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c307fff8030: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c307fff8040: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
=>0x0c307fff8050: fd fd fd fd fd fd[fd]fd fd fd fd fd fd fd fd fd
  0x0c307fff8060: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c307fff8070: fd fd fd fd fd fd fa fa fa fa fa fa fa fa fa fa
  0x0c307fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c307fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c307fff80a0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
  Left alloca redzone:     ca
  Right alloca redzone:    cb
==29087==ABORTING
```