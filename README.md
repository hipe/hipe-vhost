### this is

yet another script to manage your apache vhosts.  Zero configuration,
no dependencies (except ruby).  (At one point I used a conf file, but I
realized it was kind of ridiculous needing a config file to manage a config file.)

See a list of your vhosts in your httpd.conf whose doc root paths are missing.
See a list of your vhosts in your httpd.conf whose don't have entries in your
/etc/hosts.  See a list of entries in your /etc/hosts that don't have
corresponding vhosts.  See a list of directories in your default vhost doc
root directory that don't have /etc/hosts entries. etc.

And best of all, add an entry to all 3 with the click of a mouse.  I mean some
keys on your keyboard.

Don't use this if you have never done this by hand.  This is for if you have
done this by hand and you don't want to and you don't want to use something
like cpanel.  That way you can help me fix it when you run into the inevitable
permissions issues.

### compatibility

To be zero-configuration this needs to make some assumptions about your
environment.  These assumptions are annoying.  Suggestions on how to
improve this without requiring any configuration are welcome.

Assumptions are (in order of least annoying to most):

    1. your webserver daemon goes by the name 'apachectl' or 'apache2ctl'.
       one or the other must be in your path not both.
    2. your /etc/hosts file in at or symlinked by /etc/hosts. If this is
       a problem we could do some 'locate' hack
    3. your httpd.conf file is called httpd.conf and lives in or is reachable
       at /etc/apache2/httpd.conf.  If you use /etc/apache2/apache2.conf
       that's fine, but then Include httpd.conf.  You shoudn't be putting
       vhost stuff there anyway.   If this is a problem, ditto 2. above
    4. all of your vhost configs will live in /etc/apache2/sites-enabled/,
       per the ubuntu default setup
    5. this is pretty annoying, sure: all of your doc roots for your vhosts
       need to live at or be reachable by  /var/sites.  You can of course
       change this manually in the vhost config file.  But this is what is
       meant by 'convention over configuration'


What is this 'windows' operating system of which you speak?


### license

MIT/GPL.  use at your own risk.  no warranty expressed or implied.  this script will get your whole site p0wned and make your laptop grow legs, burst into flames, leap off your desk and jump straight into the trashcan.
