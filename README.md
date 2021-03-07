# Mail-system
This repository explains how to setup Mail server in Linux using free dynamicDNS.
Postfix is popular open source Mail server to send and receive Mail messages. In order to install mail server, run the following command:
<pre>
$ sudo apt install postfix net-tools bsd-mailx
</pre>
/etc/postfix/main.cf is the configuration file for postfix.
You must obtain the free DynamicDNS domain name from public domain registry:
<pre>
http://freedns.afraid.org/domain/registry/
</pre>
<pre>
You must run the following command for smtpd_tls.
$ cd /etc/postfix
$ sudo mkdir tls
$ sudo chown root:postfix tls
$ sudo chmod 700 tls
$ cd tls
$ sudo openssl req -new -x509 -nodes -out smtpd.pem -keyout smtpd.pem -days 3650s:
</pre>
You must at least include the following in /etc/postfix/main.cf (domain name is your_domain_name):
<pre>
smtpd_tls_CAfile =/etc/postfix/tls/smtpd.pem
smtpd_tls_cert_file=/etc/postfix/tls/smtpd.pem
smtpd_tls_key_file=/etc/postfix/tls/smtpd.pem
smtpd_use_tls=yes

myhostname = your_domain_name
local_recipient_maps =
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
myorigin = /etc/mailname
mydestination = your_domain_name, localhost.localdomain, localhost
relayhost = mail-server-address(imap.xxxx or smtp.xxxx)
</pre>

# air.sh and schedule.pl
In order to build an automated mail reply system, you should add the following one line in /etc/aliases:
<pre>
In aliases
air: "| /etc/air.sh"
air.sh program returns motd file to sender.

$ cat air.sh
#!/bin/sh
t=`sed -n '/^From/p'|sed -n '1 p'|awk '{print $2}'`
/usr/bin/mail -s "reply" $t -A /etc/motd </dev/null
</pre>

schedule.pl (perl program)
<pre>
In aliases
schedule: "| /etc/schedule.pl"

$ cat schedule.pl 
#!/usr/bin/perl
$sendmail="/usr/bin/mail";
$schedule="/etc/motd";
$sub="-s schedule ";
while(<>){
        if(/^From: *(.+) */i){
                $to = $1;
                if (/<(.+)>/){
                        $adrs = $1;
                }
                elsif(/([a-z0-9\.]+@[a-z0-9\.]+)/i){
                        $adrs = $1;
                }
                else{
                        exit(1);
                }
                system("$sendmail $sub $adrs <$schedule");
        }
}
</pre>

# /etc/mailname
/etc/mailname should be changed:
<pre>
$ cat /etc/mailname
your_domain_name
</pre>

# /etc/aliases
You should activate the change of aliases file:
<pre>
$ sudo postalias /etc/aliases
$ sudo newaliases
</pre>

# /etc/hostname
Make sure that /etc/hostname should be modified:
<pre>
$ sudo hostname xxx       xxx is the domain name
</pre>

# postfix
To be able to activate the postfix program:
<pre>
$ sudo service postfix restart
</pre>
In order to see what is going on in postfix, type the following command:
<pre>
$ sudo service postfix status
</pre>
# bind9
According to Wikipedia, BIND (Berkeley Internet Name Domain) is an implementation of the Domain Name System (DNS) of the Internet. It performs both of the main DNS server roles, acting as an authoritative name server for domains, and acting as a recursive resolver in the network. As of 2015, it is the most widely used domain name server software and is the de facto standard on Unix-like operating systems.
<pre>
$ sudo apt install bind9
$ sudo service bind9 start
</pre>
# dovecot
<pre>
$ sudo apt install dovecot-common dovecot-imapd dovecot-pop3d
$ sudo service dovecot status
</pre>
