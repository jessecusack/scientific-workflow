# Starting a scientific project

To me, a scientific project is a collection of data, code, and documentation that are all connected by a relatively narrow line of scientific investigation (or set of questions) in a well defined folder structure. Sometimes a project is very clearly defined, with well understood sources of data and logical flow of analysis to answer specific questions. Other times it can be a fuzzy blob of stuff, where data comes from many different (perhaps confused) sources and analysis draws on many different methods and tools in a confusing maze of pipelines to investigate several complex/interrelated questions. Sometimes I find projects start as the former and end up as the latter, in which case they may need to be split up. The goal when starting a scientific project, is to create a framework that avoids fuzzy blobs and that can also be easily reused or repeated by someone else, PARTICULARLY YOU IN THE FUTURE!

To achieve a repeatable scientific project I make use of:
* *git* and *github.com* for version control and backup.
* *scripts* or a set of *instructions* to download and link external data so everybody knows where it comes from.
* *environment specifications* for python, R or Julia and a separate matlab toolbox directory so that everybody knows exactly which packages are required to run the project

## Directory structure

I find myself using some variation if this directory structure often. So much so, that I made it into a [cookiecutter](https://cookiecutter.readthedocs.io) template! You can [find the template here](https://github.com/jessecusack/cookiecutter-research-project).

```
project/
│   README.md
│   .gitignore
│   environment.yml
│   renv.lock
│   Project.toml
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
└───analysis/
│   │   python_notebook.ipynb
│   │   python_notebook.py
│   │   matlab_code.m
│   │   R_script.R
│   │   julia_notebook.ipynb
│   │   julia_notebook.jl
│   │   ...
│
└───figures/
│   │   figure_1.png
│   │   figure_2.pdf
│   │   ...
│
└───matlab_toolboxes/
│   │   .gitignore
│   │   script_to_download_matlab_toolboxes.sh
│   │   toolbox_1/
│   │   generic_function.m
│   │   ...
```

The structure is just a general skeleton that should be modified as appropriate rather than a strict set of rules. Below I detail the purpose of each directory and file.

### Breakdown of a project

#### Instructions, environments and ignore

Perhaps the most important element of a project is the README file.

```
project/
│   README.md
```

The `.md` file type indicates that it is a [markdown](https://www.markdownguide.org/getting-started/) document. The document should contain a description of the project and instructions on how to get it up and running. These should be detailed enough that someone competent with the tools would not have any trouble. 

Anything that you do not want to be included in version control goes in the `.gitignore` file (datasets are a great example explained below).

```
│   .gitignore
```

`.gitignore` is simply a list of all the files and folders to be ignored. It understands wildcard completion e.g. `fig_*.jpg` and you can have more than one in different places. In general you should ignore things that can be downloaded from elsewhere or are generated from within the project. You should also ignore file types that do not version control well (such as jpeg). See the [example](.gitignore.example).

The conda package manager for python can use what is known as an environment file to install packages.

```
│   environment.yml
```

The `.yml` ending indicates that it is a [YAML](https://yaml.org/) file. It contains a list of all the python packages (including version numbers, if needed) that are required to run the code within the project. See [this example](environment.example.yml) of what such a file looks like. 

The R language [also has an environment specification](https://rstudio.github.io/renv/articles/renv.html) (although I've personally never used it). Apparently it uses `renv.lock` files.

[In Julia](https://pkgdocs.julialang.org/v1.6/environments/), package specification is done in the `Project.toml` file.

#### Data

Datasets should _not be included in version control_. Version control is designed for code or text and other non-binary data. Instead, data should be archived somewhere safe and accessible (cloud storage/remote server/RAID). When working with data, we pull in only what we need using scripts (which _are_ version controlled) or by following a set of instructions in the README. Sometimes the dataset might be so large you don't want to copy it. Depending on where it is stored, it might be better to create a symbolic link. [This page](get_snippets.md) details a variety of different methods that can be used to copy and link data from diverse sources. 

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

I have made a separation between `external` data, which is created outside of this projects code, and `internal` data, which is created by code contained within the project (perhaps by `project/analysis/some_script.py`).

#### Analysis

All the real science happens in the analysis folder, which contains scripts and notebooks. Try to avoid version control of notebooks, which can result in messy commits. Instead, version a synchronised `.py` file created using jupytext ([more below](#Version-control-and-jupyter-notebooks)).

```
└───analysis/
│   │   python_notebook.ipynb
│   │   python_notebook.py
│   │   matlab_code.m
│   │   R_script.R
│   │   julia_notebook.ipynb
│   │   julia_notebook.jl
│   │   ...
```

#### Figures

Figures are dumped into their own directory, which is not version controlled because they can always be recreated. 

```
└───figures/
│   │   figure_1.png
│   │   figure_2.pdf
│   │   ...
```

#### Matlab toolboxes

Finally 3rd party matlab toolboxes are treated like datasets. They should be pulled in using scripts or a set of instructions. General 3rd party toolboxes like `m_map` don't need to be version controlled because they are maintained and stored elsewhere. However, your own functions might need to be included in version control. It might also be useful to create a `.gitignore` here to distinguish between files that are in version control and that are not. 

```
└───matlab_toolboxes/
│   │   .gitignore
│   │   script_to_download_matlab_toolboxes.sh
│   │   toolbox_1/
│   │   generic_function.m
│   │   ...
```

Any matlab analysis should make copious use of the functions `addpath` and `genpath` to include the toolboxes from within the project. A great way to make sure you've included all the right toolboxes (and not some random code elsewhere on your computer) is to revert your path to the default and then re-run your analysis. If it throws errors, you missed something. If it runs, then great, you have everything!

## Initialising the project as a git repository

Assuming [github was set up](github_setup.md) already,

    git init
    gh repo create
    
will set up git and create a repository on github. When you've created a few files worth saving, do an 'add, commit, push' operation like this (don't forget the `.gitignore` file!):

    git add *
    git commit -m 'initial commit'
    git push origin main
    
Now your changes are safely backed up on github!

## Managing everything with jupyter lab

Unfinished.

## Version control and jupyter notebooks

To put it bluntly, Jupyter Notebooks suck with version control, even though they're text files in a human readable data format (JSON). This is because they contain lots binary data (like figures) represented by strings of seemingly random characters. Any change to the code that results in changed figure will also change the binary string. Git sees all these changes and wants to account them. However, we can always regenerate the figure by running the code and are not interested in storing such changes. A very good solution is to use [`jupytext`](https://github.com/mwouts/jupytext). This little package can convert, both from and to, `.ipynb` and `.py` files (among many others), removing binary strings in the process. Better yet, you can pair a notebook with a `.py` file, which will be updated every time you save the notebook. After doing this, you only need to version control the `.py` file. If you ever lose your notebook file, you can regenerate it using `jupytext` and rerun it to create the figures and output. It even works with other languages like Julia and R!
