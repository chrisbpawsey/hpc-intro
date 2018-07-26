---
title: "How does parallel computing work"
teaching: 40
exercises: 20
start: true
questions:
- "What are the different ways in which we can use resources in parallel?"
- "What implications do different parallel models have for performance and my use of HPC?"
- "How do I use different parallel strategies on an HPC system?"
objectives:
- "Be able to describe different approaches to parallelisation."
- "Identify how different approaches impact your use of an HPC system."
keypoints:
- "There are different parallel strategies available to get performance on HPC systems."
- "The approach you use depends on the software you are using and your research problem."
- "Most HPC systems allow you to use different parallel strategies in combination."
---

## Serial and Parallel applications

The basic idea of parallel computing is simple to understand: we divide our job into a number tasks that can be executed at the same time so that we finish the job in a fraction of the time that it would have taken if the tasks were executed one by one. With serial computing, tasks are completed one at a time after each other.

Implementing parallel computations however, is not always easy or possible.  Some HPC users will be able to write their own code and make use of one or more **parallel frameworks** to enable their code to run in parallel. Other HPC users will be using codes and applications written by other people and are dependent on the choices made by that application developer.

Even if the code or application you want to use is a **serial** application, HPC will often offer you some alternatives to running on your desktop PC such as access to nodes with more memory or even the opportunity to run many analyses concurrently (this is another form of parallelism).

Consider the following analogy:

Suppose that we want to paint the four walls in a room. We’ll call this the problem. We can divide our problem into 4 different tasks: paint each of the walls. In principle, our 4 tasks are independent from each other in the sense that we don’t need to finish one to start one another. We say that we have 4 concurrent tasks; the tasks can be executed within the same time frame. However, this does not mean that the tasks can be executed simultaneously or in parallel. It all depends on the amount of resources that we have for the tasks. If there is only one painter, this guy could work for a while in one wall, then start painting another one, then work for a little bit on the third one, and so on. The tasks are being executed concurrently but not in parallel. If we have two painters for the job, then more parallelism can be introduced. Four painters could executed the tasks truly in parallel.

## Types of parallelism

Many HPC users will encounter two forms of parallelism:

**Shared Memory Parallelism**: Tasks share the resources on a compute node and communicate between themselves by reading and writing data to shared memory.  Common terms to look for are *OpenMP* and *Threading* to help identify this type of parallelism.

Using this form of parallelism, applications are (usually) limited to the number of cores and the amount of memory on a single compute node.

**Distributed Memory Parallelism**: In this mode of parallelism, tasks can be distributed onto compute cores all over the cluster; these cores do not necessarily need to be on the same compute node. The tasks (and cores) communicate with each other using a special protocol over a high-speed network (*an interconnect*). The typical term you will see that identifies this type of parallelism is *MPI*.

> ## Am I parallel or serial?
>
> What codes or applications are you planning to use?  Are they serial or parallel applications?  
> How do you know?
> Have you an idea of the optimum number of cores a job will need? 
{: .challenge}

> ## Getting some files
>
> We are going to **compile** and run some serial and parallel applications and explore how they work.  
> You will need some files, so login to Cirrus and use `wget` to download some files:
> ```
> wget https://github.com/EPCCed/hpc-intro/tree/gh-pages/files/parallel_files.tar.gz
> ```
> {: .bash}
> 
> This is a compressed file. How can we uncompress it to examine it's contents?
{: .challenge}

> ## Compiling codes
> All of these short example codes are written in Fortran. They need to be **compiled** to an executable file so that we can run them on Cirrus.
> To do so, we need to load a compiler module:
> ```
> module load gcc/6.2.0
> ```
> {: .bash}
>
> and to compile the `serial_pi.f90` code we would do:
>
> ```
> gfortran -o serial_pi.x serial_pi.f90
> ```
> {: .bash}
>
> This will create a new file, `serial_pi.x` which you can execute.
{: .challenge}

> ## Submitting a job
> Create and submit a job script to run the `serial_pi.x` code and examine the result. Did it work?
>
{: .challenge}

## Compiling and running OpenMP codes
The files you downloaded includes an OpenMP code: `hellw.f`. To compile this code we need to tell the compiler to recognise it as an an OpenMP parallel
code:
```
gfortran -fopenmp hellw.f -o hellw.x
```
{: .bash}

If you look at the souce code, you may be able to identify some of the OpenMP *pragmas* (instructions to the compiler) that mark sections of the code that
can be executed in parallel.





