# Linux Server Configuration
This README file was created as my submission for project 4 of Udacity's Full
Stack Nanodegree program.  Project 5 encompasses setting up an Apache2 server
and deploying the application created during project 3.  Below is the required
information for submission of project 5.


## Server Information
| Topic               | Data                 |
|---------------------|----------------------|
| **IP Address**      | 52.89.209.123        |
| **SSH Port**        | 2200                 |
| **Application URL** | http://52.89.209.123 |


## Software and Configuration Changes
#### Installed Software
- **Finger** - Used to inspect created users
  - Installed with: `sudo apt-get install finger`
- [**PostgreSQL**](http://www.postgresql.org) - Used to manage data bases for application
  - Installed with: `sudo apt-get install postgresql` postgresql-contrib
- [**Apache2**](http://httpd.apache.org) - Used to host the application
  - Installed with: `sudo apt-get install apache2`
- [**libapache2-mod-wsgi**](http://flask.pocoo.org/docs/0.10/deploying/mod_wsgi/) - Used to connect Flask application to Apache Server
  - Installed with: `sudo apt-get install libapache2-mod-wsgi`
- **python-dev** - Additional functionality
  - Installed with: `sudo apt-get install python-dev`
- [**Git**](https://git-scm.com/documentation) - Used to clone application repository
  - Installed with: `sudo apt-get install git`
- [**virtualenv**](https://virtualenv.pypa.io/en/latest/) - Used to run and test Application
  - Installed with: `sudo pip install virtualenv`
- [**Flask**](http://flask.pocoo.org) - Framework used to build and run application
  - Installed with: `sudo pip install Flask `
- [**httplib2**](https://pypi.python.org/pypi/httplib2) - Http client
  - Installed with: `sudo pip install httplib2`
- [**Psycopg2**](http://initd.org/psycopg/) - Used to connect Flask app and Psycopg2
  - Installed with: `sudo apt-get install python-psycopg2`
- [**OAuth2Client**](https://github.com/google/oauth2client) - Used to to access OAuth 2.0 authentication
  - Installed with: `sudo pip install oauth2client`
- [**Unattended-Upgrades**](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) - Used to automatically install upgrades
  - Installed with: `sudo apt-get install unattended-upgrades`
- [**Fail2Ban**](http://www.fail2ban.org/wiki/index.php/Main_Page) - Used to ban IP adress after repeated failed logins
  - Installed with: `sudo apt-get install fail2ban`
- [**Glances**](https://github.com/nicolargo/glances/blob/master/docs/glances-doc.rst) - Used to monitor system information
  - Installed with: `sudo pip install glances`

#### Configuration Changes
1. Update available package list: `apt-get update`
2. Update existing software: `apt-get upgrade`
3. Add new user, “gravic”: `adduser gravic`
4. Add new user, “grader”: `adduser grader`
5. Add sudoer files for the new users in */etc/sudoers.d/*
6. Create SSH keys for new users
  1. Create ssh directory for grader: `mkdir /home/grader/.ssh`
  2. Adjust permissions for this new folder: `chmod 700 /home/grader/.ssh`
  3. Create file and paste contents from the public key: `vim /home/grader/.ssh/authorized_keys`
  4. Adjust permissions for this new file: `chmod 644 /home/grader/.ssh/authorized_keys`
  5. Adjust group and owner for .ssh and authorized_keys: `chown grader:grader /home/grader/.ssh` & `/authorized_keys`
  6. Repeat steps for user gravic
7. Setup fire wall
  1. Block all incoming connections: `sudo ufw default deny incoming`
  2. Allow all outgoing connections: `sudo ufw default allow outgoing`
  3. Open SSH(2200), HTTP(80), and NTP(123) ports: `sudo ufw allow 2200`, `sudo ufw allow www`, `sudo ufw allow 123`
  4. Enable firewall: sudo ufw enable
8. Set local timezone to UTC: `sudo dpkg-reconfigure tzdata`
9. Setup PostgreSQL
  1. Login as the default user: `sudo -i -u postgres`
  2. Create new user catalog: `createuser --interactive catalog`
10. Enable mod_wsgi: `sudo a2enmod wsgi`
11. Create a directory for the Flask Application: `sudo mkdir /var/www/FlaskApp`
12. Cloned git repository for "The Library" Application
13. Create the configuration file for the application: `sudo vim /etc/apache2/sites-available/FlaskApp.conf`
  1. Add *Options -Indexes* to any directories that should not visible throu Http
14. Enable the virtual host: `sudo a2ensite FlaskApp`
15. Create the wsgi file: `sudo vim /var/www/FlaskApp/library.wsgi`
16. Edit SSH config file to disable root log in: `sudo vim /etc/ssh/sshd_config`
  1. Set: *PermitRootLogin no*
17. Set Unattended Upgrade settings: `sudo vim /etc/apt/apt.conf.d/50unattended-upgrades`
18. Setup automatic banning of repeated failed logins.
  1. Copy Fail2Ban’s config file to a local version: `sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local`
  2. Edit options in this new file: `sudo vim /etc/fail2ban/jail.local`
        - If 3 incorrect logins with in 10 minutes, the IP will be banned for 10 minutes
19. Login issues were experienced and corrected by adjusting package versions
  1. `sudo pip install werkzeug==0.8.3`
  2. `sudo pip install flask==0.9`
  3. `sudo pip install Flask-Login==0.1.3`


## Third Party Resources
- Most of what was implemented came from the [Configuring Linux Web Servers](https://www.udacity.com/course/viewer#!/c-ud299-nd/) cource from Udacity
- [How To Deploy a Flask Application on an Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-deploy-a-flask-application-on-an-ubuntu-vps)
- [How To Install and Use PostgreSQL on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04)
- [Google :-)](https://www.google.com)
- Code for "The Library" can be found [here](https://github.com/gravic07/the_library.git).
