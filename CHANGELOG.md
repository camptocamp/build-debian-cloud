### 2013-01-23 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* The root volume is once again set to be deleted upon termination of the instance (bug introduced in 5f04e809d7, see issue #42)
* New plugin to publish an AMI
* New plugin to publish a Snapshot

### 2013-01-21 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* Added support for wheezy
* Implemented support for additional tasks based on the debian version codename

#### Minor changes: ####
* List operations have been refactored (tasks, known_regions)
* Prevent two tasks from having the same basename
* Fixed a bug where the help message for registering an AMI was not displayed correctly

### 2013-01-17 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* Fixed a bug that prevented boto from being patched that was introduced when switching to wget in 0d96ebc83e

#### Minor changes: ####
* Check `$PIPESTATUS` to make sure the boto patch was successful
* Use sudoers.d in `admin-user` plugin (by @tmatilai)
* Install `grub-pc` instead of the `grub` dummypackage (by @tmatilai)
* Verify the AMI ID with a regex

### 2012-11-19 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* ext4 is now the default filesystem

#### Minor changes: ####
* `wget` is installed per default on debian, use that instead
* Alternative `-h` help param for packaging
* Fallback to `/usr/share/ec2debian-build-ami` when tasks dir is not found
* Disable the grub boot menu and respect some config vars in /etc/default/grub

#### Bugfixes: ####
* Fixed an issue where the bootstrapper would halt if a task ended in an if statement that failed
* The mountpoint was announced twice

### 2012-11-18 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* Add `change-root-uuid` script
  (see [this issue for more info](https://github.com/andsens/build-debian-cloud/issues/40))
* Remove contrib and non-free from apt/sources.list  
(thanks to zack for finding this and James for the patch)

#### Bugfixes: ####
* Autoapply patch to boto  
(thanks to James for the patch)
* Fix bug in `generate-ssh-hostkeys`

### 2012-11-15 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* Don't attach ephemeral volumes, they can be added in the AWS console now

#### Minor changes: ####
* Remove hardcoded location specific apt mirror and use geo redirector instead
* Fail if euca2ools make fails
* Only install euca2ools if they are not installed already or have the wrong version

### 2012-11-12 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* Replace ec2-a[mp]i-tools with euca2ools

#### Minor changes: ####
* Added regions: ap-southeast-2 (Australia)
* Changed the way `11-attach-volume` finds a free device letter

### 2012-11-10 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* Plugin removed: "ec2-run-user-data" is back in the standard bootstrapping process and has been simplified
* New init.d script "expand-volume": Try to grow the root volume size when booting

#### Minor changes: ####
* Rename "ec2-ssh-host-keys-gen" to "generate-ssh-hostkeys" and refactor
* Plugin modified: "ec2-user" renamed to "admin-user". Login name is now "admin"
* Fix help message on AMI registration failure

### 2012-11-06 Anders Ingemann <anders@ingemann.de> ###

#### Major changes: ####
* New plugin: "ec2-user". It creates a normal unix user. root ssh login will be disabled, with this plugin. Fixes #31
* New plugin: "remount". It remounts the bootstrapped volume
* Rename task variables to fit common scheme
* Split up tasks, to get better granularity
* Refactor of ec2-get-credentials
* Update PV-Grub AKI IDs, reorder some regions and add us-gov-west-1

#### Minor changes: ####
* Add Apache Software License 2.0 Include the license for ec2ubuntu, the original script
* Create /etc/timezone with cat
* Append to /etc/network/interfaces, don't overwrite
* The /tmp dir isn't missing, don't create it
* Append to fstab, don't overwrite
* Make `cat` commands more readable
* Clean up hostkeys. Simplifies ec2-ssh-host-key-gen a LOT.
* Don't delete root password, it's not set to begin with.
* Use printf instead of echo
* When bootstrapping fails jump to $originaldir instead of $scriptdir
* Don't overwrite /etc/hosts, the original works just fine.
* Log when a task is removed
* Introduce logplain, logging without color
* Move networking into 30s group
* Format standard-packages the same way as the other plugins.
* Fixed bug in task functions. No error was displayed when removing or adding a non-existent task.
* Minor fixes to utils
