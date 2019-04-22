### NASM 2.14.02
- Bug type: use-after-free
- CVE ID: [CVE-2019-8343](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8343), [CVE-2018-20535](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20535), [CVE-2018-20538](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20538)
- Download: https://repo.or.cz/w/nasm.git
- Reproduce: `./nasm -f bin $POC -o /dev/null`
- ASAN dumps the backtrace:

**POC1**
```
=================================================================
==20174==ERROR: AddressSanitizer: heap-use-after-free on address 0x6020000089f0 at pc 0x000000447aa4 bp 0x7ffe6f45bd30 sp 0x7ffe6f45b4e0
READ of size 2 at 0x6020000089f0 thread T0
    #0 0x447aa3 in __interceptor_strlen.part.24 (nasm-asan+0x447aa3)
    #1 0x535f62 in paste_tokens fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:3820:20
    #2 0x532cfd in expand_smacro fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:4509:13
    #3 0x5252bd in pp_getline fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:5254:21
    #4 0x505c91 in assemble_file fuzz/target/nasm/nasm-2.14.02/asm/nasm.c:1488:24
    #5 0x504645 in main fuzz/target/nasm/nasm-2.14.02/asm/nasm.c:617:9
    #6 0x7f425df2d222 in __libc_start_main (/usr/lib/libc.so.6+0x24222)
    #7 0x41c269 in _start (nasm-asan+0x41c269)

0x6020000089f0 is located 0 bytes inside of 3-byte region [0x6020000089f0,0x6020000089f3)
freed by thread T0 here:
    #0 0x4cb688 in __interceptor_free.localalias.0 (nasm-asan+0x4cb688)
    #1 0x526cdd in delete_Token fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:1232:5
    #2 0x5359b7 in paste_tokens fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:3780:20
    #3 0x532cfd in expand_smacro fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:4509:13
    #4 0x5252bd in pp_getline fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:5254:21
    #5 0x505c91 in assemble_file fuzz/target/nasm/nasm-2.14.02/asm/nasm.c:1488:24
    #6 0x504645 in main fuzz/target/nasm/nasm-2.14.02/asm/nasm.c:617:9
    #7 0x7f425df2d222 in __libc_start_main (/usr/lib/libc.so.6+0x24222)

previously allocated by thread T0 here:
    #0 0x4cb840 in malloc (nasm-asan+0x4cb840)
    #1 0x509eb8 in nasm_malloc fuzz/target/nasm/nasm-2.14.02/nasmlib/malloc.c:75:25
    #2 0x5271cb in new_Token fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:1222:19
    #3 0x528e32 in tokenize fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:1144:25
    #4 0x535ff1 in paste_tokens fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:3830:19
    #5 0x532cfd in expand_smacro fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:4509:13
    #6 0x5252bd in pp_getline fuzz/target/nasm/nasm-2.14.02/asm/preproc.c:5254:21
    #7 0x505c91 in assemble_file fuzz/target/nasm/nasm-2.14.02/asm/nasm.c:1488:24
    #8 0x504645 in main fuzz/target/nasm/nasm-2.14.02/asm/nasm.c:617:9
    #9 0x7f425df2d222 in __libc_start_main (/usr/lib/libc.so.6+0x24222)

SUMMARY: AddressSanitizer: heap-use-after-free (nasm-asan+0x447aa3) in __interceptor_strlen.part.24
Shadow bytes around the buggy address:
  0x0c047fff90e0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff90f0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9100: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9110: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9120: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9130: fa fa fa fa fa fa fa fa fa fa 02 fa fa fa[fd]fa
  0x0c047fff9140: fa fa fd fa fa fa fd fa fa fa 03 fa fa fa 02 fa
  0x0c047fff9150: fa fa 02 fa fa fa 02 fa fa fa fd fd fa fa fd fa
  0x0c047fff9160: fa fa fd fa fa fa fd fa fa fa fd fd fa fa fd fa
  0x0c047fff9170: fa fa 00 07 fa fa 00 07 fa fa 00 05 fa fa 00 05
  0x0c047fff9180: fa fa 00 05 fa fa 00 05 fa fa 00 05 fa fa 00 05
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
  Left alloca redzone:     ca
  Right alloca redzone:    cb
==20174==ABORTING
```

**POC2**
```
=================================================================
==113140==ERROR: AddressSanitizer: heap-use-after-free on address 0x60f00000d4b0 at pc 0x00000044758d bp 0x7ffd29bf6fe0 sp 0x7ffd29bf6fd0
READ of size 4 at 0x60f00000d4b0 thread T0
    #0 0x44758c in pp_getline asm/preproc.c:5153
    #1 0x40d791 in assemble_file asm/nasm.c:1442
    #2 0x40640d in main asm/nasm.c:573
    #3 0x7f234eaeea3f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x20a3f)
    #4 0x4072f8 in _start (/home/company/real_sanitize/poc_check/nasm/nasm_new_addr+0x4072f8)

0x60f00000d4b0 is located 144 bytes inside of 176-byte region [0x60f00000d420,0x60f00000d4d0)
freed by thread T0 here:
    #0 0x7f234ef306aa in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x986aa)
    #1 0x443fa0 in free_mmacro asm/preproc.c:630
    #2 0x443fa0 in do_directive asm/preproc.c:2957

previously allocated by thread T0 here:
    #0 0x7f234ef30b49 in __interceptor_calloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98b49)
    #1 0x40e4e0 in nasm_zalloc nasmlib/malloc.c:69
    #2 0x4bd707  (/home/company/real_sanitize/poc_check/nasm/nasm_new_addr+0x4bd707)

SUMMARY: AddressSanitizer: heap-use-after-free asm/preproc.c:5153 pp_getline
Shadow bytes around the buggy address:
  0x0c1e7fff9a40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a50: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a60: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a70: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a80: fa fa fa fa fd fd fd fd fd fd fd fd fd fd fd fd
=>0x0c1e7fff9a90: fd fd fd fd fd fd[fd]fd fd fd fa fa fa fa fa fa
  0x0c1e7fff9aa0: fa fa 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1e7fff9ab0: 00 00 00 00 00 00 00 00 fa fa fa fa fa fa fa fa
  0x0c1e7fff9ac0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1e7fff9ad0: 00 00 00 00 00 00 fa fa fa fa fa fa fa fa 00 00
  0x0c1e7fff9ae0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
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
==113140==ABORTING
```

**POC3**
```
=================================================================
==113343==ERROR: AddressSanitizer: heap-use-after-free on address 0x60f00000d430 at pc 0x00000044769e bp 0x7ffe333a6c90 sp 0x7ffe333a6c80
READ of size 8 at 0x60f00000d430 thread T0
    #0 0x44769d in pp_getline asm/preproc.c:5055
    #1 0x40d791 in assemble_file asm/nasm.c:1442
    #2 0x40640d in main asm/nasm.c:573
    #3 0x7feb98dbea3f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x20a3f)
    #4 0x4072f8 in _start (/home/company/real_sanitize/poc_check/nasm/nasm_new_addr+0x4072f8)

0x60f00000d430 is located 16 bytes inside of 176-byte region [0x60f00000d420,0x60f00000d4d0)
freed by thread T0 here:
    #0 0x7feb992006aa in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x986aa)
    #1 0x443fa0 in free_mmacro asm/preproc.c:630
    #2 0x443fa0 in do_directive asm/preproc.c:2957

previously allocated by thread T0 here:
    #0 0x7feb99200b49 in __interceptor_calloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98b49)
    #1 0x40e4e0 in nasm_zalloc nasmlib/malloc.c:69
    #2 0x4bd707  (/home/company/real_sanitize/poc_check/nasm/nasm_new_addr+0x4bd707)

SUMMARY: AddressSanitizer: heap-use-after-free asm/preproc.c:5055 pp_getline
Shadow bytes around the buggy address:
  0x0c1e7fff9a30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a50: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a60: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c1e7fff9a70: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c1e7fff9a80: fa fa fa fa fd fd[fd]fd fd fd fd fd fd fd fd fd
  0x0c1e7fff9a90: fd fd fd fd fd fd fd fd fd fd fa fa fa fa fa fa
  0x0c1e7fff9aa0: fa fa 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1e7fff9ab0: 00 00 00 00 00 00 00 00 fa fa fa fa fa fa fa fa
  0x0c1e7fff9ac0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c1e7fff9ad0: 00 00 00 00 00 00 fa fa fa fa fa fa fa fa 00 00
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
==113343==ABORTING
```