# Mail-system
This repository explains how to setup Mail server in Linux.
Postfix is popular open source mail server. In order to install, run the following command:
<pre>
$ sudo apt install postfix
</pre>
/etc/postfix/main.cf is the configuration file for postfix.
You must at least include the following in main.cf for example:
<pre>
myhostname = mac.dob.jp
local_recipient_maps =
alias_maps = hash:/etc/aliases
alias_database = hash:/etc/aliases
myorigin = /etc/mailname
mydestination = xxx.yyy.zz, xxx, localhost.localdomain, localhost
relayhost = imap.sfc.keio.ac.jp
</pre>
