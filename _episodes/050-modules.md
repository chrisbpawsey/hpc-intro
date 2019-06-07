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
--------------------------------------- /gpfs/panther/local/apps/Modules/modulefiles/production ---------------------------------------
   R/3.4.3                 cuda/9.0                ibm/16.1.1      (D)    perl5/5.20.3-thread-multi        totalview/16.07.11
   ant/1.10.1              cuda/9.1                ibmmpi/xl.debug        perl5/5.20.3              (D)    utilities
   arm-tools/19.0.1        cuda/9.2                ibmmpi/xl.perf  (D)    pgi/16.10                        utilities-fen
   camfort/0.901           extrae-gcc/3.4.3        ibmmpi/xl.trace        pgi/17.4                         utilities-gcc
   clang/coral0.2          gcc6/test               java/8.0               pgi/18.10                 (D)    valgrind-gcc/3.12.0
   clang/3.8.0      (D)    gcc6/6.1.1              lmod/6.5               python2/anaconda                 viz
   cmake/3.4.1             gcc6/6.3.0       (D)    maven/3.5.2            python2/2.7.8             (D)    xalt/1.1.2          (L)
   cmake/3.6.2             gcc7/7.1.0              mysql/5.7.17           python3/anaconda          (L)    xlc/16.1.1.0-180913
   cmake/3.7.1             gdb/7.11                nano/2.5.3             python3/3.6.0             (D)    xlc/16.1.1.1        (D)
   cmake/3.10.2     (D)    gperftools/2.6.1        ofc/27Oct16            spectrum_mpi/10.1.0              xlf/16.1.1.1
   cuda/7.5                ibm/13.1.5              papi-gcc/5.5.1         spectrum_mpi/10.2.0       (D)
   cuda/8.0         (D)    ibm/16.1.0              papi/5.4.1             swig/3.0.10

---------------------------------------- /gpfs/panther/local/apps/Modules/modulefiles/packages ----------------------------------------
   beast2/2.4.5                  foam-extend-gcc/4.0         namd/2.12b                 spark/2.1.0
   biobuilds/2016.11             geopy/1.11.0                notebook/4.2.0             spark/2.1.1            (D)
   caffe/20Oct16                 gromacs-gcc/5.1.4           notebook/5.0.0      (D)    tensorflow/ibm
   castep/16.11                  gromacs-gcc/2016.1          opencv-gcc/3.1             tensorflow/1.0.1
   cgns-gcc/3.3                  gromacs-gcc/2019     (D)    openfoam-gcc7/v1806        tensorflow/1.4.1       (D)
   code_saturne-gcc/4.2.2        infernal/1.1.2              openfoam-gcc7/5.0   (D)    tflearn/0.3
   code_saturne-gcc/5.0.4 (D)    interproscan/5.26.64        ranger/1.8.1               theano/0.7.0
   code_saturne/4.2.2            keras/1.1.0                 ruby/2.4.2                 theano/0.8.2
   cuda-convnet2/Nervana         keras/2.0.3          (D)    scala/2.12.1               theano/0.9.0           (D)
   cuda-convnet2/10Oct16  (D)    lammps/17Nov16              sklearn/0.18               utils4cfd-gcc/unstable
   dlmeso/2.6                    metis/5.1.0                 spacy/2.0.3                vim/8.0
   dlpoly/4.08                   namd/2.11                   spark/2.0.0

----------------------------------------- /gpfs/panther/local/apps/Modules/modulefiles/other ------------------------------------------
   atlas-gcc/3.10.3        git/2.9.5              ibmessl/5.4             openmpi-gcc/1.10.2 (g,O)    openmpi/2.1.1
   boost-gcc/1.62.0        google-gcc             ibmpessl/5.2            openmpi-gcc/2.0.2  (g,D)    plplot-gcc/5.11.1
   boost-gcc/1.65.1 (D)    gsl-gcc/2.4            mvapich2-gcc/2.2        openmpi-gcc/3.0.0  (g)      plplot/5.11.1
   fftw2/2.1.5             hdf5-gcc/serial        mvapich2-pgi/2.2        openmpi-pgi/1.10.2          protoc-gcc/3.1.0
   fftw3-gcc/3.3.4         hdf5-gcc/1.8.11        mvapich2/2.2a    (O)    openmpi-pgi/2.0.2  (D)
   fftw3/3.3.5             hdf5-gcc/1.10.0 (D)    netcdf-gcc/4.3.3        openmpi-pgi/2.1.2
   getexample/1.0          hdf5/1.10.1            netcdf/4.3.3            openmpi/2.0.2



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
[remote]$ module load python3/anaconda
[remote]$ which python3
rrb67-pxs01:hcplogin2 >mod+ python3/anaconda
	Python anaconda is now loaded in your environment.

rrb67-pxs01:hcplogin2 >which python3
```
{: .bash}
```
/gpfs/panther/local/apps/python3/anaconda/bin/python3
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
rrb67-pxs01:hcplogin2 >echo $PATH
/gpfs/panther/local/apps/gcc/xalt/1.1.2/bin:/gpfs/panther/local/apps/python3/anaconda/bin:/gpfs/panther/local/apps/hartree/bin:/shared/lsf913/10.1/linux3.10-glibc2.17-ppc64le/etc:/shared/lsf913/10.1/linux3.10-glibc2.17-ppc64le/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/usr/local/cuda/bin:/opt/ibutils/bin
```
{: .output}

You'll notice a similarity to the output of the `which` command. 
In this case, there's only one difference:
the `/gpfs/panther/local/apps/python3/anaconda/bin` directory near the beginning.
When we used `module load python3/anaconda`, 
it added this directory to the beginning of our `$PATH`.
Let's examine what's there:

```
[remote]$ ls /gpfs/panther/local/apps/python3/anaconda/bin
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

```
rrb67-pxs01:hcplogin2 >module list

Currently Loaded Modules:
  1) StdEnv   2) xalt/1.1.2   3) use.panther   4) python3/anaconda
```
{: .output}


So using `module unload` "un-loads" a module along with it's dependencies.
If we wanted to unload everything at once, we could run `module purge` (unloads everything).

```
[remote]$ module load gromacs-gcc/2019
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
------------------------------- /gpfs/panther/local/apps/Modules/modulefiles/production -------------------------------
   R/3.4.3                 extrae-gcc/3.4.3        maven/3.5.2                      spectrum_mpi/10.1.0
   ant/1.10.1              gcc6/test               mysql/5.7.17                     spectrum_mpi/10.2.0 (D)
   arm-tools/19.0.1        gcc6/6.1.1              nano/2.5.3                       swig/3.0.10
   camfort/0.901           gcc6/6.3.0       (D)    ofc/27Oct16                      totalview/16.07.11
   clang/coral0.2          gcc7/7.1.0              papi-gcc/5.5.1                   utilities
   clang/3.8.0      (D)    gdb/7.11                papi/5.4.1                       utilities-fen
   cmake/3.4.1             gperftools/2.6.1        perl5/5.20.3-thread-multi        utilities-gcc
   cmake/3.6.2             ibm/13.1.5              perl5/5.20.3              (D)    valgrind-gcc/3.12.0
   cmake/3.7.1             ibm/16.1.0              pgi/16.10                        viz
   cmake/3.10.2     (D)    ibm/16.1.1       (D)    pgi/17.4                         xalt/1.1.2          (L)
   cuda/7.5                ibmmpi/xl.debug         pgi/18.10                 (D)    xlc/16.1.1.0-180913
   cuda/8.0         (D)    ibmmpi/xl.perf   (D)    python2/anaconda                 xlc/16.1.1.1        (D)
   cuda/9.0                ibmmpi/xl.trace         python2/2.7.8             (D)    xlf/16.1.1.1
   cuda/9.1                java/8.0                python3/anaconda          (L)
   cuda/9.2                lmod/6.5                python3/3.6.0             (D)


-------------------------------- /gpfs/panther/local/apps/Modules/modulefiles/packages --------------------------------
   beast2/2.4.5                  foam-extend-gcc/4.0         namd/2.12b                 spark/2.1.0
   biobuilds/2016.11             geopy/1.11.0                notebook/4.2.0             spark/2.1.1            (D)
   caffe/20Oct16                 gromacs-gcc/5.1.4           notebook/5.0.0      (D)    tensorflow/ibm
   castep/16.11                  gromacs-gcc/2016.1          opencv-gcc/3.1             tensorflow/1.0.1
   cgns-gcc/3.3                  gromacs-gcc/2019     (D)    openfoam-gcc7/v1806        tensorflow/1.4.1       (D)
   code_saturne-gcc/4.2.2        infernal/1.1.2              openfoam-gcc7/5.0   (D)    tflearn/0.3
   code_saturne-gcc/5.0.4 (D)    interproscan/5.26.64        ranger/1.8.1               theano/0.7.0
   code_saturne/4.2.2            keras/1.1.0                 ruby/2.4.2                 theano/0.8.2
   cuda-convnet2/Nervana         keras/2.0.3          (D)    scala/2.12.1               theano/0.9.0           (D)
   cuda-convnet2/10Oct16  (D)    lammps/17Nov16              sklearn/0.18               utils4cfd-gcc/unstable
   dlmeso/2.6                    metis/5.1.0                 spacy/2.0.3                vim/8.0
   dlpoly/4.08                   namd/2.11                   spark/2.0.0

--------------------------------- /gpfs/panther/local/apps/Modules/modulefiles/other ----------------------------------
   atlas-gcc/3.10.3        google-gcc             mvapich2-gcc/2.2            openmpi-pgi/1.10.2
   boost-gcc/1.62.0        gsl-gcc/2.4            mvapich2-pgi/2.2            openmpi-pgi/2.0.2  (D)
   boost-gcc/1.65.1 (D)    hdf5-gcc/serial        mvapich2/2.2a      (O)      openmpi-pgi/2.1.2
   fftw2/2.1.5             hdf5-gcc/1.8.11        netcdf-gcc/4.3.3            openmpi/2.0.2
   fftw3-gcc/3.3.4         hdf5-gcc/1.10.0 (D)    netcdf/4.3.3                openmpi/2.1.1
   fftw3/3.3.5             hdf5/1.10.1            openmpi-gcc/1.10.2 (g,O)    plplot-gcc/5.11.1
   getexample/1.0          ibmessl/5.4            openmpi-gcc/2.0.2  (g,D)    plplot/5.11.1
   git/2.9.5               ibmpessl/5.2           openmpi-gcc/3.0.0  (g)      protoc-gcc/3.1.0



[removed most of the output here for clarity]

```
{: .output}

Let's take a closer look at the `gcc` module.
GCC is an extremely widely used C/C++/Fortran compiler.
Lots of software is dependent on the GCC version, 
and might not compile or run if the wrong version is loaded.
In this case, there are three different versions: `gcc/6.2.0`, `gcc/6.3.0` and `gcc/7.2.0`.
How do we load each copy and which copy is the default?

In this case, `gcc6/6.3.0` has a `(D)` next to it.
This indicates that it is the default - 
if we type `module load gcc6`, this is the copy that will be loaded.

```
rrb67-pxs01:hcplogin2 >module load gcc6
	general utilities loaded

	GCC-6.3.0 environment now loaded



[remote]$ gcc --version
```
{: .bash}
```
rrb67-pxs01:hcplogin2 >gcc --version
gcc (GCC) 6.3.0
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
{: .output}

So how do we load the non-default copy of a software package?
In this case, the only change we need to make is be more specific about the module we are loading.
There are four GCC modules:  `gcc6/test`, `gcc6/6.1.1`, `gcc6/6.3.0` and `gcc7/7.1.0`
To load a non-default module, we need to make add the version number after the `/` in our `module load` command

```
[remote]$ module load gcc7/7.1.0
```
{: .bash}
```
rrb67-pxs01:hcplogin2 >module load gcc7/7.1.0
	general utilities loaded

	GCC-7.1.0 environment now loaded
```
{: .output}

What happened? The module command is telling us that we have `gcc7/7.1.0` loaded.

```
rrb67-pxs01:hcplogin2 >module list
```
{: .bash}
```
Currently Loaded Modules:
  1) StdEnv   2) xalt/1.1.2   3) use.panther   4) python3/anaconda   5) gcc6/6.3.0   6) utilities-gcc   7) gcc7/7.1.0
```
{: .output}
 
We should not have two `gcc` modules loaded at the same time as this could cause confusion 
about which version you are using. We need to remove the default version before we load the new version.

```
[remote]$ module unload gcc6
```
{: bash }
```
rrb67-pxs01:hcplogin2 >module unload gcc6
	Thank you for using Xalt to monitor your applications

	group: pxs01 
	user: rrb67-pxs01 
	project: HCP004 
	base path: /gpfs/panther/local/HCP004/pxs01/rrb67-pxs01 
	path to CDS: /gpfs/cds/local/HCP004/pxs01/rrb67-pxs01 
	Python anaconda is now loaded in your environment.

	GCC-7.1.0 environment now loaded
```
{: .output}
```
rrb67-pxs01:hcplogin2 >gcc --version
```
{: bash }
```
gcc (GCC) 7.1.0
Copyright (C) 2017 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

```
{: .output }

We now have successfully switched from GCC 6.2.0 to GCC 7.1.0.

As switching between different versions of the same module is often used you can use `module swap` 
rather than unloading one version before loading another. The equivalent of the steps above would be:

```
[remote]$ module purge
[remote]$ module load gcc6
[remote]$ gcc --version

```
{: .bash}
```
gcc (GCC) 6.2.0
Copyright (C) 2016 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.
```
{: .output }

```
[remote]$ module swap gcc6 gcc/7.1.0
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
