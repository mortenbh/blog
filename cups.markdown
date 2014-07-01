# Printing in Linux using CUPS

First, install CUPS.

	$ sudo aptitude install cups

For the printers in IST Austria (as of July 2014), I needed to install
additional drivers from OpenPrinting.org. The specific print driver I needed
was for Ricoh Aficio MP C4502A. The driver is easily installed, since it
comes as a Debian package.

Next, go to `http://localhost:631`, click the administration tab and the `Add
Printer` button. You will be prompted for a (via HTTP Auth) username and
password. In my case, these were my Linux root login credentials. The next
few steps are:

* AppSocket/HP JetDirect; continue.
* socket://10.1.17.103; continue.
* Give the printer a name and description; continue.
* Ricoh Aficio MP C4502A , pxlcolor-Ricoh 20140319 (OpenPrinting LSB 3.2) (en)
* Add Printer

P.S.

In retrospect I discovered the Debian package called `openprinting-ppds`. It
is possible that you can avoid downloading the driver from OpenPrinting.org
by installing this package, though I did not try.
