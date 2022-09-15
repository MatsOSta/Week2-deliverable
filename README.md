# Week2-deliverable


(a) How many lines does the file contain? // wc -l auth.log            
== 5564 auth.log


(b) How many different types of message are present?
HINT: This is not straight forward and may require a number of iterations of filtering. The
purpose of this is to start working out what message parts are static and where information
changes. Discard information that changes.
(c) How many hosts connected to this system?
HINT: Consider the records present for the host 200.54.189.102. What do these tell us ?
(d) One group of hosts stands out as there being a large number from within the same /16,
and specifically a single /24 netblock. What are the /16 and the /24 networks?
(e) Which organisation owns these blocks?
(f) Which Autonomous System Number (ASN)13 does this netblock belong to?
(g) What do you think the reason for this could be?
(h) What were the top 10 hosts, based on the number of connection attempts?
(i) Build up a list of usernames tried against this system. How many are there?
(j) What were the top 10 usernames (buy number of occurrences) attempted?
(k) For the username pi, how many different (unique) hosts tried this?
(l) How many of the usernames appear on the list of default accounts used by the Mirai IOT
worm?
HINT: This means searching for each of the names that exist in your list with those appearing in the Mirai list.
(m) Building on your solutions for h) and j) produce a script called details.sh. This should be
able to be executed with a filename (you can assume the same format as you are working
with) passed on the command line, and it should produce a listing of top 10 usernames
and top 10 hosts. For example: details.sh sshlog-june22.log
