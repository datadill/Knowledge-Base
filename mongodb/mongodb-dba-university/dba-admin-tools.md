# DBA Admin Tools

CLI Tools allow you to Import/Export data, restore backups, and view diagnostics

* These tools must be installed if on **Atlas** before using

1.  In the terminal, use the following command to import the public key used by the package management system:

    `wget -qO - https://www.mongodb.org/static/pgp/server-6.0.asc | sudo apt-key add -`
2.  Create a list file for MongoDB:

    `echo “deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse” | sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list`
3.  Reload the local package database:

    `sudo apt-get update`
4.  Install the latest stable version of MongoDB Community Edition:

    `sudo apt-get install -y mongodb-org`



### Backup Tools

Mongodump

* Can be used as part of a backup strategy

Key things to know..

* Can back up contents of a simple cluster
* Suited for transfer of small DBs and or collections from one cluster to another
  * Not good for complicated situations
* Creates files that are stored in a directory called "dump" in the working directory
* Can be used on standalone and replica sets, but not sharded clusters
* handles network partition poorly and does not pick up where it left off on restart
* provides limited support for PIT restores
* Does NOT contain index data leading to long restore times



For production quality backup and recovery:

* MongoDB Atlas
* MongoDB Cloud Managers
* Ops Managers

### Syntax for backup

* mongodump \<options> \<connection-string>
* When auth is enabled, user must have find priv for each db they are backing up
  * backup role provides required privs for all DBs
* Useful options include: --out, --db, --collection, --readPreference, --gzip, --archive, --oplog
* \--out
  * changes the default directory
* \--db
  * limits backup to single db
* \--collection
  * limits to single collection
* \--readPreference
  * reduces pressure on primary, but may result in stale data
* \--gzip
  * compresses data
* \--archive
  * collapse all data into a single file
* \--oplog
  * captures incoming write operations during a backup phbase
  * produces an additional bson file at the top of output folder
  * Works only when dumping an entire cluster
    * Not compatible with --db or --collection
  * Backup will fail if another client renames a collection or issues an aggregation with the $out param during the backup phase
  * Only available on Atlas M10 if using Atlas
    * If using atlas, you must specify conn string



### &#x20;Syntax for restore

mongorestore

* utility that loads data from mongopdump
* Can be used on standalone and replica sets, but not sharded clusters
* Must be version compatible between source and target clusters



