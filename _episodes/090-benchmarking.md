---
title: "Understanding what resources to use"
teaching: 30
exercises: 15
questions:
- "How can I work out what resources to request in my HPC jobs?"
- "What should I measure to understand if I am using the HPC system effectively?"
objectives:
- "Understand key performance metrics and how to measure them."
- "Be able to choose resource requests in jobs based on performance metrics."
keypoints:
- "Basic benchmarking allows you to use HPC resources more effecively."
- "Performance is generally more useful metric than runtime or speedup."
- "A small number of measurements can make a large difference."
---

Once you finished testing your batch job scripts to ensure that your job script is
doing what you expect (data files are in the right place, etc. ).  You want to submit you batch job scripts
to run on your local HPC resource, using the "scheduler", however you are not sure resources you should
request. Specifically what resources do you need initially and for parallel applicatiosns.

Remember the basic resources that are mananged by the scheduler on a HPC system are the *number of nodes*  and the *walltime* which is how long do you want to use them.

This leads us to a number of questions:
- "How do I do this in LSF?"
- "What size should you request?"
- "How much wall time do I need to specify for the 'CHUNK' you requested?"
- "How does any of this relate to benchmarking?"

So we will go through these questions in reverse order and hopefully that will make things clearer!

## Benchmarking
*In HPC we love to benchmark everything but not without good reason!*  Benchmarking provides you with 
insight into how well something performs in a controlled environment.  Within the scope of HPC carpentry,
you need to have a strong understanding how your application performs (or scalibitly) on different 
architecture. Being able to estimate the average runtime for a single job will allow you to:
- *right size* your jobs and reduce the *wait* time for you jobs to start.
- scope out your computational research timelines.
- prepare better applications for access to large HPC centres like EPCC.

### Parallelism 
The two methods for parallelising your applications are shared memory and distributed memory.

#### Distributed Memory
The distributed memory method or Multiple Instruction, Multiple Data (MIMD) is for applications that 
have large amount of data that does not fit into the memory space of a single node.  So we can 
split the data up across a large number nodes this is known as "domain decomposition", the goal 
is that if we can keep the data in memory and not on disc. This will require some data movement 
and rather have a single node do all the work we divide the work into smaller chunks. This does 
require some the nodes to communicate with their "neighbors" nodes. This uses the MPI library
for communication.

#### Shared Memory
The shared memory method or Single Instruction, Multiple Data (SIMD) is done on a single node using 
multi-cores. This is the Serial-Fork model of parallelism, this is openMP.  OpenMP are compiler directives
that you add to your source code.  Typically splitting up the work done in loops across all the cores on
a processor(s) within a single node.

### Benchmarking checklist
To start you will need:
 - know how your application(s) is parallelized *shared memory* or *distributed memory* (or a combination of the two known as hybrid) 
 - a representative test case, ideally something you have run before!  
 - a method to time the runs, here a couple of examples

#### Sample Timing methods

The shell *time* utility see (*man time*)
```
time ./dgemm_ex.exe 
```
{: .bash}

returns 
```
    hpc-intro >time
    real	0m0.001s
    user	0m0.000s
    sys	0m0.000s
```
{: .bash}

or build your own using the *date* function into your job script such as this
exampler for running the 3D animation software Maya.
```
    echo "start Render"
    start=`date +%s`
    Render -r $RENDER_METHOD  $SCENE -log output.log

    stop=`date +%s`
    echo "finish Render" 

    finish=$(($stop-$start))
    echo `date  +%c` >> ~/Work/maya/timings.log
    echo $SCENE was rendered in $finish seconds using $RENDER_METHOD >> ~/Work/maya/timings.log
    echo " " >> ~/Work/maya/timings.log
```
{: .bash}

**NOTE: This is nice as it automatically creates a seperate timing log file of the runs and I can easily 
reformat it to plot it.**
 
What you trying to understand is how to optimize your resource usage. 
Running a set of jobs where you increase the number of nodes (1, 2, 4, 16, 32,...) or chunk size and 
measure how the runtime changes.  Once you reach the point of diminishing returns you should have a feel
for what size CHUNK you should be requesting and able to extrapolate what the runtime will be for your actual models. 

It is a good practice as well to run the same benchmark 3 times as it helps to insure you have consistent performance data. 
  

## LSF Resource
There are lot of different [LSF directives][1] that you will need to understand but 
we will focus on how to do use very basic "#PBS -l" resource requests
specifically for benchmarking.
```
    #BSUB -W hh:mm
    #BSUB -n <number of cores>
    #BSUB -R "span=[ptile=<number of MPI tasks>]"
   
```
{: .bash}

#### -W walltime
Ideally you have run your application someplace and have some idea about how long it
takes to run a job, ie  your laptop.  That would be a suitable starting time.
```
   #BSUB -W 01:00
```
{: .bash}

#### -x exclusive

```
   #BSUB -x 
```
{: .bash}

With benchmarking you want to run exclusively on the resource, your jobs maybe in the 
queue a bit longer but you don't want any resource contention when benchmarking.

#### -a 
This is the complicate one as it is dependent on your code, and the best advice is to 
read the documentation about the HPC systems.   
For simple mpi codes:
  To get 16 cores/MPI tasks

```
   #BSUB -n 16
   #BSUB -R "span[ptile=16]"
```
{: .bash}

To get 24 cores/MPI tasks across two nodes

```
   #BSUB -n 24
   #BSUB -R "span[ptile=12]"
```
{: .bash}

## Benchmarking and scalability

With your runtimes collected you should be able to determine if you application is *strong* scaling or *weak* scaling.  Codes that are *strong* scaling (or cpu-bound) for a fixed problem size as the number of cores/nodes increases.  

Weak scaling (or Memory-bound) codes should have the same runtime for problems where the model double in size and the number of nodes doubles.  The amount of work per node stays the same. 

See ["Measuring Parallel Scaling Performance"][2] from SHARCNET in Canada they have really nice detailed description on "strong" scaling and "weak" scaling. 

   
## REMEMBER
- resources are finite and everyone's time is valuable!
- be cautious about benchmarking you can burn through your allocation quickly!
- the best advice is be a domain expert with you application and to read the 
  documentation about the HPC systems.  
- Benchmarking is important! It can help you improve you and your group's understanding of  
  the applications and tools you need to do your science!  


#### References

[1]: https://www.ibm.com/support/knowledgecenter/en/SSWRJV_10.1.0/lsf_users_guide/job_submit.html
[2]: https://www.sharcnet.ca/help/index.php/Measuring_Parallel_Scaling_Performance

