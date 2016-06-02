Convert XChat IRC logs to Polari
================================

I've migrated my IRC client from XChat-GNOME to Polari, and I wanted to move
my chat logs too.

This will be a script to do exactly that.


Plan
----

XChat keeps logs in `~/.xchat2/xchatlogs/`, named `{network}-#{channel}.log`.

For extra confusion, `{network}` changes, e.g. could be "FreeNode", could be
"FreeNode (ZNC)" even though it's the same network really.

Polari keeps logs in `~/.local/share/TpLogger/logs/`, named
`idle_irc_{id}/chatrooms/#{channel}/{yyyymmdd}.log`.

Mapping `{network}` to `{id}` will be ... fun.  e.g.  GimpNet is in
`idle_irc_mgedmin1`, FreeNode is in `idle_irc_mgedmin0`, and I also have
`idle_irc_mgedmin_5fpolari0` from my earliest experiments (on GimpNet too).

XChat's logs look like

    **** ŽURNALAS PRADĖTAS Wed Oct 29 08:57:27 2014

    Spa 29 08:57:27 -->	Jūs dabar kalbate #polari
    Spa 29 09:04:31 <mgedmin>	can I run polari from the build tree without running 'make install'?
    Spa 29 09:08:05 <mgedmin>	I guess https://bugzilla.gnome.org/show_bug.cgi?id=710014 is about that
    Spa 29 09:08:17 <Services>	Bug 710014: normal, Normal, ---, polari-maint, UNCONFIRMED, src/polari script is not executable
    Spa 29 10:34:40 *	csoriano- (~carlos@nat-pool-brq-t.redhat.com) atėjo į #polari
    Spa 29 12:13:53 *	mclasen (~mclasen@pool-108-20-138-235.bstnma.fios.verizon.net) atėjo į #polari
    Spa 29 12:51:52 *	yoseforb__ (~yoseforb@213.151.54.108) atėjo į #polari
    Spa 29 13:08:32 ---	mclasen dabar žinoma(s) kaip mclasen_afk

Yay, everything's translated!  Timestamps are in local time!  This'll be fun to
parse.  I've some code to do that in irclog2html, specifically
https://github.com/mgedmin/irclog2html/blob/master/src/irclog2html/xchatlogsplit.py

Polari's logs look like

    <?xml version='1.0' encoding='utf-8'?>
    <?xml-stylesheet type="text/xsl" href="log-store-xml.xsl"?>
    <log>
    <message time='20160602T08:24:12' id='mgedmin' name='mgedmin' token='' isuser='false' type='normal'>trying now</message>
    <message time='20160602T08:24:42' id='mgedmin' name='mgedmin' token='' isuser='true' type='normal'>ok, this is interesting: I added a 2nd ZNC connection, with a different server password, and it works!</message>
    <message time='20160602T08:32:37' id='mgedmin' name='mgedmin' token='' isuser='true' type='action'>removes the account and adds it back</message>
    </log>

The timestamps seem to be in UTC.  Join/part messages don't seem to be logged.
