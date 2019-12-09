**make your backup image with dcfldd (better than dd because it shows the progress)**

sudo dcfldd if=/dev/mmcblk0 of=rpi_backup.img

**verify with fdisk**

sudo fdisk -l rpi_backup.img

**resize using our perl script**

sudo perl ./resizeimage.pl /path/to/file/rpi_backup.img

**use fdisk again to view size modifications. also take note of START block size on second partition (last and largest partition)**

sudo fdisk -l rpi_backup.img

**######### verification steps (optional) ###########**

**prepare first step for mounting image by using losetup**

**if "Warning: file does not fit into 512-byte sector", ignore it and move on**

sudo losetup /dev/loop0 rpi_backup.img -o $((START*512))

**if it fails to mount issues arrise then make sure /dev/loop0 is cleared**

sudo losetup -d /dev/loop0

**view partitions with gparted**

sudo gparted /dev/loop0

**mount the partitions so we can view the actual contents**

sudo mkdir /media/rpi-image

sudo mount /dev/loop0 /media/rpi-image

**view the contents and do what you like**

ls /media/rpi-image

**clean up by unmounting image and then detach with losetup**

sudo umount /media/rpi-image

sudo losetup /dev/loop0

**That's it!**

