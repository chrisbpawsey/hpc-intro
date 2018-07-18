---
title: "Accessing software"
teaching: 30
exercises: 15
questions:
- "How do we load and unload software packages?"
objectives:
- "Understand how to load and use a software package."
- "Understand how to use modules to load software packages in scheduler job scripts."
keypoints:
- "Load software with `module load softwareName`"
- "The module system handles software versioning and package conflicts for you automatically."
- "Loading modules must also be done in scheduler job scripts."
---

Before we start using individual software packages, 
we need to understand the why multiple versions of software are available
on HPC systems and why users need to have a way to control which version
they are using. The three biggest factors are:

- software incompatibilities;
- versioning;
- dependencies.

Software incompatibility is a major headache for programmers.
Sometimes the presence (or absence) 
of a software package will break others that depend on it.
Two of the most famous examples are Python 2 and 3 and C compiler versions.
Python 3 famously provides a `python` command 
that conflicts with that provided by Python 2. 
Software compiled against a newer version of the C libraries 
and then used when they are not present will result in a nasty
`'GLIBCXX_3.4.20' not found` error, for instance. 

Software versioning is another common issue.
A team might depend on a certain package version for their research project - 
if the software version was to change (for instance, if a package was updated),
it might affect their results.
Having access to multiple software versions allow a set of researchers to prevent
software versioning issues from affecting their results.

Dependencies are where a particular software package (or even a particular version)
depends on having access to another software package (or even a particular version of
another software package). For example, the VASP materials science software may 
depend on having a particular version of the FFTW (Fastest Fourer Transform in the West)
software library availble for it to work.

## Environment modules

Environment modules are the solution to these problems.
A *module* is a self-contained description of a software package - 
it contains the settings required to run a software packace 
and, usually, encodes required dependencies on other software packages.

There are a number of different environment module implementations commonly
used on HPC systems: the two most common are *TCL modules* and *Lmod*. Both of
these use similar syntax and the concepts are the same so learning to use one will
allow you to use whichever is installed on the system you are using. In both 
implementations the `module` command is used to interact with environment modules. An
additional subcommand is usually added to the command to specify what you want to do. For a list
of subcommnands you can use `module -h` or `module help`. As for all commands, you can 
access the full help on the *man* pages with `man module`.

On login you may start out with a default set of modules loaded or you may start out
with an empty environemnt, this depends on the setup of the system you are using.

### Listing currently loaded modules

You can use the `module list` command to see which modules you currently have loaded
in your environment. If you have no modules loaded, you will see a message telling you
so

```
[remote]$ module list
```
{: .bash}
```
No Modulefiles Currently Loaded.
```
{: .output}

### Listing available modules

To see available modules, use `module avail`

```
[remote]$ module avail
```
{: .bash}
```

------------------------------------------------- /usr/share/Modules/modulefiles -------------------------------------------------
dot         module-git  module-info modules     mpt/2.16    null        perfboost   perfcatcher use.own

----------------------------------------------------- /lustre/sw/modulefiles -----------------------------------------------------
abinit/8.2.3-intel17-mpt214(default)    hdf5parallel/1.10.1-intel17-mpt214      molpro/2012.1.22(default)
allinea/7.0.0(default)                  intel-cc-16/16.0.2.181                  mpt/2.14
altair-hwsolvers/13.0.213               intel-cc-16/16.0.3.210(default)         namd/2.12(default)
altair-hwsolvers/14.0.210               intel-cc-17/17.0.2.174(default)         ncl/6.4.0
amber/16                                intel-cmkl-16/16.0.2.181                nco/4.6.9
anaconda/python2(default)               intel-cmkl-16/16.0.3.210(default)       ncview/2.1.7
anaconda/python3                        intel-cmkl-17/17.0.2.174(default)       netcdf/4.4.1
ansys/17.2                              intel-compilers-16/16.0.2.181           netcdf-parallel/4.5.0
ansys/18.0                              intel-compilers-16/16.0.3.210(default)  netcdf-parallel/4.5.0-gcc6-mpt214
ansys/19.0                              intel-compilers-17/17.0.2.174(default)  netcdf-parallel/4.5.0-intel17
castep/16.11(default)                   intel-fc-16/16.0.2.181                  netcdf-parallel/4.5.0-intel17-mpt214
castep/18.1.0-intel17                   intel-fc-16/16.0.3.210(default)         openfoam/foundation/5.0
cp2k/4.1                                intel-fc-17/17.0.2.174(default)         openfoam/v1706
cp2k-mpt/4.1                            intel-itac/9.1.2.024                    openfoam/v1712
dolfin/2017.1.0(default)                intel-itac-17/2017.2.028(default)       qe/6.1(default)
dolfin/2017.1.0-python-2.7              intel-mpi-16/16.0.3.210                 qe/6.1+d3q
dolfin/2017.2.0                         intel-mpi-17/17.0.2.174                 scalasca/2.3.1-gcc6
eclipse/4.2                             intel-tbb/16.0.2.181                    scalasca/2.3.1-intel17
epcc-tools/1.0(default)                 intel-tbb/16.0.3.210(default)           singularity/2.3.1
fenics/2016.2.0(default)                intel-tbb-17/17.0.2.174(default)        singularity/2.3.2
fenics/2017.2.0                         intel-tools-16/16.0.2.181               singularity/2.4(default)
flacs/10.5.1                            intel-tools-16/16.0.3.210(default)      spack/20161205(default)
flacs/10.6.3                            intel-tools-17/17.0.2.174(default)      spack/cirrus
gaussian/09.E01(default)                intel-vtune-16/2016.2.0.444464          starccm+/12.04.011(default)
gaussian/16.A03                         intel-vtune-16/2016.3.0.463186(default) starccm+/12.04.011-R8
gcc/6.2.0(default)                      intel-vtune-17/2017.2.0.499904(default) szip/2.1.1
gcc/6.3.0                               ipm/2.0.6-impi                          testing/qe/6.1-intel
gnu-parallel/20170322(default)          lammps/31Mar2017-gcc6-mpt214(default)   tinker/8.2.1
gnuplot/5.0.5(default)                  matlab/R2016b(default)                  valgrind/3.11.0
gnuplot/5.0.5-x11                       matlab/R2018a                           vasp/5.4.4-intel17-mpt214(default)
gromacs/2016.3(default)                 miniconda/python2                       xflow/98.00
hdf5parallel/1.10.1-gcc6-mpt214         miniconda/python3


[removed most of the output here for clarity]

```
{: .output}

### Loading and unloading modules

To load a software module, use `module load`.
In this example we will use Python 3.

Intially, Python 3 is not loaded. 
We can test this by using the `which` command.
`which` looks for programs the same way that Bash does,
so we can use it to tell us where a particular piece of software is stored.

```
[remote]$ which python3
```
{: .bash}
```
/usr/bin/which: no python3 in (/opt/sgi/sbin:/opt/sgi/bin:/usr/lib64/qt-3.3/bin:/opt/pbs/default/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/c3/bin:/sbin:/bin)
```
{: .output}

We can load the `python3` command with `module load`:

```
[remote]$ module load anaconda/python3
[remote]$ which python3
```
{: .bash}
```
/lustre/sw/anaconda/anaconda3-5.1.0/bin/python3
```
{: .output}

([Anaconda](https://anaconda.org/) is a scientific Python distribution.)

So what just happened?

To understand the output, first we need to understand the nature of the 
`$PATH` environment variable.
`$PATH` is a special ennvironment variable that controls where a Linux *operating system* (OS) looks for software.
Specifically `$PATH` is a list of directories (separated by `:`)
that the OS searches through for a command before giving up and telling us it can't find it.
As with all environment variables, we can print it out using `echo`.

```
[remote]$ echo $PATH
```
{: .bash}
```
/lustre/sw/anaconda/anaconda3-5.1.0/bin:/opt/sgi/sbin:/opt/sgi/bin:/usr/lib64/qt-3.3/bin:/opt/pbs/default/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/c3/bin:/sbin:/bin
```
{: .output}

You'll notice a similarity to the output of the `which` command. 
In this case, there's only one difference:
the `/lustre/sw/anaconda/anaconda3-5.1.0/bin` directory at the beginning.
When we used `module load anaconda/python3`, 
it added this directory to the beginning of our `$PATH`.
Let's examine what's there:

```
[remote]$ ls /lustre/sw/anaconda/anaconda3-5.1.0/bin
```
{: .bash}
```

[output truncated] 

conda-convert                       gio-querymodules        jupyter-run               python                tiff2rgba
conda-develop                       glacier                 jupyter-serverextension   python3               tiffcmp
conda-env                           glib-compile-resources  jupyter-troubleshoot      python3.6             tiffcp
conda-index                         glib-compile-schemas    jupyter-trust             python3.6-config      tiffcrop
conda-inspect                       glib-genmarshal         kill_instance             python3.6m            tiffdither
conda-metapackage                   glib-gettextize         launch_instance           python3.6m-config     tiffdump
conda-render                        glib-mkenums            lconvert                  python3-config        tiffinfo
conda-server                        gobject-query           libpng16-config           pyuic5                tiffmedian
conda-skeleton                      gresource               libpng-config             pyvenv                tiffset
elbadmin                            hb-view                 patchelf                  rst2man.py

[output truncated] 

```
{: .output}

Taking this to it's conclusion, `module load` will add software to your `$PATH`.
It "loads" software.
A special note on this - 
depending on which version of the `module` program that is installed at your site,
`module load` may also load required software dependencies.
To demonstrate, let's load the `abinit` module and then use the `module list` command to show
which modules we currently have loaded in our environment. ([Abinit](https://www.abinit.org/) is an open source 
materials science modelling software package.)

```
[remote]$ module load abinit
[remote]$ module list
```
{: .bash}
```
Currently Loaded Modulefiles:
  1) anaconda/python3                  5) intel-compilers-17/17.0.2.174     9) netcdf/4.4.1
  2) mpt/2.16                          6) intel-cmkl-17/17.0.2.174         10) abinit/8.2.3-intel17-mpt214
  3) intel-cc-17/17.0.2.174            7) gcc/6.2.0
  4) intel-fc-17/17.0.2.174            8) fftw-3.3.5-intel-17.0.2-dxt2dzn
```
{: .output}

So in this case, loading the `abinit` module
also loaded a variety of other modules.
Let's try unloading the `abinit` package.

```
[remote]$ module unload abinit
[remote]$ module list
```
{: .bash}
```
Currently Loaded Modulefiles:
  1) anaconda/python3
```
{: .output}

So using `module unload` "un-loads" a module along with it's dependencies.
If we wanted to unload everything at once, we could run `module purge` (unloads everything).

```
[remote]$ module load abinit
[remote]$ module purge
```
{: .bash}
```
No Modulefiles Currently Loaded.
```
{: .output}

Note that `module purge` has removed the `anaconda/python3` module as well as `abinit` and its dependencies.

## Software versioning

So far, we've learned how to load and unload software packages.
This is very useful.
However, we have not yet addressed the issue of software versioning.
At some point or other, 
you will run into issues where only one particular version of some software will be suitable.
Perhaps a key bugfix only happened in a certain version, 
or version X broke compatibility with a file format you use.
In either of these example cases, 
it helps to be very specific about what software is loaded.

Let's examine the output of `module avail` more closely.

```
[remote]$ module avail
```
{: .bash}
```

------------------------------------------------- /usr/share/Modules/modulefiles -------------------------------------------------
dot         module-git  module-info modules     mpt/2.16    null        perfboost   perfcatcher use.own

----------------------------------------------------- /lustre/sw/modulefiles -----------------------------------------------------
abinit/8.2.3-intel17-mpt214(default)    hdf5parallel/1.10.1-gcc6-mpt214         miniconda/python3
allinea/7.0.0(default)                  hdf5parallel/1.10.1-intel17-mpt214      molpro/2012.1.22(default)
altair-hwsolvers/13.0.213               intel-cc-16/16.0.2.181                  mpt/2.14
altair-hwsolvers/14.0.210               intel-cc-16/16.0.3.210(default)         namd/2.12(default)
amber/16                                intel-cc-17/17.0.2.174(default)         ncl/6.4.0
anaconda/python2(default)               intel-cmkl-16/16.0.2.181                nco/4.6.9
anaconda/python3                        intel-cmkl-16/16.0.3.210(default)       ncview/2.1.7
ansys/17.2                              intel-cmkl-17/17.0.2.174(default)       netcdf/4.4.1
ansys/18.0                              intel-compilers-16/16.0.2.181           netcdf-parallel/4.5.0
ansys/19.0                              intel-compilers-16/16.0.3.210(default)  netcdf-parallel/4.5.0-gcc6-mpt214
castep/16.11(default)                   intel-compilers-17/17.0.2.174(default)  netcdf-parallel/4.5.0-intel17
castep/18.1.0-intel17                   intel-fc-16/16.0.2.181                  netcdf-parallel/4.5.0-intel17-mpt214
cp2k/4.1                                intel-fc-16/16.0.3.210(default)         openfoam/foundation/5.0
cp2k-mpt/4.1                            intel-fc-17/17.0.2.174(default)         openfoam/v1706
dolfin/2017.1.0(default)                intel-itac/9.1.2.024                    openfoam/v1712
dolfin/2017.1.0-python-2.7              intel-itac-17/2017.2.028(default)       qe/6.1(default)
dolfin/2017.2.0                         intel-mpi-16/16.0.3.210                 qe/6.1+d3q
eclipse/4.2                             intel-mpi-17/17.0.2.174                 scalasca/2.3.1-gcc6
epcc-tools/1.0(default)                 intel-tbb/16.0.2.181                    scalasca/2.3.1-intel17
fenics/2016.2.0(default)                intel-tbb/16.0.3.210(default)           singularity/2.3.1
fenics/2017.2.0                         intel-tbb-17/17.0.2.174(default)        singularity/2.3.2
flacs/10.5.1                            intel-tools-16/16.0.2.181               singularity/2.4(default)
flacs/10.6.3                            intel-tools-16/16.0.3.210(default)      spack/20161205(default)
gaussian/09.E01(default)                intel-tools-17/17.0.2.174(default)      spack/cirrus
gaussian/16.A03                         intel-vtune-16/2016.2.0.444464          starccm+/12.04.011(default)
gcc/6.2.0(default)                      intel-vtune-16/2016.3.0.463186(default) starccm+/12.04.011-R8
gcc/6.3.0                               intel-vtune-17/2017.2.0.499904(default) szip/2.1.1
gcc/7.2.0                               ipm/2.0.6-impi                          testing/qe/6.1-intel
gnu-parallel/20170322(default)          lammps/31Mar2017-gcc6-mpt214(default)   tinker/8.2.1
gnuplot/5.0.5(default)                  matlab/R2016b(default)                  valgrind/3.11.0
gnuplot/5.0.5-x11                       matlab/R2018a                           vasp/5.4.4-intel17-mpt214(default)
gromacs/2016.3(default)                 miniconda/python2                       xflow/98.00

[removed most of the output here for clarity]

```
{: .output}

Let's take a closer look at the `gcc` module.
GCC is an extremely widely used C/C++/Fortran compiler.
Lots of software is dependent on the GCC version, 
and might not compile or run if the wrong version is loaded.
In this case, there are three different versions: `gcc/6.2.0`, `gcc/6.3.0` and `gcc/7.2.0`.
How do we load each copy and which copy is the default?

In this case, `gcc/6.2.0` has a `(default)` next to it.
This indicates that it is the default - 
if we type `module load gcc`, this is the copy that will be loaded.

```
[remote]$ module load gcc
[remote]$ gcc --version
```
{: .bash}
```
gcc (GCC) 6.2.0
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
{: .output}

So how do we load the non-default copy of a software package?
In this case, the only change we need to make is be more specific about the module we are loading.
There are three GCC modules:  `gcc/6.2.0`, `gcc/6.3.0` and `gcc/7.2.0`
To load a non-default module, we need to make add the version number after the `/` in our `module load` command

```
[remote]$ module load gcc/7.2.0
```
{: .bash}
```
gcc/7.2.0(17):ERROR:150: Module 'gcc/7.2.0' conflicts with the currently loaded module(s) 'gcc/6.2.0'
gcc/7.2.0(17):ERROR:102: Tcl command execution failed: conflict gcc
```
{: .output}

What happened? The module command is telling us that we cannot have two `gcc` modules loaded at the 
same time as this could cause confusion about which version you are using. We need to remove the
default version before we load the new version.

```
[remote]$ module unload gcc
[remote]$ module load gcc/7.2.0
[remote]$ gcc --version
```
{: .bash}
```
gcc (GCC) 7.2.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
{: .output}


We now have successfully switched from GCC 6.2.0 to GCC 7.2.0.

As switching between different versions of the same module is often used you can use `module swap` 
rather than unloading one version before loading another. The equivalent of the steps above would be:

```
[remote]$ module purge
[remote]$ module load gcc
[remote]$ gcc --version
[remote]$ module swap gcc gcc/7.2.0
[remote]$ gcc --version
```
{: .bash}
```
gcc (GCC) 6.2.0
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

gcc (GCC) 7.2.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
{: .output}

This achieves the same result as unload followed by load but in a single step.

> ## Using software modules in scripts
>
> Create a job that is able to run `python3 --version`.
> Remember, no software is loaded by default!
> Running a job is just like logging on to the system 
> (you should not assume a module loaded on the login node is loaded on a worker node).
{: .challenge}

> ## Loading a module by default
> 
> Adding a set of `module load` commands to all of your scripts and
> having to manually load modules every time you log on can be tiresome.
> Fortunately, there is a way of specifying a set of "default modules"
> that always get loaded, regardless of whether or not you're logged on or running a job.
>
> Every user has two hidden files in their home directory: `.bashrc` and `.bash_profile`
> (you can see these files with `ls -la ~`).
> These scripts are run every time you log on or run a job.
> Adding a `module load` command to one of these shell scripts means that
> that module will always be loaded.
> Modify either your `.bashrc` or `.bash_profile` scripts to load a commonly used module like Python.
> Does your `python3 --version` job from before still need `module load` to run?
{: .challenge}

## Installing software of our own

Most HPC systems have a pretty large set of preinstalled software.
Nonetheless, it's unlikely that all of the software we'll need will be available.
Sooner or later, we'll need to install some software of our own. 

Though software installation differs from package to package,
the general process is the same:
download the software, read the installation instructions (important!),
install dependencies, compile, then start using our software.

As an example we will install the bioinformatics toolkit `seqtk`.
We'll first need to obtain the source code from Github using `git`.

```
[remote]$ git clone https://github.com/lh3/seqtk.git
```
{: .bash}
```
Cloning into 'seqtk'...
remote: Counting objects: 331, done.
remote: Compressing objects: 100% (10/10), done.
remote: Total 331 (delta 5), reused 8 (delta 2), pack-reused 319
Receiving objects: 100% (331/331), 159.82 KiB | 0 bytes/s, done.
Resolving deltas: 100% (188/188), done.
```
{: .output}

Now, using the instructions in the README.md file, 
to complete the install we should `cd` into the `seqtk` folder, load the `gcc` module (so we have the
required compiler) and run the command `make`.

```
[remote]$ cd seqtk
[remote]$ module load gcc
[remote]$ make
```
{: .bash}
```
gcc -g -Wall -O2 -Wno-unused-function seqtk.c -o seqtk -lz -lm
seqtk.c: In function ‘stk_comp’:
seqtk.c:399:16: warning: variable ‘lc’ set but not used [-Wunused-but-set-variable]
    int la, lb, lc, na, nb, nc, cnt[11];
                ^
```
{: .output}

It's done!
Now all we need to do to use the program is invoke it like any other program.

```
[remote]$ ./seqtk
```
{: .bash}
```
Usage:   seqtk <command> <arguments>
Version: 1.3-r106

Command: seq       common transformation of FASTA/Q
         comp      get the nucleotide composition of FASTA/Q
         sample    subsample sequences
         subseq    extract subsequences from FASTA/Q
         fqchk     fastq QC (base/quality summary)
         mergepe   interleave two PE FASTA/Q files
         trimfq    trim FASTQ using the Phred algorithm

         hety      regional heterozygosity
         gc        identify high- or low-GC regions
         mutfa     point mutate FASTA at specified positions
         mergefa   merge two FASTA/Q files
         famask    apply a X-coded FASTA to a source FASTA
         dropse    drop unpaired from interleaved PE FASTA/Q
         rename    rename sequence names
         randbase  choose a random base from hets
         cutN      cut sequence at long N
         listhet   extract the position of each het
```
{: .output}

We've successfully installed our first piece of software! In this case, there 
were no additional dependencies to install.
