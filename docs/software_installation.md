# Software installation

How to install common pulsar software and some of the common problems you may encounter.

## Containers

An easy and quick was to install software is to use a container.
Have you ever been debugging software with someone and they say "well it works on my machine",
containers tries reduce such problems by enabling you to virtually ship and entire computer
(operating system and install software dependencies) to another user.
These containers can be downloaded and run on your machine so the software
is much more likely to behave in the same way.

The two most common container software packages are [Docker](https://docs.docker.com/) (for most computers)
and [Singularity/Apptainer](http://apptainer.org/) (for use on supercomputers).
If you are not doing large scale processing (hundreds of jobs) then a container may be all you need.
The following are a list of pulsar software docker containers that may suit your needs

- [psr-analysis](https://hub.docker.com/repository/docker/cirapulsarsandtransients/psr-analysis/general) ([repo](https://github.com/CIRA-Pulsars-and-Transients-Group/psrchive_docker/blob/main/Dockerfile.psr-analysis)) (Tempo, Tempo2, PINT, psrcat, PSRCHIVE, DSPSR and PulsePortraiture)
- [psr-search](https://hub.docker.com/repository/docker/cirapulsarsandtransients/psr-search/general) ([repo](https://github.com/CIRA-Pulsars-and-Transients-Group/psrchive_docker/blob/main/Dockerfile.psr-search)) (Tempo, psrcat, PRESTO, and riptide (FFA))
- [presto](https://hub.docker.com/repository/docker/cirapulsarsandtransients/presto/general) ([repo](https://github.com/scottransom/presto/blob/master/Dockerfile))

## Dependencies

You will need to install the following packages with a package manager:

```bash
libtool
libfftw3-3
libfftw3-dev
libcfitsio8
libcfitsio-dev
wget
libpng-dev
libgd-dev
autoconf
automake
libtool
m4
git
gsl-bin
libgsl-dev
flex
bison
fort77
libglib2.0-dev
gnuplot
python-dev
python-numpy
python-scipy
python-matplotlib
python-setuptools
python-sympy
python-nose
swig
libltdl-dev
libltdl7
cmake
libblas3
liblapack3
libblas-dev
liblapack-dev
libxext-dev
libx11-dev
libopenmpi-dev
openmpi-bin
libhdf5-openmpi-dev
mpich
libmpich-dev
libhdf5-mpich-dev
tcsh
pgplot5
imagemagick
bc
latex2html
gfortran
```

We will assume you are going to install this into a directory `ASTROSOFT` where you will run most of
the following commands and install the software to.
Define this location and other environmental variables by adding the following to your `.bashrc` (based on [Lawrence Tommey's documentation](https://www.atnf.csiro.au/people/Lawrence.Toomey/pulsarref/pulsar-software-install-ubuntu-64bit.html))


```bash
# Path to the pulsar software installation directory e.g:
export ASTROSOFT=/home/${USER}/pulsar_software

# OSTYPE
export OSTYPE=linux

# PSRCAT
export PSRCAT_RUNDIR=$ASTROSOFT/psrcat_tar
export PSRCAT_FILE=$ASTROSOFT/psrcat_tar/psrcat.db

# Tempo
export TEMPO=$ASTROSOFT/tempo

# Tempo2
export TEMPO2=$ASTROSOFT/tempo2/T2runtime

# PRESTO
export PRESTO=$ASTROSOFT/presto

# MULTINEST
export MULTINEST_DIR=$ASTROSOFT/TempoNest/MultiNest

# LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/lib:/usr/lib/x86_64-linux-gnu:$PGPLOT_DIR:$ASTROSOFT/lib:$PRESTO/lib:$PRESTO/lib64:$MULTINEST_DIR

# PATH
# Some Presto executables match sigproc executables so keep separate -
# all other executables are found in $ASTROSOFT/bin
export PATH=$PATH:$ASTROSOFT/bin:$PRESTO/bin:$PGPLOT_DIR

# PYTHON PATH eg.
export PYTHONPATH=$PRESTO/lib/python:$PRESTO/lib64/python:/usr/lib/python2.7/site-packages/:/usr/lib64/python2.7/site-packages:$ASTROSOFT/lib/python2.7/site-packages
```

### Installing in a debian OS (easy)

If you would like to install this software on your personal computer (not a supercomputer)
the following command will install the required dependencies on linux debian operating systems (Ubuntu, Mint, etc.).

```bash
sudo apt install libtool libfftw3-3 libfftw3-dev libcfitsio8 libcfitsio-dev wget libpng-dev libgd-dev autoconf automake libtool m4 git gsl-bin libgsl-dev flex bison fort77 libglib2.0-dev gnuplot python-dev python-numpy python-scipy python-matplotlib python-setuptools python-sympy python-nose swig libltdl-dev libltdl7 cmake libblas3 liblapack3 libblas-dev liblapack-dev libxext-dev libx11-dev libopenmpi-dev openmpi-bin libhdf5-openmpi-dev mpich libmpich-dev libhdf5-mpich-dev tcsh pgplot5 imagemagick bc latex2html gfortran
```

### Install manually (hard)

If you would like to install these dependencies manually
(because you are installing it on supercomputer perhaps)
then the following sections will show you how install each dependency manually.

#### FFTW

```bash
autoreconf --verbose --install --symlink --force
./configure --enable-shared --enable-float --enable-sse --disable-dependency-tracking [--prefix=${install_dir}]
make clean
make
make install
```

#### psrfits_utils

#### psrxml

#### calceph

#### cfitsio

#### splinter

#### psrdada

```
git clone ssh://ajameson@git.code.sf.net/p/psrdada/code psrdada"
./bootstrap
./configure --prefix=${install_dir} --with-cuda-dir=$EBROOTCUDA --with-sofa-dir=$PSRHOME_SOFA_PATH --with-fftw3-dir=$PSRHOME_FFTW_SP_PATH --enable-shared
# make clean
make
make install
make clean
```

You may see an error that looks like this:
```
... multiple definition of ... first defined here
collect2: error: ld returned 1 exit status
```
This is due to new compilers (gcc version >9) not liking the old variable definition style.
You can make it ignore these issues by adding a compiler option like so:
```bash
make CFLAGS=-fcommon
```



## pgplot

## psrcat

```bash
wget https://www.atnf.csiro.au/research/pulsar/psrcat/downloads/psrcat_pkg.tar.gz
gunzip -c psrcat_pkg.tar.gz | tar xvf -
cd psrcat_tar
source makeit
# Move the psrcat executable to the path:
cp psrcat $ASTROSOFT/bin
```

You may see an error that looks like this:
```
... multiple definition of ... first defined here
collect2: error: ld returned 1 exit status
```
This is due to new compilers (gcc version >9) not liking the old variable definition style.
You can make it ignore these issues by adding a compiler option like so:
```bash
sed -i 's&/usr/bin/gcc&/usr/bin/gcc -fcommon&g' makeit
```
Then recompile (run `source makeit` again)




## tempo

## tempo2

```
git clone https://bitbucket.org/psrsoft/tempo2.git
cd tempo2
./bootstrap
./configure --prefix=${ASTROSOFT} --with-calceph=${PSRHOME_CALCEPH_PATH}
make
make plugins
make install
make plugins-install
make clean
```


## presto

## psrchive


```
git clone git://git.code.sf.net/p/psrchive/code psrchive
cd psrchive
./bootstrap
./configure --prefix=$ASTROSOFT --with-psrxml-dir=${PSRHOME_PSRXML_PATH} --enable-shared
make
make install
make clean
```

If you see an error that looks similar to:
```
 1063 |         CALL REGFA1(HEF,HMF2,XE2H,NMF2,0.001,NMF1,XE2,SCHALT,HMF1)
      |                                             2
......
 1311 |       CALL REGFA1(130.0,500.0,TI13,TI50,0.01,TI1,TEDER,SCHALT,HS)
      |                                             1
Error: Type mismatch between actual argument at (1) and actual argument at (2) (REAL(4)/INTEGER(4)).
```
This is due to a type missmatch that modern gfortran compilers don't like, to ignore it run
```
export FFLAGS="-fallow-argument-mismatch"
```
Then recompile (run the `make` commands again)


## dspsr


```
git clone git://git.code.sf.net/p/dspsr/code dspsr
cd dspsr
./bootstrap
./configure --prefix=${install_dir} --with-psrxml-dir=${PSRHOME_PSRXML_PATH} >& ${build_dir}/build.log
make
make install
make clean
```

If you see errors like this:
```
error: call to non-'constexpr' function 'long int sysconf(int)'
```
and
```
error: size of array 'altStackMem' is not an integral constant-expression
```

This is because it using a version of `catch2` with a bug in it that can be fixed by running
```
sed -i 's/constexpr static std::size_t sigStackSize = 32768 >= MINSIGSTKSZ ? 32768 : MINSIGSTKSZ;/constexpr static std::size_t sigStackSize = 32768;/' test/catch.hpp
```
then recompiling (running the `make` commands again)


## sigproc

```
git clone https://github.com/SixByNine/sigproc.git
cd sigproc
./bootstrap
./configure --prefix=${install_dir} --with-psrxml-dir=${PSRHOME_PSRXML_PATH}
make
make install
make clean
```

You may get an error like this
```
Error: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
```
You can make the compiler ignore this with the option `-fallow-argument-mismatch` by running make again like so
```
make FFLAGS=-fallow-argument-mismatch


If you get an error like this
```
... multiple definition of ... first defined here
collect2: error: ld returned 1 exit status
```
it is the old gcc error again. Fix it like this
```
make CFLAGS=-fcommon
```

If you get an error like this
```
inject_pulsar.c:548:35: error: 'nprof' not specified in enclosing 'parallel'
  548 |                 for(unsigned i=0; i < nprof;i++){
      |                                   ^
inject_pulsar.c:547:9: note: enclosing 'parallel'
  547 | #pragma omp parallel for  default(none) shared(subpulse_map)
      |         ^~~
```
This is because some of the variables in the parallel for loop
were not explictly labeled as private or shared.
The easiest way to fix this is to turn off `default(none)` like so
