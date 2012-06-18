## Plugins ##
In this folder you will find plugins made for the bootstrapper.  
You can run them via the ``--plugin`` option when bootstrapping:  
```
./ec2debian-build-ami --plugin plugins/no-ec2-run-user-data
```

* ``no-ec2-run-user-data``: Replicates the old ``-no-run-user-data`` option. This removes the ``ec-run-user-data`` script from the init scripts to be bootstrapped.

## Other plugins ##
The following is a list of external plugins you can use with ec2debian-build-ami.

* [ec2-autohostname](https://github.com/secoya/ec2-autohostname)  
  *Create a Route 53 CNAME record via an instance tag when booting.*

That's it for now. If you have made a plugin for ec2debian-build-ami and would like to share it,
send me a pull request or drop me an email.
