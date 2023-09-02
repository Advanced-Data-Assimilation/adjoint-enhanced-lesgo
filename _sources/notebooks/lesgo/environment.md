# LESGO Environment Setup

This file is to record how to setup the required environments for lesgo.

## Local Machine

Required packages:
1. cmake/3.15.4   
2. intel/2018.2.199
3. openmpi/intel/2018.2.199/4.0.0


### Environment Modules

Download the latest [Environment Modules](https://modules.sourceforge.net/) and install following this [guide](https://modules.readthedocs.io/en/stable/INSTALL.html).

### Cmake


### Fortran Compiler

There are two option for fortran compiler:
1. Intel [Guidance](https://www.intel.com/content/www/us/en/developer/articles/guide/installing-intel-parallel-studio-xe-runtime-2018-using-apt-repository.html)
2. [GNU GCC](https://gcc.gnu.org/install/) and [Guidance](https://iamsorush.com/posts/build-gcc11/)

```bash
../gcc/configure -v --build=x86_64-linux-gnu --host=x86_64-linux-gnu --target=x86_64-linux-gnu --prefix=/usr/local/gcc-9.3.0 --enable-checking=release --enable-languages=c,c++,fortran --disable-multilib
```

### OPENMPI

Download [Open-MPI](https://www.open-mpi.org/software/ompi/v4.1/) and install following the [guidance](https://www.open-mpi.org/faq/?category=building).


```bash
./configure --prefix=/usr/local/openmpi-4.1.5/9.3.0
```


```bash
./configure --prefix=/usr/local/openmpi-4.1.5/9.3.0 --with-cuda=/usr/local/cuda-11.8
```


### FFTW3

Load GCC 9.3.0
```bash
./configure --prefix=/usr/local/fftw/3.3.10-GNU-9.3.0-OpenMPI-4.1.5 --enable-mpi
```