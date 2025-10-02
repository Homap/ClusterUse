export CONDA_ENVS_PATH=/proj/snic2022-23-81/algae_conda
export CONDA_PKGS_DIRS=/proj/snic2022-23-81/algae_conda/algae_phylo_pkgs
conda create --name algae_phylo_env python=3.10.5


Something useful if you're running anything on Dardel that produces lots of cache files while running, for example, snakemake which redirects the cache files while running under .cache in your home directory. The problem is that the jobs fail because Dardel is strict on the number of files allowed in home directory, so you get the error "Disk quota exceeded" pretty fast. You can redirect the cache files to the scratch directory like this:

export XDG_CACHE_HOME=/cfs/klemming/scratch/h/username/.cache