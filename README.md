pulledpork
==========

PulledPork for Snort and Suricata rule management (from Google code)

Find us on Freenode (IRC) [`#ppork`](https://webchat.freenode.net/?channels=ppork)

Copyright (C) 2009-2017 JJ Cummings, Michael Shirk and the PulledPork Team!


Thank you for choosing to use PulledPork!  This file provides some basic
guidance on the usage of PulledPork.  Please be sure to read this file
thoroughly so that you don't overlook something!

## My Changes
1. Modified Rules Detection
2. Rule Text Parsing and save to Mysql Database ( Detecting Rules )
3. =)

### Connection Mysql Database using DBI Module
1. Change $password strings, pulledpork.pl LINE 1492 (mysql_connect)
2. Create Database "sfsnort" from Sample_sfsnort.sql
3. Install DBI Module and Other Modules
~~~~
# yum -y install perl-DBI perl-DBD-MySQL
# cd ~/
# wget https://cpan.metacpan.org/authors/id/B/BA/BANB/UUID-Object-0.81.tar.gz
# wget https://cpan.metacpan.org/authors/id/B/BA/BANB/UUID-Generator-PurePerl-0.05.tar.gz
# wget https://cpan.metacpan.org/authors/id/R/RH/RHARMAN/Parse-Snort-0.9.tar.gz
# tar xvzf UUID-Object-0.81.tar.gz
# cd UUID-Object-0.81
# perl Makefile.PL
# make && make install
# cd ~/
# tar xvzf UUID-Generator-PurePerl-0.05.tar.gz
# cd UUID-Generator-PurePerl-0.05
# perl Makefile.PL
# make && make install
# cd ~/
# tar xvzf Parse-Snort-0.9.tar.gz
# cd Parse-Snort-0.9
# perl Makefile.PL
# make && make install
# cd ~/
~~~~
4. Enjoy


~~~~
************************************************************
OS: CentOS Linux release 7.5.1804 (Core)
Snort:    
   ,,_     -*> Snort! <*-
  o"  )~   Version 2.9.11 GRE (Build 125)
   ''''    By Martin Roesch & The Snort Team: http://www.snort.org/contact#team
           Copyright (C) 2014-2017 Cisco and/or its affiliates. All rights reserved.
           Copyright (C) 1998-2013 Sourcefire, Inc., et al.
           Using libpcap version 1.5.3
           Using PCRE version: 8.32 2012-11-30
           Using ZLIB version: 1.2.7
Perl:
    This is perl 5, version 16, subversion 3 (v5.16.3) built for x86_64-linux-thread-multi
    (with 33 registered patches, see perl -V for more detail)
    Copyright 1987-2012, Larry Wall
    Perl may be copied only under the terms of either the Artistic License or the
    GNU General Public License, which may be found in the Perl 5 source kit.
    Complete documentation for Perl, including FAQ lists, should be found on
    this system using "man perl" or "perldoc perl".  If you have access to the
    Internet, point your browser at http://www.perl.org/, the Perl Home Page.
DB: MySQL 5.5.5-10.3.10-MariaDB
Database Name: sfsnort
************************************************************

Sample_sfsnort.sql

Table Dump rule_header
------------------------------------------------------------


DROP TABLE IF EXISTS `rule_header`;

CREATE TABLE `rule_header` (
  `action` varchar(64) DEFAULT NULL,
  `proto` varchar(10) DEFAULT NULL,
  `sip` text DEFAULT NULL,
  `sport` varchar(64) DEFAULT NULL,
  `dip` text DEFAULT NULL,
  `dport` varchar(64) DEFAULT NULL,
  `diroperator` varchar(64) DEFAULT NULL,
  `category` varchar(64) NOT NULL DEFAULT '',
  `gid` int(11) NOT NULL DEFAULT 1,
  `sid` int(11) NOT NULL DEFAULT 0,
  `revision` int(11) NOT NULL DEFAULT 0,
  `timestamp` timestamp NOT NULL DEFAULT current_timestamp(),
  `last_changed` int(10) unsigned NOT NULL DEFAULT 0,
  `uuid` varchar(37) NOT NULL DEFAULT '',
  `msg` text DEFAULT NULL,
  `rule_text` text DEFAULT NULL,
  `deleted` tinyint(4) NOT NULL DEFAULT 0,
  PRIMARY KEY (`uuid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



# Table Dump rule_opts
# ------------------------------------------------------------

DROP TABLE IF EXISTS `rule_opts`;

CREATE TABLE `rule_opts` (
  `gid` int(11) NOT NULL DEFAULT 1,
  `sid` int(11) NOT NULL DEFAULT 0,
  `revision` int(11) NOT NULL DEFAULT 0,
  `options` varchar(64) DEFAULT NULL,
  `args` text DEFAULT NULL,
  `timestamp` timestamp NOT NULL DEFAULT current_timestamp(),
  `ord` int(10) unsigned NOT NULL DEFAULT 0,
  `last_changed` int(10) unsigned NOT NULL DEFAULT 0,
  `uuid` varchar(37) NOT NULL DEFAULT '',
  `deleted` tinyint(4) NOT NULL DEFAULT 0,
  PRIMARY KEY (`ord`,`uuid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



# Table Dump SRU_import_log
# ------------------------------------------------------------

DROP TABLE IF EXISTS `SRU_import_log`;

CREATE TABLE `SRU_import_log` (
  `time` bigint(20) DEFAULT NULL,
  `type` varchar(255) DEFAULT NULL,
  `name` varchar(255) DEFAULT NULL,
  `gid` int(11) DEFAULT NULL,
  `sid` int(11) DEFAULT NULL,
  `rev` int(11) DEFAULT NULL,
  `SRU_uuid` varchar(36) DEFAULT NULL,
  `rule_text` text DEFAULT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



# Table Dump SRU_index
# ------------------------------------------------------------

DROP TABLE IF EXISTS `SRU_index`;

CREATE TABLE `SRU_index` (
  `time` bigint(20) DEFAULT NULL,
  `name` varchar(255) DEFAULT NULL,
  `SRU_uuid` varchar(36) DEFAULT NULL,
  `finished` int(11) DEFAULT 0
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

~~~~

## Appendix
~~~~
# mysql -u root -p
 ENTER: password(root)
> CREATE DATABASE sfsnort;
> grant all privileges on sfsnort.* to root@localhost identified by '********';
> flush privileges;
> exit;
# mysql -u root -p sfsnort < Sample_sfsnort.sql
 ENTER: password(root)
# mkdir -p /var/log/snort/
# mkdir -p /var/log/snort/rules/
# chmod a+x /usr/local/bin/pulledpork/pulledpork.pl
~~~~

### Standard Processing
~~~~
# /usr/local/bin/pulledpork/pulledpork.pl -c /etc/snort/pulledpork.conf -h /var/log/snort/sid_changes.log.`date "+%Y%m%d-%H%M%S"`
~~~~

### Test Processing (TALOS SNORT.ORG rules not being updated)
~~~~
1. Download Only (Using -g Option) Not Process
# /usr/local/bin/pulledpork/pulledpork.pl -c /etc/snort/pulledpork.conf -h /var/log/snort/sid_changes.log.`date "+%Y%m%d-%H%M%S"` -g
2. Testing New Rule Detection
# vi /etc/snort/snort.rules
 Delete lines roughly (ex. dd)
3. Processing Only (Using -P Option)
# /usr/local/bin/pulledpork/pulledpork.pl -c /etc/snort/pulledpork.conf -h /var/log/snort/sid_changes.log.`date "+%Y%m%d-%H%M%S"` -P
~~~~

Please notify me know if you know of any better method than the mentioned above.
