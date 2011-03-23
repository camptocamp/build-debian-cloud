Latest Debian 6.0.1 "squeeze" AMIs provided by Camptocamp
=========================================================

+---------------------------------+----------------------------------+--------------------------------+
| **Region / Location**           | Server 32-bits (instance-store)  | Server 64-bit (instance-store) | 
+---------------------------------+----------------------------------+--------------------------------+
| us-east-1 / US                  | **ami-aa46b4c3**                 | **ami-de46b4b7**               |
+---------------------------------+----------------------------------+--------------------------------+
| us-west-1 / us-west-1           | **ami-a30655e6**                 | **ami-a70655e2**               |
+---------------------------------+----------------------------------+--------------------------------+
| eu-west-1 / EU                  | **ami-17447363**                 | **ami-2b44735f**               |
+---------------------------------+----------------------------------+--------------------------------+
| ap-southeast-1 / ap-southeast-1 | **ami-68c4ba3a**                 | **ami-6ac4ba38**               |
+---------------------------------+----------------------------------+--------------------------------+
| ap-northeast-1 / ap-northeast-1 | **ami-a0e44ea1**                 | **ami-9ee44e9f**               |
+---------------------------------+----------------------------------+--------------------------------+

ec2debian-build-ami
===================

This script builds, bundles, and uploads an Debian AMI for
Amazon EC2.

This script is a fork of ec2ubuntu-build-ami available here:
http://ec2ubuntu.googlecode.com/svn/trunk/bin/ec2ubuntu-build-ami


create-ec2-boot-bundle
======================

This script creates a bundle from the running system with the kernel,
the grub config and the kernel modules.


init.d/
=======

These scripts are forked from http://code.google.com/p/ec2ubuntu/ and
adapted for debian squeeze (mostly made compatible with insserv).
