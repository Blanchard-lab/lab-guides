* Updated: March 27, 2025

- [Anaconda Guide](#anaconda-guide)
  - [Getting Started](#getting-started)
    - [Prerequisit](#prerequisit)
    - [Conda Setup](#conda-setup)
  - [Introduction to Environments](#introduction-to-environments)
    - [1. Using Someone Else's Code](#1-using-someone-elses-code)
    - [2. Maintaining Backward Compatibility for Your Own Code](#2-maintaining-backward-compatibility-for-your-own-code)
    - [3. Sharing Environments With Collaborators](#3-sharing-environments-with-collaborators)
  - [Details on Environment](#details-on-environment)
    - [New Base Environment](#new-base-environment)
    - [Pre-Configured Environments](#pre-configured-environments)
    - [Cloning An Existing Environment](#cloning-an-existing-environment)
  - [Commands of Conda](#commands-of-conda)
    

# Anaconda Guide

Anaconda (usually shortened to "conda") is a package and environment management system for Python targeted towards the scientific community. We have our own custom version of conda available to us in the IMPACT lab on the `babbage` server and we have the ability to share environments between lab members. Conda allows us to:

- Create isolated Python virtual environments for projects
- Install our own packages into these environments
- Store environments and packages on `babbage` so they **do not** take up home directory space
- Share pre-built environments for our projects so others can get going faster

## Getting Started

### Prerequisit
Our shared conda exists under the `babbage` file server. To access `babbage`, you will have `strawhats` access. If you do not have access, please contact SNA and cc Nate to get access.
* Email to: CompSci_sna@cs.colostate.edu
* CC: nathaniel.blanchard@colostate.edu
* Title: Request for Strawhats Access
- Email template:
```markdown
Dear SNA Team,

I hope this message finds you well.
My name is <Name>, and I am a student in the IMPACT Lab. My username is <username>. I would like to request access for the `strawhats` group to `babbage`.
Could you please help me to get the access?
Thank you for your support.

Best regards,
<Name>
```

* You can double-check if you have access by running the following command:
```bash
$   groups
```
If you see `strawhats` in the output, you have access to `babbage`.

### Conda Setup
* It is one-time setup.
1. Login to any of the CSU network machines (e.g., `luffy`).
 - If you are off-campus, you will need to use the GlobalVPN to access the CSU network.
2. Once logged in, please open your `.bashrc` file using your favorite text editor (e.g., `vim`, `nano`, `gedit`, etc.).
3. Add the following line to the end of the file:
```bash
# >>> conda initialize >>>
. /s/babbage/h/nobackup/nblancha/conda/mar2025/etc/profile.d/conda.sh
conda activate base
# <<< conda initialize <<<
umask 002
```
4. Save the file and exit the text editor.
5. Run the following command to apply the changes:
```bash
$   source ~/.bashrc
```
6. If you can see `(base)` in the prompt, you have successfully set up the conda environment.
```bash
(base) luffy:~$
```
* If you do not see `(base)` in the prompt, please log out and log back in to the machine. If you still do not see `(base)` in the prompt, please contact Changsoo Jung or Sifatul Anindho.
* The `(base)` prefix indicates that conda is active and that you are currently working in the "base" environment.
* **Note: Please do not install any packages in the `(base)` environment. It is recommended to create a new environment for each project.**

## Introduction to Environments

Conda environments are self-contained Python installations containing packages, scripts, and environment variables needed to run a project. Different environments can have different versions of packages that would normally conflict if you tried to install them all at once on your machine. This can even include having different versions of Python, CUDA libraries, Tensorflow or PyTorch.

Creating new environments or cloning existing environments is **cheap and fast** in conda because conda makes use of links to packages.

The goal is to **create an environment for each project**. Ideally, this would mean having a conda environment per git repository you use. There are several use cases for this:

### 1. Using Someone Else's Code

Many paper authors are now publishing a `requirements.txt` or an `environment.yml` file in their repositories that contain a list of necessary packages and versions to help people get up and running faster. By creating a new, isolated environment when using that repository we can install whatever crazy custom versions and packages the original authors used without fear of breaking our own projects.

### 2. Maintaining Backward Compatibility for Your Own Code

With our own projects, maintaining one environment per project means we don't have to worry about maintaining backwards compatibility with older projects when we move on to new projects. The speed of development of core libraries like Tensorflow or PyTorch is lightning fast. Even minor revision changes in libraries regularly break things. By maintaining separate environments for each project, we know we can freeze the versions in time so we can easily return months or years later. Have a codebase that still uses Tensorflow 1, but you are currently working with a bleeding edge Tensorflow 2.X? No problem, just switch back to the old environments.

### 3. Sharing Environments With Collaborators

With our shared environment structure, it makes it really easy for collaborators to get up and running. Once the the primary author has an environment setup, all a collaborator needs to do is activate that environment and they are all set!

## Details on Environment

### New Base Environment

> :warning: **WARNING:** For those of you already using the department's version of Anaconda, there are some differences with the new IMPACT `(base)` environment. 

An effort has been made to include many of the same packages as were previously available with the department's `(base)` environment. There are some differences:

- Package version numbers are different and **will change over time!**
- Default channel is `conda-forge`, which may have newer versions of packages that are less stable
- The actual packages is installed in `/s/babbage/h/nobackup/nblancha/conda/mar2025/pkgs` directory. Please do not remove or modify the packages in this directory.


### Pre-Configured Environments

In addition to the base environment, several other environments have been setup that are available for use. The get a full listing of available environments use the command:

```
conda env list
```

### Cloning An Existing Environment

Only the owners of an environment can modify it, but if you wish to use an existing environment as a base for a project you can clone an environment:

```
conda create -n {NEW_NAME} --clone {EXISTING_ENV_NAME}
```

## Commands of Conda
* To create a new environment:
```bash
$   conda create -n {ENV_NAME} python=<python_version> -y
```
* To activate an environment:
```bash
$   conda activate {ENV_NAME}
```
* To deactivate an environment:
```bash
$   conda deactivate
```
* To list all environments:
```bash
$   conda env list
```
* To remove an environment (Please be careful and do not remove as `rm -r`):
```bash
$   conda env remove -n {ENV_NAME}
```
* To install a package in an environment (Please activate the environment first):
```bash
$   conda install {PACKAGE_NAME}
```
* To install a package in a specific version:
```bash
$   conda install {PACKAGE_NAME}={VERSION}
```
* To install a package from a specific channel:
```bash
$   conda install -c {CHANNEL_NAME} {PACKAGE_NAME}
```
* To delete a package from an environment (Please activate the environment first):
```bash
$   conda remove -n {ENV_NAME} {PACKAGE_NAME}
```
* If you have a .yml or .yaml file, you can create an environment from the file (Please remove prefix path from the file):
```bash
$   conda env create -f {FILE_NAME}
```
* To export an environment to a file (Please activate the environment first):
```bash
$   conda env export > {FILE_NAME}
```