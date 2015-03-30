# Provision a Vagrant machine to run Xen and Mirage

This is all based on [this post](http://www.skjegstad.com/blog/2015/01/19/mirageos-xen-virtualbox/) from Magnus Skjegstad.

## Prerequisites

### VirtualBox

### Vagrant

### Vagrant Reload plugin

Install this with:

vagrant plugin install vagrant-reload

## Installation

Assuming you have all the prereqs installed already, you should be able to simply run `vagrant up` and then it should all be ready to go.

## Usage

Run `vagrant ssh` to connect to the VM. Then execute `git clone http://github.com/mirage/mirage-skeleton.git` and `cd mirage-skeleton/static-website` when it's done. Now, now `env DHCP=true mirage configure --xen`. Next, you can run `make` to build everything. You'll probably see an error that looks like the build failed, but as long as you end up with a file called `www.xl`, everything should be fine. If that file was created successfully, you can run `sudo xl create www.xl -c` to start it within Xen.


## TODOs

* Figure out how to get network config to work as described in Magnus' post.
