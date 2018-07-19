---
title: "Bootstrapping your use of HPC"
teaching: 10
exercises: 140
questions:
- "How can I get started on using HPC?"
- "Where can I get help to start using HPC?"
objectives:
- "Get help to get your work up and running on an HPC system"
- "Understand where you can get help from in the future"
keypoints:
- "Understand the next steps for you in using HPC."
- "Understand how you can access help and support to use HPC."
---

Now you know enough about HPC to explore how to use it for your work or to understand
what its potential benefits are you. You may also have ideas around where the 
barriers and difficulties may lie and have further questions on how you can 
start using and/or trying HPC in your area.

This session is designed to give oyu the opprotunity to explore these questions and
issues. The instructors and helpers on the course will be on hand to answer your
questions and discuss next steps with you.

> ## Potential discussions
>
> 

If you have a practical example of something from your area of work that you would like
help with getting up and running on an HPC system or exploring the performance of
on an HPC system, this is great! Please feel free to discuss this with us and ask
questions (both technical and non-technical).

If you do not have an example of something that you would like look at right now, that
is not a problem either. We have prepared an exercise for you that should allow you to 
practice all the skills you have learned over the lesson and explore the use of HPC
in a practical way.

> ## Exploring the performance of GROMACS
>
> [GROMACS](http://www.gromacs.org) is a world-leading biomolecular modelling package
> that is heavily used on HPC systems around the world. Choosing the best resources
> for GROMACS calculations is non-trivial as it depends on may factors, including:
>
> - The underlying hardware of the HPC system being used
> - The actual system being modelled by the GROMACS package
> - The balance of processes to threads used for the parallel calculation
>
> In this exercise, you should try and decide on a good choice of resources and settings
> on Cirrus for a typical biomolecular system. This will involve:
>
> - Downloading the input file for GROMACS from https://epcced.github.io/hpc-intro/files/ion-channel.tpr
> - Writing a job submission script to run GROMACS on Cirrus using the system documentation
> - Varying the number of nodes (from 1 to 32 nodes is a good starting point) used for the GROMACS job
>   and benchmarking the performance (in ns/day)
> - Using the results from this study to propose a good resource choice for this GROMACS calculation
>
> If you want to explore further than this initial task then there are a number of 
> different interesting ways to do this. For example:
> 
> - Vary the number of threads used per process
> - Reduce the number of cores used per node
> - Allow the calculation to use both hyperthreads on each core
>
> Please ask for more information on these options from a helper!
{: .challenge}

