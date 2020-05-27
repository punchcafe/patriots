### Setting up traffic forwarding on interfaces
enable forwarding traffic by uncommenting:
```
# etc/sysctl.conf

net.ipv4.ip_forward=1
```
reboot to see effects.
forwarding is set with 
```
sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE
# where wlan0 is the name of your outfacing interface
```


### Setting default gateways on local clusters
Default gateways need to be configured correctly for both interfaces on 
the primary cluster, wlan0 and eth0. current config:
```
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.1.1     0.0.0.0         UG    0      0        0 wlan0
0.0.0.0         192.168.0.10    0.0.0.0         UG    202    0        0 eth0
192.168.0.0     0.0.0.0         255.255.255.0   U     202    0        0 eth0
192.168.1.0     0.0.0.0         255.255.255.0   U     303    0        0 wlan0
```
```
sudo route add default gw 192.168.0.10
```
additionally, one can use 
```
sudo route -n
```
to view visible routes.

### dnsmasq config
```
#/etc/dnsmasq.d/dns.conf

domain-needed
bogus-priv
no-resolv
no-poll

server=192.168.1.1
#local dns
```