### Binutils 2.31.51.20190109 
- Bug type: use-after-free
- CVE ID: [CVE-2018-20623](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20623)
- Download: 
```
git clone git://sourceware.org/git/binutils-gdb.git
git checkout 923c6a756476f3a1f92d6625aacbbf5253b7739b
```
- Reproduce: `./readelf -a $POC`
- ASAN dumps the backtrace:
```
==5903==ERROR: AddressSanitizer: heap-use-after-free on address 0x60300000ef80 at pc 0x000000460cb6 bp 0x7ffcaada7ef0 sp 0x7ffcaada76a0
READ of size 2 at 0x60300000ef80 thread T0
    #0 0x460cb5 in printf_common(void*, char const*, __va_list_tag*) (/home/hjwang/UAF_Objects/binutils-2.31.51_afl_asan/build/bin/readelf+0x460cb5)
    #1 0x4605b2 in printf_common(void*, char const*, __va_list_tag*) (/home/hjwang/UAF_Objects/binutils-2.31.51_afl_asan/build/bin/readelf+0x4605b2)
    #2 0x4614aa in __interceptor_vfprintf (/home/hjwang/UAF_Objects/binutils-2.31.51_afl_asan/build/bin/readelf+0x4614aa)
    #3 0x6096a2 in error /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/elfcomm.c:43:3
    #4 0x504975 in process_archive /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/readelf.c:19375:8
    #5 0x4ef3b3 in main /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/readelf.c:19588:13
    #6 0x7faeaccaa82f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291
    #7 0x418f18 in _start (/home/hjwang/UAF_Objects/binutils-2.31.51_afl_asan/build/bin/readelf+0x418f18)

0x60300000ef80 is located 0 bytes inside of 19-byte region [0x60300000ef80,0x60300000ef93)
freed by thread T0 here:
    #0 0x4b8ec0 in __interceptor_cfree.localalias.0 (/home/hjwang/UAF_Objects/binutils-2.31.51_afl_asan/build/bin/readelf+0x4b8ec0)
    #1 0x504567 in process_archive /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/readelf.c:19524:7
    #2 0x4ef3b3 in main /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/readelf.c:19588:13
    #3 0x7faeaccaa82f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

previously allocated by thread T0 here:
    #0 0x4b9048 in __interceptor_malloc (/home/hjwang/UAF_Objects/binutils-2.31.51_afl_asan/build/bin/readelf+0x4b9048)
    #1 0x6122fa in make_qualified_name /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/elfcomm.c:906:19
    #2 0x5040ea in process_archive /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/readelf.c:19435:24
    #3 0x4ef3b3 in main /home/hjwang/UAF_Objects/binutils-2.31.51_Fuzzing/binutils/readelf.c:19588:13
    #4 0x7faeaccaa82f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

SUMMARY: AddressSanitizer: heap-use-after-free (/home/hjwang/UAF_Objects/binutils-2.31.51_afl_asan/build/bin/readelf+0x460cb5) in printf_common(void*, char const*, __va_list_tag*)
Shadow bytes around the buggy address:
  0x0c067fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9de0: fa fa fa fa fd fd fd fa fa fa fd fd fd fd fa fa
=>0x0c067fff9df0:[fd]fd fd fa fa fa fd fd fd fa fa fa 00 00 01 fa
  0x0c067fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff9e40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==5903==ABORTING
```