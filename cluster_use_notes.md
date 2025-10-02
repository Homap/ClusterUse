Useful notes and tips on general use of cluster use and in particular, Dardel

Computer project: naiss2025-5-325
Storage project: /cfs/klemming/projects/supr/naiss2025-6-220

Useful tips:
In your macOS ~/.zshrc, you can add the following line:
alias dardel='ssh -i ~/.ssh/id-ed25519-pdc homapy@dardel.pdc.kth.se'

In your dardel ~/.bashrc, you can add the following lines:
alias rm='rm -i'
alias mv='mv -i'

In this way, you are always warned before deleting or overwriting another file.
You could also create an alias for your project directories to make you remember the path more easily, for example:
alias cpdna='cd /cfs/klemming/projects/supr/naiss2025-6-220/members/homa/projects/organellar_DNA_volvocine'

I cannot imagine a terminal without colors, you can get that by adding the following lines to your ~/.bash_profile
alias ls="ls --color=auto"
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\[\033[m\]\$ "
. "$HOME/.cargo/env"

You need to refresh ~/.bashrc and ~/.bash_profile after adding these lines by:
source ~/.bashrc 
source ~/.bash_profile

An amazing tool for mounting cluster to the Finder of macOS is MountainDuck. Price 49$ for one user.

About Dardel partitions

On Dardel there are two kinds of partitions in a broader sense - exclusive
(main, long, memory, gpu) and non-exclusive (shared). When you submit a job to
any of the exclusive partitions, you avail the entire node (all the cores and
all the memory as well), and your allocation is charged according to the total
number of cores in a node, irrespective of how many your program makes use of.
On the shared partition, you avail just as many cores as the job requests, and
the memory available to the job is proportional to the number of cores (~1.7
GB/core). However, there are a few things to wary of when requesting
allocations on shared nodes. If you don't specify -N 1, i.e. 1 node, the job
might end up receiving the cores on different nodes and the memory would be
distributed, and yet again the job might run into OOM/out of memory issues,
although you request enough cores/memory. 
Furthermore, the total cores the job avails is determined by the combination of
number of tasks and cpus per task. By default the cpus per task (-c ) is 1. So
typically you avail as many cores as the number of tasks specified. Choosing a
combination of number of tasks (-n) and cpus per task (-c) is dependent on the
program. Also, specifying -n/-c along with salloc or #SBATCH doesn't launch the
program with as many tasks/cores. This only helps with the job allocation. When
you run the program, using srun or otherwise, you need to state how many cores
it needs to make use of.

If you are only looking to get a certain amount of memory, without making use
of any parallelism when running the program, you can add the flag --mem=<XYZ>GB
(along with salloc or #SBATCH) to receive a certain amount of memory, and the
number of cores availed for the job is adjusted automatically (and the
allocation is charged accordingly). If your estimate on the number of cores
(based on ~1.7GB/core) is close to the total number of cores on a node, the job
might begin quicker if it is submitted to the main/exclusive partition, as they
have higher number of nodes. The job begin time is also influenced by your
previous usage in the past 30 days.
