BBSSH
=====

An ultra-simple, experimental, probably insecure\*, probably distributable\*\*?
Bulletin Board System based on SSH and the ash shell\*.

What it is
----------

  * A client-side program, which is a shell script
  * 3 server-side programs, which are 3 shell scripts
  * A rule for authorized-keys which restricts bbs users to bbs functions
  * A way to request a new account via SSH.
  
Probably Insecure?
------------------

  * I don't really know why people don't generally do some message boards this
  way. It took me like, 30 minutes to hack the whole thing together. I'm pretty
  sure I must be missing something.
  * It may be possible, for instance, to send a message which causes the bbssh
  server to misinterpret it's own settings by changing the value of one of it's
  settings variables(Although I think I've got that under control). If that's
  possible, you can make bbssh write data to a more-or-less arbitrary location.
  * For instance, even though it only requires basic Almquist-Shell type
  functions, it will use whatever is linked to /bin/sh. On Debian-based
  systems, this will usually be dash. Some systems it'll be bash. Some systems
  it'll be mksh. Vulnerabilities in those shells will translate into
  vulnerabilities in bbssh.
  * Simple plugins are planned\*\*\*. If that's not enough said, then you need
  to study more.
  * In general, I don't bill anything I do as secure, even though I do try and
  take security into account. I don't want to create a false sense of security.
  It's a small, simple program, and I haven't been deliberately obfuscative.
  Please review the code.
  
That said, it:

  * Does not require root, does not require system-wide installation. In fact,
  running it entirely as a user outside of routing system directories is
  probably a good idea.
  * Does not have many moving parts and by it's very nature requires SSH
  key-based authentication to use.
  * Users are distinguished by their SSH identities and don't need to have
  accounts on the machine or access to any commands on the machine, except for
  bbssh-server.
  * In it's default configuration, it enforces the signing of messages with GPG
  and this is the authoritative way to identify a person and create a pseudonym
  in the bbs.
  * Anything unsigned is considered "Anonymous." One can disown previous 
  unsigned posts by publishing one's private SSH key for others to post 
  anonymously with. Combined with hidden services this could make for some
  seriously anonymous bbs's.
  
How to use it
-------------

###Client Side

####Client Configuration

On the client, bbssh is configured using variables in a shell-script based
configuration file named /etc/bbssh.conf, which loads a user configuration file
called $HOME/.bbsshrc. In normal configurations, the user configuration file
options supercede the global configuration options.

**BBSSH_SERVICE**=This is the address of the bbssh server you want to use
**BBSSH_PORT**=This is the port on that computer you want to use

####Client Usage

**bbssh -p** $MESSAGE_CONTENTS
**bbssh -r** $MESSAGE_BOARD
**bbssh -f** $SEARCH_STRING
**bbssh -l** Load without executing any functions, for including in other
scripts.

###Server Side

On the server-side, bbssh-server is the program that the client-users are
allowed to invoke. It takes an argument, and everything behind that argument is
treated as one long string.

####Server Configuration

On the server, bbssh is also configured simply using a shell-script based
configuration file, this time named /etc/bbsshserv.conf. It can load fragments
of configuration files by sourcing them from other directories.

**BBSSH_SERVER_STOR**=This is the folder where bbssh-server will create files.
**BBSSH_SERVER_PORT**=This is the port where bbssh-server will listen for
commands.
**BBSSH_SERVER_LOGS**=This is the folder where bbssh-server will log activity
and errors.
**BBSSH_LOG_SEARCH**=Make this "YES" all caps to keep track of what people
search for through your node.

####Server Usage

Just place **bbssh-server** into your path. It doesn't need to run as a daemon
or anything, sshd serves that purpose. The client sends messages to the ssh
server like

        ssh user@server bbssh-server post message
        
Which will invoke bbssh-server to post their message.

The other scripts, bbssh-cleanup and bbssh-newuser, are optional(And
bbssh-newuser isn't ready yet). bbssh-cleanup simply reads the same config files
as bbssh-server and cleans up any log files it created.

\*\*Distributing the bbssh-server Between Multiple Servers
----------------------------------------------------------

A demo plugin has been created which uses git and ssh to sync the contents of
multiple message boards on a per-topic basis. This allows you to read, post, and
discover topics and users on message boards that are "Peered" with your message
board instance.

###Peering with other Services



###Full-Syncing Mode

###Fetch-and-Push Mode

\*\*\*Creating Plugins for the bbssh-server
-------------------------------------------

###Creating your Plugin

###Verifying your Plugin

###Configuring your Plugin

