# Setting up macOS

How would we go about setting up a brand new apple computer for scientific work? Below I outline how I would proceed with this task, but it will vary significantly from person to person (particularly in what is considered "essential" software).

### Contents

1) [Install the essentials](#Step-1---install-the-essentials)
1) [Install conda](#Step-2---install-conda)
1) [Install matlab](#Step-3---install-matlab)
1) [Upgrade iterm2](#Step-4---upgrade-iterm2)

## Step 1 - install the essentials

The following assumes we made it through the most basic setup like starting the wifi, logging in with an apple ID, and turning Siri off. 

First, open a terminal and install the package manager [homebrew](https://brew.sh/).

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    
At this point we probably need to __install Xcode from the app store__ and then run,

    xcode-select --install
    
in the terminal. (I think brew needs one or perhaps both of these steps to be completed before it will install some software...)

Now use brew to install all the software we might need, in no particular order:

    brew install git gh gcc netcdf ncview wget ffmpeg rclone
    
I might have forgotten some useful stuff. Note that I don't install python this way, instead I do that with miniconda in the next step. 
    
Some software comes in the form of casks and can be install as such: (I'm not sure it is necessary to include the `--cask` option since brew is smart enough to figure out if something is a cask or a formulae)

    brew install --cask atom adobe-acrobat-reader appcleaner caffeine firefox vlc julia mactex iterm2 dropbox gimp google-chrome google-drive r rstudio slack spotify inkscape zoom texmaker mendeley bitwarden box-drive
    
All of the above software is entirely optional. Of course, it will probably take a long time to install and use up significant bandwidth.

A brief explanation of some of the software:

* [`atom`](https://atom.io/) - cool GUI text/code editor
* `appcleaner` - very useful tool for uninstalling macOS applications
* `mactex` - TeX/LaTeX installation
* `r` - the R programming language
* `caffeine` - a handy command line utility that stops your operating system from sleeping for a set amount of time
* `bitwarden` - password manager

### Step 1.1 - configure git

The most basic git configuration is to set a name and email.

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com

## Step 2 - install conda

I like to install miniconda rather than the full anaconda distribution. The following should work:

    mkdir -p ~/miniconda3
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh -O ~/miniconda3/miniconda.sh
    bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
    rm -rf ~/miniconda3/miniconda.sh
    miniconda3/bin/conda init bash
    miniconda3/bin/conda init zsh
    
[Conda-forge](https://conda-forge.org/) tends to contain a more complete list of packages than the default channel, so I modify the conda default:

    conda config --add channels conda-forge
    conda config --set channel_priority strict
    
### Step 2.1 - install the base environment

The default conda environment (which is called `base`) doesn't contain any packages. For convenience I like to install code formatters, jupyter lab plus some extensions here. You could also create a new environment (perhaps called `jlab`) to do this, leaving `base` completely clean, but then you'll have to activate `jlab` every time you want to start jupyter. Ultimately, there is no right or wrong way to proceed. 

    conda activate base
    conda install jupyterlab black isort jupytext jupyterlab-system-monitor jupyterlab-spellchecker
    jupyter labextension install @jupyterlab/toc
    
The last line installs the [table of contents](https://github.com/jupyterlab/jupyterlab-toc) lab extension which makes it _a lot_ easier to work with big notebooks.

For reference we can create a new named environment like so:

    conda create -n <NAME> python=3.* <MORE PACKAGES>
    
replacing bracketed variables as appropriate. Delete an environment like this:

    conda remove --name <NAME> --all
    
[More options detailed here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).
    
## Step 3 - install matlab

Usually this involves logging into [mathworks.com](https://www.mathworks.com/), downloading the latest version, and activating. 

## Step 4 - upgrade iterm2

I like to use `iterm2` as my terminal application because it is both straightforward and extensively modifiable. Perhaps my favourite modification is to set `ctrl + ~` to activate a terminal that drops down from the top of the screen. [Instructions here](https://blog.mestwin.net/drop-down-terminal-in-macos-with-iterm2/).

The default shell in macOS is now zsh. [Oh my zsh](https://github.com/ohmyzsh/ohmyzsh/) makes zsh look great and provides a bunch of useful auto-complete options, especially when working with git. 

    sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    
If you use bash rather than zsh, there is also [oh my bash](https://github.com/ohmybash/oh-my-bash):

    bash -c "$(wget https://raw.githubusercontent.com/ohmybash/oh-my-bash/master/tools/install.sh -O -)"
    