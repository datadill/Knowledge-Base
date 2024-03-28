# DBA

## Connecting to MongoDB Server



Running mongosh with no params will default to 127.0.0.1:27017



Localhost Exception&#x20;

* Allows all connections from the localhost interface to have full access to that instance so that the first user can be created
  * This applies only when there are no users or roles created in the MongoDB instance
* It can only create the first user and then it is disabled

use {db\_name}

* Swtiches database context

db.createUser()

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

*   Root role gives full control over instance and ability to create other users

    * Typically you want to user least viable privileges



show users

* Show all existing users
* May fail with "command requires authentication"&#x20;
  * db.auth("dbaTestAdmin", passwordPrompt() )
    * Prompts user for password
    *

        <figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>



db.adminCommand(\<command>)

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

* The above examples are the exact same, but the 2nd command is preferred as it does not require you to switch database context
* The main purpose of this admin database is to store system collections and user authentication and authorization data, which includes the administrator and user's usernames, passwords, and roles. Access is limited to only to administrators, who have the ability to create, update, and delete users and assign roles.

MongoDB Shell (mongosh)

* Fully functional REPL environment for interacting with MongoDB deployment



## Logging Basics

db.serverCmdLineOpts().parsed.systemLog.path

* Command utility to help you find the location of the logs on disk

show logs

* displays tag names

show log global

* Displays all logs tagged as global
* By default, "show log" will default to global

MongoDB Logs are structured in JSON format

* Logs have severity levels indicated by the "I" key
  * ex. {"s":"I"} is an information log
    * verbosity must be set to 0 for informational logs and is the default level
* "c" is the category tag
* "id" is a unique identifier
* "ctx" is the name of the thread that caused the message

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

* Logs will only contain keys that they have info for

Log Rotation

* prevents log files from consuming too much disk space
* MongoDB does not automatically rotate the logs, but a system admin can do so
  * db.adminCommand ({logRotate: "server"})

