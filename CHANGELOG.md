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
