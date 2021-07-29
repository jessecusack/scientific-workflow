# High performance computing clusters

All HPC clusters work slightly differently, but some common elements are:

* Use linux operating system
* Usually access via a command line interface through ssh (although some GUI's do exist)
* Separation of CPU/GPU resources (login/compute/visualise)
* You have to schedule jobs (e.g. via slurm or PBS)
* Separation of storage (home/work/scratch)

## Working efficiently

### Storage

Stuff in the home directory is usually backed up, so put code or analysis (git repositories) here and then create symbolic links to large datasets or model output which will likely be stored in a workspace or on scratch. 

### ssh keys

Make logging into the HPC as easy as possible with ssh keys. Each cluster usually has its own set of instructions for this and it is worth following them closely. If there are no specific instructions, [these general ones usually work](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-2).

## Setting up conda

In my experiance you can usually install miniconda in your home directory. This is beneficial because you can then heavily customise and upgrade your environment to suite your needs. Some clusters have anaconda pre-install or make use of jupyter hub, which is great too, but I often find it kind of clunky (but am happy to be shown otherwise)

Installation should follow closely [step 2 of setting up macOS](macOS_setup.md#Step-2---install-conda).

## Starting jupyter lab on a compute node

Log in to the HPC

    tmux new -s jlab
    <START INTERACTIVE JOB>
    echo $(hostname)
    
Hit `ctrl + b, d` to detach from tmux. 

## Dask

## MITgcm example