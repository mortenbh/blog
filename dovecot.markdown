# Dovecot

Last modified: 15-05-2013 21:21:53

	$ aptitude install dovecot-imapd dovecot-mysql

## Using SSL with certificate from CAcert:

Edit `/etc/dovecot/conf.d/10-ssl.conf`:

	ssl = yes
	ssl_cert = </etc/ssl/certs/alas_cert.pem
	ssl_key = </etc/ssl/private/alas_privatekey.pem


## SASL SMTP authentication with Postfix

Edit `/etc/dovecot/conf.d/10-master.conf`:

	service auth {
	  unix_listener /var/spool/postfix/private/auth {
		 mode = 0666
	  }
	}

## IMAP with MySQL backend

Edit `/etc/dovecot/conf.d/10-auth.conf`:

	#!include auth-system.conf.ext
	!include auth-sql.conf.ext

Edit `/etc/dovecot/conf.d/auth-sql.conf.ext`:

	userdb {
	  driver = static
	  args = uid=vmail gid=vmail home=/home/vmail/%d/%n
	}

Edit `/etc/dovecot/dovecot-sqlauth-sql.conf.ext`:

	driver = mysql
	connect = host=localhost dbname=vmail user=vmail password=my_password
	default_pass_scheme = SSHA256
	password_query = SELECT email AS username, password \
	FROM view_mailboxes \
	WHERE email='%u'
