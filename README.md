# debian "squeeze" bootstrapping script for EC2 #

This is a fork of camptocamps bootstrapping script for EC2 AMIs. It creates a
vanilla debian squeeze machine image, no latent logfiles no .bash\_history or
even apt package cache.  The machine configuration this script creates has been
thoroughly tested.

*This script is only tested on debian squeeze.*
*You will need an EC2 server to run this bootstrapper.*

## Usage ##

The script is started with ``./ec2debian-build-ami`` it has sensible defaults
but can be configured with options and plugins. To see a list of options run
``./ec2debian-build-ami --help``.
At the very least, the script needs to know your AWS credentials.

There are no interactive prompts, the bootstrapping can run entirely unattended
from start till finish.

Some plugins are included in the [plugins directory](https://github.com/andsens/ec2debian-build-ami/tree/master/plugins).
A list of external plugins is also provided there. If none of those scratch
your itch, you can of course [write your own plugin](https://github.com/andsens/ec2debian-build-ami/blob/master/plugins/HOWTO.md).

## Features ##

### AMI features ###

* EBS booted
* Base installation uses only 289MB
* Base installation bootup time ~45s* (from AMI launch to SSH connectivity)
* xfs filesystem
* Uses standard debian Xen kernel from apt
* update-grub creates an actual menu.lst which pvGrub can read
* ec2 startup scripts:
  * `ec2-get-credentials`: Copies the ec2 keypair to `~/.ssh/authorized_keys`
  * `ec2-run-user-data`: If the userdata starts with `#!` it will be executed
  * `generate-ssh-hostkeys`: Generates hostkeys for sshd on first boot
  * `expand-volume`: Expands the root partition to the volume size
  * `change-root-uuid`: Regenerates the UUID of the root volume on first boot

**The bootup time was measured with [this script](https://gist.github.com/3813743)*

### Bootstrapper features ###

* EBS volume is automatically created, mounted, formatted, unmounted, "snapshotted" and deleted
* AMI is automatically registered with the right kernels for the current region of the host machine
* Can create both 32-bit and 64-bit AMIs
* Plugin system to keep the bootstrapping process automated
* The process is divided into simple task based scripts
