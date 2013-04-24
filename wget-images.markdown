# How to download every image in a folder using wget

Last modified: 24-04-2013 11:12:16

It is often useful to download every image in a folder exposed e.g. as an
Apache index. The following `wget` command has proven useful for this purpose:

	$ wget -b -r -np -nd -A png http://myserver/myfolder/

A quick explanation of the command-line switches:

* The `-b` switch runs `wget` in the background.
* The `-r` switch turns on recursion, while the `-np` switch avoids following
  links to parent folders.
* The `-nd` switch makes wget dump everything into one directory, without
  replicating the directory hierarchy on the server.a
* The `-A` switch is a comma-separated list of accepted file suffixes.
