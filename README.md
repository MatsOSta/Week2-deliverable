# Week2-deliverable
auth.log = orignal
temp.log = ready for awk
uniq.log = uniq entires present


(a) How many lines does the file contain? // wc -l auth.log            
== 5564 auth.log


(b) How many different types of message are present?
first iteration
Cleanup prosses ID and Preauth tag save to temp.log//
sed -e  's/ \[preauth]/ /g' auth.log  | sed -e  's/sshd\[.*\]/sshd/g' > temp.log
second iteration
cut system info,  print the first word sort and count per uniq hit save to uniq.log
cut -c31- temp.log | awk '{print $1}' | sort | uniq -c | sort -nr > uniq.log
final iteration
word count -lines the uniq.log
wc -l uniq.log
== 18
HINT: This is not straight forward and may require a number of iterations of filtering. The
purpose of this is to start working out what message parts are static and where information
changes. Discard information that changes.


(c) How many hosts connected to this system?
HINT: Consider the records present for the host 200.54.189.102. What do these tell us ?
cut -c31- temp.log | fgrep 'Received' > received.log
sed -e  's/error: //g' received.log | awk '{print $4}' | sort | uniq -c | sort -n > hosts.uniq
wc -l hosts.uniq
== 61

(d) One group of hosts stands out as there being a large number from within the same /16,
and specifically a single /24 netblock. What are the /16 and the /24 networks?
/16 68.183.0.0 DigitalOcean, LLC (DO-13)
/16 61.177.173.0  CHINANET jiangsu province network
/24 92.255.85.0 Chang Way Technologies Co. Limited
/24 150.95.27.175 ZCOM-THAI
/16 42.193.55.104 Tencent cloud computing (Beijing) Co., Ltd.
..
(e) Which organisation owns these blocks?
(f) Which Autonomous System Number (ASN)13 does this netblock belong to?
whois -h whois.cymru.com "-v 61.177.0.0"
==
4134
(g) What do you think the reason for this could be?
(h) What were the top 10 hosts, based on the number of connection attempts?
tail -10 hosts.uniq 
==
     47 61.177.173.48
     52 61.177.173.46
     56 61.177.173.36
     61 61.177.173.51
     77 104.41.60.238
     91 103.103.200.20
    129 92.255.85.69
    130 42.193.55.104
    133 92.255.85.70
    143 150.95.27.175

(i) Build up a list of usernames tried against this system. How many are there?
cut -c31- temp.log | fgrep 'invalid user' | awk '{print $4}' | sort | uniq -c | sort -nr > usernames.uniq
wc -l usernames.uniq   
== 
341 usernames.uniq


(j) What were the top 10 usernames (buy number of occurrences) attempted?
head -10 usernames.uniq
==
    116 admin
     36 user
     36 test
     31 attempts
     28 invalid
     22 pi
     17 support
     16 ubuntu
     13 ftpuser
     12 default

(k) For the username pi, how many different (unique) hosts tried this?
ut -c31- temp.log | fgrep -w 'Invalid user pi from' | awk '{print $5}' | sort | uniq -c | wc -l
== 
13
(l) How many of the usernames appear on the list of default accounts used by the Mirai IOT
worm?
admin
user
support
ftpuser
ubnt
user1
guest
username
testuser
tech
service
administrator
user4
user3
user2
supervisor
superuser
srvadmin
phpmyadmin
ossuser
ncuser
mysqladmin
gpadmin
ftproot
ftpadmin
ec2-user
cvsuser
cvsroot
apache_user
admin@fashion

HINT: This means searching for each of the names that exist in your list with those appearing in the Mirai list.
(m) Building on your solutions for h) and j) produce a script called details.sh. This should be
able to be executed with a filename (you can assume the same format as you are working
with) passed on the command line, and it should produce a listing of top 10 usernames
and top 10 hosts. For example: details.sh sshlog-june22.log
==
!/usr/bin/bash

sed -e  's/ \[preauth]/ /g' $*  | sed -e  's/sshd\[.*\]/sshd/g' | cut -c31- | fgrep 'invalid user' | awk '{print $4}' | sort | uniq -c | sort -nr | head -10

sed -e  's/ \[preauth]/ /g' $*  | sed -e  's/sshd\[.*\]/sshd/g' | cut -c31- | fgrep 'Received' | sed -e  's/error: //g' | awk '{print $4}' | sort | uniq -c | sort -nr | head -10 
