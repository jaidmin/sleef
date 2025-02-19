# SLEEF

![Github Actions](https://github.com/shibatch/sleef/actions/workflows/build_and_test.yml/badge.svg?event=push&branch=master)
[![DOI:10.1109/TPDS.2019.2960333](http://img.shields.io/badge/DOI-10.1109/TPDS.2019.2960333-blue.svg)](https://ieeexplore.ieee.org/document/8936472)
[![License](https://img.shields.io/badge/License-Boost_1.0-lightblue.svg)](https://www.boost.org/LICENSE_1_0.txt)
![CMake](https://img.shields.io/badge/cmake-v3.18+-yellow.svg)
[![Spack](https://img.shields.io/spack/v/sleef)](https://spack.readthedocs.io/en/v0.16.2/package_list.html#sleef)
[![SourceForge Downloads](https://img.shields.io/sourceforge/dt/sleef)](https://sourceforge.net/projects/sleef/)

SLEEF is a library that implements vectorized versions of C standard math functions. This library also includes DFT subroutines.

- **Web Page:** [https://sleef.org/][webpage_url]
- **Sources:** [https://github.com/shibatch/sleef][repo_url]

## Supported vector extensions

### Warning

Due to limited test capacities, SLEEF is currently only officially supported on Linux with gcc or llvm/clang.
[This issue](https://github.com/shibatch/sleef/issues/481) tracks progress on improving test coverage.
Compilation of SLEEF on previously supported environments might still be safe, we just cannot verify it yet.

### Summary

The following table summarises currently supported vector extensions, compilers and OS-es.

:green_circle: : Tested extensively in CI.

:x: : Currently failing some tests in CI.

:white_circle: : Not tested in CI. Might have passed tests in previous CI framework.

<table>
<tr>
  <th colspan="2" rowspan="2"></th>
  <th colspan="8">OS/Compiler</th>
</tr>
<tr>
  <th colspan="3">Linux</th>
  <th colspan="2">MacOS</th>
  <th colspan="3">Windows</th>
</tr>
<tr>
  <th>Arch.</th>
  <th>Vector Extensions</th>
  <th>gcc</th><th>llvm</th><th>icc</th>
  <th>gcc</th><th>llvm</th>
  <th>gcc</th><th>llvm</th><th>msvc</th>
</tr>
<tr align="center"><th>x86_64</th><th>SSE2, SSE4,<br>AVX, AVX2, AVX512F</th>
  <td>:green_circle:</td><td>:green_circle:</td><td>:white_circle:</td>
  <td>:white_circle:</td><td>:white_circle:</td>
  <td>:white_circle:</td><td>:white_circle:</td><td>:white_circle:</td>
</tr>
<tr align="center"><th>x86 32bit<br>(i386)</th><th>SSE</th>
  <td>:green_circle:</td><td>:green_circle:</td><td>:white_circle:</td>
  <td>:white_circle:</td><td>:white_circle:</td>
  <td>:white_circle:</td><td>:white_circle:</td><td>:white_circle:</td>
</tr>
<tr align="center"><th>AArch64<br>(arm)</th><th>Neon, SVE</th>
  <td>:green_circle:</td><td>:green_circle:</td><td>N/A</td>
  <td colspan="2">Preliminary</td>
  <td colspan="3">N/A</td>
</tr>
<tr align="center"><th>AArch32<br>(armhf)</th><th>NEON</th>
  <td>:green_circle:</td><td>:green_circle:</td><td>N/A</td>
  <td colspan="2">Preliminary</td>
  <td colspan="3">N/A</td>
</tr>
<tr align="center"><th>PowerPC<br>(ppc64el)</th><th>VSX, VSX3</th>
  <td>:green_circle:</td><td>:green_circle:</td><td>N/A</td>
  <td colspan="2">N/A</td>
  <td colspan="3">N/A</td>
</tr>
<tr align="center"><th>IBM/Z<br>(s390x)</th><th>VXE, VXE2</th>
  <td>:green_circle:</td><td>:green_circle:</td><td>N/A</td>
  <td colspan="2">N/A</td>
  <td colspan="3">N/A</td>
</tr>
<tr align="center"><th>RISC-V<br>(riscv64)</th><th>RVV1, RVV2</th>
  <td>N/A (14+)</td><td>:green_circle:</td><td>N/A</td>
  <td colspan="2">N/A</td>
  <td colspan="3">N/A</td>
</tr>
</table>

### Component support

The above table is valid for libm in single, double and quadruple precision, as well as fast Discrete Fourier Transform (DFT).

Generation of inline headers is also supported for most vector extensions.

#### Work in progress

- DFT, Quad. and inline headers generations are not supported with RISC-V yet.
- LTO is not tested in CI yet.

### Compiler support

Results are displayed for gcc 11 and llvm 17, the compiler versions used in CI tests with GitHub Actions.

Older versions should be supported too, while newer ones are either not tested or have known issues.

### OS support

Only Linux distributions are currently tested in CI and thus officially supported.

Building SLEEF for MacOS and Windows on x86 machines was officially supported ( :white_circle: ), as of 3.5.1, however it is not currently tested.

Support for MacOS, iOS and Android is only preliminary on AArch64.

SVE is not supported on Darwin-based system and therefore automatically disabled by SLEEF on Darwin.

### More on supported environment

Refer to our web page for [more on supported environment][supported_env_url].

## Install SLEEF dependencies

The library itself does not have any additional dependency.

However some tests require:

- libssl and libcrypto, that can be provided by installing openssl.
- libm, libgmp and libmpfr
- libfftw.

These tests can be disabled if necessary.

## How to build SLEEF

We recommend relying on CMake as much as possible in the build process to ensure portability.
**CMake 3.18+** is the minimum required.

1. Check out the source code from our GitHub repository

```
git clone https://github.com/shibatch/sleef
```

2. Make a separate directory to create an out-of-source build

```
cd sleef && mkdir build
```

3. Run cmake to configure the project

```
cmake -S . -B build
```

4. Run make to build the project

```
cmake --build build -j --clean-first
```

5. Run tests using ctests

```
ctest --test-dir build -j
```

For more detailed build instructions please refer to the [dedicated section on CMake](./docs/build-with-cmake.md) or to [our web page][build_info_url].

## Install SLEEF

### From source

Assuming following instructions were followed.

6. Install to specified directory `<prefix>`

```
cmake --install build --prefix=<prefix>
```

### Using Spack

SLEEF can also be directly installed using Spack.

```
spack install sleef@master
```

### Uninstall

In order to uninstall SLEEF, run

```
sudo xargs rm -v &lt; install_manifest.txt
```

## License

The software is distributed under the Boost Software License, Version 1.0.
See accompanying file [LICENSE.txt](./LICENSE.txt) or copy at [http://www.boost.org/LICENSE_1_0.txt][license_url].
Contributions to this project are accepted under the same license.

Copyright &copy; 2010-2024 SLEEF Project, Naoki Shibata and contributors.<br/>


<!-- Repository links -->

[webpage_url]: https://sleef.org/
[build_info_url]: https://sleef.org/compile.xhtml
[supported_env_url]: https://sleef.org/index.xhtml#environment
[repo_url]: https://github.com/shibatch/sleef
[repo_license_url]: https://github.com/shibatch/sleef/blob/main/LICENSE.txt
[license_url]: http://www.boost.org/LICENSE_1_0.txt
