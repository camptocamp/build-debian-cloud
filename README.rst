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
* Plugin system to keep the bootstrapping process automated
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
--plugin FILE
	Path to scripts which change the behavior of the bootstrapper. Repeat for multiple plugins.
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

Plugins
-------
If you want to change the behavior of the bootstrapper you can either modify the tasks directly or write plugins. Writing plugins has several advantages.

* You can easily include and exclude them with a command line option.
* The code is held separate from the bare bones bootstrapping setup.
* You don't need to fork this repo.
* No need to merge updates into your own repo.
* Easily share your plugins with others.

All plugins specified when bootstrapping, will be sourced *before* any tasks are run. The plugins can modify the task list and add their own tasks.
Tasks are simply paths to scripts. They will be sourced as well.
I recommend namespacing the function and variable names in a task to avoid naming clashes.

Adding tasks is quite easy. To have a custom task run before another, call ``insert_task_before``. The first argument is the basename of the path to the task script. The second argument is the path to the task that is inserted.
eg.:
::
	insert_task_before "14-bootstrap" "/root/someplugin/addpackages.sh"

To insert a task after any other task call ``insert_task_after``. The arguments are the same.

To remove a task, call ``remove_task`` with the basename of the script as an argument.

If you want to install additional packages, simply append them to the ``packages`` variable. The ``exclude_packages`` excludes packages that would otherwise have been installed.

If you need to install init.d scripts, simply add their path to the ``init_scripts`` variable and they will be automatically installed.

You can append to an array in bash by doing this:
::

	packages+=('vim')

Other useful variables include:

* ``scriptdir``: Holds the path to the bootstrapping script folder
* ``imagedir``: The path to where the EBS volume is mounted.

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
