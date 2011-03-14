# Postfix

Last modified: 20-08-2010 00:53:35

First install postfix with mysql map support, choosing 'Internet site' when prompted.

	$ aptitude install postfix-mysql

Basic setup...

	$ postconf -e 'myorigin = $mydomain'
	$ postconf -e 'mydestination = localhost'

	$ postconf -e 'virtual_mailbox_base = /var/mail/'
	$ postconf -e 'virtual_mailbox_domains = mysql:/etc/postfix/virtual_mailbox_domains.cf'
	$ postconf -e 'virtual_mailbox_maps = mysql:/etc/postfix/virtual_mailbox_maps.cf'
	$ postconf -e 'virtual_alias_maps = mysql:/etc/postfix/virtual_alias_maps.cf'

	$ mysql -u root -p
	mysql> CREATE DATABASE vmail;
	mysql> GRANT ALL ON vmail.* TO 'vmail'@'localhost' IDENTIFIED BY 'your_password';

	$ mysql -u vmail -p vmail < virtual_mailbox.sql

	:::MySQL
	DROP VIEW IF EXISTS `view_mailboxes`;
	DROP VIEW IF EXISTS `view_aliases`;
	DROP TABLE IF EXISTS `mailboxes`;
	DROP TABLE IF EXISTS `aliases`;
	DROP TABLE IF EXISTS `domains`;

	CREATE TABLE `domains` (
	   `id` INT(11) NOT NULL auto_increment,
	   `name` VARCHAR(50) NOT NULL,
	   PRIMARY KEY (`id`),
	   UNIQUE KEY `unique_name` (`name`)
	) ENGINE=InnoDB ;

	CREATE TABLE `mailboxes` (
	   `id` INT(11) NOT NULL auto_increment,
	   `domain_id` INT(11) NOT NULL,
	   `name` VARCHAR(40) NOT NULL,
	   `password` VARCHAR(48) NOT NULL,
	   PRIMARY KEY (`id`),
	   UNIQUE KEY `unique_email` (`domain_id`,`name`),
	   FOREIGN KEY (`domain_id`) REFERENCES `domains`(`id`) ON DELETE CASCADE
	) ENGINE=InnoDB;

	CREATE TABLE `aliases` (
	   `id` int(11) NOT NULL AUTO_INCREMENT PRIMARY KEY,
	   `domain_id` INT(11) NOT NULL,
	   `source` VARCHAR(40) NOT NULL,
	   `destination` VARCHAR(80) NOT NULL,
	   FOREIGN KEY (`domain_id`) REFERENCES domains(`id`) ON DELETE CASCADE
	) ENGINE=InnoDB;

	CREATE VIEW `view_mailboxes` AS
		SELECT CONCAT(m.name, '@', d.name) as email, m.password as password
		FROM mailboxes m, domains d
		WHERE m.domain_id = d.id;
	
	CREATE VIEW `view_aliases` AS
		SELECT CONCAT(a.source, '@', d.name) as source, a.destination as destination
		FROM aliases a, domains d
		WHERE a.domain_id = d.id;

	INSERT INTO `domains` (`name`) VALUES ('alas.dk̈́');
	INSERT INTO `mailboxes` (`domain_id`,`name`,`password`) VALUES (1,'morten','/fdJ/Wzs4sTS+KvbPqmbygDTk0dETf3b37CK4eMRaCmqlURS');
	INSERT INTO `aliases` (`domain_id`,`source`,`destination`) VALUES (1,'spicedham','morten@alas.dk');

	# dovecot lda
	dovecot   unix  -       n       n       -       -       pipe
	   flags=DRhu user=vmail:vmail argv=/usr/lib/dovecot/deliver -f ${sender} -d ${recipient}

	$ postconf -e 'virtual_transport = dovecot'
	$ adduser --system --uid 5000 --group vmail

Message (and thus attachment) size limit defaults to 10MB
	
	$ postconf -e 'message_size_limit = 51200000'


