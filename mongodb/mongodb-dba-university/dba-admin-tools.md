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



