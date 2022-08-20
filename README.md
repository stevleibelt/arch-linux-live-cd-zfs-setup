# Arch linux live cd zfs setup

This repository contains a simple, free as in freedom, bash script to enable [openzfs](https://openzfs.org/) support on your regular [archlinux.iso](https://archlinux.org/download/).

The current change log can be found [here](CHANGELOG).

See my [archzfs](https://archzfs.leibelt.de/) webpage if you want to know more.

This is a hard fork from [eoli3n/archiso-zfs](https://github.com/eoli3n/archiso-zfs). For the history, [eoli3n](https://github.com/eoli3n/archiso-zfs/pull/11) asked me kindly to do a hard fork, so I did.

## How to use

Boot your archiso and run the folloging code

```sh
curl -s https://archzfs.leibelt.de/media/arch-linux-live-cd-zfs-setup/init | bash
```

![dkms-screenshot](./screenshot.png)

### Debug

To run the script in verbose mode, use:
```
curl -s https://archzfs.leibelt.de/media/arch-linux-live-cd-zfs-setup/init | sed 's- &>/dev/null--' | bash &> debug.log
```

## Why does this repository exists?

If you want to install Archlinux on ZFS, you need to deal with the [ZFS licensing problem](https://wiki.archlinux.org/index.php/ZFS). The kernel module isn't included in the default archiso image, you need to [include it](https://wiki.archlinux.org/index.php/ZFS#Embed_the_archzfs_packages_into_an_archiso) into a custom archiso image to be able to install ZFS.

It can happen, that you need a running archiso with the latest kernel quick.   
This script lets you include the zfs kernel module on any archiso image without creating a custom one.

## What the init-script does in short

The [Archzfs](https://github.com/archzfs/archzfs/wiki) unofficial user repository offers multiple ways to install the ZFS kernel module.  
We can install precompiled module with ``zfs-linux`` package or compile the zfs module using [DKMS method](https://wiki.archlinux.org/index.php/ZFS#DKMS).  
In order to build the module, DKMS needs the ``linux-headers`` package for the running kernel.

### How does the init-script work?

It extracts running kernel version and try to find a matching ZFS module in ``Archzfs`` repositories.   
If it doesn't, it fallbacks to the DKMS build of the ZFS module.   
In that case, the script uses [Arch Linux Archive](https://wiki.archlinux.org/index.php/Arch_Linux_Archive#How_to_restore_all_packages_to_a_specific_date) to install the ``linux-headers`` and ``base-devel`` packages required for DKMS. You need at least ~6Gb RAM to use that method to be able to store packages in cowspace.   

In some very specific cases, you won't be able to get ZFS module working for a specific archiso version.  
In that case, just switch to the previous month iso.

## Developer informations

```bash
loadkeys <your keymap, e.g. de-latin1
# In pacman.conf, comment SigLevel line
# Set "SigLevel = Never"
pacman -Syy git
# In pacman.conf, restore Siglevel line
```

## Links

* [Archzfs issue : Dynamically build/load ZFS module on default archiso #337](https://github.com/archzfs/archzfs/issues/337) - 20220820
* [Archlinux Wiki : Install Arch Linux On ZFS](https://wiki.archlinux.org/index.php/Install_Arch_Linux_on_ZFS#Get_ZFS_module_on_archiso_system) - 20220820
* [eoli3n/archiso-zfs](https://github.com/eoli3n/archiso-zfs) - 20220820

