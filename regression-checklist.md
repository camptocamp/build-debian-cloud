# Regression checklist #
This is a checklist, to keep a record of subtle bugs in the AMI that can be hard to spot.  
Before publishing any AMI, this list should be consulted first.

*Note that the bootstrapper does all the listed things for you. However, regressions are always a possibility.*

* EBS root volume, should be set to "Delete on Termination"
* The system log on instance startup should contain no error other than "hostname: the specified hostname is invalid"
* The AWS credentials should not be obtainable from the root volume (see below for instructions)
* The ssh hostkeys of the bootstrapping machine are copied into the image by debootstrap  
  to remove them properly they have to be shredded with `shred --remove FILENAME`
* There should be no `.*_history` files in any home directories
* The default locale should be generated
* A timezone should be set
* The aptitude sources list should at least contain a security update mirror and a main repository mirror
* There should be no broken dependencies or un-upgraded packages
* eth0 should be configured
* The aptitude package cache should be deleted
* The motd contains a reference to the host (debootstrap put it there), that should be removed



To check a volume for any sensitive data run the following commands:

```
apt-get update
apt-get install binutils
strings /dev/xvda1 | grep -A10 -B10 'AWS_|EC2_'
```
