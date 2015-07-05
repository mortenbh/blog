# GnuPG

Last modified: 13-06-2015

## Generating a key pair

First, you must generate a (public-private) key pair. Running the following
command and using the defaults should do.

	$ gpg2 --gen-key

## Backing up your key pair

First, export your public key. You can share this publicly (hence the name).
If you have a dotfiles  repository on [Github](https://github.com, I suggest
it in there.

	$ gpg2 --export --armor > your@email-public.asc

Next, export your secret key. Be careful not transfer this over an insecure
channel. I suggest putting it on a pen drive and storing it in a secure
location. Note, if your key is protected by a passphrase (as it should be!),
the exported file will also be.

	$ gpg2 --export-secret-keys --armor > your@email-secret.asc

As an additional precaution and to avoid bit rot, you should also print your
key to paper. I suggest storing it as a QR code. Fortunately, this is easy to
do using `paperkey` (for compression) and `qrencode` (to generate the QR code).

	$ gpg2 --export-secret-keys | paperkey --output-type raw \
	| base64 | qrencode -o secret.png

We use `base64` since some QR code encoders and decoders have trouble with
binary data. You can use [ZBar](http://zbar.sourceforge.net/) to read the QR code.

	$ zbarimg --raw secret.png | base64 -d | paperkey --pubring \
	your@email-public.asc --input-type=raw > your@email-secret.gpg

At this point, you should print the QR code of your secret key onto paper.
Verify that you are able to reconstruct your private key by either
photographing or scanning the QR code and using ZBar as above. In my
experience, ZBar has trouble with excessively large resolutions (such as you
would get from modern high resolution cameras or scanners), so you may have to
resize the resulting image down to a more reasonable size (say, 800x800).
Remember to crop the image before resizing. Finally, I noticed that it can
sometimes help to rotate the image if ZBar still refuses to recognize the QR
code.

	$ convert -rotate 90 secret.png secret.png

Before you start using GnuPG, you should verify that everything went smoothing
by deleting your key pair and importing it again. The secret key contains a
copy of your public key, so it's only necessary to import that.

	$ gpg2 --delete-secret-key KEYID
	$ gpg2 --delete-key KEYID
	$ gpg2 --import your@email-secret.gpg

## Replacing ssh-agent with gpg-agent

It can be a little annoying having to maintain keys and remember passphrases
for both PGP and SSH. Luckily, GnuPG can be configured to store your SSH keys,
so you only have to remember the passphrase for your PGP key. This is
accomplished by replacing `ssh-agent` with `gpg-agent`.

The following assumes Debian/jessie. If you are using a different
distribution, things may vary. First, you must disable `ssh-agent` by
commenting the line that says `use-ssh-agent` in `/etc/X11/Xsession.options`.
Next you must tell `gpg-agent` to emulate `ssh-agent`.

	$ echo enable-ssh-support >> ~/.gnupg/gpg-agent.conf

Now `gpg-agent` will start automatically the next time you log into X. The
final step is to add your SSH key to GnuPG.

	$ ssh-add

You may want to verify that your key was added successfully.

	$ ssh-add -l
	4096 18:d8:e7:97:87:31:d7:4f:a1:0e:e5:e3:2d:98:7c:95 /home/mortenbh/.ssh/id_rsa (RSA)

You will only have to add your key one time; `gpg-agent` remembers your key
across reboots. Whenever SSH needs your key, `gpg-agent` will ask for your PGP
passphrase.
