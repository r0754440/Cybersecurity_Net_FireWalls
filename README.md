# Cybersecurity_Net_FireWalls

#Taak gedeelte van firewal

# Apache
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT

#ProFTPD
iptables -A INPUT -p tcp --match multiport --dport 20,21 -j ACCEPT
iptables -A OUTPUT -p tcp --match multiport --dport 20,21 -j ACCEPT

#bind9
iptables -A INPUT -p udp --dport 53 -j ACCEPT
iptables -A OUTPUT -p udp --dport 53 -j ACCEPT

#not allow zone transfers
iptables -A INPUT -p udp --dport 53 -m state --state ESTABLISHED,RELATED -j DROP
iptables -A OUTPUT -p udp --dport 53 -m state --state ESTABLISHED,RELATED -j DROP

#prevent ping flooding
iptables -t filter -A INPUT -p icmp --icmp-type echo-request -m limit --limit 5/minute -j ACCEPT
iptables -t filter -A INPUT -p icmp -j DROP
iptables -t filter -A OUTPUT -p icmp --icmp-type echo-reply -j ACCEPT

#Not allowed to make ongoing connection except securiy updates
iptables -A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A OUTPUT -p tcp --dport 80 -m state --state NEW -j ACCEPT
iptables -A OUTPUT -p tcp --dport 53 -m state --state NEW -j ACCEPT
iptables -A OUTPUT -p udp --dport 53 -m state --state NEW -j ACCEPT
iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT

#De laatste regel

iptables -P INPUT DROP
iptables -P OUTPUT DROP 



#Einde taak
