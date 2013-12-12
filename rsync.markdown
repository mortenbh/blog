# Useful `rsync` one-liners

Last modified: 12-12-2013 18:59:59

## Copying files through an intermediate host

It often happens that you want to copy files from a host `C` to another host
`A`, but you only have access (using `ssh`) to `C` through an intermediate
host `B`. More precisely, the following connections are possible.

	A->B
	B->C (maybe also C->B, but it's not essential)

For me, this often happens when I am working on my laptop and need to access
my office PC, which is only accessible remotely through an intermediate
"proxy" server.

In this case, the following `rsync` one-liner executed on `A` has proven
useful.

	$ rsync --rsh "ssh B ssh" C:/remote/file /local/dir/

You may, of course, pass the `-r` argument to `rsync` to copy whole
directories.
