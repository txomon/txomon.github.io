---
categories:
- Off-topic
date: "2012-12-14T02:16:18+01:00"
title: Install bugzilla in ubuntu 12.04
---

### Introduction

Now will document the process to install bugzilla.

I will follow several docs, which I wont put here because I am lazy atm.

First of all, we create our folder, named consultas (I am spanish as many know and I dont want to be all the time doing transcriptions/translations).

```
mkdir -p proyectos/consultas
cd proyectos/consultas
```

### MariaDB system configuration and apache2 install

Then we start to configure the system to install mariadb (I don't like anymore mysql), we go here [https://downloads.mariadb.org/mariadb/repositories/](https://downloads.mariadb.org/mariadb/repositories/) and we just copy what they output for us.

```
# MariaDB 5.5 repository list - created 2012-12-13 18:13 UTC
# http://downloads.mariadb.org/mariadb/repositories/
deb http://mirror2.hs-esslingen.de/mariadb/repo/5.5/ubuntu precise main
deb-src http://mirror2.hs-esslingen.de/mariadb/repo/5.5/ubuntu precise main
```

And we put it in a new file in /etc/apt/sources.list/mariadb.list

Now we run a command to add the key as we are told in ([https://kb.askmonty.org/en/installing-mariadb-deb-files/](https://kb.askmonty.org/en/installing-mariadb-deb-files/)):

```
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 0xcbcb082a1bb943db
```

And now we are prepared to install mariadb:

```
sudo apt-get update && sudo apt-get install mariadb-server apache2 -y
#We enter here the root password...
```

### Installing bugzilla

I didn't have very clear in which order must be done, but as I like seeing my results, I will make the installation before.

So, we download bugzilla, untar it, and modify to fix it to be able to connect to gmail:

```
wget http://ftp.mozilla.org/pub/mozilla.org/webtools/bugzilla-4.2.4.tar.gz
tar xf bugzilla-4.2.4.tar.gz
cd bugzilla-4.2.4
vim Bugzilla/Mailer.pm #Edit line 166 to be: if($method eq "SMTP" || $method eq "Gmail"){
```

And now, we have to check about the perl modules, etc.

```
./checksetup.pl --check-modules
sudo cpan CPAN YAML Email::Send::Gmail
sudo apt-get install libgd2-xpm-dev libapache2-mod-perl-dev libapache2-mod-auth-mysql patchutils# this is for GD, mod_perl and mysql. patchutils is for later use in configuring bugzilla
sudo a2enmod headers expires # mod_headers and mod_expires dependencies have to be enabled
sudo perl install-modules.pl --all #I don't care of this, just want bugzilla get installed!
```

In theory, you should have everything installed. Sample output here:

```
./checksetup.pl --check-modules
* This is Bugzilla 4.2.4 on perl 5.14.2
* Running on Linux 3.2.0-34-generic #53-Ubuntu SMP Thu Nov 15 10:48:16 UTC 2012

Checking perl modules...
Checking for CGI.pm (v3.51) ok: found v3.62
Checking for Digest-SHA (any) ok: found v5.61
Checking for TimeDate (v2.21) ok: found v2.24
Checking for DateTime (v0.28) ok: found v0.78
Checking for DateTime-TimeZone (v0.71) ok: found v1.56
Checking for DBI (v1.614) ok: found v1.622
Checking for Template-Toolkit (v2.22) ok: found v2.24
Checking for Email-Send (v2.00) ok: found v2.198
Checking for Email-MIME (v1.904) ok: found v1.911
Checking for URI (v1.37) ok: found v1.59
Checking for List-MoreUtils (v0.22) ok: found v0.33
Checking for Math-Random-ISAAC (v1.0.1) ok: found v1.004

Checking available perl DBD modules...
Checking for DBD-Pg (v1.45) not found
Checking for DBD-mysql (v4.001) ok: found v4.020
Checking for DBD-SQLite (v1.29) ok: found v1.37
Checking for DBD-Oracle (v1.19) not found

The following Perl modules are optional:
Checking for GD (v1.20) ok: found v2.46
Checking for Chart (v2.1) ok: found v2.4.6
Checking for Template-GD (any) ok: found v1.56
Checking for GDTextUtil (any) ok: found v0.86
Checking for GDGraph (any) ok: found v1.44
Checking for MIME-tools (v5.406) ok: found v5.503
Checking for libwww-perl (any) ok: found v6.03
Checking for XML-Twig (any) ok: found v3.42
Checking for PatchReader (v0.9.6) ok: found v0.9.6
Checking for perl-ldap (any) ok: found v0.51
Checking for Authen-SASL (any) ok: found v2.16
Checking for RadiusPerl (any) ok: found v0.22
Checking for SOAP-Lite (v0.712) ok: found v0.715
Checking for JSON-RPC (any) ok: found v1.03
Checking for JSON-XS (v2.0) ok: found v2.33
Checking for Test-Taint (any) ok: found v1.06
Checking for HTML-Parser (v3.67) ok: found v3.69
Checking for HTML-Scrubber (any) ok: found v0.09
Checking for Encode (v2.21) ok: found v2.42_01
Checking for Encode-Detect (any) ok: found v1.01
Checking for Email-MIME-Attachment-Stripper (any) ok: found v1.316
Checking for Email-Reply (any) ok: found v1.202
Checking for TheSchwartz (any) ok: found v1.10
Checking for Daemon-Generic (any) ok: found v0.82
Checking for mod_perl (v1.999022) ok: found v2.000005
Checking for Apache-SizeLimit (v0.96) ok: found v0.96
Checking for mod_headers (any) ok
Checking for mod_expires (any) ok
Checking for mod_env (any) ok
```

So, with all this in OK state, we now continue to configure mariadb and apache.

### Configure apache for use of ssl always

Now, I want to have everything under ssl. The first thing is to create all the redirects from 80 to 443, and set up virtualhosts, etc. For that, I create a file named delegación in /etc/apache2/sites-available with this content:

```
<VirtualHost *:80>
ServerName u102508.ehu.es
Redirect permanent / https://u102508.ehu.es/
</VirtualHost>

<VirtualHost *:443>
ServerName u102508.ehu.es
# Normal config here
DocumentRoot /var/www/

# SSL configuration here
SSLEngine On
SSLCertificateFile /etc/apache2/private/delegacion.crt
SSLCertificateKeyFile /etc/apache2/private/delegacion.key

</VirtualHost>
```

Create the certificate, move it to a private folder, I also disable default site, add a ServerName directive to the ports.conf file, and disable the NameVirtualHost *:80 in there also.

```
openssl req -x509 -nodes -days 10000 -newkey rsa:8192 -keyout delegacion.key -out delegacion.crt
sudo mkdir -p /etc/apache2/private
sudo mv delegacion* /etc/apache2/private
sudo chown -R www-data:www-data /etc/apache2/private
sudo chmod 500 -R /etc/apache2/private
sudo a2enmod ssl
sudo a2ensite delegacion
sudo a2dissite default
sudo apachectl restart
```

And now in theory everything should work correctly. Now we will add bugzilla's dir etc. to the apache conf, and also move the dir to where we will work:

```
sudo mkdir -p /smb/web # Here we host the files
cd ..
sudo mv bugzilla-4.2.4 /smb/web/tracker
sudo chown -R administrador:www-data /smb/web #administrador is root user
```

Also we modify /etc/apache/sites-available/delegacion to add the directory entry...

```
Alias /tracker "/smb/web/tracker/"
<Directory /smb/web/tracker/>
    AddHandler cgi-script .cgi
    Options +Indexes +ExecCGI +FollowSymLinks
    Allow from all
    DirectoryIndex index.cgi
    AllowOverride Limit FileInfo Indexes
</Directory>
```

And it is in theory complete. Now we have to configure the mariadb user/password and modify /smb/web/tracker/localconfig

### MariaDB user creation

Now, we create the user bugs, and give him access:

```
mysql -uroot -e "GRANT ALL ON bugs.* TO 'bugs'@'localhost' IDENTIFIED BY 'aspdofihapson1234o8y2e3';" -p
```

just enter root password and work done. Remember password ('aspdofihapson1234o...') because that is what we have to put in the localconfig file.

So now edit localconfig, you will just need to change:

* $webservergroup="www-data"
* $db_pass (to the pass we used)
* $db_sock = "/var/run/mysqld/mysqld.sock"
* $interdiffbin="/usr/bin/interdiff"

and now run sudo ./checksetup.pl, and ingress al what you need.

And now, we are in theory done with the shell.

### Configure Bugzilla and Gmail

Now we enter in the web page (for me u102508.ehu.es) and go straigth to configure email:

1. mailfrom: gmail address `<whatever>@gmail.com`
2.  smtpserver: smtp.gmail.com
3.  smtp_username: gmail address
4.  smtp_password: gmail password

And click on save. Now go straight to LDAP:

1.  LDAPserver: ldaps://ldaps.lg.ehu.es/ in my case
2.  LDAPstarttls: off (for me)
3.  LDAPBaseDN: ou=people,dc=ehu,dc=es
4.  Didnt touch LDAPuidattribute nor LDAPmailattribute
5.  LDAPFilter: in my case, I just want the ones in my faculty => (SambaDomainName=345)

And click on save. Now go to user authentication:

1. user_verify_class: put LDAP the first (just move ldap with the up arrow, leave the other like they are)
2.  createemailregexp: set to blank ( =>  ) ((XD))

And its all done. Now you may have profit modifying all bugzilla to fix your needs
