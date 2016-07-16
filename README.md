mantisbt on OpenShift
=====================

This git repository is for mantisbt sites packaged and organized for direct use
with openshift.  This means that among other things the web application paths
will match up to how openshift expects them.    You should be able to create a
local git repository for yourself that uses both this and your actual openshift
application repository as remote sources, and then migrate future updates
posted into this repository simply by merging from this remote source and then
pushing back to your openshift application repo.  The backend database is MySql
and the database name is the same as your application name (using
$_ENV['OPENSHIFT_APP_NAME']). You can name your application whatever you want.
However, the name of the database will always match the application so you
might have to update .openshift/action_hooks/build.

Running on OpenShift
--------------------

Create an account at http://openshift.redhat.com/

Create a php-5.3 application (you can call your application whatever
you want)

    rhc app create -a bugs -t php-5.4

Add the mysql and phpmyadmin

    rhc app cartridge add -a bugs -c mysql-5.1
    rhc app cartridge add -a bugs -c phpmyadmin-5.4

Add this upstream bugs repo:

    cd bugs

    # you may need to remove index.php
    git rm php/index.php
    git commit -a

    git remote add upstream -m master git://github.com/dyfet/openshift-mantisbt.git
    git pull -s recursive -X theirs upstream master
    # note that the git pull above can be used later to pull updates to
    # Etherpad the rm and ln is there until I can figure out github and
    # symbolic links
Then push the repo upstream

    git push

You can now login to your mantisbt instance and go through it's initial
setup.  Thats it, your mantisbt is then up and running!


