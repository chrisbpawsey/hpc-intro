---
title: "What is an HPC system?"
teaching: 15
exercises: 10
questions:
- "What is a high-performance computer?!"
- "How are high-performance computers different from personal computers?"
- "How do these differences influence how I use HPC systems most effectively?"
objectives:
- "Understand the general HPC architecture."
keypoints:
- "A high-performance computer system provides a larger compute capability than is possible to package in a personal computer."
- "HPC systems are typically an aggregation of a bunch computers, each one of which can look pretty similar to your personal computer."
- "HPC systems are usually accessed remotely, over the network."
- "HPC systems are usually shared among many users.  Each user typically gets a dedicated portion of the computer's resources for a period of time."
- "Special measures have to be taken to provide a file system that can keep up with an HPC system."
- "HPC systems often provide a lot of different software packages, and provide ways of selecting and configuring them to get the environment you need."
---

# What is a High-Performance Computer?

A high-performance computer (HPC system) is a tool used by computational scientists and engineers to tackle problems that require more computing resources or time than they can obtain on the personal computers available to them. HPC systems range in size from the equivalent of just a few personal computers to tens, or even hundreds of thousands of them.  They tend to be expensive to buy and operate, so they are often shared at the departmental or institutional level.  There are also many regional and national HPC centers.  Because of this, most HPC systems are accessed remotely, over the network.

HPC systems are generally constructed from many individual computers, similar in capability to many personal computers.  Each of these individual computers is often referred to as a **node**. HPC systems often include several different types of nodes, which are specialized for different purposes.  **Head** (or **front-end** or **login**) nodes are where you login to interact with the computer.  **Compute** nodes are where the real computing is done.

<!-- It would be nice to have a diagram that showed the different types of nodes, and the network. 
Something like http://www.archer.ac.uk/training/course-material/2018/03/intro-hw/slides/L01_WhyHPC.pdf
-->

Depending on the HPC system, the compute nodes, even individually, might be much more powerful than a typical personal computer.  They often have multiple processors (each with many cores), and may have accelerators (such as GPUs) and other capabilities less common on personal computers.

In order to share these large systems among many users, it is common to allocate subsets of the compute nodes to tasks (or **jobs**), based on requests from users.  These jobs may take a long time to complete, so they come and go in time. To manage the sharing of the compute nodes among all of the jobs, HPC systems use a **batch system** or **scheduler**.  The batch system usually has commands for submitting jobs, inquiring about their status, and modifying them.  The HPC center defines the algorithms by which jobs are prioritized for execution on the compute nodes, while ensuring that the compute nodes are not overloaded. <!-- reference to episode 30 -->

The kind of computing that people do on HPC systems often involves very large files, and/or many of them.  Further, the files have to be accessible from all of the front-end and compute nodes on the system.  So most HPC systems have specialized filesystems that are designed to do a better job of meeting these needs than typical network filesystems, like NFS. Frequently, these specialized filesystems are intended to be used only for short- or medium-term storage, not permanent storage.  So HPC systems often have several different filesystems available -- for example **home**, and **scratch** filesystems.  It can be very important to select the right filesystem to get the results you want (performance or permanence are the typical trade-offs).  <!-- reference to episode 35 -->

Because HPC systems serve many users with different software needs, HPC systems often have multiple versions of commonly used software packages installed.  Since you can't easily install and use different versions of a package easily at the same time, HPC systems often use an approach called **modules**, which allows you to configure your software environment with the particular versions of software that you need. <!-- reference to episode 40 -->

## Is there any difference between HPC and Cloud computing?

The words "cloud", "cluster", and "high-performance computing" get thrown around a lot.
So what do they mean exactly?
And more importantly, how do we use them for our work?

The *cloud* is a generic term commonly used to refer to remote computing
resources of any kind -- that is, any computers that you use but are not
right in front of you.  
Cloud can refer to webservers, remote storage, API endpoints,
as well as more traditional "compute" resources.
A *cluster* on the other hand, is a term used to describe a network of computers.
The computers in a cluster typically share a common purpose,
and are used to accomplish tasks that might otherwise be too big for any one computer.

![The cloud is made of Linux](../fig/linux-cloud.jpg)

## Where are we?

Very often, many users are tempted to think of a high-performance
computing installation as one giant, magical machine.
Sometimes, people will assume that the computer they've logged onto is the entire computing cluster.
So what's really happening? What computer have we logged on to?
The name of the current computer we are logged onto can be checked with the `hostname` command.

```
[remote]$ hostname
```
{: .bash}
```
gra-login3
```
{: .output}

Individual computers that compose a cluster are typically called *nodes* (although
  you will also hear people call them *servers*, *computers* and *machines*).  On a cluster,
  there are different types of nodes for different types of tasks.  
The node where you are right now is called the *head node*, *login node* or
*submit node*.  A login node serves as an access point to the cluster.
As a gateway, it is well suited for uploading and downloading files,
setting up software, and running quick tests.
It should never be used for doing actual work.

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
