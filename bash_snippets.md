# Useful bash code

Bash scripting can simplify repetative, complex, or difficult to remember tasks. Below are some useful bash snippets. 

#### Contents

1) [Conda utilities](#Conda-utilities)

## Conda utilities

The function below creates a conda environment from an environment file. It will also install an ipython kernel for the environment. Place it in `.zshrc` or `.bashrc`. If you have not installed [`mamba`](https://anaconda.org/conda-forge/mamba) (e.g. `conda activate base && conda install mamba`) then this code will work if you replace every instance of `mamba` with `conda`.

```bash
create_env() {
  if [ "$#" -ne 1 ]; then
    echo "Usage: create_env <path/to/environment.yml>"
    return 1
  fi
  local env_file="$1"
  local env_name
  # Extract the environment name from the file, if specified
  env_name=$(grep "name:" "$env_file" | awk '{print $2}')
  if [ -z "$env_name" ]; then
    env_name=$(basename "$env_file" .yml)
  fi
  # Create the Conda environment
  mamba env create --file "$env_file" --name "$env_name"
  # Activate the Conda environment
  mamba activate "$env_name"
  # Install ipykernel into the environment if not specified in the file
  if ! mamba list --name "$env_name" ipykernel | grep ipykernel &>/dev/null; then
    mamba install -y ipykernel
  fi
  # Install the IPython kernel for Jupyter
  python -m ipykernel install --user --name "$env_name" --display-name "$env_name"
  # Deactivate the Conda environment
  mamba deactivate
}
```

This function removes a conda environment and a kernel of the same name. 

```bash
remove_env() {
  if [ "$#" -ne 1 ]; then
    echo "Usage: remove_env <environment_name>"
    return 1
  fi
  local env_name="$1"
  mamba activate "$env_name"
  # Uninstall the IPython kernel for Jupyter
  jupyter kernelspec uninstall -y "$env_name"
  # Deactivate the Conda environment
  mamba deactivate
  # Remove the Conda environment
  mamba env remove --name "$env_name"
}
```