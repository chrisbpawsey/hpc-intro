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

<!-- Still need to add exercises to this episode -->

# What is a High-Performance Computer?

A high-performance computer (HPC system) is a tool used by computational scientists and engineers to tackle problems that require more computing resources or time than they can obtain on the personal computers available to them. HPC systems range in size from the equivalent of just a few personal computers to tens, or even hundreds of thousands of them.  They tend to be expensive to buy and operate, so they are often shared at the departmental or institutional level.  There are also many regional and national HPC centers.  Because of this, most HPC systems are accessed remotely, over the network.

HPC systems are generally constructed from many individual computers, similar in capability to many personal computers, connected together by some type of network (often referred to as the *interconnect*).  Each of these individual computers is often referred to as a *node*. HPC systems often include several different types of nodes, which are specialized for different purposes.  *Head* (or *front-end* or *login*) nodes are where you login to interact with the HPC system.  *Compute* nodes are where the real computing is done. You generally do not have access to the compute nodes
directly - access to these resources is controlled by a *scheduler* or *batch system* (more on this later!).

![Generic HPC system structure](../fig/hpc_system_diagram.png)

Depending on the HPC system, the compute nodes, even individually, might be much more powerful than a typical personal computer.  They often have multiple processors (each with many cores), and may have accelerators (such as *Graphics Processing Units (GPUs)*) and other capabilities less common on personal computers.

> ## HPC systems are made up of many computers linked together
> Each individual "computer" component of an HPC system is known as a *node*. Different types of node exist for different tasks. The nodes are connected together by a network usually known as the *interconnect*.
{: .callout}

## Nodes

Each node on an HPC system is essentially an individual computer:


![Generic node structure](../fig/node_diagram.png)

The *processor* contains multiple *compute cores* (usually shortened to *core*); 4 in the diagram above. Each core contains a *floating point unit (FPU)* which is responsible for actually performning the computations on the data and various fast *memory caches* which are responsible for holding data that is currently being worked on. The compute power of a processor generally depends on three things:

* The speed of the processor (2-3 GHz are common speeds on modern processors)
* The power of the floating point unit (generally the more modern the processor, the more powerful the FPU is)
* The number of cores available (12-16 cores are typical on modern processors)

Often, HPC nodes have multiple processors (usually 2 processors per node) so the number of cores available on a node is doubled (i.e. 24-26 cores per node, rather than 12-16 cores per node). This configuration can have implications for performance.

Each node also has a certain amount of *memory* available (also referred to as *RAM* or *DRAM*) in addtion to the processor memory caches. Modern compute nodes typically have in the range 64-256 GB of memory per node.

Finally, each node also has access to *storage* (also called *disk* or *file system*) for persistent storage of data. As we shall see later, this storage is often shared across all nodes and there are often multiple different types of storage connected to a node.


## The Scheduler

In order to share these large systems among many users, it is common to allocate subsets of the compute nodes to tasks (or *jobs*), based on requests from users.  These jobs may take a long time to complete, so they come and go in time. To manage the sharing of the compute nodes among all of the jobs, HPC systems use a *batch system* or *scheduler*.  The batch system usually has commands for submitting jobs, inquiring about their status, and modifying them.  The HPC center defines the priorities of different jobs for execution on the compute nodes, while ensuring that the compute nodes are not overloaded.

For example, a typical HPC workflow could look something like this:

1. Transfer input datasets to the HPC system (via the login nodes)
2. Create a job submission script to perform your computation (on the login nodes)
3. Submit your job submission script to the scheduler (on the login nodes)
4. Scheduler runs your computation (on the compute nodes)
5. Analyse results from your computation (on the login or compute nodes, or transfer data for analysis elsewhere)

We will discuss the scheduler more later.

## Storage and *File Systems*

The kind of computing that people do on HPC systems often involves very large files, and/or many of them.  Further, the files have to be accessible from all of the front-end and compute nodes on the system.  So most HPC systems have specialized file systems that are designed to meet these needs. Frequently, these specialized file systems are intended to be used only for short- or medium-term storage, not permanent storage.  As a consequence of this, most HPC systems often have several different file systems available -- for example *home*, and *scratch* file systems.  It can be very important to select the right file system to get the results you want (performance or permanence are the typical trade-offs).

Your instructor will provide details of the file systems that are available on the HPC system you are using for this lesson.

## Accessing Software on HPC Systems

Because HPC systems serve many users with different software needs, HPC systems often have multiple versions of commonly used software packages installed.  Since you cannot easily install and use different versions of a package at the same time without causing potential issues, HPC systems often use *environment modules* (often shortened to *modules*) that allow you to configure your software environment with the particular versions of software that you need. We will learn more about modules and how they work later in this lesson.

Many HPC systems also have a custom environment that means that binary software packages (for example, those you may download from websites) will not simply work "out-of-the-box". They may need differnt options or settings in your job script to make them work or, at worst, may need to be recompiled from source code to work on the HPC system.

# What does all this mean for me?

A few properties of HPC systems mean that you often need to modify your computational workflow from your local system:

* **HPC systems are a shared resource** 
  - You cannot get administrator access to the system to install software
  - A scheduler is used to control access to compute resources meaning that your work may not run instantaneously
  - Environment modules are used to control your software environment
* **HPC systems are accessed remotely**
  - You will often need to transfer data to/from the HPC system
  - Access is generally via a command line interface rather than a graphical interface
* **HPC systems often have more than one file system**
  - These file systems differ in purpose and properties
  - You often need to be aware of which file system you are using and why
* **HPC systems are parallel resources**
  - You usually need to be able to use resources in parallel to benefit from HPC
  - This parallelism can be achieved in a number of ways: from many independent tasks (HTC) to a single large parallel computation

## ...and what about Performance

The "P" in HPC does stand for performance and the make up of an HPC system is designed to allow researchers to access higher performance (or capabilities) than they could on their local systems. The way in which an HPC system is put together does have an impact on performance and workfows are often categorised according to which part of the HPC system constrains their performance. You may see the following terms used to describe performance on an HPC system:

* Compute bound. The performance of the workflow is limited by the floating point (FPU) performance of the nodes. This is typically seen when peforming math-heavy operations such as diagonalising matrices in computational chemistry software or computing pairwise force interactions in biomolecular simulations.
* Memory bound. The performance of the workflow is limited by access to memory (usually in terms of *bandwidth*: how much data can you access at one time). Almost all current HPC applications have a part of their workflow that is memory bound. A typical example of a memory bound application would be something like computational fluid dynamics.
* I/O bound. The performance of the workflow is limited by access to storage (usually in terms of bandwidth but sometimes in terms of numbers of files being accessed simultaneously). This is often seen in climate modelling and bioinformatics workflows.
* Communication bound. The performance of the workflow is limited by how quickly the parallel tasks can exchange information across the interconnect. This is often seen when single applications scale out to very large numbers of nodes and the amount of traffic flowing across the interconnect becomes high. Particular common algorithms, such as multidimensional Fourier transformations, can also exhibit this behaviour.

In reality, most research workflows exhibit many of these bottlenecks at different points in their execution.

> ## What is limiting me?
> Think about a research workflow you use. Discuss this with your neighbor. Can you identify which of the bottlenecks described above may apply to your workflow?
>
> Identify 1 of these bottlenecks in the Etherpad and explain why you think it applies to your workflow.
>
> This activity should take about 5 minutes.
{: .challenge}

# Why all this conceptual stuff?

Understanding more about how HPC systems work and achieve high performance will allow you to appreciate better the opportunities and challenges for using HPC for your research and also give you the tools required to help yourself out if (when!) you run into problems with using HPC systems in the future;.

Now lets get going and actually connect to an HPC system!

