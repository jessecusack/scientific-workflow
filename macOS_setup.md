# Setting up macOS

How would we go about setting up a brand new apple computer for scientific work? Below I outline how I would proceed with this task, but it will vary significantly from person to person (particularly in what is considered "essential" software).

### Contents

1) [Install essential software](#Step-1---install-essentials-software)
1) [Install conda](#Step-2---install-conda)
1) [Install matlab](#Step-3---install-matlab)
1) [Upgrade iterm2](#Step-4---customize-iterm2)

## Step 1 - install essential software

The following assumes we made it through the most basic setup like starting the wifi, logging in with an apple ID, and turning Siri off. 

First, open a terminal and install the package manager [homebrew](https://brew.sh/).

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

Homebrew can be used to install the majority of the software that is needed for science. You _could_ install all the software mentioned below manually, but it would be incredibly tedious and error prone. 
Occasionally, software on homebrew needs Apples developer toolkit to work (Xcode), so next we [__install Xcode from the app store__](https://apps.apple.com/us/app/xcode/id497799835?mt=12) and run,

    xcode-select --install
    
in the terminal. 

Now use brew to install all the command line tools we might need, in no particular order:

    brew install git gh gcc netcdf ncview wget ffmpeg rclone rsync tree java neovim

An explanation of some of these tools:

* `git` - a version control system
* `gh` - a command line interface to GitHub.com
* `gcc` - the GNU compiler collection
* `ncview` - a super simple netCDF file viewer
* `wget` - a utility for downloading things from the internet
* `ffmpeg` - an audio and video converter
* `rclone` - a tool for connecting to cloud storage like Dropbox and Google Drive
* `neovim` - a modern version of the classic terminal text editor [`vim`](https://www.vim.org/)

Some of these might already be on your system, but often they are out of date versions. Note that I don't install python this way, instead I do that with miniconda in the next step. 
    
Homebrew can be used to install almost any software, including graphical user interface (GUI) software like the browser firefox, as well as other packages. To do so, we need to use the `--cask` option.

    brew install --cask visual-studio-code adobe-acrobat-reader appcleaner caffeine firefox vlc julia mactex iterm2 dropbox gimp google-drive r rstudio inkscape zoom texmaker zotero bitwarden box-drive calibre djview
    
All of the above software is entirely optional. Of course, it will probably take a long time to install and use up significant bandwidth.

An explanation of some of the software:

* `visual-studio-code` - a good code editor
* `appcleaner` - handy tool for uninstalling macOS applications
* `mactex` - TeX/LaTeX installation, which needs to be coupled with a tex editor like `texmaker`
* `r` - the R programming language
* `caffeine` - a handy command line utility that stops your operating system from sleeping for a set amount of time
* `bitwarden` - password manager
* `iterm2` - terminal
* `gimp` - image editor
* `inkscape` - vector graphics editor
* `zotero` - my current favourite reference manager (which needs to be coupled with the [better bibtex extension](https://retorque.re/zotero-better-bibtex/))
* `calibre` - an ebook manager
* `djview` - for opening djv files, which are similar to pdf's 

### Step 1.1 - energize your command line with "oh my zsh"

This amusingly named configuration of your shell adds very useful features like auto-completion. Find out more at https://ohmyz.sh/. Install using:

    sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    
If you use bash rather than zsh, there is also [oh my bash](https://github.com/ohmybash/oh-my-bash):

### Step 1.2 - configure git

The most basic git configuration is to set a name and email.

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com

## Step 2 - install conda

I like to install miniconda rather than the full anaconda distribution. The following lines should automate the process:

    mkdir -p ~/miniconda3
    # Note the link below is for the new M1 processor Macs, for intel use https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-arm64.sh -O ~/miniconda3/miniconda.sh
    bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
    rm -rf ~/miniconda3/miniconda.sh
    ~/miniconda3/bin/conda init zsh
    
[Conda-forge](https://conda-forge.org/) often contains a more complete list of useful scientific packages than the default channel, so I modify the conda default:

    conda config --add channels conda-forge
    conda config --set channel_priority strict
    
### Step 2.1 - fill your base environment with packages

When using miniconda, the default conda environment (which is called `base`) doesn't contain any packages. For convenience I like to install code style formatters and [jupyter lab](https://jupyter.org/) with some extensions into `base`. You could also create a new environment (perhaps called `jlab`) to do this, leaving `base` completely clean, but then you'll have to activate `jlab` every time you want to start jupyter. Ultimately, there is no right or wrong way to proceed. I do:

    conda activate base
    conda install jupyterlab black isort cookiecutter jupyter-forward jupytext jupyterlab-system-monitor jupyterlab-spellchecker jupyterlab-git

For reference we can create a new named environment like so:

    conda create -n [name] python=3.* [more packages]
    
replacing bracketed variables as appropriate. Delete an environment like this:

    conda remove --name [name] --all
    
[More options detailed here](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

At this point I might add an alias to `~/.zshrc` for convenience, e.g.

    alias jlab="jupyter lab"
    
### Step 2.2 - create global jupytext config

To set global jupytext configuration that will ignore jupytext version number metadata, create the following file:

```bash
mkdir -p ~/.config/jupytext
echo "notebook_metadata_filter: -jupytext.text_representation.jupytext_version" > ~/.config/jupytext/jupytext.yml
```

### Step 2.3 - install Julia and R kernels

If you installed Julia or R via `brew install --cask` above, then you need to install the jupyter kernel for these languages.

To install the R kernel, first open a terminal and run `R` to start the R console. Then run:

```R
install.packages('IRkernel') 
IRkernel::installspec()
```

To install the Julia kernel, first open a terminal and run `julia` to start the julia console. Then hit `]` to enter the REPL and follow up with:

```julia
add IJulia
```

That _should_ be all you need to do.
    
## Step 3 - install matlab

Usually this involves logging into [mathworks.com](https://www.mathworks.com/), downloading the latest version, and activating.

Add an alias to your `~/.zshrc` file so that you can start up matlab from the terminal without the desktop environment. I might add,

```
alias mlab="matlab -nosplash -nodesktop"
```

## Step 4 - customize iTerm2

I like to use [iTerm2](https://iterm2.com/) as my terminal application because it is both straightforward and extensively modifiable. Perhaps my favourite modification is to set `ctrl + ~` to activate a terminal that drops down from the top of the screen. [Instructions here](https://blog.mestwin.net/drop-down-terminal-in-macos-with-iterm2/).

### Step 4.1 - profiles and settings across computers
In iTerm2, you can create unique profiles for each of your working _spaces_ (e.g., local computer, server, vim). For each of these profiles, you can customize colors, specify shortcut keys to activate them, as well as starting commands (e.g., `ssh [server]`). This makes it far easier to know where you are.

`iTerm2 > Preferences > Profiles`

If you use multiple computers (e.g., laptop and workstation), you can sync your iTerm2 setting (including profiles) by saving them in a Dropbox folder.

On your main computer, go to `iTerm2 > Preferences > General > Preferences` and click `Load preferences from a custom folder or URL`. Select a Dropbox folder and save your local settings. Repeat the process on a secondary computer, but this time, do not save your local settings. After you restart iTerm2, the settings from your main computer should be available on your secondary computer.
