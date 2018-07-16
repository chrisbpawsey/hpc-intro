---
title: "Using resources effectively"
teaching: 20
exercises: 10
questions:
- "How do we monitor our jobs?"
- "How can I get my jobs scheduled more easily and improve throughput?" 
objectives:
- "Understand how to look up job statistics."
- "Understand job size implications."
keypoints:
- "The better your resource request estimates, the better throughput you will see."
---

We now know most of the basic mechanics of getting research up and running on an HPC system.
We can log on, submit different types of jobs, use preinstalled software, 
and install and use software of our own.
What we need to do now is understand how we can use the systems effectively.

## Estimating required resources using the scheduler

Although we covered requesting resources from the scheduler earlier,
how do we know how much and what type of resources we will need in the first place?

Answer: we don't. 
Not until we've tried it ourselves at least once.
We'll need to benchmark our job and experiment with it before
we know how much it needs in the way of resources.

The most effective way of figuring out how much resources a job needs is to submit a test job,
and then ask the scheduler how many resources it used.
A good rule of thumb is to ask the scheduler for more time and memory than your job can use.
This value is typically two to three times what you think your job will need.

> ## Benchmarking Computational Fluid Dynamics
>
> Copy the Python 2D CFD application from the course website using the following command:
>
> ```
> wget https://epcced.github.io/hpc-intro/files/cfd.tar.gz
> ```
> {: .bash}
>
> (`wget` is a command that lets you download files directly from a website.)
>
> Then unpack it using
>
> ```
> tar -xvf cfd.tar.gz
> ```
> {: .bash}
> 
> (`tar` is a bit like zip, it allows you to create/expand an archive file from multiple files.)
>
> Create a job that runs the following commands in the directory containing the `cfd.py` program.
> 
> ```
> module load anaconda/python2
> python cfd.py 3 20000
> ```
> {: .bash}
> 
> You'll need to figure out a good amount of resources to ask for for this first "test run".
> You might also want to have the scheduler email you to tell you when the job is done.
>
> Hint: the job only needs 1 cpu and not too much time.
>  The trick is figuring out just how much you'll need!
{: .challenge}

Once the job completes, we can query the scheduler to see how long our job took.
We will use `qstat -x` to get statistics about our job.

By itself, `qstat -x -u yourUsername` shows all jobs that you have run on the system recently

```
[remote]$ qstat -x -u yourUsername
```
{: .bash}
```
NEED TO ADD
```
{: .output}


## Measuring the statistics of currently running tasks

As we saw in the previous episode on the scheduler, you can use the `qstat` command to 
monitor how much time and how many nodes are being used by current jobs. To list
details of your jobs (queued and running), use `qstat -u yourUsernname` and to list the
details of all jobs in the queue use `qstat -a`.

