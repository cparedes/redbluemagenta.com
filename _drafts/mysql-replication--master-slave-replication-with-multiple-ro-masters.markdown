---
layout: post
title: MySQL Replication - master slave replication with multiple R/O masters
categories:
- Systems Administration
- Databases
- MySQL
---

So, after battling MySQL for a couple of days, I've come out with a bit more
experience with replication and a few pitfalls that I've encountered that you
may encounter as well.

The MySQL installation I was wrestling with consisted of a single R/W master,
a couple of R/O slaves, a R/O slave that also acted as an R/O master, and
several R/O slave machines slaving off of the R/O master.

Apologies for the ASCII art:

<pre><code>
                 * - R/W Master
                 |
                /|\
               / | \
 R/O slave  - *  *  * - R/O slave
                 |\_ R/O slave/master
                 |
                /|\
               * * * - R/O slaves
</code></pre>

We assume that the R/W master already has a bunch of data, and that we need
to copy all of the data over to each machine.

The biggest thing to keep in mind is that it's REALLY REALLY easy to mess
this up - if your replication settings are off by just a little bit on
the slave machines, then you're going to corrupt your data on the slave
machines.

Also, when we do the data copy to each machine, we will want to wipe out any
relay logs, any master binary logs, the master.info file, and the
slave-relay-bin.info file on the slave machines.  We'll recreate the
master.info file through the MySQL CLI.

So, without further ado, here's how you set everything up:

Step 1: Create a backup and ship it off to all of the slave machines.
---------------------------------------------------------------------

You'll have to make sure that either an existing slave server, or your master
server, either has a lock on all tables (for mysqldump), or the daemon is
switched off completely (for raw data backup with tar.)  In either case,
you'll need to make sure you can read all of the data without it being written
to at the same time.

In my particular case, I've shut off the daemon and grabbed a snapshot using
tar, since I had a spare slave server with an up to date copy of the data.
I streamed the compressed tar data over SSH to the slave servers:

<pre><code>cd /var/lib ; tar -czf - mysql | ssh <host> "tar -C /destination/folder -zxvf -" </code></pre>

Depending on how large your database is, it may or may not be popcorn time. :)

Step 2: Remove existing relay logs, master.info, etc.
-----------------------------------------------------

By default, these files are stored in /var/lib/mysql.

Remove all of them:

<pre><code>cd /var/lib/mysql ; rm mysql-bin* slave-relay-bin* relay-log* mysqld-relay-bin* master.info</code></pre>

Step 3: Setup my.cnf on the master and all slave machines.
----------------------------------------------------------

Okay, so I know this is pedantic, but we know for sure how to identify
different machines from each other in some way or another, either by an
IP address, a hostname, or some other metadata.  MySQL uses an internal
identification scheme for identifying different machines in an MySQL
replication environment.  You can define this ID under the [mysqld] section
in my.cnf (usually located in /etc/mysql/my.cnf), using the configuration
option "server-id".  These must be unique for every machine in the entire
set of machines that's participating as either a master or a slave in
the replication setup.

I usually start my numbering from the master, and work my way down.  For
instance, I'll number my R/W master as server-id = 1, my R/O master as
server-id = 2, one of the slaves as server-id = 3, etc.

