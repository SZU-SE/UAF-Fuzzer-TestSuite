### lrzip
- Bug type: use-after-free
- CVE ID: [CVE-2018-11496](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11496)
- Download: https://github.com/ckolivas/lrzip
```
git clone https://github.com/ckolivas/lrzip.git
git checkout ed51e14a4b7e921cd5e633100ec7403e120f6477
```
- Reproduce: `./lrzip -t $POC`
- ASAN dumps the backtrace:
```
==3378==ERROR: AddressSanitizer: heap-use-after-free on address 0x60200000efd0 at pc 0x7f4dbb805935 bp 0x7ffe50203a50 sp 0x7ffe502031f8
READ of size 1 at 0x60200000efd0 thread T0
    #0 0x7f4dbb805934 in __asan_memcpy (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x8c934)
    #1 0x434d81 in read_stream /home/hjwang/UAF_Objects/lrzip-gdb_asan/stream.c:1756
    #2 0x4234d9 in read_vchars /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:79
    #3 0x423ec5 in read_header /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:147
    #4 0x4254f1 in runzip_chunk /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:316
    #5 0x4259eb in runzip_fd /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:384
    #6 0x411b48 in decompress_file /home/hjwang/UAF_Objects/lrzip-gdb_asan/lrzip.c:838
    #7 0x409e5a in main /home/hjwang/UAF_Objects/lrzip-gdb_asan/main.c:675
    #8 0x7f4dba2c382f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #9 0x4033f8 in _start (/home/hjwang/UAF_Objects/lrzip-gdb_asan/build/bin/lrzip+0x4033f8)

0x60200000efd0 is located 0 bytes inside of 10-byte region [0x60200000efd0,0x60200000efda)
freed by thread T0 here:
    #0 0x7f4dbb8112ca in __interceptor_free (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x982ca)
    #1 0x432c9b in fill_buffer /home/hjwang/UAF_Objects/lrzip-gdb_asan/stream.c:1573
    #2 0x434e35 in read_stream /home/hjwang/UAF_Objects/lrzip-gdb_asan/stream.c:1764
    #3 0x4234d9 in read_vchars /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:79
    #4 0x423ec5 in read_header /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:147
    #5 0x4254f1 in runzip_chunk /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:316
    #6 0x4259eb in runzip_fd /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:384
    #7 0x411b48 in decompress_file /home/hjwang/UAF_Objects/lrzip-gdb_asan/lrzip.c:838
    #8 0x409e5a in main /home/hjwang/UAF_Objects/lrzip-gdb_asan/main.c:675
    #9 0x7f4dba2c382f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

previously allocated by thread T0 here:
    #0 0x7f4dbb811602 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98602)
    #1 0x433b4a in fill_buffer /home/hjwang/UAF_Objects/lrzip-gdb_asan/stream.c:1651
    #2 0x434e35 in read_stream /home/hjwang/UAF_Objects/lrzip-gdb_asan/stream.c:1764
    #3 0x423145 in read_u8 /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:55
    #4 0x423e0a in read_header /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:144
    #5 0x4254f1 in runzip_chunk /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:316
    #6 0x4259eb in runzip_fd /home/hjwang/UAF_Objects/lrzip-gdb_asan/runzip.c:384
    #7 0x411b48 in decompress_file /home/hjwang/UAF_Objects/lrzip-gdb_asan/lrzip.c:838
    #8 0x409e5a in main /home/hjwang/UAF_Objects/lrzip-gdb_asan/main.c:675
    #9 0x7f4dba2c382f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-use-after-free ??:0 __asan_memcpy
Shadow bytes around the buggy address:
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dd0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9de0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
=>0x0c047fff9df0: fa fa fa fa fa fa fd fd fa fa[fd]fd fa fa 04 fa
  0x0c047fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e40: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==3378==ABORTING
```