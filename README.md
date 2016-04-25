#################################################################
#
# Program to track weekly requesters and related error statistics
#  Suspicious behavior on the web

# $Rev$ 1.0
# $Author$ SLE
# $Date$ 16/12/2015
#
#################################################################
# IPTracker
IPTracker


   _____            _             ____
  / ____|          (_)           |  _ \
 | (___  _ __  _ __ _ _ __   __ _| |_) | _____  __
  \___ \| '_ \| '__| | '_ \ / _` |  _ < / _ \ \/ /
  ____) | |_) | |  | | | | | (_| | |_) | (_) >  <
 |_____/| .__/|_|  |_|_| |_|\__, |____/ \___/_/\_\
        | |                  __/ |
        |_|                 |___/


TO INSTALL:

0) Install IPligence database (after creating "trackR" database) using MySQL:

CREATE TABLE ipligence (
	ip_from int UNSIGNED ZEROFILL NOT NULL DEFAULT '0000000000',
	ip_to int UNSIGNED ZEROFILL NOT NULL DEFAULT '0000000000',
	country_code varchar(10) NOT NULL,
	country_name varchar(255) NOT NULL,
	continent_code varchar(10) NOT NULL,
	continent_name varchar(255) NOT NULL,
	time_zone varchar(10) NOT NULL,
	region_code varchar(10) NOT NULL,
	region_name varchar(255) NOT NULL,
	owner varchar(255) NOT NULL,
	city_name varchar(255) NOT NULL,
	county_name varchar(255) NOT NULL,
	latitude double NOT NULL,
	longitude double NOT NULL,
	PRIMARY KEY( ip_to)
);

CREATE TABLE tracktb (
	ip_from varchar(30) NOT NULL,
	ip_to varchar(30) NOT NULL,
	owner varchar(50) NOT NULL,
	date_time datetime NOT NULL,
	country_code varchar(10) NOT NULL,
	country_name varchar(50) NOT NULL,
	continent_code varchar(10) NOT NULL,
	continent_name varchar(50) NOT NULL,
	region_code varchar(10) NOT NULL,
	region_name varchar(50) NOT NULL,
	city_name varchar(50) NOT NULL,
	county_name varchar(50) NOT NULL,
	latitude double NOT NULL,
	longitude double NOT NULL,
	url varchar(255),
	PRIMARY KEY( ip_from, owner, date_time)
);

CREATE TABLE tracktbs (
	ip_from varchar(30) NOT NULL,
	ip_to varchar(30) NOT NULL,
	date_time_from datetime NOT NULL,
	date_time_to datetime NOT NULL,
	status int,
	nbr_gets double,
	nbr_errors double,
	url varchar(255),
	PRIMARY KEY( ip_from)
);

Load data into this table:
$ mysql -u root -p trackR < ipligence-max.mysqldump.sql.gz

1) Install SpringBox files: Images + theme (SpringBox graphical )

2) Upload files to your Apache root directory (called here: <web_root>)

3) Chmod 'a+rx' to uploaded files and set the directory "<web_root>/trackRequesters" writable.

4) Configure cron:

@hourly cd <web_root>/trackRequesters/ && ./prepare_log.sh >>  <web_root>/trackRequesters/prepare.log 2>&1


5) Possibly, configure Apache (note that there is a local ".htaccess" which contains required secured configuration):

    Alias /trackR/ "<web_root>/trackRequesters/"

    <Directory "<web_root>/trackRequesters/">
        Options -Indexes
        AllowOverride All
        DirectoryIndex index.php
        Order allow,deny
        allow from all
    </Directory>


	
	<!--	
    <tr>
        <td><b><?php echo $ip_client; ?></b></td>
        <td><?php echo $ip_owner; ?></td>
		<td> <?php echo $IPdateTime[$ip_client]; ?></td>
        <td><a href="<?php echo $wikiUrl; ?>" target="_blank"><?php echo $city; ?></a></td>
        <td class="centerText"><img src="blank.gif" class="flag flag-<?php echo $cc; ?>" alt="<?php echo $country; ?>" title="<?php echo $country; ?>" /></td>
        <td><a href="<?php echo $wikiRegionUrl; ?>" target="_blank"><?php echo $region; ?></a></td>
        <td><?php echo $latitude; ?></td>
        <td><?php echo $longitude; ?></td>
        <td><?php echo $clientListCount[$ip_client]; ?></td>
        <td><?php echo $IPerrorCounters[$ip_client]; ?></td>
        <td><?php echo $message1[0]; ?></td>
    </tr>
-->
