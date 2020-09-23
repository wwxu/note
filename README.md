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

