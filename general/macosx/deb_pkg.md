## [Build a .Deb file](http://ubuntuforums.org/showthread.php?t=910717)

```bash
$ cd

## create a dir to hold package content
$ mkdir helloworld_1.0-1 # major.minor-package
$ cd helloworld_1.0-1
$ mkdir -p usr/local/bin
$ mkdir DEBIAN

## create install contents
## create usr/local/bin/helloworld with 
#!/bin/bash
echo "Hello World! Deb Package!"

$ chmod 755 usr/local/bin/helloworld

## Create meta file
## create DEBIN/control with (white space is important)
Package: helloworld
Version: 1.0-1
Section: base
Priority: optional
Architecture: i386
Depends:
Maintainer: Alex Li <ali@email.com>
Description: Hello World
 When you need some sunshine, just run this
 small program!

## create .deb file
$ cd
$ dpkg-deb --build helloworld_1.0-1 # this generate the helloworld_1.0-1.deb

## install .deb
$ sudo dpkg -i ./helloworld_1.0-1.deb

## At the end, /usr/local/bin/helloworld is installed.

```

## [Create an Debian package repository](http://askubuntu.com/questions/170348/how-to-make-my-own-local-repository)
```bash

# install dpkg-dev
$ sudo apt-get install dpkg-dev

# mkdir for package repo
$ sudo mkdir -p /usr/local/mydebs

# move the package to /usr/local/mydebs
$ cp helloworld_1.0-1.deb /usr/local/mydebs

# create a script to update mydebs in ~/bin/update-mydebs
#! /bin/bash
 cd /usr/local/mydebs
 dpkg-scanpackages . /dev/null | gzip -9c > Packages.gz

$ chmod u+x ~/bin/update-mydebs

# change /etc/apt/sources.list, adding
deb file:/usr/local/mydebs ./

# update and install
sudo ~/bin/update-mydebs
sudo apt-get update
sudo apt-get install helloworld

# in case you have installed already, and want to remove before install
$ sudo aptitude search helloworld
$ sudo dpkg -r helloworld:i386
$ sudo apt-get install helloworld

```
