debian "squeeze" bootstrapping script for EC2
=============================================

This is a fork of camptocamps bootstrapping script for EC2 AMIs.
It creates a vanilla debian squeeze machine image, no latent logfiles no .bash_history or even apt package cache.
The machine configuration this script creates has been thoroughly tested.

Features
--------
AMI features
""""""""""""
* EBS booted
* xfs filesystem
* Ephemeral storage is properly mapped
* Standard ec2 startup scripts
* Uses standard debian Xen kernel from apt
* update-grub creates an actual menu.lst which pvGrub can read

Bootstrapper features
"""""""""""""""""""""
* EBS volume is automatically created, mounted, formatted, unmounted, "snapshotted" and deleted
* AMI is automatically registered with the right kernels for the current region of the host machine
* Can create both 32-bit and 64-bit AMIs
* Custom script inclusion supported to keep the bootstrapping process automated
* Custom packages
* The process is divided into simple task based scripts
* Bootstrapping server mirror depends on AWS region
* APT source mirror depends on AWS region

Installation
------------
Simply clone the repo :-)

I recommend setting the AWS credentials via an `Environment script`_,
this way you do not need to specify the parameters for every api call.

Usage
-----

Bootstrapping options
"""""""""""""""""""""
--arch ARCHITECTURE
	i386 amd64 [default: amd64]
--debug
	Use -x option in bash

Environment options
"""""""""""""""""""
--timezone ZONE
	Defaults to UTC
--locale LOCALE
	Defaults to en_US
--charmap CHARMAP
	Defaults to UTF-8
--package NAME
	Additional package to install
--no-run-user-data
	Do not run user-data script on first boot
--script FILE
	External script/command to run before bundle
--imagedir DIR
	Bootstrap directory [default: /mnt/image]

AWS options
"""""""""""
--user ID
	Defaults to $AWS_USER_ID
--access-key ID
	Defaults to $AWS_ACCESS_KEY_ID
--secret-key ID
	Defaults to $AWS_SECRET_ACCESS_KEY_ID
--private-key PATH
	Defaults to $EC2_PRIVATE_KEY
--cert PATH
	Defaults to $EC2_CERT

Environment script
------------------
Include with `source env-script` for the variables to be present on the commandline.
::

	export EC2_URL='https://ec2.eu-west-1.amazonaws.com'
	export EC2_HOME="/root/ec2/ec2-api-tools-1.5.2.3"
	export EC2_AMITOOL_HOME="/root/ec2/ec2-ami-tools-1.4.0.5"
	export EC2_PRIVATE_KEY="/root/root.key"
	export EC2_CERT="/root/root.crt"
	export AWS_USER_ID='1234-4567-8910'
	export AWS_ACCESS_KEY_ID='SOM3L0NG4CC3SSK3Y000'
	export AWS_SECRET_ACCESS_KEY='SomeBase64EncodedString'
	export PATH="$PATH:${EC2_HOME}/bin:${EC2_AMITOOL_HOME}/bin"

If you are using IAM to access AWS you may need to create the certificate first. You can use `this gist <https://gist.github.com/2629062>`_ for that.
