**Conda official doc**: https://docs.conda.io/projects/conda/en/stable/

# Install Miniconda

Find the installer from the [official doc](https://conda.io/projects/conda/en/stable/user-guide/install/linux.html), and then run `bash {installer_path}`.

# Update Conda

At the base environment, run `conda update conda`.

# Manage Environments

Find complete guidance at the [official doc1](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) and [official doc2](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#setting-environment-variables).

`conda create --name {env_name} python=3.8 scipy=0.17.3 pip` to create an environment with a specified version of python and package.

`conda list` to list packages of an environment.

`conda env list` to list all environments.

`conda env remove --name {env_name}` to remove an environment.

`conda env config vars set MY_VAR=value` to set an environment variable for the current environment, noted that 'MY_VAR' must be in all CAP. Reactivate the environment to make this change take effect.

`conda env config vars unset MY_VAR` to unset an environmnet variable for the current environment. Reactivate the environment to make this change take effect.

`conda env config vars list` to list all environment variables of the current environment.

# Manage Channels & Packages

Find complete guidance at the [official doc1](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-channels.html), [official doc2](https://conda.io/projects/conda/en/latest/user-guide/concepts/channels.html), and [official doc3](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html#).

Channels are locations where packages are stored, hosted, and managed. Different channels may have the same package, which may give rise to collisions.

Search channel and packages online at https://anaconda.org/

`conda search --override-channel --channel defaults {package_name}` to search a package in the default channel.

`conda search --override-channel --channel defaults {package_name}` to search a package in a specified channel.

`conda install {package_name}={version} --channel {channel_name}` to install a package from a specified channel.

`conda remove {package_name}` to remove a package under the current environment.