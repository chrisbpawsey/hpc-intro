---
title: "Connecting to the HPC system"
teaching: 10 
exercises: 5
questions:
- "How do I open a terminal?"
- "How do I connect to a remote computer?"
objectives:
- "Succesfully connect to a remote HPC system."
keypoints:
- "To connect to a remote HPC system using SSH: `ssh yourUsername@remote.computer.address`"
---

## Opening a Terminal

Connecting to an HPC system is most often done through a tool known as "SSH"
(Secure SHell) and usually SSH is run through a terminal. So, to begin using
an HPC system we need to begin by opening a terminal. Different operating
systems have different terminals, none of which are exactly the same in terms
of their features and abilities while working on the operating system. When
connected to the remote system the experience between terminals will be
identical as each will faithfully present the same experience of using that
system.

Here is the process for opening a terminal in each operating system.

### Linux

There are many different versions (aka "flavours") of Linux and how to open a
terminal window can change between flavours. Fortunately most Linux users
already know how to open a terminal window since it is a common part of the
workflow for Linux users. If this is something that you do not know how to do
then a quick search on the Internet for "how to open a terminal window in"
with your particular Linux flavour appended to the end should quickly give
you the directions you need.

A very popular version of Linux is Ubuntu. There are many ways to open a
terminal window in Ubuntu but a very fast way is to use the terminal shortcut
key sequence: Ctrl+Alt+T.

### Mac

Macs have had a terminal built in since the first version of macOS (OSX) as it is
built on a Linux flavour known as BSD (Berkeley Systems Designs). 
The terminal can be quickly opened through the use of the Spotlight tool. 
Hold down the command key and press the spacebar. 
In the search bar that shows up type "terminal", choose the terminal app from the list of results (it will
look like a tiny, black computer screen) and you will be presented with a terminal window.
Alternatively, you can find Terminal under "Utilities" in the Applications menu.

### Windows

While Windows does have a command-line interface known as the "Command
Prompt" that has its roots in MS-DOS (Microsoft Disk Operating System) it
does not have an SSH tool built into it and so one needs to be installed.
There are a variety of programs that can be used for this, two common ones we
describe here, as follows:

#### MobaXterm

MobaXterm is a terminal window emulator for Windows and the home edition can
be downloaded for free from
[mobatek.net](https://mobaxterm.mobatek.net/download-home-edition.html). If
you follow the link you will note that there are two editions of the home
version available: Portable and Installer. The portable edition puts all
MobaXterm content in a folder on the desktop (or anywhere else you would like
it) so that it is easy add plug-ins or remove the software. The installer
edition adds MobaXterm to your Windows installation as any other program you
might install. If you are not sure that you will continue to use MobaXterm in
the future you are likely best to choose the portable edition.

Download the version that you would like to use and install it as you would
any other software on your Windows installation. Once the software is
installed you can run it by either opening the folder installed with the
portable edition and double-clicking on the file named
*MobaXterm_Personal_10.2* or, if the installer edition was used, finding the
executable through either the start menu or the Windows search option.

Once the MobaXterm window is open you should see a large button in the middle
of that window with the text "Start Local Terminal". Click this button and
you will have a terminal window at your disposal.

#### PuTTY

It is strictly speaking not necessary to have a terminal running on your local
computer in order to access and use a remote system, only a window into the remote system once connected.  PuTTy is likely the oldest, most well-known, and widely used software solution to take this approach.

PuTTY is available for free download from [www.putty.org](http://www.putty.org/).  Download the version that is correct for your operating system and install it as you would other software on you Windows system.  Once installed it will be available through the start menu or similar.

Running PuTTY will not initially produce a terminal but intsead a window full of connection options.  Putting the address of the remote system in the "Host Name (or IP Address)" box and either pressing enter or clicking the "Open" button should begin the connection process.

If this works you will see a terminal window open that prompts you for a username through the "login as:" prompt and then for a password.  If both of these are passed correctly then you will be given access to the system and will see a message saying so within the terminal.  If you need to escape the authentication process you can hold the control/ctrl key and press the c key to exit and start again.

Note that you may want to paste in your password rather than typing it.  Use control/ctrl plus a right-click of the mouse to paste content from the clipboard to the PuTTY terminal.

For those logging in with PuTTY it would likely be best to cover the terminal basics already mentioned above before moving on to navigating the remote system.

## Logging onto the system

With all of this in mind, let's connect to a cluster. 

For this lesson, we will use [Panther](http://community.hartree.stfc.ac.uk/wiki/site/admin/power8%20-%20further%20info.html) - an HPC system located at the [Hartree Centre](https://www.hartree.stfc.ac.uk/Pages/home.aspx).
Although it's unlikely that systems elsewhere will be like Panther, the fundamental major components of what you can expect from an HPC system are there it is just a matter of how they are accessed and used.
To connect to our example computer, we will use SSH (if you are using PuTTY, see above for instructions). 

SSH allows us to connect to Linux computers remotely, and use them as if they were our own.
The general syntax of the connection command follows the format `ssh yourUsername@some.computer.address`
Let's attempt to connect to the HPC system now:

```
ssh yourUsername@hcplogin1.hartree.stfc.ac.uk

```
{: .bash}
or 

```
ssh yourUsername@hcplogin2.hartree.stfc.ac.uk
```
{: .bash}

Your Instructor will give you the correct username to use in place of "yourUsername".

```{.output}
1@hcplogin2.hartree.stfc.ac.uk's password: 
Warning: No xauth data; using fake authentication data for X11 forwarding.
Last failed login: Wed Jun  5 17:06:24 BST 2019 from 193.62.120.140 on ssh:notty
There was 1 failed login attempt since the last successful login.
Last login: Tue Jun  4 08:16:18 2019 from host86-133-141-225.range86-133.btcentralplus.com
##########################################################################
##                                                                      ##
##                  Upcoming Maintenance Sessions                       ##
##                                                                      ##
##                  Start: Jun 05 2019 08:00                            ##
##                  End:   Jun 05 2019 18:00                            ##
##                                                                      ##
##                  Start: Jun 19 2019 08:00                            ##
##                  End:   Jun 19 2019 18:00                            ##
##                                                                      ##
##########################################################################
##                                                                      ##
## Notice 2018-09-06:                                                   ##
##                                                                      ##
## With effect from Monday 10th September, there will be a run time     ##
## limit of 120 minutes on the Paragon interactive queue. Interactive   ##
## nodes will be available from 08:00 - 18:00 Mon - Thurs and           ##
## 08:00 - 12:00 Fri. The interactive nodes will be made available to   ##
## the batch queues at all other times.                                 ##
##                                                                      ##
##########################################################################
##                                                                      ##
##   System status info:                                                ##
##                                                                      ##
##   https://stfc.service-now.com                                       ##
##                                                                      ##
##########################################################################
##                                                                      ##
## Disk quotas on /gpfs/fairthorpe                                      ##
##                                                                      ##
## Home directories in /gpfs/fairthorpe are quota enforced, having      ##
## soft and hard limits of 100 MB and 110 MB, respectively. These       ##
## directories are only intended for the storage of configuration files ##
## ('dot' files), ssh keys and scripts. The CDS filesystem should be    ##
## for the storage of all data files.                                   ##
##									##
## Your current quota and usage can be queried by simply issuing the    ##
## command "quota". Add the "-s" flag for human friendly output. Note   ##
## that the name of the remote filesystem will be reported, rather than ##
## the local mountpoint.                                                ##
##									##
##########################################################################
```

If you've connected successfully, you should see a prompt like the one below. 
This prompt is informative, and lets you grasp certain information at a glance:
in this case `[yourUsername@computerName workingDirectory]$`.
(If you don't understand what these things are, don't worry! 
We will cover things in depth as we explore the system further.)

```{.output}
yourUsername:hcplogin2 >

```

## Telling the Difference between the Local Terminal and the Remote Terminal

You may have noticed that the prompt changed when you logged into the remote
system using the terminal (if you logged in using PuTTY this will not apply
because it does not offer a local terminal). This change is important because
it makes it clear on which system the commands you type will be run when you
pass them into the terminal. This change is also a small complication that we
will need to navigate throughout the workshop. Exactly what is reported
before the `$` in the terminal when it is connected to the local system and
the remote system will typically be different for every user. We still need
to indicate which system we are entering commands on though so we will adopt
the following convention:

`[local]$` when the command is to be entered on a terminal connected to your local computer

`[remote]$` when the command is to be entered on a terminal connected to the remote system

`$` when it really doesn't matter which system the terminal is connected to.

> ## Being Certain Which System your Terminal is connected to
> If you ever need to be certain which system a terminal you are using is connected to 
> then use the follwing command: `$ hostname`.
{: .callout}

> ## Keep Two Terminal Windows Open
> It is strongly recommended that you have two terminals open, one connected
> to the local system and one connected to the remote system, that you can
> switch back and forth between. If you only use one terminal window then you
> will need to reconnect to the remote system using one of the methods above
> when you see a change from `[local]$` to `[remote]$` and disconnect when you
> see the reverse.
{: .callout}

## Examining the nodes

Now we can log into the Panther HPC system we will look at the nodes. You should remember that there 
are at least two  types of node on the system: *login nodes* and *compute nodes*.

We can use the `lscpu` command to print information on the processors on the login nodes to the terminal:

```
[remote]$ lscpu
```
{: .bash}
```
hcplogin2 >lscpu
Architecture:          ppc64le
Byte Order:            Little Endian
CPU(s):                128
On-line CPU(s) list:   0-127
Thread(s) per core:    8
Core(s) per socket:    8
Socket(s):             2
NUMA node(s):          2
Model:                 2.0 (pvr 004d 0200)
Model name:            POWER8 (raw), altivec supported
L1d cache:             64K
L1i cache:             32K
L2 cache:              512K
L3 cache:              8192K
NUMA node0 CPU(s):     0-63
NUMA node8 CPU(s):     64-127

{: .output}

We can get some useful information from this output:

* The processor model is:  POWER8 (raw), altivec supported (you could use  along with the model number to get more information if you wanted)
* There are 8 cores (or Physical threads) per processor: ``Core(s) per socket: 8``
* There are 8 threads per core: ``Thread(s) per core: 8``
* There are two processors per node: ``Sockets: 2``
* This means that there are 2 * 8 = 16 Physical cores on the node
* There are 2 NUMA node(s) per node:`` NUMA node(s):  2``

We can extract information about the amount of memory available from the `/proc/meminfo` file. In this case we only want the first line as it will give
us the total amount of memory available so we use the ``head -1`` command:

```
[remote]$ head -1 /proc/meminfo
```
{: .bash}
```
MemTotal:       1067494144 kB
```
{: .output}

This tells us that there are approximately 1067 GB of memory (RAM) available on the login node.

> ## Units
> 
> A computer's memory and disk are measured in units called *bytes*.  The magnitude 
> of a file or memory use is measured using the same prefixes of the metric system: 
> kilo, mega, giga, tera.  So 1000 bytes is a kilobyte (kB), 1000 kilobytes is a megabyte (MB), 
> and so on.  
>
> You may also see units that use multiples of 1024 rather than 1000. So, 1024 bytes is a 
> kibibyte (KiB), 1024 KiB is a mebibyte (MiB), 1024 MiB is a gibibyte (GiB) and so on.
>
> For small amounts of storage the differences between these two unit systems are negligible but as the
> size increases the differences can be significant.
{: .callout}

Now we know how to connect to the HPC system, we will next learn how to transfer data on and off the system.

