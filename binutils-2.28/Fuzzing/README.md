### Binutils 2.28
- Bug type: use-after-free
- CVE ID: [CVE-2017-6966](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6966)
- Download: https://ftp.gnu.org/gnu/binutils/
- Reproduce: `./readelf -w $POC`
- ASAN dumps the backtrace:
```
==5922==ERROR: AddressSanitizer: heap-use-after-free on address 0x61700000fe00 at pc 0x0000004f2e2b bp 0x7fffe4978c00 sp 0x7fffe4978bf8
READ of size 8 at 0x61700000fe00 thread T0
    #0 0x4f2e2a in target_specific_reloc_handling /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:11631:21
    #1 0x4ef1ec in apply_relocations /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12327:8
    #2 0x4ebccb in load_specific_debug_section /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12889:5
    #3 0x556958 in display_debug_section /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12993:6
    #4 0x511a73 in process_section_contents /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:13075:2
    #5 0x4f7e14 in process_object /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:16764:3
    #6 0x4ed870 in process_file /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:17138:13
    #7 0x4ec42c in main /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:17209:12
    #8 0x7fea0ad6782f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291
    #9 0x418dd8 in _start (/home/hjwang/UAF_Objects/binutils-2.28_asan/build/bin/readelf+0x418dd8)

0x61700000fe00 is located 384 bytes inside of 704-byte region [0x61700000fc80,0x61700000ff40)
freed by thread T0 here:
    #0 0x4b8d80 in __interceptor_cfree.localalias.0 (/home/hjwang/UAF_Objects/binutils-2.28_asan/build/bin/readelf+0x4b8d80)
    #1 0x4efa60 in apply_relocations /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12422:7
    #2 0x4ebccb in load_specific_debug_section /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12889:5
    #3 0x556958 in display_debug_section /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12993:6
    #4 0x511a73 in process_section_contents /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:13075:2
    #5 0x4f7e14 in process_object /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:16764:3
    #6 0x4ed870 in process_file /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:17138:13
    #7 0x4ec42c in main /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:17209:12
    #8 0x7fea0ad6782f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

previously allocated by thread T0 here:
    #0 0x4b8f08 in __interceptor_malloc (/home/hjwang/UAF_Objects/binutils-2.28_asan/build/bin/readelf+0x4b8f08)
    #1 0x5bd1d7 in xmalloc /home/hjwang/UAF_Objects/binutils-2.28/libiberty/./xmalloc.c:148:12
    #2 0x56ff59 in cmalloc /home/hjwang/UAF_Objects/binutils-2.28/binutils/dwarf.c:7450:10
    #3 0x4f234e in get_64bit_elf_symbols /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:5410:32
    #4 0x4ef0b5 in apply_relocations /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12315:16
    #5 0x4ebccb in load_specific_debug_section /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12889:5
    #6 0x556958 in display_debug_section /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:12993:6
    #7 0x511a73 in process_section_contents /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:13075:2
    #8 0x4f7e14 in process_object /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:16764:3
    #9 0x4ed870 in process_file /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:17138:13
    #10 0x4ec42c in main /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:17209:12
    #11 0x7fea0ad6782f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

SUMMARY: AddressSanitizer: heap-use-after-free /home/hjwang/UAF_Objects/binutils-2.28/binutils/readelf.c:11631:21 in target_specific_reloc_handling
Shadow bytes around the buggy address:
  0x0c2e7fff9f70: 00 00 00 00 00 00 00 00 fa fa fa fa fa fa fa fa
  0x0c2e7fff9f80: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2e7fff9f90: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c2e7fff9fa0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c2e7fff9fb0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
=>0x0c2e7fff9fc0:[fd]fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c2e7fff9fd0: fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd fd
  0x0c2e7fff9fe0: fd fd fd fd fd fd fd fd fa fa fa fa fa fa fa fa
  0x0c2e7fff9ff0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2e7fffa000: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c2e7fffa010: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==5922==ABORTING
```