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
│   │   script_to_download_matlab_toolboxes.sh
│   │   toolbox_1/
│   │   generic_function.m
```

The structure is just a general skeleton that should be modified as appropriate rather than a strict set of rules. Below I detail to purpose of each directory/file.

### Breakdown of project features

Perhaps the most important element of a project is the README file.

```
project/
│   README.md
```

The `.md` file type indicates that it is a [markdown](https://www.markdownguide.org/getting-started/) document. The document should contain a description of the project and instructions on how to get it up and running.

The conda package manager for python can use what is known as an environment file to install packages.

```
│   environment.yml
```

The `.yml` ending indicates that it is a [YAML](https://yaml.org/) file. Essentially, it is a list of all the python packages (including version numbers) that are needed to run the code within the project. 

The R language [also has an environment specification](https://rstudio.github.io/renv/articles/renv.html) (although I've personally never used it). 

## Initialising the project as a git repository

Assuming [github was set up](github_setup.md) already,

    git init
    gh create repo
    
which will prompt you for some information. 