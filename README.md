### this is

yet another script to manage your apache vhosts.  Zero configuration,
no dependencies (except ruby).  (At one point I used a conf file, but I
realized it was kind of ridiculous needing a config file to manage a config file.)

in a perfect world, adding a new vhost *should* take only a few seconds.  if you've done it many times it *does* take you a few minutes.  writing this *did* take me many hours

you can:
  - see a list of your vhosts in your httpd.conf whose doc root paths
      are missing.
  - see a list of your vhosts in your httpd.conf who don't have entries in
      your /etc/hosts.
  - see a list of entries in your /etc/hosts that don't have
      corresponding vhosts.
  - see a list of directories in your default vhost doc root directory that
      don't have /etc/hosts entries. etc.

Output pretty ascii tables for all of this.

And best of all, add an entry to all 3 with the click of a mouse.  I mean some
keys on your keyboard.

### installation

for now:

    git clone git://github.com/hipe/hipe-vhost.git

put that wherever, and from a folder somewhere in your path
(e.g. "/usr/local/bin", or "~/bin") make a symlink to "hipe-vhost/bin/vhost",
wherever you put it.)

we could make it a gem and all that, but really it's just one file.


### usage overview


#### see a list of the things in /etc/hosts:

    ~ > vhost hosts
    +------------------------+
    | ip        | name       |
    +------------------------+
    | 127.0.0.1 | fonteye    |
    | 127.0.0.1 | localhost  |
    | 127.0.0.1 | chip       |
    | 127.0.0.1 | metacms    |
    | 127.0.0.1 | sf3.local  |
    | 127.0.0.1 | socialsync |
    | 127.0.0.1 | wisteria   |
    +------------------------+


#### see a list of the known vhost confs:

    ~ > vhost confs
    +-----------------------------------------------+
    | server_name | document_root                   |
    +-----------------------------------------------+
    | feeber      | /Users/chip/Sites/feeber        |
    | fonteye     | /Users/chip/Sites/fonteye       |
    | foovur      | /Users/chip/Sites/foovur        |
    | metacms     | /Users/chip/Sites/metacms       |
    | push-graph  | /Users/chip/Sites/push-graph    |
    | sf3.local   | /Users/chip/Sites/sf3.local/web |
    | wisteria    | /Users/chip/Sites/wisteria      |
    +-----------------------------------------------+


#### see a list of the vhosts according to apache, with warnings:

    ~ > vhost dump
    +-----------------------------------------------------------------+
    | server_name | port | conf_path                                  |
    +-----------------------------------------------------------------+
    | feeber      |      | /etc/apache2/sites-enabled/feeber.conf     |
    | feeber      | *    | /etc/apache2/sites-enabled/feeber.conf     |
    | fonteye     | *    | /etc/apache2/sites-enabled/fonteye.conf    |
    | metacms     | *    | /etc/apache2/sites-enabled/metacms.conf    |
    | sf3.local   | *    | /etc/apache2/sites-enabled/sf3.conf        |
    | wisteria    | *    | /etc/apache2/sites-enabled/wisteria.conf   |
    +-----------------------------------------------------------------+

    +---------------------------------+
    | missing_docroot                 |
    +---------------------------------+
    | /Users/chip/Sites/wisteria      |
    +---------------------------------+


#### see the merged list of all this information:

    ~ > vhost list
    +-------------------------------------------------------------------------------------------------------------------------------------+
    | host       | host_ok | ip        | port | docroot                         | docroot_ok | conf                                       |
    +-------------------------------------------------------------------------------------------------------------------------------------+
    | feeber     | ok      | 127.0.0.1 | *    | /Users/chip/Sites/feeber        | ok         | /etc/apache2/sites-enabled/feeber.conf     |
    | fonteye    | ok      | 127.0.0.1 | *    | /Users/chip/Sites/fonteye       | ok         | /etc/apache2/sites-enabled/fonteye.conf    |
    | foovur     | ok      | 127.0.0.1 | *    | /Users/chip/Sites/foovur        | ok         | /etc/apache2/sites-enabled/foovur.conf     |
    | localhost  | ok      | 127.0.0.1 |      |                                 |            |                                            |
    | chip       | ok      | 127.0.0.1 |      |                                 |            |                                            |
    | metacms    | ok      | 127.0.0.1 | *    | /Users/chip/Sites/metacms       | ok         | /etc/apache2/sites-enabled/metacms.conf    |
    | push-graph |         |           | *    | /Users/chip/Sites/push-graph    | ok         | /etc/apache2/sites-enabled/push-graph.conf |
    | sf3.local  | ok      | 127.0.0.1 | *    | /Users/chip/Sites/sf3.local/web | ok         | /etc/apache2/sites-enabled/sf3.conf        |
    | socialsync | ok      | 127.0.0.1 |      |                                 |            |                                            |
    | wisteria   | ok      | 127.0.0.1 | *    | /Users/chip/Sites/wisteria      | missing    | /etc/apache2/sites-enabled/wisteria.conf   |
    +-------------------------------------------------------------------------------------------------------------------------------------+


#### and finally, add a new vhost:

    ~ > vhost add zombo
    not writable: /etc/apache2/sites-enabled/ - do you need to run this with sudo?
    ~ > sudo vhost add zombo
    Password:
    add line to /etc/hosts:
      make backup: /etc/hosts.bak.2010-02-13_16:05:49
      add "zombo" to /etc/hosts
    add config file for zombo:
      write /etc/apache2/sites-enabled/zombo.conf
    add stub website to /Users/chip/Sites:
      mkdir -m 755 /Users/chip/Sites/zombo
      write /Users/chip/Sites/zombo/index.html
    if this wasn't a dry run, you could restart apache with:
      /usr/sbin/apachectl graceful
    and go to:
      http://zombo/index.html
    done.
    ~ >


### why use it?

Don't use this if you have never done this "by hand".  This is for if you have
done this by hand and you don't want to and you don't want to/can't use
something like cpanel.  That way you can help me fix this when you run into
inevitable permissions/security issues.

This can also help as a learning aide, if you develop locally and want to
try to create a different vhost for all your projects, (or sometimes even
'beta' or 'development' subdomains for the same project) and want to know the
steps involved.  If you run 'add' with the '--dry-run' option, this won't
actually do anything, and you can just use it to see the steps you need to
take.

This thing makes it easier than doing this by hand (and I find it impossible
to manage more than a few vhosts without this, keeping track of all these
little loose ends) and it's "closer to the metal" than something like cpanel.


### compatibility

To be zero-configuration this needs to make some assumptions about your
environment.  These assumptions are annoying.  Suggestions on how to improve
this without requiring configuration are welcome.

Assumptions are (in order from least to most annoying):

1. your webserver invocation script goes by the name 'apachectl' or
   'apache2ctl'.  one or the other must be in your path not both.
   If you are familiar with starting your server with 'httpd', apachectl
   is just a script that wraps it with some other options.
2. your /etc/hosts-like file is reachable by the path "/etc/hosts". (if
   this is a problem we could do some 'locate' hack.)
3. your httpd.conf file is called httpd.conf and lives in or is reachable
   at /etc/apache2/httpd.conf.  If you use /etc/apache2/apache2.conf
   that's fine, so do i, but then Include httpd.conf.  You shoudn't be
   putting vhost stuff there anyway.  If this is a problem, ditto 2. above
4. by default the new docroots for the new sites created by this thing
   will go into ~/Sites. That's not generally the best place to put
   a production site so you can indicate something more reasonable
   with the --docroot option.  (you can also always change it in
   your vhost config file!)
5. all of your vhost configs will live in a folder indicated in an
   Include in your httpd.conf, a path which must end in 'sites-enabled',
   per the ubuntu default setup.  (no good reason not to follow suit
   on mac!)  We could add options to change this if we really needed to.
6. What is this 'windows' operating system of which you speak?





### issues/limitations/known bugs

 - Does not remove entries/files in the 3 places, only adds them
 - Only used/tested on two servers so far! (one mac one ubuntu)
 - Needs security audit!!
 - in general this is very beta


### license

dual MIT/GPL.  use at your own risk.  no warranty expressed or implied.  this script will get your whole site p0wned and make your laptop grow legs, burst into flames, leap off your desk and jump straight into the trashcan.
