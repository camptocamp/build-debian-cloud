## List of plugins ##
This is a list of plugins you can use with ec2debian-build-ami.
You can specify them via the ``--plugin`` option when bootstrapping like this  
```
ec2debian-build-ami/ec2debian-build-ami --plugin ec2-autohostname/bootstrap-ec2-autohostname
```

* [ec2-autohostname](https://github.com/secoya/ec2-autohostname)  
  *Create a Route 53 CNAME record via an instance tag when booting.*
