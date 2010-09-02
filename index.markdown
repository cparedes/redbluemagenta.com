---
layout: default
title: redbluemagenta
blurb: Ian is a SAGE Level II System Administrator.
---

I'm a junior systems administrator working mostly with BSD and Linux systems,
especially with Debian, CentOS/RHEL, and OpenBSD.  I'm a member of [SAGE] [1]
and [LOPSA] [2].

Also, you might be wondering what "SAGE Level II" means: [here's a list of
job descriptions from SAGE] [3].

Wait, what's a sysadmin anyway?
-------------------------------

Good question.  It's someone (usually in an IT department) who is typically
seen as the jack of all trades; they're the ones who fix your computers and
also handle your mail server.  They don't usually handle one server
(what most people think of when they think "system"): they handle *systems*
of computers, intertwined together with a network.  They make sure that
when you hit the "send" button on your mail client, the mail *reliably*
gets to the other side of the internet.  When there's an incident that causes
a lot of data to be wiped out, the sysadmin is usually one step ahead and
made sure that there's multiple backups of critical systems.  We manage
hidden complexities so no one else has to. :)

Specializations
---------------

I specialize in HPC administration and web services.  I've also fallen in love
recently with modern configuration management tools, such as Puppet and
various others. 

Current Interests in Systems Administration
-------------------------------------------

I’m currently interested in investigating more into self-healing networks and
systems; while configuration management tools such as Puppet help to achieve
this goal, what if there was an increased demand in a service that "wounds"
the system in a way that usually requires manual intervention? What we’d
usually do is provision more systems and designate them in Puppet as specific
servers; however, can we automate this step so that our tool of choice (or
some other new tool) can auto-provision systems when there’s an increased load
on our service? (I’d imagine that it’d require monitoring of specific metrics,
along with a language that describes what the system should do in order to
ease the load requirements; I could at least imagine a scenario when such a
self-healing system for, say, a web service, can throw too many slave servers
underneath a load balancer, or provision one too many load balancers for the
amount of slave hosts.)

About Ian Paredes
-----------------

I currently work for [3TIER] [4] as a systems administrator. I also administer
an OpenBSD router for [Cafe Allegro][5] with two other volunteer sysadmins. I
design a few websites as a hobby with XHTML and CSS, and I occasionally
volunteer my time for various nonprofit and academic organizations, such as
[The Critical Gaming Project] [6] and [Noise for the Needy] [7]. I also
program in Ruby as a hobby, and more often than not, they are usually apps
on Ruby on Rails.

[Read more...][8]

[1]: http://sage.org
[2]: http://lopsa.org
[3]: http://www.sage.org/field/jobs-descriptions.html
[4]: http://3tier.com
[5]: http://seattleallegro.com
[6]: http://depts.washington.edu/critgame
[7]: http://noisefortheneedy.org
[8]: /about.html
