# Starting a scientific project

To me, a scientific project is a collection of data, code, and documentation that are all connected by a relatively narrow line of scientific investigation (or set of questions) in a well defined folder structure. Sometimes a project is very clearly defined, with well understood sources of data and logical flow of analysis to answer specific questions. Other times it can be a fuzzy blob of stuff, where data comes from many different (perhaps confused) sources and analysis draws on many different methods and tools in a confusing maze of pipelines to investigate several complex/interelated questions. Sometimes I find projects start as the former and end up as the latter, in which case they may need to be split up. The goal when starting a scientific project, is to create a framework that avoids fuzzy blobs and that can also be easily reused or reproduced by someone else, and ESPECIALLY YOU IN THE FUTURE!

To achieve a reproducible scientific project I make use of:
* *git* and *github.com* for version control and backup
* *shell scripts* that grab or link external data automatically
* *environment specifications* for python, R and a separate matlab toolbox directory

## Examples

To be continued... 

## Directory structure

I find myself using some variation if this directory structure often (eventually I will create a [cookiecutter](https://cookiecutter.readthedocs.io) template)

```
project/
│   README.md
│   environment.yml
│   renv.lock
│   .gitignore
│
└───data/
│   │   script_to_download_or_link_external_data.sh
│   │   script_to_convert_external_data.py
│   │
│   └───external/
│   │   │   datafile.nc
│   │   │   datafile.bin
│   │   │   datafile.mat
│   │   │   ...
│   │
│   └───internal/
│       │   datafile.nc
│       │   ...
│   
│───analysis/
│   │   python_notebook.ipynb
│   │   python_notebook.py
│   │   matlab_code.m
│   │   R_script.R
│
└───figures/
│   │   figure_1.png
│   │   figure_2.pdf
│
└───matlab_toolboxes/
│   │   .gitignore
│   │   script_to_download_matlab_toolboxes.sh
│   │   toolbox_1/
│   │   generic_function.m
```

The structure is just a general skeleton that should be modified as appropriate rather than a strict set of rules. Below I detail to purpose of each directory and file.

### Breakdown of a project

Perhaps the most important element of a project is the README file.

```
project/
│   README.md
```

The `.md` file type indicates that it is a [markdown](https://www.markdownguide.org/getting-started/) document. The document should contain a description of the project and instructions on how to get it up and running. These should be detailed and concise enough that someone competent with the tools would not have trouble, but not so detailed that it becomes a tome. 

The conda package manager for python can use what is known as an environment file to install packages.

```
│   environment.yml
```

The `.yml` ending indicates that it is a [YAML](https://yaml.org/) file. Essentially, it is a list of all the python packages (including version numbers) that are needed to run the code within the project. 

The R language [also has an environment specification](https://rstudio.github.io/renv/articles/renv.html) (although I've personally never used it). Apparently it uses `renv.lock` files.

Stuff that you do not want to be included in version control goes in the `.gitignore` file (datasets are a great example explained below).

```
│   .gitignore
```

`.gitignore` is simply a list of all the files and folders to be ignored. It understands wildcard completion e.g. `fig_*.jpg` and you can have more than one in different places. In general you should ignore things that can be downloaded from elsewhere or are generated from within the project. You should also ignore file types that do not version control well (such as jpeg). See the [example](.gitignore.example).

Datasets should NOT BE INCLUDED IN VERSION CONTROL. Version control is designed for code or text and other non-binary data. Instead, data should be archived somewhere safe and accessible (cloud storage/remote server/RAID). When working with data, we pull in only what we need using scripts (which ARE version controlled) or by following a set of instructions in the README. Sometimes the dataset might be so large you don't want to copy it, so instead, create a symbolic link. 

```
└───data/
│   │   script_to_download_or_link_external_data.sh
│   │   script_to_convert_external_data.py
│   │
│   └───external/
│   │   │   datafile.nc
│   │   │   datafile.bin
│   │   │   datafile.mat
│   │   │   ...
│   │
│   └───internal/
│       │   datafile.nc
│       │   ...
```

Here I make a separation between `external` data, which is created outside of this projects code, and `internal` data, which is created by code contained within the project (perhaps by `project/analysis/some_script.py`).

All the real science happens in the analysis folder, which contains scripts and notebooks that analyse data.

```
│───analysis/
│   │   python_notebook.ipynb
│   │   python_notebook.py
│   │   matlab_code.m
│   │   R_script.R
```

Figures are dumped into their own directory, which is not version controlled because they can always be recreated. 

```
└───figures/
│   │   figure_1.png
│   │   figure_2.pdf
```

Finally 3rd party matlab toolboxes are a bit like datasets. They should be pulled in using scripts or a set of instructions. General 3rd party toolboxes like `m_map` don't need to be version controlled because they exist elsewhere. However, your own functions might need to be included in version control. It might also be useful to create a `.gitignore` here to distinguish between files that are in version control and that are not. 

```
└───matlab_toolboxes/
│   │   .gitignore
│   │   script_to_download_matlab_toolboxes.sh
│   │   toolbox_1/
│   │   generic_function.m
```

Any matlab analysis should make use of the function `addpath` to include the toolboxes from within the project. A great way to make sure you've included all the bits of matlab required (and are not using some random code elsewhere on your computer) is to revert your path to the default and then re-run your analysis. If it throws errors, you missed something. If it runs, then great, you have everything!

## Initialising the project as a git repository

Assuming [github was set up](github_setup.md) already,

    git init
    gh create repo
    
will set up git and put your project on github.

## Managing everything with jupyter lab