# packer-templates: Configurations for generating Vagrant base boxes

# EXAMPLE

```console
$ cd debian
$ time make install-box-virtualbox
packer build -force -only virtualbox-iso debian.json

...

      529.04 real        20.08 user        11.78 sys

vagrant box add -f --name mcandre/debian --provider virtualbox debian-virtualbox.box

...

$ vagrant box list
mcandre/debian                           (virtualbox, 0)

$ cd test
$ vagrant up
$ vagrant ssh -c 'uname -a'
Linux debian 4.9.0-4-amd64 #1 SMP Debian 4.9.51-1 (2017-09-28) x86_64 GNU/Linux
$ vagrant ssh -c 'ls /vagrant'
bootstrap.sh  flag.txt	Vagrantfile
```

# REQUIREMENTS

* [Packer](https://www.packer.io/)
* [Vagrant](https://www.vagrantup.com/) 2.2.2+
* [bzip2](http://www.bzip.org)
* [wget](https://www.gnu.org/software/wget/)
* [VirtualBox](https://www.virtualbox.org/)

Note: Windows hosts are affected by a packer bug where attempts to kill a packer process by sending a Control+C signal, result in a half-dead packer that often awakes during subsequent builds, corrupting them. Task Manager is your friend.

## Notes

Be sure to change directory to the guest OS desired (e.g. `debian/`), as Packer builds are relative to the current working directory, rather than relative to the packer JSON directory.

### VirtualBox

* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

One cleanup tip: As with all Vagrant hypervisors, VirtualBox sometimes leaves virtual machine data around when `vagrant destroy [-f]`, or a signal interrupted `vagrant up` should have deleted these artifacts. When this happens, the user can launch the VirtualBox application and delete these files manually. VirtualBox will likely complain with multiple error prompts, but these can largely be ignored.

## Optional

* a keyboard cover, in case of nearby cats that may scamper around and corrupt sensitive boot commands
* [Taurine](https://itunes.apple.com/us/app/taurine/id960276676) (macOS), [Caffeine](http://www.zhornsoftware.co.uk/caffeine/) (Windows), [Caffeine](https://launchpad.net/caffeine) (Linux) can prevent hibernation during any long builds
* [make](https://www.gnu.org/software/make://www.gnu.org/software/make/)
* [coreutils](https://www.gnu.org/software/coreutils/coreutils.html)
* [shfmt](https://github.com/mvdan/sh) (e.g. `go get github.com/mvdan/sh/cmd/shfmt`)

# TESTING

These boxes are designed as minimal bases for constructing build bot virtual machines, so that [mcandre/tonixxx](tonixxx) can use the boxes to conveniently cross-compile applications for many different kernels. The boxes are expected to feature:

* working package manager, for installation of devopment tools like gcc, curl, lua, etc.
* bidirectional-capable host->guest and guest->host synced folders, for copying source code to the box and copying artifacts back to the host, via [vagrant-rsync-back](https://github.com/smerrill/vagrant-rsync-back)

The best way to ensure that the boxes are suitable for this development workflow is to attempt to install some package, and to check that files can be copied from the host and guest and back again. This workflow is automated in a testing script. Example:

```console
$ cd debian/test
$ vagrant up
$ make test
...
```
