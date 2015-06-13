# GnuPG

Last modified: 13-06-2015

## Generating a key pair

First, you must generate a (public-private) key pair. Running the following
command and using the defaults should do.

	$ gpg2 --gen-key

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
