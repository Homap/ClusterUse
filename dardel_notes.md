# About Dardel partitions

On Dardel there are two kinds of partitions in a broader sense:
1. exclusive (main, long, memory, gpu)
2. non-exclusive (shared)

When you submit a job to any of the exclusive partitions, you avail the entire node (all the cores and
all the memory as well), and your allocation is charged according to the total
number of cores in a node, irrespective of how many your program makes use of.
On the shared partition, you avail just as many cores as the job requests, and
the memory available to the job is proportional to the number of cores (~1.7
GB/core). 

However, there are a few things to wary of when requesting
allocations on shared nodes. If you don't specify --nodes 1, i.e. 1 node, the job
might end up receiving the cores on different nodes and the memory would be
distributed, and yet again the job might run into OOM/out of memory issues,
although you request enough cores/memory. 

```#SBATCH -p shared
#SBATCH --nodes=1            # keep everything on one node
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=4    # 4 threads for your program
#SBATCH --mem=10G            # request 10 GB memory 
```

Furthermore, the total cores the job avails is determined by the combination of
number of tasks (ntasks) and cpus per task (cpus-per-task). Choosing a
combination of number of tasks (-n) and cpus per task (-c) is dependent on the
program. 

If you are only looking to get a certain amount of memory, without making use
of any parallelism when running the program, you can add the flag --mem=<XYZ>GB
to receive a certain amount of memory, and the
number of cores availed for the job is adjusted automatically (and the
allocation is charged accordingly). If your estimate on the number of cores
(based on ~1.7GB/core) is close to the total number of cores on a node, the job
might begin quicker if it is submitted to the main/exclusive partition, as they
have higher number of nodes. The job begin time is also influenced by your
previous usage in the past 30 days.

## Check on your job usage

You can use 'seff <jobID>' to list a summary of the cpu and memory usage. Note that
you can use these to check the usage of jobs which have completed running
though. 

`seff 12797991`

If you wish to check the usage while the job is still running, you can
open a shell session and ssh to the compute node (after logging in to the login
node), and use top or htop (you need to load the htop module to use this) to
check the usage.

Let's say you've run a job with such settings:

```#SBATCH -N 1
#SBATCH -p shared -n 2 
#SBATCH --mem 10G
```

- Job ID: 12797991

Cluster: dardel <br>
User/Group: homapy/homapy <br>
State: COMPLETED (exit code 0) <br>
Nodes: 1 <br>
Cores per node: 12 <br>
CPU Utilized: 00:00:18 -> This is the actual CPU time consumed by all cores combined.

    It measures how much "work" was done across all your cores. 
    Examples:
    If 1 core worked hard for 18 seconds while the others did nothing → total = 18 sec.
    If 6 cores worked for 3 seconds each → total also = 18 sec.
    If all 12 cores worked fully for 12 seconds → total = 144 sec.
    So in your job, only 18 core-seconds of actual computation happened.

CPU Efficiency: 12.50% of 00:02:24 core-walltime -> core-walltime = wall-clock time × number of cores allocated -> 12 sec × 12 cores = 144 sec = 00:02:24

    CPU Utilized = 18 sec
    Core-walltime = 144 sec
    18 ÷ 144 = 0.125 = 12.5%
    So only 12.5% of the CPU capacity you reserved was actually used.

Job Wall-clock time: 00:00:12 -> This is the real elapsed time between job start and end. <br>
Memory Utilized: 14.43 MB <br>
Memory Efficiency: 0.14% of 10.00 GB (10.00 GB/node)

