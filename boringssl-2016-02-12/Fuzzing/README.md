### boringssl-2016-02-12 (In google testsuite)
- Bug type: use-after-free
- Download: https://github.com/google/fuzzer-test-suite/tree/master/boringssl-2016-02-12
- Time-cost: >1h
- Note: afl need a dirver
- ASAN dumps the backtrace:
```
=================================================================
==24118==ERROR: AddressSanitizer: heap-use-after-free on address 0x603000000258 at pc 0x0000006510d1 bp 0x7fff4d24d240 sp 0x7fff4d24d238
READ of size 8 at 0x603000000258 thread T0
    #0 0x6510d0 in ASN1_STRING_free /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/asn1/asn1_lib.c:459:12
    #1 0x66846b in ASN1_primitive_free /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/asn1/tasn_fre.c:241:9
    #2 0x6686ec in ASN1_primitive_free /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/asn1/tasn_fre.c:236:9
    #3 0x667147 in asn1_item_combine_free /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/asn1/tasn_fre.c:173:1
    #4 0x666596 in ASN1_item_free /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/asn1/tasn_fre.c:69:5
    #5 0x5560f2 in sk_pop_free /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/stack/stack.c:142:7
    #6 0x527f25 in dsa_priv_decode /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/evp/p_dsa_asn1.c:288:3
    #7 0x54435c in EVP_PKCS82PKEY /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/pkcs8/pkcs8.c:616:10
    #8 0x5211b7 in d2i_AutoPrivateKey /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/evp/evp_asn1.c:151:11
    #9 0x51aebd in main /home/hjwang/UAF_Objects/boringssl_afl_asan/Fuzzing/./afldirver_boringssl.c:27:23
    #10 0x7fb15596682f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291
    #11 0x41ac58 in _start (/home/hjwang/UAF_Objects/boringssl_afl_asan/Fuzzing/boringssl_test+0x41ac58)

0x603000000258 is located 8 bytes inside of 24-byte region [0x603000000250,0x603000000268)
freed by thread T0 here:
    #0 0x4decc8 in __interceptor_cfree.localalias.0 /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:76
    #1 0x527f18 in dsa_priv_decode /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/evp/p_dsa_asn1.c:287:3
    #2 0x54435c in EVP_PKCS82PKEY /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/pkcs8/pkcs8.c:616:10
    #3 0x5211b7 in d2i_AutoPrivateKey /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/evp/evp_asn1.c:151:11
    #4 0x51aebd in main /home/hjwang/UAF_Objects/boringssl_afl_asan/Fuzzing/./afldirver_boringssl.c:27:23
    #5 0x7fb15596682f in __libc_start_main /build/glibc-LK5gWL/glibc-2.23/csu/../csu/libc-start.c:291

previously allocated by thread T0 here:
    #0 0x4dee88 in __interceptor_malloc /home/hjwang/Tools/llvm-6.0.1/projects/compiler-rt/lib/asan/asan_malloc_linux.cc:88
    #1 0x651316 in ASN1_STRING_type_new /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/asn1/asn1_lib.c:443:26

SUMMARY: AddressSanitizer: heap-use-after-free /home/hjwang/UAF_Objects/boringssl_afl_asan/crypto/asn1/asn1_lib.c:459:12 in ASN1_STRING_free
Shadow bytes around the buggy address:
  0x0c067fff7ff0: 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
  0x0c067fff8000: fa fa fd fd fd fd fa fa fd fd fd fa fa fa fd fd
  0x0c067fff8010: fd fa fa fa fd fd fd fa fa fa fd fd fd fa fa fa
  0x0c067fff8020: 00 00 00 fa fa fa 00 00 00 fa fa fa 00 00 00 fa
  0x0c067fff8030: fa fa 00 00 02 fa fa fa 00 00 00 fa fa fa 00 00
=>0x0c067fff8040: 00 00 fa fa fd fd fd fa fa fa fd[fd]fd fa fa fa
  0x0c067fff8050: 00 00 00 00 fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8060: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8070: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8080: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c067fff8090: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
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
==24118==ABORTING
```