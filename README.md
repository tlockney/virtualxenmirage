# Provision a Vagrant machine to run Xen and Mirage

This is all based on [this post](http://www.skjegstad.com/blog/2015/01/19/mirageos-xen-virtualbox/) from Magnus Skjegstad.

## Prerequisites

* [VirtualBox](https://www.virtualbox.org)
* [Vagrant](https://www.vagrantup.com)
* Vagrant Reload plugin - install via `vagrant plugin install vagrant-reload`

## Installation

Assuming you have all the prereqs installed already, you should be able to simply run `vagrant up`. After a few minutes—depending on your network speed—it should all be ready to go.

## Usage

1. Run `vagrant ssh` to connect to the VM.
1. Then execute `git clone http://github.com/mirage/mirage-skeleton.git` and `cd mirage-skeleton/static-website` when it's done.
1. Now, now `env DHCP=true mirage configure --xen`.
1. Next, you can run `make` to build everything. You'll probably see an error that looks like the build failed, but as long as you end up with a file called `www.xl`, everything should be fine.
1. If that file was created successfully, you can run `sudo xl create www.xl -c` to start it within Xen.


## TODOs

* Figure out how to get network config to work as described in Magnus' post.
