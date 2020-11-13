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
You must at least include the following in /etc/postfix/main.cf (domain name is your_domain_name):
<pre>
myhostname = your_domain_name
local_recipient_maps =
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
myorigin = /etc/mailname
mydestination = your_domain_name, localhost.localdomain, localhost
relayhost = mail-server-address(imap.xxxx or smtp.xxxx)
</pre>
In order to build an automated mail reply system, you should add the following one line in /etc/aliases:
<pre>
air: "| /etc/air.sh"
</pre>
air.sh program returns motd file to sender.
<pre>
$ cat air.sh
#!/bin/sh
t=`sed -n '/^From/p'|sed -n '1 p'|awk '{print $2}'`
/usr/bin/mail -s "reply" $t -A /etc/motd </dev/null
</pre>
</pre>
/etc/mailname should be changed:
<pre>
$ cat /etc/mailname
your_domain_name
</pre>

</pre>
You should activate the change of aliases file:
<pre>
$ sudo postalias /etc/aliases
$ sudo newaliases
</pre>
Make sure that /etc/hostname should be modified:
<pre>
$ sudo hostname xxx       xxx is the domain name
</pre>
To be able to activate the postfix program:
<pre>
$ sudo service postfix restart
</pre>
In order to see what is going on in postfix, type the following command:
<pre>
$ sudo service postfix status
</pre>
According to Wikipedia, BIND (Berkeley Internet Name Domain) is an implementation of the Domain Name System (DNS) of the Internet. It performs both of the main DNS server roles, acting as an authoritative name server for domains, and acting as a recursive resolver in the network. As of 2015, it is the most widely used domain name server software and is the de facto standard on Unix-like operating systems.
<pre>
$ sudo apt install bind9
$ sudo service bind9 start
$ sudo apt install dovecot-common dovecot-imapd dovecot-pop3d
$ sudo service dovecot status
</pre>
