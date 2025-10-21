# Tips for transferring large folders across directories or between Dardel and your local computer

**Transfer large files using Dardel transfer node:** To avoid causing problems for other users we should be using the transfer node on Dardel whenever we transfer large folders (e.g. creating back-ups) from or to Dardel. To log into the Dardel transfer node use (change the 'user' to your username): 

`ssh user@dardel-ftn01.pdc.kth.se`

When transferring files this way, the file transfer does not need to be submitted as a job. If you transfer files on one of the 'normal' nodes, then this should be submitted as a job to slurm.

## rsync 
rsync is a good option for transferring files and folders both between your local computer and Dardel as well as around Dardel. For transfering a small number of files it may be ok to do this on the login node but if you are transferring many files, then this should be submitted as a job using SBATCH (or on transfer node if you are transfering very large files from your computer to Dardel - see above). 

When transsferring files from elsewhere to a project directory we are equested not use the `-a` flag (https://support.pdc.kth.se/doc/data_management/file_transfer/). The `-a` flag preserves file ownership and permissions, which results in the storage space used by those files counting against your personal quota, not the project's. Instead use `rsync -r`. Note that you then need to maually set any other flags that are warpped up in ´-a´ yourself. The ´-a´flag includes:
- r → recursive (copy directories and their contents)
- l → copy symlinks as symlinks
- p → preserve permissions
- t → preserve modification times
- g → preserve group
- o → preserve owner (requires root)
- D → preserve device files and special files

A possible option for using rsync to transfer files on Dardel avoiding the `-a` flag and issues with file permissions / allocation could be:

`rsync -rlt`

#### Hard links
When transferring folders that contain many hard links (such can be created when running e.g. snakemake, nextflow or conda) the default behaviour of rsync is to not not preserve the hard links but rather to create duplicate files. This can be problematic as it can increase the copied folder size and disrupt the integrity of the file paths data integrity, versioning, or incremental backups. However, preserving hard links is more resource intensive as rsync then has to track all the connections so more I/O operations. So it's a judgment call. The flag to tell rsync to preserve hard links is `-H`. Note that hard links will not work across different file systems (e.g. MacOS & linux). But may still be best to preserve them to save space.  

#### Sparse files
Sparse files are often created many bioinformatic tools as well as workflow managers like Nextflow. By default rsync does not preserve sparse files but instead fills the spaces in sparse files with zeros. This can inflate the folder size in the new destination. So it's good practice to get rsync to preserve sparse files using `-S`. Note: you can check for sparse files bu testing whether the disk usage (`du -sh`) and apparent size (`du -sh --apparent-size`) of your folders are different (in the case of folders with sparse files the disk usage will be smaller than the apparent size.  

## suggested rsync code to preserve hard links and sparse files
`rsync -rhvtlPHS path/to/source path/to/destination`

**-r** =   recursive: descend into directories

**-h** = --human-readable
Make numbers in the output human-friendly (e.g., 1.2G instead of 1288490188). It affects display only, not behaviour.

**-v**   verbose: show what rsync is doing

**-t**   times: preserve modification times

**-l**   links: copy symlinks as symlinks (don’t dereference)

**-P**
Shorthand for two options:
--partial keep partially transferred files (so a later retry can resume instead of restarting)
--progress show per-file progress during the transfer

**-H** = --hard-links
Preserve hard links: files that are hard-linked together on the source will remain hard-linked on the destination.
Note: This requires rsync to track all files to identify link groups; it can be memory/CPU intensive on very large trees.

**-S** = --sparse
Handle sparse files efficiently: sequences of zero bytes are written as holes on the destination instead of real zeros, saving disk space. Note: It may add some CPU overhead; it only helps when files actually contain long zero ranges.




