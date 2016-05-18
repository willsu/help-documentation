---
layout: page
title:  "Importing a VHD image that uses linux volume management for its root directory"
tags: [openstack, import VHD image, linux]
author: Jason Kennedy
dateAdded: December 11th, 2015
featured: true
---

To import a VHD image, three steps are required:

1.**Get Your VHD Image into Blue Box.** To get the image into **Blue Box** you have 2 options.

You can use the OpenStack API command line tools and run a `glance image-create` command pointing to the VHD file:

{% highlight bash %}
$ glance image-create --name "xen-template-disk1" --is-
public false --disk-format vhd --container-format bare --
file ./xen-template-disk1.vhd --progress
{% endhighlight %}

or you can log in to the Horizon console and navigate to **Project >> Images >> Create Image** to upload the image.

![Create an Image](https://cloud.githubusercontent.com/assets/17212946/15376527/15b68246-1d1b-11e6-994a-4a8e69bbc1a7.png)

2.**Create a VM flavor specific to the VHD image.** You can do so by using the `nova flavor-create` command, as shown, which in this example will create a flavor for a 8GB Memory, 65GB root partition and 2vCPU virtual machine.

{% highlight bash %}
$ nova flavor-create ML_LVM_FLVR auto 8192 65 2
{% endhighlight %}

3.**Launch the VM, based on the xen-template-disk1 VHD image.** To do so, navigate to **Project >> Compute >> Instances** and select **Launch VM** using the credentials shown in the screenshot.

![Screenshot of Launch VM] (https://cloud.githubusercontent.com/assets/17212946/15376548/2e267368-1d1b-11e6-8eab-db4662701591.png)


Afterward, you should observe that the VM has successfully launched, and that it is shown in “Running” status in the Horizon console.

![Running status](https://cloud.githubusercontent.com/assets/17212946/15376572/4e6d95de-1d1b-11e6-8f79-c1bb7fe770d5.png)


You can verify success by clicking the instance name and navigating to the console, where you will see the boot sequence for RHEL and ultimately the login prompt for the OS.

![Console view](https://cloud.githubusercontent.com/assets/17212946/15376582/68d290f0-1d1b-11e6-899c-2e9e0f07c363.png)
