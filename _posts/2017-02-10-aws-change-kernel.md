---
layout:     post
title:      Changing kernel of a AWS CentOS 7 instance
date:       2014-06-09 12:32:18
summary:    Notes on C++
categories: jekyll pixyll
---

* spawn the base image of Centos 7 HVM :
  aws ec2 run-instances --image-id ami-d2c924b2 \
        --key-name pankaj-ec2-key --instance-type t2.micro\
        --block-device-mapping '[{ "DeviceName" : "/dev/sda1","Ebs" : { "DeleteOnTermination": true,   "VolumeSize" : 15 }}]'\
        --placement AvailabilityZone=us-west-2b\
            --security-groups DataCabinetSSHGroup default 

* login to the VM and download the better kernel from epel repo
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum remove kernel-headers kernel-tools kernel-tools-libs
yum --enablerepo=elrepo-kernel install kernel-ml
grep -P "submenu|^menuentry" /boot/grub2/grub.cfg  | cut -d "'" -f2 | grep elrepo | xargs -d "\n" grub2-set-default
grub2-mkconfig -o /boot/grub2/grub.cfg

#3 Reboot and make sure that the new kernel is getting used
 [centos@ip-172-31-26-71 ~]$ uname -a
    Linux ip-172-31-26-71.us-west-2.compute.internal 4.9.8-1.el7.elrepo.x86_64 #1 SMP Sat Feb 4 11:47:44 EST 2017 x86_64 x86_64 x86_64 GNU/Linux


#4 Stop the instance
 aws ec2 stop-instances --instance-ids $instance_id
 aws ec2 wait instance-stopped --instance-ids $instance_id

This step is necessary otherwise aws screws up the grub2 configuration when imaging live instance

#5 Take image
image_id=`aws ec2 create-image --instance-id $instance_id --name $1 |  grep ImageId | sed 's/\".*\"://' |  tr -d '[:space:]' | sed -e 's/^"//' -e 's/"$//'`
aws ec2 wait image-available --image-id $image_id



