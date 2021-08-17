# Anaconda Guide

Anaconda (usually shortened to "conda") is a package and environment management system for Python targeted towards the scientific community. We have our own custom version of conda available to us in the CwC lab on the `babbage` server and we have the ability to share environments between lab members. Conda allows us to:

- Create isolated Python virtual environments for projects
- Install our own packages into these environments
- Store environments and packages on `babbage` so they **do not** take up home directory space
- Share pre-built environments for our projects so others can get going faster

## Environments

Conda environments are self-contained Python installations containing packages, scripts, and environment variables needed to run a project. Different environments can have different versions of packages that would normally conflict if you tried to install them all at once on your machine. This can even include having different versions of Python, CUDA libraries, Tensorflow or PyTorch.

Creating new environments or cloning existing environments is **cheap and fast** in conda because conda makes use of links to packages.

The goal is to **create an environment for each project**. Ideally, this would mean having a conda environment per git repository you use. There are several use cases for this:

### 1. Using Someone Else's Code

Many paper authors are now publishing a `requirements.txt` or an `environment.yml` file in their repositories that contain a list of necessary packages and versions to help people get up and running faster. By creating a new, isolated environment when using that repository we can install whatever crazy custom versions and packages the original authors used without fear of breaking our own projects.

### 2. Maintaining Backward Compatibility for Your Own Code

With our own projects, maintaining one environment per project means we don't have to worry about maintaining backwards compatibility with older projects when we move on to new projects. The speed of development of core libraries like Tensorflow or PyTorch is lightning fast. Even minor revision changes in libraries regularly break things. By maintaining separate environments for each project, we know we can freeze the versions in time so we can easily return months or years later. Have a codebase that still uses Tensorflow 1, but you are currently working with a bleeding edge Tensorflow 2.X? No problem, just switch back to the old environments.

### 3. Sharing Environments With Collaborators

With our shared environment structure, it makes it really easy for collaborators to get up and running. Once the the primary author has an environment setup, all a collaborator needs to do is activate that environment and they are all set!

## Getting Started

First, login to any of the CSU network machines (e.g., `luffy`).

Once logged in, getting setup with the CwC lab's versions of conda is as simple as running a single command:

```
/s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/condabin/conda init
```

The output should look something like:

```
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/condabin/conda
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/bin/conda
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/bin/conda-env
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/bin/activate
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/bin/deactivate
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/etc/profile.d/conda.sh
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/etc/fish/conf.d/conda.fish
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/shell/condabin/Conda.psm1
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/shell/condabin/conda-hook.ps1
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/lib/python3.9/site-packages/xontrib/conda.xsh
no change     /s/babbage/b/nobackup/nblancha/merry/conda/miniconda3/etc/profile.d/conda.csh
modified      /s/chopin/k/grad/cwctest/.bashrc

==> For changes to take effect, close and re-open your current shell. <==
```

It is recommended from here to completely log out and log back into the machine to reset the environment. Once you log back in you should see that your prompt now looks something like:

```
(base) luffy:~$
```

The `(base)` prefix indicates that conda is active and that you are currently working in the "base" environment.

## New Base Environment

> :warning: **WARNING:** For those of you already using the department's version of Anaconda, there are some differences with the new CwC `(base)` environment. 

An effort has been made to include many of the same packages as were previously available with the department's base environment. There are some differences:

- Package version numbers are different and **will change over time!**
- Default channel is `conda-forge`, which may have newer versions of packages that are less stable
- CUDA 11.0 is included to support newer RTX 30XX GPUs
- PyTorch GPU acceleration is working but, **Tensorflow GPU acceleration is not**
  - This is an issue with the conda package repository. If you need Tensorflow GPU support with CUDA 11, please see the `tf25-cuda11-pip` pre-built environment for CUDA 11 GPU accelerated support.

## Existing Environments

In addition to the base environment, several other environments have been setup that are available for use. The get a full listing of available environments use the command:

```
conda env list
```

### Cloning An Existing Environment

Only the owners of an environment can modify it, but if you wish to use an existing environment as a base for a project you can clone an environment:

```
conda create -n {NEW_NAME} --clone {EXISTING_NAME}
```

### Table of Environments

Name|Major Package Versions|Notes
:---|:---|:---
py27|python=2.7|
tf1-cuda10|python=3.7, tensorflow-gpu=1.15.0, cudatoolkit=10.0|CUDA 11 support unavailable
tf24-cuda10|python=3.9, tensorflow-gpu=2.4.1, cudatoolkit=10.1|No supported on RTX 30XX
tf25-cuda11-pip|python=3.8, tensorflow-gpu=2.5, cudatoolkit=11.2|ONLY USE PIP FOR PACKAGES
torch18-cuda11|python=3.8, pytorch=1.8-lts, cudatoolkit=11.1|PyTorch LTS branch
torch19-cuda11|python=3.8, pytorch=1.9, cudatoolkit=11.1|
