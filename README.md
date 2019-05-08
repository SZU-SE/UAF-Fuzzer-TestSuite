# UAF-Fuzzer-TestSuite

Use-after-free testsuite used for fuzzing experiment

Seed and POC in the Fuzzing folder

### 1. Elfutils 0.173 [[Detail Info]](./elfutils-0.173/Fuzzing/README.md)
- Bug type: double-free
- CVE ID: [CVE-2018-16402](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-16402)
- Download: ftp://sourceware.org/pub/elfutils/0.173/
- Reproduce: `./eu-nm $POC`


### 2. Mini Xml v2.12 [[Detail Info]](./mxml/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2018-20592](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20592)
- Download: https://github.com/michaelrsweet/mxml
```
git clone https://github.com/michaelrsweet/mxml.git
git checkout 53c75b04c133a79fbf81782fa83d45a6c7d2dcf1
```
- Reproduce: `./mxmldoc $POC`


### 3. openh264 [[Detail Info]](./openh264/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [pending](https://github.com/cisco/openh264/issues/3108)
- Download: https://github.com/cisco/openh264
```
git clone https://github.com/cisco/openh264.git
git checkout 8684722271ac16118df2fe50322ffe218b9507a7
```
- Reproduce: `h264dec $POC ./tmp`


### 4. boolector [[Detail Info]](./boolector/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [pending](https://github.com/Boolector/boolector/issues/41)
- Download: https://github.com/Boolector/boolector
```
git clone https://github.com/Boolector/boolector.git
git checkout 0874a185cd98711b3e4a0b1a0c10e858ff4a23e6
```
- Reproduce: `boolector $POC`


### 5. libpff [[Detail Info]](./libpff/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [pending](https://github.com/libyal/libpff/issues/61)
- Download: https://github.com/libyal/libpff
```
git clone https://github.com/libyal/libpff.git
git checkout 4938b7a891c6ec2112e5f059e13426915ae49adb
```
- Reproduce: ```./pffexport $POC```
- Note: Compile failed

### 6. GNU cflow 1.6 [[Detail Info]](./cflow-1.6/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [pending](http://lists.gnu.org/archive/html/bug-cflow/2019-04/msg00001.html)
- Download: https://ftp.gnu.org/gnu/cflow/
- Reproduce: `./cflow $POC`


### 7. mjs
- Bug type: use-after-free
- Reference 1: https://github.com/cesanta/mjs/issues/73
- Reference 2: https://github.com/cesanta/mjs/issues/78


### 8. ImageMagick
- ImageMagick version: 7.0.8-43 Q16 x86_64 2019-04-27 
- Bug type: use-after-free
- CVE ID: [pending](https://github.com/ImageMagick/ImageMagick/issues/1554)
- Download:
```
git clone https://github.com/ImageMagick/ImageMagick.git
git checkout 3183a88ae19674b4625e447d5a29da2a12d742c0
```
- Reproduce: convert $POC /dev/null

### 9. mupdf
- Bug type: use-after-free
- CVE ID: [pending](https://bugs.ghostscript.com/show_bug.cgi?id=701018)
- Download:
```
git clone git://git.ghostscript.com/mupdf.git
git checkout 4422de4f6756b7dc19ca915ae6d63f5ada718ae7
git submodule update --init --recursive
make sanitize
```
- Reproduce: muraster $POC


### 10. NASM 2.14.02 [[Detail Info]](./nasm/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2019-8343](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8343), [CVE-2018-20535](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20535), [CVE-2018-20538](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20538)
- Download: https://repo.or.cz/w/nasm.git
- Reproduce: `./nasm -f bin $POC -o /dev/null`


### 11. Binutils 2.31.51.20190109 [[Detail Info]](./binutils-2.31.51/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2018-20623](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-20623)
- Download: 
```
git clone git://sourceware.org/git/binutils-gdb.git
git checkout 923c6a756476f3a1f92d6625aacbbf5253b7739b
```
- Reproduce: `./readelf -a $POC`


### 12. Binutils 2.28 [[Detail Info]](./binutils-2.28/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2017-6966](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6966)
- Download: https://ftp.gnu.org/gnu/binutils/
- Reproduce: `./readelf -w $POC`


### 13. lrzip [[Detail Info]](./lrzip/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2018-11496](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11496)
- Download: https://github.com/ckolivas/lrzip
```
git clone https://github.com/ckolivas/lrzip.git
git checkout ed51e14a4b7e921cd5e633100ec7403e120f6477
```
- Reproduce: `./lrzip -t $POC`


### 14. jpegoptim [[Detail Info]](./jpegoptim/Fuzzing/README.md)
- Bug type: double free
- CVE ID: [CVE-2018-11416](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-11416)
- Download: https://github.com/tjko/jpegoptim
```
git clone https://github.com/tjko/jpegoptim.git
git checkout d23abf2c59692e0e3638ce8c89d98a3628c686b7
```
- Reproduce: `./jpegoptim $POC`


### 15. yara [[Detail Info]](./yara/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2017-5924](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5924)
- Download: https://github.com/VirusTotal/yara
```
git clone https://github.com/VirusTotal/yara.git
git checkout 890c3f850293176c0e996a602ffa88b315f4e98f
```
- Reproduce: `yara $POC strings`


### 16. liblouis v3.2.0 [[Detail Info]](./liblouis-3.2.0/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2017-13741](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-13741)
- Download: https://github.com/liblouis/liblouis/releases/tag/v3.2.0
- Reproduce: `./lou_checktable $POC`


### 17. GraphicsMagick 1.3.26 [[Detail Info]](./GraphicsMagick-1.3.26/Fuzzing/README.md)
- Bug type: use-after-free
- CVE ID: [CVE-2017-11403](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-11403)
- Download: https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick/1.3.26/
- Reproduce: `./gm identify $POC`


### 18. boringssl-2016-02-12 (In google testsuite) [[Detail Info]](./boringssl-2016-02-12/Fuzzing/README.md)
- Bug type: use-after-free
- Download: https://github.com/google/fuzzer-test-suite/tree/master/boringssl-2016-02-12
- Time-cost: >1h
- Note: afl need a dirver