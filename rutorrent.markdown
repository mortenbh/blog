# Installing rtorrent and ruTorrent web interface

Last modified: 18-02-2011 23:11:02

	$ aptitude install rtorrent libapache2-mod-scgi
	$ a2enmod scgi
	
Add to `/etc/apache2/sites-available/000-default`:

	SCGIMount /RPC2 127.0.0.1:5000

Add to `~/.rtorrent.rc`:

	scgi_port = localhost:5000

Some svn checkout stuff for rutorrent, chown and chmod(?) www-data:www-data.

## Security

Basically there is no security right now. We need to fix this obviously.
TODO!
