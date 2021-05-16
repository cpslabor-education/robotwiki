# Create images (presumably for backup)

## Create an image of the SDCard
Insert the SDCard, then enter the following instruction (the device and the output folder might change, on my computer the sdcard was identified as _/dev/mmcblk0_):
```
sudo dd bs=4M if=/dev/mmcblk0 of=/home/username/archive_folder/arhcive_image.img
```