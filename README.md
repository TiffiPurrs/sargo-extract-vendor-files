# sargo-extract-vendor-files
These are scripts to extract missing vendor files from a factory image for AOSP

In making default AOSP, not all of the needed proprietary files are included. These are helper scripts to include the necessary changes, and were adapted from lineageOS.

Note that I made this specific to the Google Pixel 3a (Sargo), but it can be adapted for other devices.


Usage:
Follow the official guide to downloading and creating the AOSP build here:
https://source.android.com/setup/build/downloading

then here:

https://source.android.com/setup/build/building

AFTER unpacking the drivers from here:

https://developers.google.com/android/drivers

You need to download the factory image from here:

https://developers.google.com/android/images

 And extract the system image. It will be in a sparse format, so unpack it via this command:

$ simg2img system.img system.ext4

From there, you can mount the ext4 image to a directory:

$ mkdir /tmp/sargo

(as root) # mount -o ro system.ext4 /tmp/sargo

Now, you can copy everything in the "scripts" folder to the root of your source tree. (this must be done after unpacking the drivers because the unpacking of the drivers will overwite a file in {src-tree}vendor/google_devices/bonito/proprietary/, and without it, the bild process will not include the extra files) Then:

$cd  {root of source tree}/device/google/bonito/sargo/
$./extract-files.sh --sargo /tmp/sargo

This will populate {root of source tree}/vendor/google_extra/ with the make files and the proprietary files needed.

If you want to experiement with which files are needed, you need to edit scripts/device/google/bonito/sargo-proprietary-files.txt

Both of the txt files are ones that are a) known good, and b) one I took from https://github.com/gee-one/android_device_google_bonito to experiement with.

If you figure out new files that are good for me to have, feel free to submit a pull request!

One good note:

If you are experiementing with the proprietary files, you can completely compile and then use the command

$m installclean

and that only deleted the images. That way compiling it with new files only takes ~5 minutes (versus hours for a completely new build).

