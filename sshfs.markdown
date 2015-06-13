# Using sshfs to mount remote folders

Mounting remote folder `/remote/folder` on `hostB` to local folder
`/local/folder` is done using:

	$ sshfs username@hostB:/remote/folder/ /local/folder

If `hostB` is only available by tunneling through `hostA`, the following
sequence of commands should do the trick.

	$ ssh -L1234:hostB:22 -N -f hostA
	$ sshfs -p 1234 username@localhost:/remote/folder /local/folder
