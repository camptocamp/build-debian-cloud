## Plugins ##
In this folder you will find plugins made for the bootstrapper.  
You can run them via the ``--plugin`` option when bootstrapping:  
```
./ec2debian-build-ami --plugin plugins/ec2-run-user-data
```

* ``standard-packages``  
  Adds some common packages to the AMI.
* ``unattended-upgrades``  
  Enables unattended upgrades with aptitude. Your EC2 server will upgrade itself daily.
* ``admin-user``  
  Creates a user named 'admin', gives it sudo rights and disables the root login.
* ``remount``  
  Remounts the bootstrapped volume.
  With this plugin you can inspect the results of the bootstrapping process without launching an instance.

## Other plugins ##
The following is a list of external plugins you can use with ec2debian-build-ami.

* [ec2-autohostname](https://github.com/secoya/ec2-autohostname)  
  Create a Route 53 CNAME record via an instance tag when booting.

* [ec2-minecraft](https://github.com/andsens/ec2-minecraft)  
  Installs [Minecraft Server Manager](http://marcuswhybrow.net/minecraft-server-manager/) by Marcus Whybrow.

That's it for now. If you have made a plugin for ec2debian-build-ami and would like to share it,
send me a pull request or drop me an email.
