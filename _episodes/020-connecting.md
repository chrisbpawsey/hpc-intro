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

Macs have had a terminal built in since the first version of OSX since it is
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

It is strictly speaking not necessary to have a terminal running on your local computer in order to access and use a remote system, only a window into the remote system once connected.  PuTTy is likely the oldest, most well-known, and widely used software solution to take this approach.

PuTTY is available for free download from [www.putty.org](http://www.putty.org/).  Download the version that is correct for your operating system and install it as you would other software on you Windows system.  Once installed it will be available through the start menu or similar.

Running PuTTY will not initially produce a terminal but intsead a window full of connection options.  Putting the address of the remote system in the "Host Name (or IP Address)" box and either pressing enter or clicking the "Open" button should begin the connection process.

If this works you will see a terminal window open that prompts you for a username through the "login as:" prompt and then for a password.  If both of these are passed correctly then you will be given access to the system and will see a message saying so within the terminal.  If you need to escape the authentication process you can hold the control/ctrl key and press the c key to exit and start again.

Note that you may want to paste in your password rather than typing it.  Use control/ctrl plus a right-click of the mouse to paste content from the clipboard to the PuTTY terminal.

For those logging in with PuTTY it would likely be best to cover the terminal basics already mentioned above before moving on to navigating the remote system.

## Logging onto the system

With all of this in mind, let's connect to a cluster. 

For this lesson, we will use [Cirrus](http://www.cirrus.ac.uk) - an HPC system located at [EPCC](http://www.epcc.ed.ac.uk)./
Although it's unlikely that every system will be exactly like Cirrus, it's a good example of what you can expect from an HPC system.
To connect to our example computer, we will use SSH (if you are using PuTTY, see above for instructions). 

SSH allows us to connect to Linux computers remotely, and use them as if they were our own.
The general syntax of the connection command follows the format `ssh yourUsername@some.computer.address`
Let's attempt to connect to the HPC system now:

```
ssh yourUsername@login.cirrus.ac.uk
```
{: .bash}

Your Instructor will give you the correct username to use in place of "yourUsername".

```{.output}
The authenticity of host 'login.cirrus.ac.uk (129.215.175.28)' can't be established.
ECDSA key fingerprint is SHA256:JRj286Pkqh6aeO5zx1QUkS8un5fpcapmezusceSGhok.
ECDSA key fingerprint is MD5:99:59:db:b1:3f:18:d0:2c:49:4e:c2:74:86:ac:f7:c6.
Are you sure you want to continue connecting (yes/no)?  # type "yes"!
Warning: Permanently added the ECDSA host key for IP address '129.215.175.28' to the list of known hosts.
yourUsername@login.cirrus.ac.uk password:  # no text appears as you enter your password
Last login: Mon Jun 18 16:21:52 2018 from cpc102380-sgyl38-2-0-cust601.18-2.cable.virginm.net
================================================================================

Cirrus HPC Service

--------------------------------------------------------------------------------
This is a private computing facility. Access to this system is limited to those
who have been granted access by the operating service provider on behalf of the
issuing authority and use is restricted to the purposes for which access was
granted. All access and usage are governed by the terms and conditions of access
agreed to by all registered users and are thus subject to the provisions of the
Computer Misuse Act, 1990 under which unauthorised use is a criminal offence.
--------------------------------------------------------------------------------

For help please contact the Cirrus helpdesk at: 
support@cirrus.ac.uk

================================================================================
```

If you've connected successfully, you should see a prompt like the one below. 
This prompt is informative, and lets you grasp certain information at a glance:
in this case `[yourUsername@computerName workingDirectory]$`.
(If you don't understand what these things are, don't worry! 
We will cover things in depth as we explore the system further.)

```{.output}
[yourUsername@cirrus-login0 ~]$
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

Now we can log into the Cirrus HPC system we will look at the nodes. You should remember that there 
are at least two  types of node on the system: *login nodes* and *compute nodes*.

The real work on a cluster gets done by the *worker* (or *execute*) *nodes*
Worker nodes come in many shapes and sizes,
but generally are dedicated to doing all of the heavy lifting that needs doing.

All interaction with the worker nodes is handled by a specialized piece of software called a scheduler
(the scheduler used in this lesson is called SLURM).  We'll learn more about how to use 
the scheduler to submit jobs next, but for now, it can also tell us more information about 
the worker nodes.  

For example, we can view all of the worker nodes with the `sinfo` command.

```
[remote]$ sinfo
```
{: .bash}
```
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
compute*     up 7-00:00:00      1 drain* gra259
compute*     up 7-00:00:00     11  down* gra[8,99,211,268,376,635,647,803,852,966,986]
compute*     up 7-00:00:00      1   drng gra272
compute*     up 7-00:00:00     31   comp gra[988-991,994-1002,1006-1007,1015,1017,1021-1022,1028-1033,1037-1041,1043]
compute*     up 7-00:00:00     33  drain gra[225-251,253-256,677,1026]
compute*     up 7-00:00:00    323    mix gra[7,13,25,41,43-44,56,58-77,107-108,112-113,117,125-126,163,168-169,173,180-203,205-210,220,224,257-258,300-317,320,322-349,385-387,398,400-428,448,452,460,528-529,540-541,565-601,603-606,618,622-623,628,643-646,652,657,660,665,678-699,710-711,713-728,734-735,737,741-751,753-755,765,767,774,776,778,796-798,802,805-812,827,830,832,845-846,853,856,865-866,872,875,912,914,916-917,925,928,930,934,953-954,959-960,965,969-971,973,1004,1008-1009,1011,1013-1014,1023-1025]
compute*     up 7-00:00:00    464  alloc gra[1-6,9-12,14-19,21-24,26-40,42,45-55,57,100-106,109-111,114-116,118-122,127,164-167,174-179,204,212-219,221-223,252,260-267,269-271,273-284,318-319,321,350-375,377-384,388-397,399,453-459,461-501,526-527,530-539,542-564,607-608,629-634,636-642,648-651,653-656,658-659,661-664,666-676,700-703,738,756-764,766,768-773,804,813-826,828-829,831,833-844,847-851,854-855,857-864,867-871,873-874,876-911,913,915,918-924,926-927,929,931-933,935-936,938-952,955-958,961-964,967-968,972,974-985,987,992-993,1003,1005,1010,1012,1016,1018-1020,1027,1034-1036,1042]
compute*     up 7-00:00:00    176   idle gra[78-98,123-124,128-162,170-172,285-299,429-447,449-451,502-525,602,609-617,619-621,624-627,704-709,712,729-733,736,739-740,752,775,777,779-795,799-800]
compute*     up 7-00:00:00      3   down gra[20,801,937]
```
{: .output}

There are also specialized machines used for managing disk storage, user authentication,
and other infrastructure-related tasks.
Although we do not typically logon to or interact with these machines directly,
they enable a number of key features like ensuring our user account and files are available throughout the cluster.
This is an important point to remember:
files saved on one node (computer) are available everywhere on the cluster!

## What's in a node? 

All of a cluster's nodes have the same components as your own laptop or desktop:
*CPUs* (sometimes also called
  *processors* or *cores*), *memory* (or *RAM*), and *disk* space.  
  CPUs are a computer's tool for actually running programs and calculations.
  Information about a current task is stored in the computer's memory.  Disk
  is a computer's long-term storage for information it will need later.

> ## Explore Your Computer
>
> Try to find out the number of CPUs and amount of memory available on your 
> personal computer.  
{: .challenge}

> ## Explore The Head Node
>
> Now we'll compare the size of your computer with the size of the head node: 
> To see the number of processors, run: 
> ```
> nproc --all
> ```
> {: .bash}
> or 
> ```
> cat /proc/cpuinfo
> ```
> {: .bash}
> to see full details.  
> 
> How about memory? Try running: 
> ```
> free -m
> ```
> {: .bash}
> or for more details: 
> ```
> cat /proc/meminfo free -m
> ```
> {: .bash}
{: .challenge}

> ## Explore a Worker Node
> 
> Finally, let's look at the resources available on the worker nodes where your jobs 
> will actually run.  
> Try running this command to see the name, CPUs and memory available on the worker nodes: 
> ```
> sinfo -n aci-377 -o "%n %c %m"
> ```
> {: .bash}
{: .challenge}

> ## Units
> 
> A computer's memory and disk are measured in units called *bytes*.  The magnitude 
> of a file or memory use is measured using the same prefixes of the metric system: 
> kilo, mega, giga, tera.  So 1024 bytes is a kilobyte, 1024 kilobytes is a megabyte, 
> and so on.  
>
{: .callout}

With all of this in mind, we will now cover how to talk to the cluster's scheduler,
and use it to start running our scripts and programs!
