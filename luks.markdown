# How to a LUKS encrypted volume

Last modified: 2017-01-25

This was largely taken from a [DigitalOcean
tutorial](https://www.digitalocean.com/community/tutorials/how-to-use-dm-crypt-to-create-an-encrypted-volume-on-an-ubuntu-vps).

First thing, you will want to install `cryptsetup`.

	$ aptitude install cryptsetup

Next, create a file of appropriate proportions for the encrypted volume.

	$ fallocate --length 512M encrypted_file

Then, you initialize file to be LUKS format.

	$ cryptsetup -y luksFormat encrypted_file

where the `-y` options makes sure that `cryptsetup` asks for the passphrase
twice to make sure you did not mistype it.

Next, you want to open the volume.

	$ cryptsetup luksOpen encrypted_file mapping_name

With the volume open, you can now format it with your file system of
preference. In this case, I have chosen to use ext4.

	$ mkfs.ext4 /dev/mapper/mapping_name

Finally, you can mount the new file system.

	$ mount /dev/mapper/mapping_name mount_point
