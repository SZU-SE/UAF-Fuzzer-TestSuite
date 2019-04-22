### yara 
- Bug type: use-after-free
- CVE ID: [CVE-2017-5924](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5924)
- Download: https://github.com/VirusTotal/yara
```
git clone https://github.com/VirusTotal/yara.git
git checkout 890c3f850293176c0e996a602ffa88b315f4e98f
```
- Reproduce: `yara $POC strings`
- ASAN dumps the backtrace:
```
==10220==ERROR: AddressSanitizer: heap-use-after-free on address 0x63100003c862 at pc 0x0000004ef7f5 bp 0x7ffc9082a5d0 sp 0x7ffc9082a5c8
READ of size 8 at 0x63100003c862 thread T0
    #0 0x4ef7f4 in yr_compiler_destroy XYZ/yara/libyara/compiler.c:165:35
    #1 0x4ede04 in main XYZ/yara/yara.c:1242:5
    #2 0x7f05302fa82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #3 0x41a408 in _start (/usr/local/bin/yara+0x41a408)

0x63100003c862 is located 98 bytes inside of 65536-byte region [0x63100003c800,0x63100004c800)
freed by thread T0 here:
    #0 0x4b88cb in __interceptor_free /home/development/llvm/3.9.0/final/llvm.src/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:47:3
    #1 0x587ab3 in yr_arena_destroy XYZ/yara/libyara/arena.c:300:5
    #2 0x7f05302fa82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

previously allocated by thread T0 here:
    #0 0x4b8c1c in malloc /home/development/llvm/3.9.0/final/llvm.src/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:64:3
    #1 0x58774f in _yr_arena_new_page XYZ/yara/libyara/arena.c:92:34
    #2 0x58774f in yr_arena_create XYZ/yara/libyara/arena.c:246

SUMMARY: AddressSanitizer: heap-use-after-free XYZ/yara/libyara/compiler.c:165:35 in yr_compiler_destroy
Shadow bytes around the buggy address:
  0x0c627ffff8b0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8c0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8d0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8e0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c627ffff8f0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c627ffff900: fd fd fd fd fd fd fd fd fd fd fd fd[fd]fd fd fd
  0x0c627ffff910: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c627ffff920: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c627ffff930: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c627ffff940: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c627ffff950: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
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
==10220==ABORTING
```