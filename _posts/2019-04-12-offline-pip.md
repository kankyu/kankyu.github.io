---
layout: post
title:  Offline installation with pip and python3-venv
date:   2019-04-12 
categories: ["Python"]
image: "/assets/images/2019-04-12-offline-pip/chris-ried-512801-unsplash.png"
---
Credit: Photo by Chris Ried on Unsplash

My situation:
I want to be able to download Python packages so that I can install it on an offline-machine, where both the offline and online Linux machines are the same operating systems.

Related Article: Offline installation with apt-get and yum package manager

# Creating the virtual environment

```bash
python3 -m venv <NAME_OF_VIRTUALENV_FILE>
```

Using the venv:

```bash
source <NAME_OF_VIRTUALENV>/bin/activate
```

# Creating the requirements list

Install the packages you want to move over to your offline machine

```bash
pip install <Packages>
```

Once you have installed packages that you want save the package list into `requirements.txt`

```bash
pip freeze > requirements.txt
```

Remove `package-resources == 0.0.0` in `requirements.txt`

# Download packages from the requirements list

---

[optional] create a download folder for your [packages]

```bash
mkdir <DIR_NAME>
cd <DIR_NAME>
```

---

```bash
pip download -r <PATH_TO>/requirements.txt
```

pip will download compressed files of packages in the name of `*.whl` although this is not always the case.

Transfer over the downloaded files and requirements.txt to the offline machine.

# Installing downloaded packages

I personally create a virtual environment on my offline machine, which are the same instructions mentioned in the create virtual environment section.

```bash
pip install --no-index --find-links <PATH_TO_DOWNLOADED PACKAGES> -r requirements.txt
```

`--no-index` - ignore package index, this is required to stop python from attempting to lookup the PyPi (online a directory)

`--find-links` - specifying the path to the downloaded packages