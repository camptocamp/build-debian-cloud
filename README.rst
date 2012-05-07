debian "squeeze" bootstrapping script for EC2
=============================================

This is a fork of camptocamps bootstrapping script for EC2 AMIs.

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

Environment script
------------------
Include with `source env-script` for the variables to be present on the commandline.
::

	export EC2_URL='https://ec2.eu-west-1.amazonaws.com'
	export EC2_HOME="/root/ec2/ec2-api-tools-1.5.2.3"
	export EC2_AMITOOL_HOME="/root/ec2/ec2-ami-tools-1.4.0.5"
	export EC2_PRIVATE_KEY="/root/root.key"
	export EC2_CERT="/root/root.crt"
	export EC2_ACCNO='1234-4567-8910'
	export EC2_ACCESS_KEY='SOM3L0NG4CC3SSK3Y000'
	export EC2_SECRET_KEY='SomBase64EncodedString'
	export AWS_USER_ID="$EC2_ACCNO"
	export AWS_ACCESS_KEY_ID="$EC2_ACCESS_KEY"
	export AWS_SECRET_ACCESS_KEY="$EC2_SECRET_KEY"
	export AWS_SECRET_ACCESS_KEY_ID="$EC2_SECRET_KEY"
	export PATH="$PATH:${EC2_HOME}/bin:${EC2_AMITOOL_HOME}/bin"

If you are using IAM to access AWS you may need to create the certificate first. You can use `this gist <https://gist.github.com/2629062>`_ for that.