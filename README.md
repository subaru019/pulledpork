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

Connection Mysql Database using DBI Module
1. Changes $user and $password
2. Create database "sfsnort" sample .sql
3. Install DBI Module
    (1) # yum -y install perl-DBI perl-DBD-MySQL
    (2) # perl -MCPAN -e shell
    (3) # install Parse::Snort
    (4) # install UUID::Object
    (5) # install UUID::Generator::PurePerl
4. Enjoy

~~~~
************************************************************
DB: MySQL 5.5.5-10.3.10-MariaDB`
Database Name: sfsnort
************************************************************

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
