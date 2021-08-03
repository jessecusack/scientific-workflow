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

In my experience you can usually install miniconda in your home directory. This is beneficial because you can then heavily customise and upgrade your environment to suite your needs. Some clusters have anaconda pre-install or make use of jupyter hub, which is great too, but I often find it kind of clunky (but am happy to be shown otherwise)

Installation should follow closely [step 2 of setting up macOS](macOS_setup.md#Step-2---install-conda).

## Starting jupyter lab on a compute node

I will demonstrate how I would start a jupyter lab session on Rutger's HPC, [Amarel](https://oarc.rutgers.edu/resources/amarel/).

First, of course, log in to Amarel (not forgetting to start the university VPN first): 

    ssh amarel
    
For the above to work, I set up an ssh key-pair with amarel and edited my local `~/.ssh/config` file appropriately. 

Within Amarel, start a `tmux` session, which will stay running even if you are disconnected.

    tmux new -s jlab
    
If `tmux` is not available, you might need to load it with `module load tmux`. Next, start an interactive session using the SLURM `srun` command.
    
    srun --partition=main -n 1 --mem=8G --time=08:00:00 --pty bash
    
Above I requested a single computer node with 8 GB of memory for 8 hours. This can obviously be adjusted to suit the need. After a moment we should be assigned a node. We need to know the `hostname` of the node we're on e.g.
    
    echo $(hostname)
    
Copy the result of the above command somewhere, we'll need it later. Next, start up jupyter lab, navigating to a particular project folder if necessary e.g.

    cd ~/some_project
    jupyter lab --no-browser --port=8897 --ip="$(hostname)"
    
After which I hit `ctrl + b` followed immediately by `d` to disconnect from the tmux session.
    
On my personal computer I run:

    ssh -L 8897:[hostname]:8897 -N amarel
    
where `[hostname]` is replaced by the output of `echo $(hostname)` from the amarel session.

Jupyter lab can then be accessed in a browser on my personal computer from `localhost:8897` (pop it straight into the URL bar). I highly recommend making use of jupyter lab's [workspaces](https://jupyterlab.readthedocs.io/en/stable/user/urls.html) feature to avoid a cluttered working environment if you have several projects. 

## Dask

## MITgcm example