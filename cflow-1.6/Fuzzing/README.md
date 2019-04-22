### GNU cflow 1.6
- Bug type: use-after-free
- CVE ID: [pending](http://lists.gnu.org/archive/html/bug-cflow/2019-04/msg00001.html)
- Download: https://ftp.gnu.org/gnu/cflow/
- Reproduce: `./cflow $POC`
- ASAN dumps the backtrace:
```
=================================================================
==29623==ERROR: AddressSanitizer: heap-use-after-free on address 0x60e00000c690 at pc 0x00000041a09d bp 0x7ffe78b4d100 sp 0x7ffe78b4d0f0
READ of size 8 at 0x60e00000c690 thread T0
    #0 0x41a09c in reference /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:1298
    #1 0x4170a8 in expression /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:621
    #2 0x41878c in func_body /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:1051
    #3 0x41731c in parse_function_declaration /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:690
    #4 0x416f2c in parse_declaration /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:578
    #5 0x416c9e in yyparse /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:528
    #6 0x41146d in main /home/hjwang/UAF_Objects/cflow-1.6_asan/src/main.c:812
    #7 0x7f34934bb82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #8 0x402768 in _start (/home/hjwang/UAF_Objects/cflow-1.6_asan/build/bin/cflow+0x402768)

0x60e00000c690 is located 144 bytes inside of 152-byte region [0x60e00000c600,0x60e00000c698)
freed by thread T0 here:
    #0 0x7f34938fd2ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x41c383 in delete_symbol /home/hjwang/UAF_Objects/cflow-1.6_asan/src/symbol.c:205
    #2 0x41c492 in static_free /home/hjwang/UAF_Objects/cflow-1.6_asan/src/symbol.c:234
    #3 0x40ebd9 in linked_list_destroy /home/hjwang/UAF_Objects/cflow-1.6_asan/src/linked-list.c:87
    #4 0x41c50e in delete_statics /home/hjwang/UAF_Objects/cflow-1.6_asan/src/symbol.c:248
    #5 0x40ca93 in yywrap /home/hjwang/UAF_Objects/cflow-1.6_asan/src/c.l:366
    #6 0x4080aa in yylex /home/hjwang/UAF_Objects/cflow-1.6_asan/src/c.c:1653
    #7 0x40caba in get_token /home/hjwang/UAF_Objects/cflow-1.6_asan/src/c.l:380
    #8 0x414d61 in nexttoken /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:298
    #9 0x417072 in expression /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:616
    #10 0x41878c in func_body /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:1051
    #11 0x41731c in parse_function_declaration /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:690
    #12 0x416f2c in parse_declaration /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:578
    #13 0x416c9e in yyparse /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:528
    #14 0x41146d in main /home/hjwang/UAF_Objects/cflow-1.6_asan/src/main.c:812
    #15 0x7f34934bb82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

previously allocated by thread T0 here:
    #0 0x7f34938fd602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x436573 in xmalloc /home/hjwang/UAF_Objects/cflow-1.6_asan/gnu/xmalloc.c:43
    #2 0x41b9fd in install /home/hjwang/UAF_Objects/cflow-1.6_asan/src/symbol.c:92
    #3 0x41bebf in install_ident /home/hjwang/UAF_Objects/cflow-1.6_asan/src/symbol.c:156
    #4 0x419ccd in get_symbol /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:1249
    #5 0x41904d in declare /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:1178
    #6 0x417d9c in parse_dcl /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:879
    #7 0x41773c in parse_variable_declaration /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:751
    #8 0x416f3f in parse_declaration /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:580
    #9 0x416c9e in yyparse /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:528
    #10 0x41146d in main /home/hjwang/UAF_Objects/cflow-1.6_asan/src/main.c:812
    #11 0x7f34934bb82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-use-after-free /home/hjwang/UAF_Objects/cflow-1.6_asan/src/parser.c:1298 reference
Shadow bytes around the buggy address:
  0x0c1c7fff9880: fa fa fa fa fa fa fa fa fd fd fd fd fd fd fd fd
  0x0c1c7fff9890: fd fd fd fd fd fd fd fd fd fd fd fa fa fa fa fa
  0x0c1c7fff98a0: fa fa fa fa 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1c7fff98b0: 00 00 00 00 00 00 00 fa fa fa fa fa fa fa fa fa
  0x0c1c7fff98c0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
=>0x0c1c7fff98d0: fd fd[fd]fa fa fa fa fa fa fa fa fa fd fd fd fd
  0x0c1c7fff98e0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fa
  0x0c1c7fff98f0: fa fa fa fa fa fa fa fa 00 00 00 00 00 00 00 00
  0x0c1c7fff9900: 00 00 00 00 00 00 00 00 00 00 00 fa fa fa fa fa
  0x0c1c7fff9910: fa fa fa fa 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1c7fff9920: 00 00 00 00 00 00 00 fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07
  Heap left redzone:       fa
  Heap right redzone:      fb
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack partial redzone:   f4
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
==29623==ABORTING
```