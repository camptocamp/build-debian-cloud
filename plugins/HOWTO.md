# Writing your own plugins #

If you want to change the behavior of the bootstrapper you can either modify the tasks directly or write plugins. Writing plugins has several advantages.

* You can easily include and exclude them with a command line option.
* The code is held separate from the bare bones bootstrapping setup.
* You don't need to fork this repo.
* No need to merge updates into your own repo.
* Easily share your plugins with others.

All plugins specified when bootstrapping, will be sourced *before* any tasks are run. The plugins can modify the task list and add their own tasks.
Tasks are simply paths to scripts. They will be sourced as well.
I recommend namespacing the function and variable names in a task to avoid naming clashes.

## Adding tasks ##

Adding tasks is quite easy. To have a custom task run before another, call `insert_task_before`. The first argument is one of the task variables [listed below](#task-variables). The second argument is the path to the task that is inserted.
eg.:
```
insert_task_before $TASK_INITSCRIPTS "/$plugindir/add-puppet-init.sh"
```

To insert a task after any other task call `insert_task_after`. The arguments are the same.

To remove a task, call `remove_task` with the basename of the script as an argument.
```
remove_task "40-networking"
```

### Task variables ###

The internal workings of the bootstrapper might change from time to time, this includes the filenames of the different tasks. There are however some fundamentals in the bootstrapping process that are not likely to ever change. You can hook your plugin up to those fundamentals, they are aliased in the form of variables that point to a task filename.

* `TASK_PACKAGES`: Adds packages to the `packages` array (and `exclude_packages`)
* `TASK_CREATE_VOLUME`: Creates the EBS volume
* `TASK_MOUNT_VOLUME`: Mounts the EBS volume
* `TASK_BOOTSTRAP`: Runs the bootstrapping process
* `TASK_MOUNT_SPECIALS`: Mounts things like /dev/pts and /proc
* `TASK_APT_SOURCES`: Sets the aptitude sources
* `TASK_APT_UPGRADE`: Upgrades packages and fixes broken dependencies
* `TASK_INITSCRIPTS`: Installs the init.d scripts
* `TASK_UNMOUNT_SPECIALS`: Unmounts things like /dev/pts and /proc
* `TASK_UNMOUNT_VOLUME`: Unmounts the EBS volume
* `TASK_SNAPSHOT`: Creates a snapshot of the EBS volume
* `TASK_DELETE_VOLUME`: Deletes the EBS volume
* `TASK_CREATE_AMI`: Registers the snapshot as an AMI

### Internal variables ###

If you want to install additional packages, simply append them to the `packages` variable.  
The `exclude_packages` excludes packages that would otherwise have been installed.

If you need to install init.d scripts, simply add their path to the `init_scripts` variable and they will be automatically installed.

You can append to an array in bash by doing this:
```
packages+=('vim')
```

__Other useful variables__:

* `host_packages`: Packages to be installed on the host, works just like `packages`.
* `scriptdir`: Holds the path to the bootstrapping script folder.
* `imagedir`: The path to where the EBS volume is mounted.
* `plugindir`: When adding tasks, this is the directory where the script is stored. This avoids some quirky bash magic.
* `taskdir`: When a task is run, this variable points to the directory in which the task is located.
* There are a lot of other variables, they are all declared on the first 50 lines in `ec2-debian-build-ami`

## Simple plugins ##
If your plugin is really simple, you may not need to modify the task list. The `packages`, `exclude_packages` and `init_scripts` arrays are already declared when your plugin file is sourced. Removing nano and adding vim to the bootstrap process can be done with:
```
packages+=('vim')
exclude_packages+=('nano')
```

## Utility functions ##
* `log`: Logs to the screen with blue text. Every parameter will be printed on a new line.
* `die`: Kills the bootstrapping process with a message. Prints to stderr.
* `spin`: Pipe into this function if you are running stuff that fills up the screen with verbose information. Every line of output will be printed on the same line in the console.
