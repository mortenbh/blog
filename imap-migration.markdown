# IMAP migration

Last modified: 11-08-2010 14:46:07

If recently wanted to migrate my e-mail from [Surftown](http://surftown.dk) to
my VPS at [ThrustVPS](http://thrustvps.com). I wanted to avoid doing this with
e.g. Thunderbird, since I would then be limited by my very poor upload bandwidth
of 512 kBit/s. Luckily `imapsync` came to the rescue.

`imapsync` is a Perl script that lets you synchronize IMAP mailboxes
incrementally while keeping flags such as read, unread, deleted and so on. The
script also supports TLS, so it should be able to connect to most servers. Like
all good things there is a [Debian package](http://packages.debian.org/imapsync)
for this utility too.

In my setup, I was running `imapsync` from my VPS, so the destination server was
`localhost`.

	source: mail9.surftown.com:143 with STARTTLS
	username1 / password1
	authentication: PLAIN

	destination: localhost:993 IMAPS
	username2 / password2
	authentication: PLAIN

	$ aptitude install imapsync stunnel4

	$ stunnel -c -f -d 1143 -r localhost:993 -P ''

	$ imapsync \
	  --host1 mail9.surftown.com --user1 username1 --password1 password1 --tls1 --authmech1 PLAIN \
		 --host2 localhost --port2 1143 --user2 username2 --password2 password2 --authmech2 PLAIN

*[IMAP] Internet Message Access Protocol
*[TLS] Transport Layer Security
