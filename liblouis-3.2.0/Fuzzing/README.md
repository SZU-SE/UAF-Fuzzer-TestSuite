### liblouis v3.2.0
- Bug type: use-after-free
- CVE ID: [CVE-2017-13741](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-13741)
- Download: https://github.com/liblouis/liblouis/releases/tag/v3.2.0
- Reproduce: `./lou_checktable $POC`
- ASAN dumps the backtrace:
```
=================================================================
==13287==ERROR: AddressSanitizer: heap-use-after-free on address 0x7faceea3baa4 at pc 0x7facee70e470 bp 0x7fff7e8b5f90 sp 0x7fff7e8b5f88
WRITE of size 4 at 0x7faceea3baa4 thread T0
    #0 0x7facee70e46f in compileBrailleIndicator /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:3246:9
    #1 0x7facee703c07 in compileRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:4024:2
    #2 0x7facee70108d in compileFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5217:2
    #3 0x7facee70dfc9 in includeFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5272:8
    #4 0x7facee7023f2 in compileRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:3847:11
    #5 0x7facee70108d in compileFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5217:2
    #6 0x7facee6fec61 in compileTranslationTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5325:10
    #7 0x7facee6fe60f in lou_getTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5428:19
    #8 0x51635a in main /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/tools/lou_checktable.c:121:17
    #9 0x7faced7f682f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291
    #10 0x41a188 in _start (/home/hjwang/UAF_Objects/liblouis-3.2.0_asan/build/bin/lou_checktable+0x41a188)

0x7faceea3baa4 is located 676 bytes inside of 147145-byte region [0x7faceea3b800,0x7faceea5f6c9)
freed by thread T0 here:
    #0 0x4de820 in realloc /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:107
    #1 0x7facee70f9b0 in allocateSpaceInTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:452:18
    #2 0x7facee70ee5d in addRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:1121:8
    #3 0x7facee70e3a7 in compileBrailleIndicator /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:3244:12
    #4 0x7facee703c07 in compileRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:4024:2
    #5 0x7facee70108d in compileFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5217:2
    #6 0x7facee70dfc9 in includeFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5272:8
    #7 0x7facee7023f2 in compileRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:3847:11
    #8 0x7facee70108d in compileFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5217:2
    #9 0x7facee6fec61 in compileTranslationTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5325:10
    #10 0x7facee6fe60f in lou_getTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5428:19
    #11 0x51635a in main /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/tools/lou_checktable.c:121:17
    #12 0x7faced7f682f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

previously allocated by thread T0 here:
    #0 0x4de820 in realloc /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:107
    #1 0x7facee70f9b0 in allocateSpaceInTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:452:18
    #2 0x7facee70ee5d in addRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:1121:8
    #3 0x7facee707b34 in compileRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:4528:9
    #4 0x7facee70108d in compileFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5217:2
    #5 0x7facee70dfc9 in includeFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5272:8
    #6 0x7facee7023f2 in compileRule /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:3847:11
    #7 0x7facee70108d in compileFile /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5217:2
    #8 0x7facee6fec61 in compileTranslationTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5325:10
    #9 0x7facee6fe60f in lou_getTable /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:5428:19
    #10 0x51635a in main /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/tools/lou_checktable.c:121:17
    #11 0x7faced7f682f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

SUMMARY: AddressSanitizer: heap-use-after-free /home/hjwang/UAF_Objects/liblouis-3.2.0_asan/liblouis/compileTranslationTable.c:3246:9 in compileBrailleIndicator
Shadow bytes around the buggy address:
  0x0ff61dd3f700: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f710: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f720: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f730: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f740: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
=>0x0ff61dd3f750: fd fd fd fd[fd]fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f760: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f770: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f780: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f790: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0ff61dd3f7a0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
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
==13287==ABORTING
```