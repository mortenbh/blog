# Postfix SPF

Last modified: 20-08-2010 00:53:35

This assumes a working postfix installation. First install the necessary packages

	$ aptitude install postfix-policyd-spf-python

Add the following to `/etc/postfix/master.cf`:
	
	policyd-spf  unix  -       n       n       -       0       spawn
   	user=nobody argv=/usr/bin/python /usr/bin/policyd-spf /etc/postfix-policyd-spf-python/policyd-spf.conf

Then run these commands:

	$ postconf -e 'smtpd_recipient_restrictions = permit_mynetworks, reject_unauth_destination, check_policy_service unix:private/policyd-spf'
	$ postconf -e 'policyd-spf_time_limit = 3600̈́'

Note, it is very important that `check_policy_service` comes *after* `reject_unauth_destination`,
or your server might become an open relay.
