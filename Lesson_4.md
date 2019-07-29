# Advanced Job Submission

## Job/Batch Arrays
* With a job or batch array the same batch script, and therefore the same resource requests, is used multiple  times. A typical example is to apply the same task across multiple datasets. The following example submits 10 batch jobs with myapp running against datasets dataset1.csv, dataset2.csv, ... 
dataset10.csv
`#SBATCH ­­array=1­-10`<br />
`myapp ${Slurm_ARRAY_TASK_ID}.csv`

## Job/Batch Dependencies
* A dependency condition is established on which the launching of a batch script depends, creating a conditional pipeline. The dependency directives consist of `after`, `afterok`, `afternotok`, `afterany`, `singleton` and more. A typical use case is where the output of one job is required as the input of the next job. Multiple job dependencies are specified with colon separated values.
`#SBATCH ­­dependency=afterok:myfirstjobid mysecondjob`

## Interactive Jobs
* An interactive job, based on the resource requests made on the command  line, puts the user on to a compute node. This is typically done if they user wants to run a  large script (and shouldn't do it on the login node), or wants to test or debug a job. The  following command would launch one node with two processors for ten minutes.
`sinteractive ­­--nodes=1 --­­ntasks-­per-­node=2 --­­time=0:10:0`

## Multiple Job Steps
* Sometimes a job needs to consist of several steps that need to be carried on sequence, even if the individual components are in parallel. In this case the entire job resource set can be called with an aggregation of walltime and with a maximum reduction operation for memory and resources.
* Whilst this can be convenient, to include everything in a single script, it runs the risk on a busy system of being inefficient in the queue.

## Multiple Job Steps II
```bash
#!/bin/bash
#SBATCH --­partition physical
#SBATCH ­­--nodes=2
#SBATCH ­­--ntasks=12
#SBATCH --time=24:00:00
srun -N 2 -n 12 -t 06:00:00 ./my­mpi­app
export OMP_NUM_THREADS=6
srun -N 1 -n2 -c $OMP_NUM_THREADS -t 12:00:00 ./myompapp
srun -N 1 -n 1 -t 06:00:00 ./myserialapp```

## Backfilling
* Many schedulers and resource managers use a backfilling algorithm to improve system utilisation and maximise job throughout. 
* When more resource intensive (e.g., multiple node) jobs are running it is possible that gaps ends up in the resource allocation. To fill these gaps a best effort is made for low-resource jobs to slot into these spaces.
* For example, on an 8-core node, an 8 core job is running, a 4 core job is launched, then an 8 core job, then another 4 core job. The two 4 core jobs will run before the second 8 core job.

## Memory Allocation
* By default the scheduler will set memory equal to the total amount on a compute node divided by the number of cores requested. In some cases this might not be enough (e.g., very large dataset that needs to be loaded with low level of parallelisation).
* Additional memory can be allocated with the `--mem=[mem][M|G|T]` directive (entire job) or `--mem-per-cpu=[mem][M|G|T]` (per core). Maximum should be based around total cores -1 (for system processes). The --mem-per-cpu directive is for threads for OpenMP applications and processor ranks for MPI.

## Staging
* Local disk is typically faster than shared disks. If you find that your read-writes are slow and you are making use of a lot of I/O you may need to stage your data.
