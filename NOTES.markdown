Notes
=====

Just a few notes about the theme in general, both to users and to myself.

Design Notes
------------

Using -moz-box-shadow / -webkit-box-shadow seems to slow down the page quite
a bit (this was in place a little while ago in order to shade the area to the
left and right of the content.  The page was fine until I introduced
some javascript in the page like shadowbox/slimbox.)

We can use a well positioned background image to achieve the same effect,
but what ends up happening is that when the page is sized larger than
the height of the content on the page, the user will see the background
extend toward the bottom of the viewport, instead of having the shadow
stop around the bottom of the content area.  -moz-box-shadow/-webkit-box-shadow
solved THAT issue, but introduced performance problems instead.

Here's a few options I'm thinking of:

* Figure out a way to have a shadow around the content area without introducing
performance issues and without having the background visible when the content
area doesn't extend to the bottom of the viewport.
* Figure out a sane way to extend the content area so that the footer "sticks"
to the bottom of the viewport.
* Figure out why -moz-box-shadow/-webkit-box-shadow is slowing down the page
so much.
