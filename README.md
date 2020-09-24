## Sextractor: 

# install FINK: 

download `fink-0.45.1.tar.gz` from https://www.finkproject.org/download/srcdist.php

tar -xvf fink-0.45.1.tar 

cd fink-0.45.1

vi INSTALL

./bootstrap 

. /Users/wwxu/software/autoconf/fink-0.45.1/bin/init.sh

add ` . /Users/wwxu/software/autoconf/fink-0.45.1/bin/init.sh` to ~/.bash_profile

/Users/wwxu/software/autoconf/fink-0.45.1/bin/pathsetup.sh

# install FFTW: 
download from `fftw-3.3.8.tar.gz` from http://www.fftw.org/download.html

./configure\\

make

make install


# install ATLAS:
download package from:  https://sourceforge.net/project/showfiles.php?group_id=23725

   mkdir my_build_dir ; cd my_build_dir
   
   /path/to/ATLAS/configure [flags]
   
   make              ! tune and compile library
   
   make check        ! perform sanity tests
   
   make ptcheck      ! checks of threaded code for multiprocessor systems
   
   make time         ! provide performance summary as % of clock rate
   
   make install      ! Copy library and include files to other directories

# Install SExtractor 
   sh autogen.sh
   
   ./configure --with-fftw-incdir=/usr/local/include/ --with-atlas-incdir=/usr/local/atlas/include
   ./configure --with-fftw-incdir=/usr/local/include/ --with-atlas-incdir=/usr/local/atlas/include --with-atlas=/usr/local/atlas
checking for sincosf... no
checking for strstr... yes
checking for sysconf... yes
checking for special C compiler options needed for large files... no
checking for _FILE_OFFSET_BITS value needed for large files... no
checking for _LARGEFILE_SOURCE value needed for large files... no
checking whether OpenBLAS is enabled... no
checking if model-fitting should be disabled (default=enabled)... no
checking for profiler mode... checking best linking option... no
checking /usr/local/include//fftw3.h usability... yes
checking /usr/local/include//fftw3.h presence... yes
checking for /usr/local/include//fftw3.h... yes
checking for fftwf_execute in -lfftw3f... yes
checking /usr/local/atlas/include/cblas.h usability... yes
checking /usr/local/atlas/include/cblas.h presence... yes
checking for /usr/local/atlas/include/cblas.h... yes
checking /usr/local/atlas/include/clapack.h usability... yes
checking /usr/local/atlas/include/clapack.h presence... yes
checking for /usr/local/atlas/include/clapack.h... yes
checking for library containing clapack_dpotrf... no
checking for library containing clapack_dpotrf... no
checking for library containing clapack_dpotrf... no
checking for library containing clapack_dpotrf... no
configure: error: ATLAS library files not found! Exiting.

./configure --with-fftw-incdir=/usr/local/include/ --with-atlas-incdir=/Users/wwxu/software/autoconf/ATLAS/my_build_dir/include --with-atlas=/Users/wwxu/software/autoconf/ATLAS/my_build_dir

checking for special C compiler options needed for large files... no
checking for _FILE_OFFSET_BITS value needed for large files... no
checking for _LARGEFILE_SOURCE value needed for large files... no
checking whether OpenBLAS is enabled... no
checking if model-fitting should be disabled (default=enabled)... no
checking for profiler mode... checking best linking option... no
checking /usr/local/include//fftw3.h usability... yes
checking /usr/local/include//fftw3.h presence... yes
checking for /usr/local/include//fftw3.h... yes
checking for fftwf_execute in -lfftw3f... yes
checking /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/cblas.h usability... no
checking /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/cblas.h presence... no
checking for /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/cblas.h... no
checking /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/cblas.h /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/clapack.h usability... no
checking /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/cblas.h /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/clapack.h presence... no
checking for /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/cblas.h /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/clapack.h... no
checking /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/clapack.h usability... no
checking /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/clapack.h presence... no
checking for /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/clapack.h... no
checking for /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/cblas.h /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include/include/clapack.h... (cached) no
./configure: line 15307: test: too many arguments
configure: error: ATLAS header files not found in /Users/wwxu/software/autoconf/ATLAS/my_build_dir/include! Exiting.

   make 
