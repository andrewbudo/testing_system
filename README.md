# BNTU testing system

BNTU_testing_system is a small project for testing students exercises after the theoretical lessons.  [![Build Status](https://travis-ci.org/andrewbudo/testing_system.svg?branch=devel)](https://travis-ci.org/andrewbudo/testing_system)

## Table of Contents
- [Dependencies](#dependencies)
- [Building](#building)
- [Build options](#build-options)
- [Installing dependencies](#installing-dependencies)

<a name="dependencies"></a>
## Dependencies
`BNTU testing system` depends on:

* C++17 compatible compiler
* CMake (3.2.0+, build only)
* Boost tests (optional)

<a name="building"></a>
## Building

In general, you need to install all `BNTU_testing_system` [dependencies](#dependencies) as described in [Installing dependencies](#installing-dependencies).
Then enter directory containing `BNTU_testing_system` sources and compile them using CMake:

```sh
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
cmake --build .
```

<a name="installing-dependencies"></a>
### Installing dependencies

<a name="linux"></a>
#### Linux
Install all [dependencies](#dependencies) using your package manager.

For example, run the following commands for Ubuntu 18.04:
* Cmake

```sh
sudo apt-get install cmake -y
```
* Boost.Test

```sh
sudo apt-get install libboost-test-dev -y
```

