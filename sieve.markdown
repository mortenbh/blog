# Updating your sieve script with sieve-connect

Last modified: 17-02-2011 16:33:46

If you haven't already, install `sieve-connect`.

	$ aptitude install sieve-connect

Now connect to the server and input your password when prompted.

	$ sieve-connect -s hostname -p port -u username

I use the following arguments.

	$ sieve-connect -s mail.alas.dk -p 2000 -u morten@alas.dk

When connected you use `list` to show the available sieve scripts, `download`
and `upload` respectively to download and upload (possibly overwriting) a
script and `activate` and `deactivate` to disable and enable specific scripts.

