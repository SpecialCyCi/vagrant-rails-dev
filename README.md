# A Virtual Machine for Ruby on Rails Core Development

## Introduction

This project automates the setup of a development environment for working on Ruby on Rails itself. Use this virtual machine to work on a pull request with everything ready to hack and run the test suites.

**Please note this virtual machine is not designed to be used for Rails application development.**

## Requirements

* [VirtualBox](https://www.virtualbox.org) or [VMWare Fusion](http://www.vmware.com/products/fusion) or [Parallels Desktop](http://www.parallels.com/products/desktop/)(need Vagrant 1.5+, see [vagrant-parallels](http://parallels.github.io/vagrant-parallels/docs/installation/index.html))

* [Vagrant 1.1+](http://vagrantup.com) (not a Ruby gem)

## How To Build The Virtual Machine

Building the virtual machine is this easy:

#### 1: Add system box file to Vagrant

Download .box file from [http://files.vagrantup.com/precise64.box](http://files.vagrantup.com/precise64.box) .

Then add it to Vagrant.

    host $ vagrant box add precise64 ~/box/precise64.box

#### 2: Build up system with vagrant. 

    host $ git clone https://github.com/SpecialCyCi/vagrant-rails-dev.git
    host $ cd vagrant-rails-dev
    host $ vagrant up

That's it.

(If you want to use VMWare Fusion instead of VirtualBox, write `vagrant up --provider=vmware_fusion` instead of `vagrant up` when building the VM for the first time. After that, Vagrant will remember your provider choice, and you won't need to include the `provider` flag again.)

(If you want to use Parallels Desktop instead of VirtualBox, you need Vagrant 1.5+, and write `vagrant up --provider=parallels` instead of `vagrant up` when building the VM for the first time. After that, Vagrant will remember your provider choice, and you won't need to include the `provider` flag again.)

If the base box is not present that command fetches it first. The setup itself takes about 3 minutes in my MacBook Air. After the installation has finished, you can access the virtual machine with

    host $ vagrant ssh
    Welcome to Ubuntu 12.04 LTS (GNU/Linux 3.2.0-23-generic-pae i686)
    ...
    vagrant@rails-dev-box:~$

Port 3000 in the host computer is forwarded to port 3000 in the virtual machine. Thus, applications running in the virtual machine can be accessed via localhost:3000 in the host computer.

## What's In The Box

* Git

* RVM

* Ruby 2.1.1 (binary RVM install)

* Bundler

* SQLite3, MySQL, and Mongod

* System dependencies for nokogiri, sqlite3, mysql, mysql2

* Databases and users needed to run the Active Record test suite

* Node.js for the asset pipeline

* Memcache and Redis

## Virtual Machine Management

When done just log out with `^D` and suspend the virtual machine

    host $ vagrant suspend

then, resume to hack again

    host $ vagrant resume

Run

    host $ vagrant halt

to shutdown the virtual machine, and

    host $ vagrant up

to boot it again.

You can find out the state of a virtual machine anytime by invoking

    host $ vagrant status

Finally, to completely wipe the virtual machine from the disk **destroying all its contents**:

    host $ vagrant destroy # DANGER: all is gone

Please check the [Vagrant documentation](http://docs.vagrantup.com/v2/) for more information on Vagrant.

## Faster Rails test suites

The default mechanism for sharing folders is convenient and works out the box in
all Vagrant versions, but there are a couple of alternatives that are more
performant.

### rsync

Vagrant 1.5 implements a [sharing mechanism based on rsync](https://www.vagrantup.com/blog/feature-preview-vagrant-1-5-rsync.html)
that dramatically improves read/write because files are actually stored in the
guest. Just throw

    config.vm.synced_folder '.', '/vagrant', type: 'rsync'

to the _Vagrantfile_ and either rsync manually with

    vagrant rsync

or run

    vagrant rsync-auto

for automatic syncs. See the post linked above for details.

### NFS

If you're using Mac OS X or Linux you can increase the speed of Rails test suites with Vagrant's NFS synced folders.

With a NFS server installed (already installed on Mac OS X), add the following to the Vagrantfile:

    config.vm.synced_folder '.', '/vagrant', type: 'nfs'
    config.vm.network 'private_network', ip: '192.168.50.4' # ensure this is available

Then

    host $ vagrant up

Please check the Vagrant documentation on [NFS synced folders](http://docs.vagrantup.com/v2/synced-folders/nfs.html) for more information.

## License

Released under the MIT License, Copyright (c) 2012–<i>ω</i> Xavier Noria.
