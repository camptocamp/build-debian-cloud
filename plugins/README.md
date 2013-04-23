## Plugins ##
In this folder you will find plugins made for the bootstrapper.  
You can run them via the `--plugin` option when bootstrapping:  
```
./build-debian-cloud ec2 --plugin plugins/admin-user
```

* `standard-packages`  
  Adds some common packages to the AMI.
* `unattended-upgrades`  
  Enables unattended upgrades with aptitude. Your EC2 server will upgrade itself daily.
* `build-metadata`
  Adds a build metadata output file to record the AMI and snapshot IDs for further scripting.
* `admin-user`  
  Creates a user named 'admin', gives it sudo rights and disables the root login.
* `publish-ami`  
  Grants launch permission of the new AMI to everybody.
* `publish-snapshot`  
  Grants everybody permission to create a volume from the snapshot for the AMI.
* `remount`  
  Remounts the bootstrapped volume.
  With this plugin you can inspect the results of the bootstrapping process without launching an instance.

## Other plugins ##
The following is a list of external plugins you can use with build-debian-cloud.

* [ec2-autohostname](https://github.com/secoya/ec2-autohostname)  
  Create a Route 53 CNAME record via an instance tag when booting.

* [ec2-minecraft](https://github.com/andsens/ec2-minecraft)  
  Installs [Minecraft Server Manager](http://marcuswhybrow.net/minecraft-server-manager/) by Marcus Whybrow.

* [ec2debian-chef](https://github.com/tmatilai/ec2debian-chef)  
  Installs [Opscode Chef](http://www.opscode.com/chef/) client using the Omnibus installer.

That's it for now. If you have made a plugin for build-debian-cloud and would like to share it,
send me a pull request or drop me an email.
