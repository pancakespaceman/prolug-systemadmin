# Unit 6 Lab - Firewalls

## Resources

Extra Lab

https://killercoda.com/het-tanis/course/Linux-Labs/205-setting-up-uncomplicated-firewall-UFW


(Killercoda labs)[https://killercoda.com/learn]
(firewalld doc)[https://firewalld.org/documentation/]
(RHEL firewalld)[https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_firewalls_and_packet_filters/using-and-configuring-firewalld_firewall-packet-filters]

## Pre-Lab Warm-up

Exercises

1. `cd ~`
2. `pwd (should be /home/<yourusername>)`
3. `cd /tmp`
4. `pwd (should be /tmp)`
5. `cd`
6. `pwd (should be /home/<yourusername>)`
7. `mkdir lab_firewalld`
8. `cd lab_firewalld`
9. `touch testfile1`
10. `ls`
11. `touch testfile{2..10}`
12. `ls`
13. `seq 10`
14. `seq 1 10`
15. `seq 1 2 10`
	- man seq and see what each of those values mean. It’s important to know the 
	behavior if you intend to ever use the command, as we often do with counting (for) loops.


No worries, there are two ways to fix the mess you've made. 
Nothing you've done is permanent, so logging out and reloading a 
shell (logging back in) would fix this.
We just put the aliases back.

16. `for i in seq 1 10; do touch file$i; done;`
17. `ls`
	- Think about some of those commands and when you might use them. 
	Try to change command #15 to remove all of those files (rm -rf file$i)

DONE

## Lab

This lab is designed to help you get familiar with the basics of the systems you 
will be working on.

Some of you will find that you know the basic material but the techniques here 
allow you to put it together in a more complex fashion.

It is recommended that you type these commands and do not copy and paste them. 
Browsers sometimes like to format characters in a way that doesn't always play nice with Linux.


### Check Firewall Status and settings

A very important thing to note before starting this lab. You’re connected into that 
server on ssh via port 22. If you do anything to lockout port 22 in this lab, 
you will be blocked from that connection and we’ll have to reset it.

1. Check firewall status

```
systemctl status firewalld

[root@rocky17 ~]# systemctl status firewalld
● firewalld.service - firewalld - dynamic firewall daemon
     Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; preset: enabled)
     Active: active (running) since Wed 2025-05-07 15:06:14 MST; 1h 19min ago
       Docs: man:firewalld(1)
   Main PID: 765 (firewalld)
      Tasks: 2 (limit: 24465)
     Memory: 24.2M
        CPU: 1.141s
     CGroup: /system.slice/firewalld.service
             └─765 /usr/bin/python3 -s /usr/sbin/firewalld --nofork --nopid

May 07 15:06:13 rocky17 systemd[1]: Starting firewalld - dynamic firewall daemon...
May 07 15:06:14 rocky17 systemd[1]: Started firewalld - dynamic firewall daemon.
```

If necessary start the firewalld daemon

```
systemctl start firewalld
```

Set the firewalld daermon to be persistent through reboots

```
systemctl enable firewalld
```

Verify with systemctl status firewalld again from step 1

FIREWALLD good to go

Check which zones exist:

```
firewall-cmd --get-zones

[root@rocky17 ~]# firewall-cmd --get-zones
block dmz drop external home internal nm-shared public trusted work
```

Checking the values within each zone:

```
firewall-cmd --list-all --zone=public

[root@rocky17 ~]# firewall-cmd --list-all --zone=public
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: eth0
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 9100/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

General Output

```
public (default, active)
interfaces: wlp4s0
sources:
services: dhcpv6-client ssh
ports:
masquerade: no
forward-ports:
icmp-blocks:
rich rules:
```

Checking the active and default zones:

```
firewall-cmd --get-default

[root@rocky17 ~]# firewall-cmd --get-default
public
```

Next Command

```
firewall-cmd --get-active

[root@rocky17 ~]# firewall-cmd --get-active-zones
public
  interfaces: eth0
```

NOTE: this also shows which interface the zone is applied to. Multiple interfaces
and zones can be applied.

So now you know how to see the values in your firewall. Use steps 4 and 5 to check
all the values of the different zones to see how they differ.


### Set the firewall active and default zones:

We know the zones from above, set your firewall to the different active or default
zones. Default zones are the ones that will come up when the fireall is restarted.

NOTE: It may be useful to perform an `ifconfig -a` and note your interfaces for the next part.

```
ifconfig -a | grep -i flags

[root@rocky17 ~]# ifconfig -a | grep -i flags
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
```

	1. Changing the default zones (This is permanent over a reboot, other commands
	require --permanent switch)

```
firewall-cmd --set-default-zone=work

[root@rocky17 ~]# firewall-cmd --set-default-zone=work
success
```

Next Command

```
firewall-cmd --get-active-zones

[root@rocky17 ~]# firewall-cmd --get-active-zone
work
  interfaces: eth0
```

Attempt to set it back to the original public zone and verify. Set it to one other
zone, verify, then set it back to public.

```
[root@rocky17 ~]# firewall-cmd --set-default-zone=public
success
[root@rocky17 ~]# firewall-cmd --get-active-zone
public
  interfaces: eth0
```

Changing interfaces and assigning different zones (use another interface from your
earlier `ifconfig -a`)

```
firewall-cmd --change-interface=virbr0 --zone dmz

[root@rocky17 ~]# firewall-cmd --change-interface=lo --zone dmz
success
[root@rocky17 ~]# firewall-cmd --list-all --zone=dmz
dmz (active)
  target: default
  icmp-block-inversion: no
  interfaces: lo
  sources:
  services: ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
```

Next Command:

```
firewall-cmd --add-source 192.168.11.0/24 --zone=public
```

Next Command:

```
firewall-cmd --get-active-zones

[root@rocky17 ~]# firewall-cmd --add-source 192.168.11.0/24 --zone=public
success
[root@rocky17 ~]# firewall-cmd --get-active-zones
dmz
  interfaces: lo
public
  interfaces: eth0
  sources: 192.168.11.0/24
```

### Working with ports and services

We can be even more granular with our ports and services. We can block or allow services
by port number, or we can assign port numbers to a service name and then block or
allow those service names.

1. List all services assigned in firewalld

```
firewall-cmd --get-services

[root@rocky17 ~]# firewall-cmd --get-services
RH-Satellite-6 RH-Satellite-6-capsule afp amanda-client amanda-k5-client amqp amqps apcupsd audit ausweisapp2 bacula bacula-client bareos-director bareos-filedaemon bareos-storage bb bgp bitcoin bitcoin-rpc bitcoin-testnet bitcoin-testnet-rpc bittorrent-lsd ceph ceph-exporter ceph-mon cfengine checkmk-agent cockpit collectd condor-collector cratedb ctdb dds dds-multicast dds-unicast dhcp dhcpv6 dhcpv6-client distcc dns dns-over-tls docker-registry docker-swarm dropbox-lansync elasticsearch etcd-client etcd-server finger foreman foreman-proxy freeipa-4 freeipa-ldap freeipa-ldaps freeipa-replication freeipa-trust ftp galera ganglia-client ganglia-master git gpsd grafana gre high-availability http http3 https ident imap imaps ipfs ipp ipp-client ipsec irc ircs iscsi-target isns jenkins kadmin kdeconnect kerberos kibana klogin kpasswd kprop kshell kube-api kube-apiserver kube-control-plane kube-control-plane-secure kube-controller-manager kube-controller-manager-secure kube-nodeport-services kube-scheduler kube-scheduler-secure kube-worker kubelet kubelet-readonly kubelet-worker ldap ldaps libvirt libvirt-tls lightning-network llmnr llmnr-client llmnr-tcp llmnr-udp managesieve matrix mdns memcache minidlna mongodb mosh mountd mqtt mqtt-tls ms-wbt mssql murmur mysql nbd nebula netbios-ns netdata-dashboard nfs nfs3 nmea-0183 nrpe ntp nut opentelemetry openvpn ovirt-imageio ovirt-storageconsole ovirt-vmconsole plex pmcd pmproxy pmwebapi pmwebapis pop3 pop3s postgresql privoxy prometheus prometheus-node-exporter proxy-dhcp ps2link ps3netsrv ptp pulseaudio puppetmaster quassel radius rdp redis redis-sentinel rootd rpc-bind rquotad rsh rsyncd rtsp salt-master samba samba-client samba-dc sane sip sips slp smtp smtp-submission smtps snmp snmptls snmptls-trap snmptrap spideroak-lansync spotify-sync squid ssdp ssh steam-streaming svdrp svn syncthing syncthing-gui syncthing-relay synergy syslog syslog-tls telnet tentacle tftp tile38 tinc tor-socks transmission-client upnp-client vdsm vnc-server warpinator wbem-http wbem-https wireguard ws-discovery ws-discovery-client ws-discovery-tcp ws-discovery-udp wsman wsmans xdmcp xmpp-bosh xmpp-client xmpp-local xmpp-server zabbix-agent zabbix-server zerotier
```

This next part is just to show you where the service definitions exist. They are
simple xml format and can easily be manipulated or changed to make new services.
This would require a restart of firewalld service to re-read this directory.

Next Command

```
ls /usr/lib/firewalld/services/
```

Next Command

```
cat /usr/lib/firewalld/services/http.xml

[root@rocky17 ~]# cat /usr/lib/firewalld/services/http.xml
<?xml version="1.0" encoding="utf-8"?>
<service>
  <short>WWW (HTTP)</short>
  <description>HTTP is the protocol used to serve Web pages. If you plan to make your Web server publicly available, enable this option. This option is not required for viewing pages locally or developing Web pages.</description>
  <port protocol="tcp" port="80"/>
</service>
```

2. Adding a service or port to a zone

Ensuring we are working on a public zone

```
firewall-cmd --set-default-zone=public
```

Listing Services

```
firewall-cmd --list-services

[root@rocky17 ~]# firewall-cmd --list-services
cockpit dhcpv6-client ssh
```

NOTE: we have 2 services

Permanently adding a service with the `--permanent` switch

```
firewall-cmd --permanent --add-service ftp

[root@rocky17 ~]# firewall-cmd --permanent --add-service ftp
success
```

Reloading

```
firewall-cmd --reload
```

Verifying we are in the correct Zone

```
firewall-cmd --get-default-zone

[root@rocky17 ~]# firewall-cmd --get-default-zone
public
```

Verify that we have successfully added the FTP service

```
firewall-cmd --list-services

[root@rocky17 ~]# firewall-cmd --list-services
cockpit dhcpv6-client ftp ssh
```

Alternative, we can do almost the same thing but not use a defined service name.
If I just want to allow port 1147 through for T CP traffic, it is very simple as well.

```
firewall-cmd --permanent --add-port=1147/tcp
```

Reloading once again

```
firewall-cmd --reload
```

Listing open ports now

```
firewall-cmd --list-ports

[root@rocky17 ~]# firewall-cmd --permanent --add-port=1147/tcp
success
[root@rocky17 ~]# firewall-cmd --reload
fisuccess
[root@rocky17 ~]# firewall-cmd --list-ports
1147/tcp 9100/tcp
```

3. Removing unwanted services or ports

To remove those values and permanently fix the configuration back we simply
use remove.

Firstly, we will permanently remove ftp service.

```
firewall-cmd --permanent --remove-service=ftp
```

Then we will permanently remove the ports

```
firewall-cmd --permanent --remove-port=1147/tcp
```

Now lets do a reload

```
firewall-cmd --reload
```

Now we can list services again to confirm our work

```
firewall-cmd --list-services
```

Now we can list ports

```
firewall-cmd --list-ports


[root@rocky17 ~]# firewall-cmd --permanent --remove-service=ftp
success
[root@rocky17 ~]# firewall-cmd --permanent --remove-port=1147/tcp
success
[root@rocky17 ~]# firewall-cmd --reload
success
[root@rocky17 ~]# firewall-cmd --list-services
cockpit dhcpv6-client ssh
[root@rocky17 ~]# firewall-cmd --list-ports
9100/tcp
```

Before making any more changes I recommend running the `list` commands above with
``>> /tmp/firewall.org` on them so you have all your original values saved somewhere
in case you need them.

So now take this and set up some firewalls on the interfaces of your system.
Change the default ports and services assigned to your different zones (at least
3 zones). Read the `man firewall-cmd` command or `firewall-cmd -help` to see if
there are any other useful things you should know.


