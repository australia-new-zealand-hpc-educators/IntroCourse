# Environment Modules and Job Schedulers

## A Dynamic Environment
* Environment modules provide for the dynamic modification of the user's environment via module files, such as the location of the application's executables, manual path, libraries, and so forth.
* Modulefiles also have the advantages of being shared on many users on a system (such as an HPC system) and easily allowing multiple installations of the same application but with different versions and compilation options.
* Check the current environment with the `env` (environment) command.

## Environment Modules  Commands
* The are two implementations of environment modules. The classic modules system was available on the Edward HPC, and the newer Lmod is on Spartan. LMod is considered superior for hierarchies of modules.

## Module Commands
| Command                         | Explanation                                            |
|---------------------------------|:------------------------------------------------------:|
| `module help`                 | List of switches, commands and arguments for modules   |
| `module avail`                | Lists all the modules which are available to be loaded.|
| `module display <modulefile>` | Display paths etc for modulefile                       |

## Module Commands
| Command                         | Explanation                                            |
|---------------------------------|:------------------------------------------------------:|
| `module load <modulefile>`    | Loads paths etc to user's environment                  |
| `module unload <modulefile>`  | Unloads paths etc from user's environment.             |
| `module list`                 | lists all the modules currently loaded.                |

## Module Commands
* There is also the `module switch <modulefile1> <modulefile2>`, which unloads one modulefile (modulefile1) and loads another (modulefile2).
* Lmod modules also support regular expressions, e.g., `module -r avail "^Python"`
* There is also the lmod-specific `module spider <modulename>`, which traverses through the system for all modules and provides a description.

## Portable Batch System I
* The Portable Batch System (or simply PBS) performs job scheduling by assigning unattended background tasks expressed as batch jobs, among the available resources.
* Originally developed by MRJ Technology Solutions under contract to NASA in the early 1990s. Released as an open-source product as OpenPBS. Forked by Adaptive Computing as TORQUE (Terascale Open-source Resource and QUEue Manager). Many of the original engineering team now part of Altair Engineering who have their own commercial version, PBSPro.

## Portable Batch System II
* A batch system typically consists of a resource manager (e.g., TORQUE) and a job scheduler (e.g., Maui, Moab), or a combination (e.g., PBSPro, Slurm).
* TORQUE and MOAB was used on the Edward HPC system. Slurm is used on Spartan.

## Slurm Workload Manager
* Slurm, used on Spartan, began development as a collaborative effort primarily by Lawrence Livermore National Laboratory, SchedMD, Linux NetworX, Hewlett-Packard, and Groupe Bull as a Free Software resource manager. As of November 2015, TOP500 list of most powerful computers in the world indicates that Slurm is the workload manager on six of the top ten systems. Slurm's design is very modular with about 100 optional plugins.
* There is a repository for converting PBS to Slurm: https://github.com/bjpop/pbs2Slurm

## Job Submission Principles
* The steps for job submission are (a) setup and launch., (b) monitor., and (c) retrieve results and analyse. Jobs are launched from the login node with resource requests and, when the job scheduler decides, run on compute nodes. Some directories (e.g.,. user home or project directories) are shared across the entire cluster so output is an accessible place.
* Job scripts are simply resource requests (understood by scheduler), a batch of commands (understood by shell) with output to files. Include schedular requests first!

## Fair Share
* A cluster is a shared environment thus a a resource requesting system. Policies ensure that everyone has a "fair share" to the resources (e.g., user processor limits).
* Spartan's general partition (cloud, physical) treat all jobs equally. The GPGPU has allocation based on purchasing.

## DON'T RUN JOBS ON THE LOGIN NODE!
* The login node is a **particularly** shared resource. All users will access the login node in order to check their files, submit jobs etc. If one or more users start to run computationally or I/O intensive tasks on the login node (such as forwarding of graphics, copying large files, running multicore jobs), then that will make operations difficult for everyone.

## Representation of Job Submission
<img src="http://levlafayette.com/files/rabbitjobs.png" width="100%" height="100%" title="Job submission using rabbits" />
From the IBM 'Red Book' on Job Submission.

Part 2: Partitions and Queues
* Setup and launch consists of writing a short script that initially makes resource requests  (walltime, processors, memory, queues) and then commands (loading modules, changing  directories, running executables against datasets etc), and optionally checking queueing system.
* Core command for checking paritions is `sinfo -s`, or `sinfo -p $partition` for partition and node status. Major partitions are: `cloud`, `physical`, `gpgpu`. Note also `longcloud`, and `shortgpgpu`.
* Core command for checking queue `squeue` or `showq` (on Spartan).
* Note that Slurm has full and abbreviated directives.

Part 2: Job Status
* Core command for job submission `sbatch [jobscript]` 
* Core command for checking job `squeue -j [jobid]`, detailed command `scontrol show job [jobid]` (SLURM), or all user's jobs `squeue -u [username]`.
* Core command for deleting job `scancel [jobid]`
* Basic resource usage `sacct -j [jobid] --format=JobID,JobName,MaxRSS,Elapsed` (or `-u [username]`

## Single Core Job
```bash
#!/bin/bash
#SBATCH -­p cloud
#SBATCH ­­--time=01:00:00
#SBATCH ­­--ntasks=1
module load my­app­compiler/version
my­app data ```
* A job can call other scripts. 

Part 3 : Multicore and Multithreaded Jobs
* In Slurm, `ntasks` means number of tasks, whereas `cpus-per-task` allocates processor cores. In most jobs (serial, MPI) this is 1 by default.
* With shared-memory multithreaded jobs on (e.g., OpenMP), modify the `--cpus-per-task` to a maximum of the cores in the node.<br />
`#SBATCH ­­--cpus-­per-­task=8`

Part 3 : Multinode Jobs
* For distributed-memory multicore job using message passing, the multinode partition has to be 
invoked and the resource requests altered e.g.,
`#!/bin/bash`<br />
`#SBATCH -­p physical`<br />
`#SBATCH ­­--nodes=2`<br />
`#SBATCH ­­--ntasks-per-node=8`<br />
`module load my­app­compiler/version`<br />
`srun my­mpi­app`

Part 3 : Multinode Jobs
* Multinode jobs on Spartan may be slower if they have a lot of interprocess communication and they cross compute nodes.
* This said, multinodes jobs can also request total tasks/cores rather than allocating them per node. e.g., `#SBATCH ­­--ntasks=16`<br />
