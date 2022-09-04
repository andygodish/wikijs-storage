---
title: Linux Template Using Cloud Init Image
description: Guide for creating a Proxmox template using a cloud-init image. 
published: true
date: 2022-09-04T20:54:17.043Z
tags: linux, proxmox, template
editor: markdown
dateCreated: 2022-09-04T20:54:17.043Z
---

# Cloud-Init Linux Template

- [Reference Video](https://www.youtube.com/watch?v=MJgIm03Jxdo&t=1137s)

Traditionally, I would use an iso image to install onto a Proxmox VM. I stubled upon what I view as a cleaner way of creating VM templates that utilize cloud-init `.img` files instead. 

## Create the Initial VM

Follow the standard process for creating a VM in Proxmox. Well call it `ubuntu-22.04-template` and give it an id of `104` in this example. A few notable differences during this process: 

- Select `use my own media` instead of selecting an iso to boot from
- delete the default disk as this will be based on the `.img` we downnload later

Since we can change this later, allocate 1 GB of Memory and just 1 CPU. Do NOT start the machine when finished. 

### Cloud-Init Drive

Head over to the hardware section in the VM and click `add` at the top. Add a cloud-init drive and set your storage appropriately. 

Next, navigate to the cloud-init tab and fill out a default user/password/ssh key. You also want to edit the network settings and check DHCP. Do this to allow an initial IP configuration to new machines based off of this template. Even if you want the ip to be static, it's easier to do this after the initial boot configurations have been applied. 

Click `Regenerate Image`

## Proxmox Configurations

### Download Cloud-Init Image

Begin by SSHing into the proxmox server where you creating your VM. Use `wget` (should be installed by default) to download the ubuntu-22.04.img from [Ubuntu](https://cloud-images.ubuntu.com/minimal/releases/jammy/release-20220420/). In this example, I am using the 22.04 minimal server `.img` published by Ubuntu. Right click the appropriate image and selct `copy link address`. 

```
wget <pasted-link-address>
```

This will download the `.img` to your pwd. 

### Configure Proxmox Console

I'm not clear of what this actually does. The YT video I referenced mentioned that it has to do with using the Proxmox Console on VMs cloned from the template. Note the `ID` field and make sure it matches what you assigned the VM during initial configurations:

```
qm set <ID> —serial0 socket —vga serial0
```

### Give the Cloud-Init Image a Proper Extension

Unclear as to why, but you need to rename the `.img` file to a filename with a `.pcow2` extension. 

```
mv <file>.img <file>.pcow2
```

### Resize Image Disk

```
qemu-img resize <file>.pcow2 32G
```

This will result in a default disk size of 32 GB on cloned VMs. 


### Import the Image Into Proxmox

Again, take note of the VMs id as well as the local proxmox disk you are writing to. In my example, I just used `Loacl`.

```
qm importdisk <ID> <file>.qcow2 <Proxmox-Disk>
```

### Tell the VM to use the Disk

Back in the hardware section of your VM, you should now see an unused disk associated with the image you just imported. Select the disk and click `edit`, then click `add`. A couple additional steps to note about this step if your underlying disk is an SSD:

- Make sure `discard` is checked
- Click advanced, and make sure `SSD` emulation is checked

### Change the Boot Order

In the options section of the VM, click `Boot Order`. You should see your newly added disk. `Enable` it, then drag it to position number 2. This will allow you to place an iso in position #1 in the future incase you wish to override the cloud-init boot in position 2. If #1 is left blank, proxmox will move to #2 in the boot sequence. 

#### Start on Boot

I like to enable this setting on my templates. It's in this same VM seciont (Options).

## Create Your Tempate

Review and make sure your settings are correct as this process is permanent. 

Right click your VM and select `Convert to Template`.

---

Once complete, use it to make clones. Make sure you give the new VMs time to boot completely before attempting to login. If cloud-init does not create your default user, you may introduce a race condition by logging in too early. 

Once in, install the qemu-guest-agent to enjoy additional control over the VMs from the Proxmox UI. 