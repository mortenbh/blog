# Adding fonts in Debian

Last modified: 17-02-2011 17:00:02

If you don't have a font directory yet, make one.

	$ mkdir ~/.fonts
	$ mkfontdir ~/.fonts

Now just copy your fonts to your font directory and rebuild the font cache.

	$ fc-cache -fv

Your fonts should show up in applications once you restart them.

