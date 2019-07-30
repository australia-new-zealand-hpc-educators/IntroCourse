# Core concepts of HPC
 
## Using HPC
* People need to use HPC when their datasets are "too large" or processing is "too complex" for their PC.
* HPC is the *best* tool to processing large and complex datasets
* However due to latency and bandwdith issues, it is not always the best for visualisation.

## Supercomputers and HPC
* "Supercomputer" means any single computer system that has exceptional processing power for its time. 
* One popular metric (LINPACK) is the number of floating­ point operations per second (FLOPS) such a system can carry out (http://top500.org). HPC Challenge and HPCG are broader, more interesting metrics. 
* High Performance Computer (HPC) is any computer system whose architecture allows for above average performance. 

## High Throughput Computing
* High Throughput Computing (HTC) is an architecture for maximum job completion.
* HPC for capability (can scale over many nodes) vs HTC capacity computing (many cores are available).
* Every cost is an opportunity cost! If money is spent on interconnect there may not be enough for cores.

## Clusters and Research Computing
* Clustered computing is when two or more computers serve a single resource. This improves performance and provides redundancy in case of failure system. Typically commodity systems with a high-speed local network.
* Research computing is the software applications used by the scientific community to aid research. Does not necessarily equate with high performance computing, or the use of clusters.­ It is whatever researchers use and do. Note issues of reproducibility and environments.

## Parallel Computing
* With a cluster architecture, applications can be more easily parallelised across them. *Data parallel*, running same task in parallel; the horse and cart example, Monte Carlo experiments. *Task parallel*, running independent tasks in parallel with communication; driving a car, molecular modelling.
* Further examples of serial versus parallel; weather forecasting, aerodynamic design, fluid mechanics, radiation modelling, molecullar dynamics, CGI rendering for popular movies, etc. Reality is a parallel system!

## Practical Example
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanIntro/master/Images/smartparallel.jpg" height="200%" width="200%" />

## HPC Cluster and Parallel Processing Components
* A chassis or rack, containing multiple computer system units, interconnect, and providing power.
* Computer system units or nodes, containing memory, local disk, and sockets.
* Sockets contain a processor which has one or more cores which do the processing.
* Logical processes have shared resources (e.g., memory) which may have multiple instruction stream threads.

## Generic HPC Cluster Design
<img src="https://raw.githubusercontent.com/UoM-ResPlat-DevOps/SpartanIntro/master/Images/genericcluster.png" />
Image originally from the VPAC

## HPC and Cloud Computing
* Cloud computing is someone else's computer.
* One can use cloud deployment tools for HPC compute nodes (e.g., University of Melbourne).
* One can use HPC compute nodes to launch cloud VMs (e.g., University of Freiburg).

## Logging In
* To log on to a HPC system, you will need a user account and password and a Secure Shell (ssh) client. Linux distributions almost always include SSH as part of the default installation as does 
Mac OS 10.x. For MS-­Windows users, the free PuTTY client is recommended (http://putty.org). 
* To transfer files use scp, WinSCP, Filezilla (`https://filezilla-project.org/`), or rsync.
* Example login: `lev@spartan.hpc.unimelb.edu.au`

## SSH Keys, Config, Data Transfers
* Consider using an `.ssh/config` file and using passwordless SSH by creating a keypair and adding to your `.ssh/authorized_keys` file on Spartan.
* SSH Keys will make your life easier. Follow the instructions here: `https://dashboard.hpc.unimelb.edu.au/faq/`
* Both useful for data transfers. c.f., `https://dashboard.hpc.unimelb.edu.au/managing_data/` 

## Latency, Bandwidth, Data Location
* Latency is the speed of data transfer, bandwidth is the "width" of the "band" of data transfer.
* Using a road analogy, "latency" is the smoothness of the surface, "bandwith" is the number of lanes.
* Distance is a *very* important factor. Data for processing should be kept as close as possible to the processor.
* As Grace Hopper used to say: "Mind your nanoseconds!" https://www.youtube.com/watch?v=JEpsKnWZrJ8

