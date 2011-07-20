# How to open `winfile.dat` e-mail attachments

Last modified: 20-07-2011 18:29:50

The `winfile.dat` attachment you sometimes get in e-mails is a proprietary
e-mail attachment format used in Microsoft Outlook. The format goes by the
name Transport Neutral Encapsulation Format (TNEF) and can be extracted by a
program by the same name on Linux.

	$ aptitude install tnef

To extract the contents of a TNEF file, simply run:

	$ tnef winfile.dat

This will extract the content of the TNEF archine into the current directory,
so you might want to create a folder first.
