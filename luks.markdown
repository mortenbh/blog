# How to a LUKS encrypted volume

Last modified: 2017-01-26

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

## Growing an existing volume

If you find that you did not make the volume big enough the first timea round,
it is fortunately very easy to grow an existing volume. First, make sure that
the volume is not mounted and closed.

	$ umount mount_point
	$ cryptsetup luksClose mapping_name

Next, you can increase the size of the encrypted volume similarly to when you
initially created it. For example, if you want to grow the 512M volume we
created above to 2G.

	$ fallocate --length 2G encrypted_file

We also need to make sure that the file system grows to fill the new space.
Before we do that, however, we need to open the LUKS volume.

	$ cryptsetup luksOpen encrypted_file mapping_name
	$ resize2fs /dev/mapper/mapping_name

And that is it! All that remains is the mount the volume as we did above.
