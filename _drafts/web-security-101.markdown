---
layout: post
title: Web Security 101
categories:
- System Administration
---

A friend of mine recently asked me what sort of security issues she should
keep in mind when she's designing web pages and uploading them to a web server.

Note that I'm differentiating between a web designer and a web developer, the
latter of which would mostly work with writing code in some sort of
language/framework that's meant to be used in a web application, such as
Javascript, Ruby, Python, Java, etc.  This article is meant for web designers,
who usually mock up UI prototypes and write the presentational stuff for a
given website.

The short answer is, "not too much."  If you're only designing static pages
in HTML and CSS, there's really not too much you'd have to be worried about -
the language, for instance, dictates only structure, content, and presentation,
and doesn't touch upon anything that would interact with the system in any
significant way.  The only scenario that I could think of that could really
screw with a web designer is if you're writing up a web form that talks with
a database, or if you're writing up a web form that transmits passwords over
the internet, to which you have to worry about sanitizing your inputs for
any strings that may try to execute an arbitrary SQL statement, or making
sure that the web form is strictly communicating over SSL.

The long answer is, "well, sure, there's a few.  You can't be too cautious."
The biggest issue that could arise is improper file permissions on your files.
I'll go ahead and enumerate each issue under a different header and explain
a few of the basic things that one needs to keep in mind when uploading stuff
to a server, and writing up static web pages.

Preliminary Concepts
--------------------

First, we'll talk about what happens when someone types in "google.com" in
their web browser.

Each machine on a network has an address called an "IP address" - this is
roughly equivalent to someone's mailing address.  There's also an address
called a "MAC address", which is roughly equivalent to someone's SSN
(someone can move to different houses, but that person is still the same
person.)  We contact other machines in other networks by using their IP
address - the IP address uniquely identifies a device's location, and also,
according to the numbering scheme, tells devices in between you and the
end point *where to go* to send the information to the other end.

Now, IP addresses are mostly for machines to process and figure out exactly
where to route packets across the internet.  For humans, however, it's
not exactly readable nor memorable - I'd rather remember something like
"google" than "74.125.53.103."

This is where "domain name servers" (DNS) comes in; they are servers that
tell other machines what's mapped between human readable names and IP
addresses.  Every time I use a name like "google.com" in any application,
I implicitly contacting my ISP's DNS servers to ask which IP address
"google.com" maps to.

(You might be asking, "why did he go over all of this?"  When you want to
have your website be accessible with a human readable URL, you can't just
upload your stuff to a web server and call it good - you'll also have to
sign up for a domain name, and explicitly set up the "binding" between the
IP address and your domain name for your website.  The web server contains
all of the HTML/CSS stuff, and the DNS server only contains information
on how to map your IP address for the web server to a human readable name,
like "foobar.com.")

File Permissions
----------------

Most web servers these days are hosted on a UNIX platform; there's a small
number that are hosted off of Windows, but more likely than not, when you're
signing up for a web server at a random company, it'll be on a UNIX system.

Therefore, you'll need to learn a few basics of UNIX permissions, at least
enough to know what to do.

There's three groups of people that UNIX recognizes for permissions:
"owner," "group," and "the world."  As you might expect, the owner of the file
is a single user that has complete ownership over the file.  Groups are simply
groups of users on a UNIX system that are named explicitly on the UNIX system.
Finally, "the world" is simply everyone else that is both not the owner of the
file nor a part of the group that owns the file.

On UNIX, this is expressed in a three digit number attached to each file and
directory.

<pre><code>  7    7    7
             |    |    |
          owner group world</code></pre>

These numbers specify exactly what sort of permissions are applied to the file
for each user, group, and everyone else.  (In this particular case, 777 =
read/write/execute for the user, group, and world.)

How do these numbers specify the individual permissions for users, groups,
and the world?

UNIX adds up three different numbers (4, 2, and 1), with each specifying
a different permission type.

4 signifies read access for the file/directory.
2 signifies write access for the file/directory.
Finally, 1 signifies execute access for the file/directory.

You can also deny all permissions by simply using "0" for the specific
permission bit - say we only want read/write/execute for the owner,
but not for anyone else.  Then we apply the permission "700" to the file,
where "7" is for the owner, the second digit is for the group and the
third digit is for the world.

If I wanted read/write access for all users, then I add 4 + 2 together to get
6 (since I want both read (4) and write (2) access), and then I use that number
for every single group of people.  Therefore, I get 666. :)

If I wanted read/write/execute for the owner, but only read only access for
everyone else, then I use 744. For the first bit, 4 + 2 + 1 = 7, and then
I use 4's for the other people for read-only access.

In most FTP clients, you can right click the file and set the permissions
within the FTP interface.  If you have a UNIX shell, you can execute
the following command to change permissions on a specific file:

<pre><code>$ chmod 744 [filename]</code></pre>

How does this affect security when you're uploading HTML/CSS files to a web
server?

If you have 777 permissions on *every* file, then that means you're also
granting full access to everyone on the system, *including* anyone who
decides to hack the web server.  The latter scenario isn't totally
implausible, but the former scenario is even more likely - since most shared
web servers (usually the ones costing roughly $5-$10 per month) don't have
much separation between each user on the system, that means any other customer
can easily muck up your website without as much having explicit access to
*your* account.

When in doubt, 