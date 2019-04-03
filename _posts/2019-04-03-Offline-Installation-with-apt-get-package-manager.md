---
layout: post
title:  Offline installation with apt-get and yum package manager 
date:   2019-04-03
categories: ["Linux"]
image: "/assets/images/2019-04-03/offline-install.png"
---

My situation:

You have a machine that is not connected to the internet and you would like to install a package on the system. Both my offline and online machine are using exactly the same operating systems including its version.

Benefits:

* Save bandwidth
* If the machine you want to install packages on is either not connected or you have slow / unstable internet connection.

__Warning__: There are many dependencies for a package, the way I was able to perform a sanity check was to create a virtual machine that contained no installs, turned off the internet and created a shared folder between host and guest. Then I would try to install a package on the VM and it will provide the dependency errors if there are any. Removing the dependency errors simply requires you to download the package and install them in the right order. After successfully installing everything on the VM and tested the tools are all working, I would move all the downloaded packages to the actual offline machine through CD / USB.

# apt

__Online machine__:

Download the dependencies in your current directory

```bash
apt-get download <PACKAGE_NAME>
```

Transfer over the directory.

__Offline machine__:

Install deb package in current directory

```bash
apt install ./package_name.deb
 ```

---

Check all package dependencies[^so-dep]:

Note: Even using this tool I had missed a few dependencies due to the lack of tracing back on the dependencies. For example, while looking at the dependencies for python-venv, I did not look at the dependencies of the dependencies etc.

```bash
apt-cache depends <PACKAGE_NAME>
```

Example :

```bash
>>> apt-cache depends python3

python3
  PreDepends: python3-minimal
  Depends: python3.6
  Depends: libpython3-stdlib
  Suggests: python3-doc
  Suggests: python3-tk
  Suggests: python3-venv
  Replaces: python3-minimal
```

PreDepends:
> This field is like Depends, except that it also forces dpkg to complete installation of the packages named before even starting the installation of the package which declares the pre-dependency.

Depends:
> This declares an absolute dependency. A package will not be configured unless all of the packages listed in its Depends field have been correctly configured (unless there is a circular dependency as described above).

Suggests:
> This is used to declare that one package may be more useful with one or more others. Using this field tells the packaging system and the user that the listed packages are related to this one and can perhaps enhance its usefulness, but that installing this one without them is perfectly reasonable.

PreDepends, Depends, Suggests, are retrieved from [^debianpkg]

# yum

Using the yum downloader[^yumdownload]

Python3 is not included in the built-in yum repositories, there is a community maintaining the upstream packages for centos[^iusrepo]

__Online machine__:

Add the repository to our package manager:

```bash
yum install -y https://centos7.iuscommunity.org/ius-release.rpm
yum update
```

Install yum-utils for yumdownloader.
Yumdownloader allows me to download the dependencies using `--resolve`.

```bash
yum install yum-utils
```

```bash
yumdownloader --resolve  <PACKAGE_NAME>
```

Generate metadata for our RPMs repository to make it a yum repository

```bash
createrepo -v <PATH_TO_PACKAGE_DIR>
```

Transfer over the directory

__Offline machine__:

Add the yum repository to the list of repositories on yum

`vi /etc/yum.repos.d/localrepo.repo`

```bash
[localrepo]
name=Awesome Repository
baseurl=file:///path/to/repo/folder
gpgcheck=0
enabled=1
```

__Note__: Use three slashes(///) in the baseurl.

Install rpm package located in your current directory.

```bash
yum install --disablerepo="*" --enablerepo="localrepo" <PACKAGE_NAME>
```

* Where the package name is the same as the name you would use to install a package from the internet.
* Tell yum to look for packages in the `localrepo` only.

[https://www.ostechnix.com/download-rpm-package-dependencies-centos/](https://www.ostechnix.com/download-rpm-package-dependencies-centos/)

* Method two, using yumdownloader

[https://www.unixmen.com/setup-local-yum-repository-centos-7/](https://www.unixmen.com/setup-local-yum-repository-centos-7/)

* Ignore the ftp section
* Focus on the "Build local repository" section

[https://unix.stackexchange.com/questions/259640/how-to-use-yum-to-get-all-rpms-required-for-offline-use](https://unix.stackexchange.com/questions/259640/how-to-use-yum-to-get-all-rpms-required-for-offline-use)

* Followed a similar process to the best answer

---

[^so-dep]: [https://askubuntu.com/questions/275719/reinstall-package-and-its-installed-dependencies](https://askubuntu.com/questions/275719/reinstall-package-and-its-installed-dependencies)
[^debianpkg]: [https://www.debian.org/doc/debian-policy/ch-relationships.html](https://www.debian.org/doc/debian-policy/ch-relationships.html)
[^yumdownload]: [https://access.redhat.com/solutions/10154]([https://access.redhat.com/solutions/10154)
[^iusrepo]: [https://ius.io/Philosophy/](https://ius.io/Philosophy/)


