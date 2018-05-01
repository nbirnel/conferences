Gary Smith
Cyber Security Analyst
Pacific Northwest National Laboratory

fwlogwatch turns syslogs into a nice attack report, or text outputs

What if you have a lot (60K) of CIDRs to block?
ipset - "match extension" for iptables. Stores rules as a fast hash.

running one per hour *and* once per day can catch both mass attacks,
and slow goers.

dynamically ban hosts:
'target extension' 

-A INPUT -p tcp -m multiport -dports 23,1433,2323 -j SET --add-set blacklist src
-A INPUT -m set --match-set blacklist src -j LOG --log-prefix "IP Blacklisted INPUT"
-A INPUT -m set --match-set blacklist src -j DROP

ipset create blacklist-tcp-ports bitmap:port range 0-65535
ipset add blacklist-tcp-ports 23
ipset add blacklist-tcp-ports 1344


tweak1:
Add a whitelist - eg if a client scans you. Put the whitelist first (first in
wins)
ipset create whitelist hash:net 

tweak2:
-A OUTPUT -m set --match-set blacklist dst -LOG --log-prefix "IP Blacklisted OUTPUT"
don't drop it - then they know you are on to them


fail2ban
