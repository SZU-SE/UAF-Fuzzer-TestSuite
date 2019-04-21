# UAF-Fuzzer-TestSuite

Use-after-free testsuite used for fuzzing experiment

Seed 和 POC 在 Fuzzing 文件夹下

### Bug we found:

1. Elfutils 0.173
Bug type: double-free
CVE ID: CVE-2018-16402
Download: ftp://sourceware.org/pub/elfutils/0.173/
Reproduce: ./eu-nm $POC


2. Mini Xml v2.12
Bug type: use-after-free
CVE ID: CVE-2018-20592
Download: https://github.com/michaelrsweet/mxml
git clone https://github.com/michaelrsweet/mxml.git
git checkout 53c75b04c133a79fbf81782fa83d45a6c7d2dcf1

Reproduce: ./mxmldoc $POC


3. openh264
Bug type: use-after-free
CVE ID: pending
Download: https://github.com/cisco/openh264
git clone https://github.com/cisco/openh264.git
git checkout 8684722271ac16118df2fe50322ffe218b9507a7

Reproduce: h264dec $POC ./tmp

4. boolector
Bug type: use-after-free
CVE ID: pending
Download: https://github.com/Boolector/boolector
git clone https://github.com/Boolector/boolector.git
git checkout 0874a185cd98711b3e4a0b1a0c10e858ff4a23e6

Reproduce: boolector $POC


5. libpff
Bug type: use-after-free
CVE ID: pending
Download: https://github.com/libyal/libpff
git clone https://github.com/libyal/libpff.git
git checkout 4938b7a891c6ec2112e5f059e13426915ae49adb

Reproduce:  Unknown
(Note: Compile failed)

6. GNU cflow 1.6
Bug type: use-after-free
CVE ID: pending
Download: https://ftp.gnu.org/gnu/cflow/
Reproduce: ./cflow $POC


7. mjs
Bug type: use-after-free
CVE ID: not apply
Reference: https://github.com/cesanta/mjs/issues/73
Reference: https://github.com/cesanta/mjs/issues/78


8. Binaryen 1.38.22
Bug type: use-after-free (false positive, buffer-overflow)
CVE ID: CVE-2019-7703
Download：https://github.com/WebAssembly/binaryen
git clone https://github.com/WebAssembly/binaryen.git
git checkout 45714b5fc6cf14c112bc4f188aca427464ab69d8

Reproduce: "./wasm-merge $POC


### Other test Dataset:


9. NASM 2.14.02
Bug type: use-after-free
CVE ID: CVE-2019-8343, CVE-2018-20535, CVE-2018-20538
Download: https://repo.or.cz/w/nasm.git
Reproduce: ./nasm -f bin $POC -o /dev/null



10. Binutils 2.31.51.20190109
Bug type: use-after-free
CVE ID: CVE-2018-20623
Download: 
git clone git://sourceware.org/git/binutils-gdb.git
git checkout 923c6a756476f3a1f92d6625aacbbf5253b7739b
Reproduce: ./readelf -a $POC


11. Binutils 2.28
Bug type: use-after-free
CVE ID: CVE-2017-6966
Download: https://ftp.gnu.org/gnu/binutils/
Reproduce: ./readelf -w $POC


12. lrzip
Bug type: use-after-free
CVE ID: CVE-2018-11496
Download: https://github.com/ckolivas/lrzip
git clone https://github.com/ckolivas/lrzip.git
git checkout ed51e14a4b7e921cd5e633100ec7403e120f6477

Reproduce: ./lrzip -t $POC


13. jpegoptim
Bug type: double free
CVE ID: CVE-2018-11416
Download: https://github.com/tjko/jpegoptim
git clone https://github.com/tjko/jpegoptim.git
git checkout d23abf2c59692e0e3638ce8c89d98a3628c686b7

Reproduce: ./jpegoptim $POC


14. yara
Bug type: use-after-free
CVE ID: CVE-2017-5924
Download: https://github.com/VirusTotal/yara
git clone https://github.com/VirusTotal/yara.git
git checkout 890c3f850293176c0e996a602ffa88b315f4e98f

Reproduce: yara $POC strings


15. liblouis v3.2.0
Bug type: use-after-free
CVE ID: CVE-2017-13741
Download: https://github.com/liblouis/liblouis/releases/tag/v3.2.0
Reproduce: ./lou_checktable $POC


16. GraphicsMagick 1.3.26
Bug type: use-after-free
CVE ID: CVE-2017-11403
Download: https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick/1.3.26/
Reproduce: ./gm identify $POC


17. boringssl-2016-02-12 (In google testsuite)
Bug type: use-after-free
Download: https://github.com/google/fuzzer-test-suite/tree/master/boringssl-2016-02-12
Time-cost: >1h
Note: afl need a dirver