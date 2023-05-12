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


## General compiling tips

Compilers output a huge amount of text and it can be hard to understand.
If you're not even sure if it ran successfully, one thing you can do is check the exitcode with
```
echo $?
```
If it outputs `0` then it compiled successfully.
All non-zero numbers are errorcodes which try to help describe what went wrong and mean it is time to debug.
Some programs also come with tests so you can run
```
make test
```
and if the tests pass you can be even more confident that the software is install correctly.

As you start debugging the compilation, it can be easier to pipe the output to a log file
by adding `>>& build.log` to the end of the command like so
```
make >>& build.log
```
You can then search for `error:` to more easily find what the issue is.
You should also check the outputs of `./bootstrap` and `./configure` as their main job is to find dependencies.
So if the compiler is complaining about not finding software check the `./bootstrap` and `./configure` output.

You should never copy the executable files (outputs of the compilers that you use to run code) or configure scripts from other computers/systems.
These will have been compiled on a different systems so they will be built for different infrastructure and dependencies so they likely will not run on your system.

To get software to compile you often have to add different compiler flags/options so they work for your dependencies and compiler versions.
You can add these compiler flags using `CFLAGS=<C flags here>` for the `gcc` compiler, `CXXFLAGS=<C++ flags here>`
for the `c++` compiler and `FFLAGS=<fortran flags here>` for the `gfortran` compiler.
There `configure` script offers many command line options for enabling/disabling features which you can check with the `--help` option.
If the software has a `configure` script it is best to add the flags to the `./configure` command
as they will be appended to the other flags the `configure` script determines the compiler requires.
So do this:
```bash
./configure CFLAGS=<C flags here> FFLAGS=<fortran flags here>
make clean
make
```
not this:
```bash
# BAD! DO NOT DO THIS
./configure
make clean
make CFLAGS=<C flags here> FFLAGS=<fortran flags here>
```
as adding `CFLAGS` or `FFLAGS` to the `make` command may overwrite other compiler flags.

## Dependencies

You will need to install the following packages with a package manager:

```bash
build-essential
autoconf
autotools-dev
automake
autogen
libtool
pkg-config
cmake
csh
g++
gcc
gfortran
wget
git
expect
libcfitsio-dev
hwloc
perl
pcre2-utils
libpcre2-dev
pgplot5
python3
python3-dev
python3-testresources
python3-pip
python3-setuptools
python3-tk
libfftw3-3
libfftw3-bin
libfftw3-dev
libfftw3-single3
libx11-dev
libpcre3
libpcre3-dev
libpng-dev
libpnglite-dev
libhdf5-dev
libhdf5-serial-dev
libxml2
libxml2-dev
libltdl-dev
gsl-bin
libgsl-dev
libblas-dev
liblapack-dev
libglib2.0-dev
xorg
```

We will assume you are going to install this into a directory `ASTROSOFT` where you will run most of
the following commands and install the software to.
Define this location and other environmental variables by adding the following to your `.bashrc` (based on [Lawrence Tommey's documentation](https://www.atnf.csiro.au/people/Lawrence.Toomey/pulsarref/pulsar-software-install-ubuntu-64bit.html))


```bash
# Path to the pulsar software installation directory e.g:
export ASTROSOFT=/home/${USER}/pulsar_software

# OSTYPE
export OSTYPE=linux

# PGPLOT this assumes you install pgplot5 using apt
export PGPLOT_DIR=/usr/lib/pgplot5

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
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib:/usr/lib/x86_64-linux-gnu:$PGPLOT_DIR:$ASTROSOFT/lib:$PRESTO/lib:$PRESTO/lib64:$MULTINEST_DIR

# PATH
# Some Presto executables match sigproc executables so keep separate -
# all other executables are found in $ASTROSOFT/bin
export PATH=$PATH:$ASTROSOFT/bin:$PRESTO/bin:$PGPLOT_DIR

# PYTHON PATH eg.
export PYTHONPATH=$PYTHONPATH:$PRESTO/lib/python3.10/site-packages:/usr/lib/python3.10/site-packages/:/usr/lib64/python3.10/site-packages:$ASTROSOFT/lib/python3.10/site-packages
```

You may have to change the python version in the `export PYTHONPATH` command.
For example if you use python version 3.9.6 (check with `python -V`) then change `python3.10` to `python3.9`

### Installing in a debian OS (easy)

If you would like to install this software on your personal computer (not a supercomputer)
the following command will install the required dependencies on linux debian operating systems (Ubuntu, Mint, etc.).

```bash
sudo apt install  build-essential autoconf autotools-dev automake autogen libtool pkg-config cmake csh g++ gcc gfortran wget git expect libcfitsio-dev hwloc perl pcre2-utils libpcre2-dev pgplot5 python3 python3-dev python3-testresources python3-pip python3-setuptools python3-tk libfftw3-3 libfftw3-bin libfftw3-dev libfftw3-single3 libx11-dev libpcre3 libpcre3-dev libpng-dev libpnglite-dev libhdf5-dev libhdf5-serial-dev libxml2 libxml2-dev libltdl-dev gsl-bin libgsl-dev libblas-dev liblapack-dev xorg libglib2.0-dev
```

### Install manually (hard)

If you would like to install these dependencies manually
(because you are installing it on supercomputer perhaps)
then the following sections will show you how install each dependency manually.

#### FFTW

If you want a different version you can find the tar for of it in their [downloads page](https://fftw.org/pub/fftw/)

```bash
cd $ASTROSOFT
wget https://fftw.org/pub/fftw/fftw-3.3.8.tar.gz
tar -xzf fftw-3.3.8.tar.gz
cd fftw-3.3.8
autoreconf --verbose --install --symlink --force
./configure --enable-shared --enable-float --enable-sse --disable-dependency-tracking --prefix=${ASTROSOFT}
make clean
make
make install
```

<!-- #### psrfits_utils

#### psrxml

#### calceph

#### cfitsio

#### splinter -->

#### psrdada

```bash
cd $ASTROSOFT
git clone git://git.code.sf.net/p/psrdada/code psrdada
cd psrdada
./bootstrap
./configure --prefix=${ASTROSOFT}
make clean
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
./configure CFLAGS=-fcommon
make clean
make
```

#### pgplot

```bash
cd $ASTROSOFT
wget ftp://ftp.astro.caltech.edu/pub/pgplot/pgplot5.2.tar.gz
tar -xzf pgplot5.2.tar.gz
rm -r pgplot5.2.tar.gz
cd pgplot
sed -i "s/g77/gfortran/" sys_linux/g77_gcc.conf
makemake ${ASTROSOFT}/pgplot linux g77_gcc
make
make cpg
make clean
```
Update your `~/.bashrc to include:
```bash
export PGPLOT_DIR=${ASTROSOFT}/pgplot
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:${ASTROSOFT}/pgplot
```

## Common pulsar software

The following are the commands required to download and install common pulsar software.
It is recomended to install them in the following order as some software (such as Tempo)
are dependencies for other packages.

### psrcat

```bash
cd $ASTROSOFT
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





### tempo

```bash
cd $ASTROSOFT
git clone http://git.code.sf.net/p/tempo/tempo
cd tempo
./prepare
./configure --prefix=$ASTROSOFT
make
make install
```

### tempo2

```bash
cd $ASTROSOFT
git clone https://bitbucket.org/psrsoft/tempo2.git
cd tempo2
./bootstrap
./configure --prefix=${ASTROSOFT}
make
make install
make plugins
make plugins-install
make clean
```

### psrchive


```bash
cd $ASTROSOFT
git clone git://git.code.sf.net/p/psrchive/code psrchive
cd psrchive
./bootstrap
./configure --prefix=${ASTROSOFT} --enable-shared
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
./configure --prefix=${ASTROSOFT} --enable-shared FLAGS="-fallow-argument-mismatch"
make clean
make
make install
make clean
```


### dspsr


```bash
cd $ASTROSOFT
git clone git://git.code.sf.net/p/dspsr/code dspsr
cd dspsr
./bootstrap
./configure --prefix=${ASTROSOFT}
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


### sigproc

```bash
cd $ASTROSOFT
git clone https://github.com/SixByNine/sigproc.git
cd sigproc
./bootstrap
./configure --prefix=${ASTROSOFT} F77=gfortran
make
make install
make clean
```



If you get an error like this
```
... multiple definition of ... first defined here
collect2: error: ld returned 1 exit status
```
it is the old gcc error again. Fix it like this
```bash
./configure --prefix=${ASTROSOFT} F77=gfortran CFLAGS=-fcommon
make clean
make
make install
make clean
```

You may get an error like this
```
Error: Rank mismatch between actual argument at (1) and actual argument at (2) (scalar and rank-1)
```
You can make the compiler ignore this with the option `-fallow-argument-mismatch` by running make again like so
```bash
./configure --prefix=${ASTROSOFT} F77=gfortran CFLAGS=-fcommon FFLAGS=-fallow-argument-mismatch
make clean
make
make install
make clean
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
```
sed -i "s/default(none)//" src/inject_pulsar.c
```
Then recompile




### presto

To install the C stuff (python stuff is further down)
```bash
cd $ASTROSOFT
git clone https://github.com/scottransom/presto.git
cd presto/src
make prep
make
make clean
```

If you see lots of undefined references starting with cpg like this:
```
/usr/bin/ld: xyline.c:(.text+0x12ef): undefined reference to `cpgmtxt'
/usr/bin/ld: xyline.c:(.text+0x132a): undefined reference to `cpgline'
/usr/bin/ld: xyline.c:(.text+0x1334): undefined reference to `cpgslw'
/usr/bin/ld: xyline.c:(.text+0x1359): undefined reference to `cpgiden'
```
Then it is likely something wrong with your pgplot installation.
Make sure your environment variable `PGPLOT_DIR` is pointing to the right place.

Now install the python stuff as well
```bash
cd $ASTROSOFT/presto
python3 setup.py install --prefix=$PRESTO
```

If when using some of the presto python packages you see an error like this:
```
pkg_resources.DistributionNotFound: The 'pyslalib' distribution was not found and is required by presto
```
Then you also need to install the `pyslalib` python pacakge like this
```bash
pip install pyslalib
```

