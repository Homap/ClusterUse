# Tips for transferring large folders across directories or between Dardel and your local computer

**Transfer large files (>10 GB) using Dardel transfer node:** To avoid causing problems for other users we should be using the transfer node on Dardel whenever we transfer large folders (e.g. creating back-ups) from or to Dardel. To log into the Dardel transfer node use (change the 'user' to your username): 

`ssh user@dardel-ftn01.pdc.kth.se`

## rsync 
rsync is a good option for transferring files and folders both between your local computer and Dardel as well as around Dardel. For transfering a small number of files it may be ok to do this on the login node but if you are transferring many files, then this should be submitted as a job using SBATCH. 

When transsferring files from elsewhere to a project directory we are equested not use the `-a` flag. The `-a` flag preserves file ownership and permissions, which results in the storage space used by those files counting against your personal quota, not the project's. 

#### Hard links
When transferring folders that contain many hard links (such can be created when running e.g. snakemake, nextflow or conda) the default behaviour of rsync is to not not preserve the hard links but rather to create duplicate files. This can be problematic as it can increase the copied folder size and disrupt the integrity of the file paths data integrity, versioning, or incremental backups. However, preserving hard links is more resource intensive as rsync then has to track all the connections so more I/O operations. So it's a judgment call. The flag to tell rsync to preserve hard links is `-H`. 

#### Sparse files
Sparse files are often created by e.g. Nextflow and snakemake. By default rsync does not preserve sparse files but instead fills the spaces in sparse files with zeros. This can inflate the folder size in the new destination. So it's good practice to get rsync to preserve sparse files using `-S`. Note: you can check for sparse files bu testing whether the disk usage (`du -sh`) and apparent size (`du -sh --apparent-size`) of your folders are different (in the case of folders with sparse files the disk usage will be smaller than the apparent size.  

## suggested rsync code to preserve hard links and sparse files
`rsync -hPHS path/to/source path/to/destination`

**-h** = --human-readable
Make numbers in the output human-friendly (e.g., 1.2G instead of 1288490188). It affects display only, not behaviour.

**-P**
Shorthand for two options:
--partial keep partially transferred files (so a later retry can resume instead of restarting)
--progress show per-file progress during the transfer

**-H** = --hard-links
Preserve hard links: files that are hard-linked together on the source will remain hard-linked on the destination.
Note: This requires rsync to track all files to identify link groups; it can be memory/CPU intensive on very large trees.

**-S** = --sparse
Handle sparse files efficiently: sequences of zero bytes are written as holes on the destination instead of real zeros, saving disk space. Note: It may add some CPU overhead; it only helps when files actually contain long zero ranges.

### Zipping files for transfer
It is a good idea to zip large folders before tranfer. This saves time and compute resources. It also gets around the issue of preserving hardlinks and sparse files as everything will be transferred 'as is' within the zipped filder. To zip a folder before transfer:
`...`

To transfer a zipped folder using rsync:
`rsync .....`



