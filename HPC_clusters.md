# High performance computing clusters

All HPC clusters work slightly differently, but some common elements are:

* Use a linux type operating system
* Usually access via a command line interface through ssh (although some GUI's do exist)
* Separation of CPU/GPU resources (login/compute/visualise)
* You have to schedule jobs (e.g. via slurm or PBS)
* Clear separation of storage space (home/work/scratch)

## HPC etiquette and efficiency

### Nodes

HPC clusters usually have a login node and various types of compute nodes. The login node, as the name suggests, is the place you (and everyone else) log on to (usually via ssh). Do not run computationally intensive tasks, or anything that requires significant memory, on the login nodes. Instead, request time on dedicated compute nodes using job scheduling software (e.g. [SLURM](https://slurm.schedmd.com/documentation.html)). If you're just testing things out and don't know how much time or resources you might need, you can start an interactive job for this purpose. 

On some HPC clusters, the compute nodes do not have internet access, so interacting with github and downloading files has to be done via the login nodes. This is usually ok because these tasks are not typically resource intensive. 

### Storage

Data in your home directory is usually backed up, so put code, analysis (including git repositories) and important data here. Unfortunately, your home directory may be space limited and unable to accommodate large datasets. It is usually necessary to create symbolic links from your home directory to large datasets, which will likely reside in a workspace (might be backed up) or on scratch (not backed up). Ideally, everything needed to reproduce your work should be backed up and everything not backed up should be easily reproducible. 

### ssh keys

Logging into the HPC is easiest with ssh keys. Each cluster usually has its own set of instructions for setting up ssh key access and it is worth following them closely. If there are no specific instructions, [these general ones tend to work](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-2).

## Setting up conda and jupyter lab

In my experience you can usually install miniconda in your home directory (so long at least 10 GB of storage). This is beneficial because you can then heavily customise and upgrade your environment to suit your needs. Some clusters have anaconda/miniconda pre-installed, in which case you can use that too. Others might encourage you to make use of [jupyter hub](https://jupyter.org/hub), which is great for groups with pre-planned workflows, but less great for those who might want to play around new tools and packages.

### Installing Miniconda in your home directory

Log into the HPC cluster. To download Miniconda, run:

```bash
cd ~
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

To install Miniconda, run:

```bash
bash Miniconda3-latest-Linux-x86_64.sh
```

And follow the prompts to install it in your home directory (this usually just means following the default options).

As part of the installation, you may be asked for permission to modify your terminal configuration (the `.bashrc` file that lives in your home directory). This is recommended, so that every time you log into the HPC, you will have access to the `conda` commands automatically.

Upon successful installation, I would recommend setting `conda-forge` as the default channel package searches. I find that it has a more complete list of packages than the standard Anaconda channel. To do so, run the following in the terminal:

```bash
conda config --add channels conda-forge
conda config --set channel_priority strict
```

### Install jupyter lab

From the login terminal of the HPC run:

```bash
conda activate base
conda install -c conda-forge jupyterlab
```

Follow any prompts. 

It is good practise to set a server password. You'll then use it to access the remote session. It can be relatively simple because most of your security comes from the use of SSH. 

```bash
jupyter server password
```

Don't forget to store it somewhere! (like a password manager)

## Using jupyter lab remotely

I will demonstrate how I would start a jupyter lab session on Rutger's HPC, [Amarel](https://oarc.rutgers.edu/resources/amarel/).

### The easy way with `jupyter-forward`

The package [jupyter forward](https://github.com/NCAR/jupyter-forward) makes starting jupyter lab on a remote machine an easy 1-command process. For this to work, you need to have 1) installed Miniconda on the HPC 2) installed jupyter lab on the HPC and 3) installed `jupyter-forward` on your own computer. 

1. Follow the instructions above to install Miniconda on the HPC. 

2. Follow the instructions above to install jupyter lab and set a password.

3. From a terminal on your own computer run:

```bash
conda activate base
conda install -c conda-forge jupyter-forward
```

To use `jupyter-forward` to launch a remote jupyter lab session, run a command like this on your own computer (replacing username appropriately):

```bash
jupyter-forward --port=8898 --conda-env=base --launch-command="srun --partition=main --mem=8000 --time=4:00:00" [username]@amarel.rutgers.edu
```

You may be prompted for your Amarel password (which ideally would not happen with ssh keys, but it might be a small bug with `jupyter-forward`)

The command does the following behind the scenes:
* log into the HPC
* start a SLURM job using the command you specify
* launch jupyter lab as part of the job
* forward the jupyter lab port to your local computer

Eventually, a browswer tab on your computer will open with your remote jupyter lab session (URL: `localhost:8898`). You may be prompted for your jupyter server password. All the above can be done manually too, as explained below. 

### The manual way

First, of course, log in to Amarel (not forgetting to start the university VPN first): 

    ssh amarel
    
For the above to work, I set up an ssh key-pair with amarel and edited my local `~/.ssh/config` file appropriately. 

Within Amarel, start a `tmux` session, which will stay running even if you are disconnected.

    tmux new -s jlab
    
If `tmux` is not available, you might need to load it with `module load tmux` or use `screen` instead. Next, start an interactive session using the SLURM `srun` command.
    
    srun --partition=main -n 1 --mem=8G --time=08:00:00 --pty bash
    
Above I requested a single compute node with 8 GB of memory for 8 hours. This can obviously be adjusted to suit the need. After a moment we should be assigned a node. We need to know the `hostname` of the node we're on e.g.
    
    echo $(hostname)
    
Copy the result of the above command somewhere, we'll need it later. Next, start up jupyter lab, navigating to a particular project folder if necessary e.g.

    cd ~/some_project
    jupyter lab --no-browser --port=8897 --ip="$(hostname)"
    
After which I hit `ctrl + b` followed immediately by `d` to disconnect from the tmux session.
    
On my personal computer I run:

    ssh -L 8897:[hostname]:8897 -N amarel
    
where `[hostname]` is replaced by the output of `echo $(hostname)` from the amarel session. The command establishes a connection between port 8897 on the Amarel compute node to the same port on your computer (they don't have to be the same, but it is easier to remember). 

Jupyter lab can then be accessed in a browser on you personal computer from `localhost:8897` (pop it straight into the URL bar). I highly recommend making use of jupyter lab's [workspaces](https://jupyterlab.readthedocs.io/en/stable/user/urls.html) feature to avoid a cluttered working environment if you have several projects. 

## Dask

## MITgcm example
