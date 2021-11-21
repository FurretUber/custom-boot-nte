**custom-boot-nte**

Custom boot parameters, adequate for live systems, even from already installed systems. Intended to work similarly to *casper*, but without applying changes to the base system, as adding a passwordless superuser or changing polkit.

With this project, any system can be a live system, even an already installed system.

I recognize I tested this script only with Ubuntu. It was tested with Ubuntu 18.04, 20.04 and 22.04 (development branch) 😅️

To use it on the startup, the parameter `boot=custom-boot-nte` has to be added, and the kernel module `overlay` must be enabled. If there are doubts if the configuration was applied, add `nte=1` to the Linux command line. If everything goes right, the following art should appear:
```
#######################################################################
#######################################################################
#######################################################################
##*    `##########     $##%                    %##:                  ,#
##*     `%########,    $##%                    %##:                  ,#
##*       *#######,    $##%,,,,,,,,    ,,,,,,,,%##:    ,,,,,,,,,,,,,,+#
##*        :######,    $##########-    -##########:    ,###############
##*         ,$####,    $##########-    -##########:    ,###############
##*    :-    .$###,    $##########-    -##########:                 :##
##*    :#:     %##,    $##########-    -##########:                 :##
##*    :##*     +#,    $##########-    -##########:     ,,,,,,,,,,,,:##
##*    :###%`    -,    $##########-    -##########:    ,###############
##*    :####$,         $##########-    -##########:    ,###############
##*    :######:        $##########-    -##########:    ,###############
##*    :#######+       $##########-    -##########:     ``````````````$
##*    :########%`     $##########-    -##########:                   $
##*    :#########$     $##########-    -##########:                   $
#######################################################################
#######################################################################
#######################################################################
```
As this was imagined to expand from *casper*, the custom-boot-nte also supports *squashfs* files. Here are two valid examples of the root parameter for the startup:

From a partition:
```
boot=custom-boot-nte root=/dev/sda2 rootfstype=ext4
```
From a file:
```
boot=custom-boot-nte root=UUID=ABCD-1234/oSistema.squashfs rootfstype=squashfs
```
Note that the UUID `ABCD-1234` would be, for example, a FAT32 partition containing the file `oSistema.squashfs`.

When booting the system, the `root` will appear as `/mnt/ro-fs`, but there are two possibilities, depending whether it is a partition or a file:
- If it is a partition, the mount point of the root will be `/mnt/ro-fs`;
- If it is a file, the file will be at `/mnt/ro-fs` and the partition containing the file will be mounted at `/mnt/contem-arq-root`

Notes:
- As this is a live system using overlay, it will end using RAM memory for the temporary storage. This storage was specified to be 80% of the memory, which seemed suitable: it was tested with systems using Xfce desktop 4.12, 4.14 and 4.16 which have between 1 and 20 GB of RAM memory;
- If `/etc/fstab` lists partitions beyond the root (for example, `/home`), **these partitions will be mounted as specified (maybe READ-WRITE!!!)**;
- I did not test, but if there are places beyond `/etc/fstab` that define the root as read-write (who knows...), maybe the read-only mode won't work.

Based on the file `/usr/share/initramfs-tools/scripts/local` available on Ubuntu 20.04. Also based on `casper`.