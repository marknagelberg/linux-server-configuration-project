#Linux Server Configuration Project

This is the readme file for the final project associated with the [Linux Server Configuration course](https://www.udacity.com/course/configuring-linux-web-servers--ud299), completed as part of the [Full Stack Developer Nanodegree](https://www.udacity.com/course/full-stack-web-developer-nanodegree--nd004). The purpose of this project was to set up and configure a linux web server to run my [ShowCredit](https://github.com/marknagelberg/showcredit) web application.

##Server information:
* IP address: 52.36.2.113
* SSH port of server: 2200
* Complete URL of hosted web application: [http://52.36.2.113/](http://52.36.2.113/)

##Software installed:
* finger (for identifying users)
* updated/upgraded all software on server (using apt-get update and apt-get upgrade)
* apache web server (using apt-get install apache2)
* mod_wsgi (apt-get install libapache2-mod-wsgi python-dev)
* PostgreSQL (apt-get install postgresql)
* git (apt-get install git-all)
* fail2ban (apt-get install fail2ban - to thwart brute force attacks)
* cloned my catalog application (ShowCredit) at /var/www/showcredit
* pip (for installing python packages - used sudo apt-get install python-pip)
* virtualenv (for python virtual environment for catalog app - sudo pip install virtualenv)
* ran sudo virtualenv showcredit to create temporary environment for app
* within the virtual environment, installed the following python packages: Flask, sqlalchemy, httplib2, oauth2client, requests, Werkzeug, WTForms, psycopg2
* monit (for monitoring various aspects of the server and getting automatic updates)

##Configuration changes made:
* Added a user named grader
* Gave this grader sudo ability by adding a file located at /etc/sudoers.d/grader
* grader is allowed to login via ssh. This was done by creating /home/grader/.ssh/authorized_keys file and copying the contents of /root/.ssh/authorized_keys while working as the grader user. I then changed the permissions of /home/grader/.ssh to 700 and /home/grader/.ssh/authorized_keys to 644, again while logged in as grader.
* ufw is configured to only allow traffic from port 2200 (ssh), 123 (ntp), and 80 (http)
* Outgoing requests are set using the command sudo ufw default allow outgoing
* Created and configured a database for the catalog app entitled "showcredit"
* Root login is disabled by setting "PermitRootLogin no" in /etc/ssh/sshd_config
* postgresql is configured so that remote connections are not allowed. This is indicated in the host based authenticaiton file (/etc/postgresql/9.1/main/pg_hba.conf)
* Created a user "catalog" for the showcredit database and gave this user superuser powers.

##Additional features:

* Installed and configrued fail2ban to thwart multiple login attempts. The configuration file (/etc/fail2ban/jail.conf) was changed to ensure fail2ban knows the ssh port is 2200. IPs are currently banned for 600 seconds if they attempt to login 3 times and fail.

* crontab added to manage package updates each week: the file is located in /etc/cron.weekly and contains the commands apt-get update, apt-get upgrade -y, and apt-get autoclean.

* monit installed to monitor various aspects of the VM and provide automated updating when things go wrong. monit is configured to make sure that port 80 is running, check the main website to see if a 200 http response is returned, and monitoring apache to make sure it is running (and automatically re-running it if it is not). In the event of an alert, a notification is sent to my personal email. The configuration file for monit is located at /etc/monit/monitrc.

##Third-party resources used to complete the project
* [Information on installing and configuring postgresql and ensuring remote connections are not allowed.](https://www.digitalocean.com/community/tutorials/how-to-secure-postgresql-on-an-ubuntu-vps)
* [Guide to deploying Flask app.](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
* [Resource used to fix an error encountered during the installation of psycopg2 python package.](http://stackoverflow.com/questions/28253681/you-need-to-install-postgresql-server-dev-x-y-for-building-a-server-side-extensi)
* [Resourced used for troubleshooting an error when setting up postgresql.](http://stackoverflow.com/questions/11919391/postgresql-error-fatal-role-username-does-not-exist)
* [Resource to configure the automatic package updates using cron.](https://help.ubuntu.com/community/AutoWeeklyUpdateHowTo)
* [Resource to help with installation and configuration of monit, which performs automated system monitoring tasks and notifications.](http://www.binarytides.com/install-monit-debian/)
