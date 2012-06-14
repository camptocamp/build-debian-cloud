debian "squeeze" bootstrapping script for EC2
=============================================

This is a fork of camptocamps bootstrapping script for EC2 AMIs.
It creates a vanilla debian squeeze machine image, no latent logfiles no .bash_history or even apt package cache.
The machine configuration this script creates has been thoroughly tested.

*This script is only tested on debian squeeze.*
*You will need an EC2 server to run this bootstrapper.*

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
(Defaults are in **bold**)

Bootstrapping options
"""""""""""""""""""""
--arch ARCHITECTURE
	Processor architecture of the image [ i386 | **amd64** ]
--filesystem FSTYPE
	The filesystem of the root volume [ext2 | ext3 | ext4 | **xfs** ]
--volume-size VOLSIZE
	The default size of the root volume in GB (**1**)
--plugin FILE
	Path to plugin script. Can be specified more than once.

--timezone ZONE
	Standard timezone (**UTC**)
--locale LOCALE
	Standard locale (**en_US**)
--charmap CHARMAP
	Standard charmap (**UTF-8**)

--imagedir DIR
	Bootstrap directory (**/mnt/image**)

AWS options
"""""""""""
--user ID
	User ID (**AWS_USER_ID**)
--access-key ID
	Access key ID (**AWS_ACCESS_KEY_ID**)
--secret-key KEY
	Secret access key (**AWS_SECRET_ACCESS_KEY_ID**)
--cert PATH
	Certificate (**EC2_CERT**)
--private-key PATH
	Certificate private key (**EC2_PRIVATE_KEY**)

Deprecated options
"""""""""""""""""""
--script FILE
	External script/command to run before bundle
--package NAME
	Additional package to install
--no-run-user-data
	Do not run user-data script on first boot

Other options
"""""""""""""
--debug
	Use -x option in bash
--help
	Show a list of options

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

Adding tasks is quite easy. To have a custom task run before another, call ``insert_task_before``. The first argument is one of the task variables listed below. The second argument is the path to the task that is inserted.
eg.:
::

	insert_task_before $TASK_INITSCRIPTS "/root/someplugin/add-puppet-init.sh"

To insert a task after any other task call ``insert_task_after``. The arguments are the same.

The task variables are:

* ``TASK_PACKAGES``: Adds packages to the ``packages`` array (and ``exclude_packages``)
* ``TASK_VOLUME``: Creates the EBS volume
* ``TASK_BOOTSTRAP``: Runs the bootstrapping process
* ``TASK_MOUNT``: Mounts things like /dev/pts and /proc
* ``TASK_APTSOURCES``: Sets the aptitude sources
* ``TASK_INITSCRIPTS``: Installs the init.d scripts
* ``TASK_UNMOUNT``: Unmounts the EBS volume
* ``TASK_SNAPSHOT``: Creates a snapshot of the EBS volume
* ``TASK_CREATEAMI``: Registers the snapshot as an AMI

To remove a task, call ``remove_task`` with the basename of the script as an argument.
::

	remove_task "40-networking"

If you want to install additional packages, simply append them to the ``packages`` variable. The ``exclude_packages`` excludes packages that would otherwise have been installed.

If you need to install init.d scripts, simply add their path to the ``init_scripts`` variable and they will be automatically installed.

You can append to an array in bash by doing this:
::

	packages+=('vim')

Other useful variables:

* ``scriptdir``: Holds the path to the bootstrapping script folder
* ``imagedir``: The path to where the EBS volume is mounted.
* ``plugindir``: When adding tasks, this is the directory where the script is stored. This avoids some quirky bash magic.
* There are a lot of other variables, they are all declared on the first 50 lines in ``ec2-debian-build-ami``

Simple plugins
""""""""""""""
If your plugin is really simple, you may not need to modify the task list. The ``packages``, ``excluded_packages`` and ``init_scripts`` arrays are already declared when your plugin file is sourced. Removing nano and adding vim to the bootstrap process can be done with:
::

	packages+=('vim')
	excluded_packages+=('nano')

Utility functions
"""""""""""""""""
* ``log``: Logs to the screen with blue text. Every parameter will be printed on a new line.
* ``die``: Kills the bootstrapping process with a message. Prints to stderr.
* ``spin``: Pipe into this function if you are running stuff that fills up the screen with verbose information. Every line of output will be printed on the same line in the console.

external-scripts task
"""""""""""""""""""""
The external-scripts task, which can be used with the ``--script`` parameter is deprecated in favor of the plugin system.

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
