# Unit 6 Worksheet - Firewalls

## Resources / Important Links

- (Official Firewalld Documentation)[https://firewalld.org/documentation/]
- (RedHat Firewalld Documentation)[https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9/html/configuring_firewalls_and_packet_filters/using-and-configuring-firewalld_firewall-packet-filters]
- (Official UFW Documentation)[https://help.ubuntu.com/community/UFW]
- (Wikipedia entry for Next-Gen Firewalls)[https://en.wikipedia.org/wiki/Next-generation_firewall]

## Discussion Posts

### 1

Scenario:

A ticket has come in from an application team. Some of the servers your team built 
for them last week have not been reporting up to enterprise monitoring and they 
need it to be able to troubleshoot a current issue, but they have no data. 
You jump on the new servers and find that your engineer built everything correctly 
and the agents for node_exporter, ceph_exporter and logstash exporter that your 
teams use. But, they also have adhered to the new company standard of firewalld 
must be running. No one has documented the ports that need to be open, so you’re 
stuck between the new standards and fixing this problem on live systems.

Next, answer these questions here:

https://prometheus.io/docs/guides/node-exporter/


1. As you’re looking this up, what terms and concepts are new to you?

```
Prometheus, node exporter, ceph exporter, logstash
```

2. What are the ports that you need to expose? How did you find the answer?

```
These are default port values. You may need to check if your org uses different
ports from the default.

Node Exporter needs port 9100 (documentation)[https://prometheus.io/docs/guides/node-exporter/]
Ceph Exporter needs port 9283 (documentation)[https://docs.ceph.com/en/quincy/mgr/prometheus/]
logstash needs port 9600 (documentation)[https://docs.bitnami.com/aws/apps/elk/get-started/understand-default-config]
If you're using beats with logstash, you'll also need port 5044 (documentation)[https://www.elastic.co/docs/reference/beats/winlogbeat/logstash-output]
```

3. What are you going to do to fix this on your firewall?

```
add these ports to firewalld to allow traffic.

sudo firewall-cmd --permanent --add-port=9100/tcp
sudo firewall-cmd --permanent --add-port=9283/tcp
sudo firewall-cmd --permanent --add-port=9600/tcp
sudo firewall-cmd --permanent --add-port=5044/tcp
```

### 2


Scenario:

A manager heard you were the one that saved the new application by fixing the firewall.
They get your manager to approach you with a request to review some documentation 
from a vendor that is pushing them hard to run a WAF in front of their web application. 
You are “the firewall” guy now, and they’re asking you to give them a review of the 
differences between the firewalls you set up (which they think should be enough to protect them) 
and what a WAF is doing.

1. What do you know about the differences now?

```
As of reading this I don't really understand what are the differences.
My first guess would be that a WAF allows for more fine tuned control and monitoring
for web applications.
```

2. What are you going to do to figure out more?

```
Find documentation and articles on both firewalld and WAFs to better understand their
use cases.
Such as this one https://www.fortinet.com/resources/cyberglossary/waf-vs-firewall
```

3. Prepare a report for them comparing it to the firewall you did in the first discussion.

```
Currently we are using firewalld on our systems.


Firewalld is a network-level firewall that allows us to monitor traffic coming in
or going out of our network.
Firewalld CAN:
	- allow/block traffic based on IP address, port, and protocols
Firewalld CANNOT:
	- see the content inside of a request, such as an HTTP request.
		- this means it cannot protect against web specific attacks.


Web Application Firewalls (WAF) specifically monitor all web based communications
(HTTP/HTTPS)
WAFs CAN:
	- defend against attacks like cookie manipulation, SQL injection, cross-site
	scripting, and distributed denial-of-service

Including a WAF in our line of defense will greatly reduce the risk of our company's
web application falling victim to web based attacks.
```

### Responses

[x] Question 1 Done
[x] Question 2 Done

## Definitions

Firewall:
	- A firewall is a network security device that monitors and filters incoming
	and outgoing network traffic based on an organization's previously established
	security policies. It acts as a barrier between a trusted internal network and 
	untrusted external networks, such as the internet.
	- https://www.cisco.com/site/us/en/learn/topics/security/what-is-a-firewall.html#tabs-a107e9a621-item-6caff3e5bb-tab
	- https://www.fortinet.com/resources/cyberglossary/firewall

Zone:
	- In network security, a zone refers to a segmented section of a network that
	contains systems and components with similar security requirements. Each zone
	is governed by specific policies and protocols to manage and control traffic,
	enhancing the overall security posture.
	- https://www.techslang.com/definition/what-is-a-security-zone/
	- https://www.juniper.net/documentation/us/en/software/junos/security-policies/topics/topic-map/security-zone-configuration.html

Service:
	- In the context of network security, a service refers to a specific function
	or application that runs on a network, such as email, web hosting, or file
	sharing. Securing these services involves implementing measures to protect
	against unauthorized access and vulnerabilities.
	- https://www.tutorialspoint.com/data_communication_computer_network/services_of_network_security.htm
	- https://www.cisco.com/c/en/us/solutions/enterprise-networks/what-are-network-services.html

DMZ:
	- A DMZ (Demilitarized Zone) is a physical or logical subnetwork that separates
	an internal local area network (LAN) from untrusted external networks, such
	as the internet. It contains and exposes an organization's external-facing
	services, adding an additional layer of security to the internal network.
	- https://www.fortinet.com/resources/cyberglossary/what-is-dmz
	- https://www.techtarget.com/searchsecurity/definition/DMZ

Proxy:
	- A proxy server acts as an intermediary between a client requesting a resource
	and the server providing that resource. It can be used to enhance security,
	manage traffic, and provide anonymity by masking the client's IP address.
	- https://www.digitalguardian.com/blog/what-proxy-server-definition-how-it-works-more
	- https://www.geeksforgeeks.org/what-is-proxy-server/

Stateful packet filtering:
	- Stateful packet filtering monitors the state of active connections and
	makes decisions based on the context of the traffic. It keeps track of the state
	of network connections, such as TCP streams, and only allows packets that match
	a known active connection.
	- https://www.fortinet.com/resources/cyberglossary/stateful-firewall
	- https://www.checkpoint.com/cyber-hub/network-security/firewall-as-a-service-fwaas/what-is-a-stateful-packet-inspection-firewall/

Stateless packet filtering:
	- Stateless packet filtering examins each packet in isolation, without considering
	the state of the connection. Decisions are made based solely on predefined
	rules, such as source and destination IP addresses, ports, and protocols.
	- https://www.checkpoint.com/cyber-hub/network-security/firewall-as-a-service-fwaas/what-is-a-stateful-packet-inspection-firewall/
	- https://www.checkpoint.com/cyber-hub/network-security/what-is-firewall/what-is-a-stateless-firewall/

WAF:
	- A Web Application Firewall (WAF) is a security system that protects web
	applications by monitoring and filtering HTTP traffic between a web application
	and the internet. It helps defend against attacks such as cross-site scripting
	(XSS), SQL injection, and other web application vulnerabilities.
	- https://www.cloudflare.com/learning/ddos/glossary/web-application-firewall-waf/
	- https://owasp.org/www-community/Web_Application_Firewall

NGFW:
	- A Next-Generation Firewall (NGFW) is an advanced firewall that provides
	capabilties beyond traditional firewalls. It includes features like application
	awareness and control, integrated intrusion prevention, and cloud-delivered
	threat intelligence to detect and prevent modern cyber threats.
	- https://www.cisco.com/site/us/en/learn/topics/security/what-is-a-next-generation-firewall.html#tabs-35d568e0ff-item-4bd7dc8124-tab
	- https://www.cloudflare.com/learning/security/what-is-next-generation-firewall-ngfw/

## Digging Deeper

1. Read https://docs.rockylinux.org/zh/guides/security/firewalld-beginners/
	- What new things did you learn that you didn’t learn in the lab?
	- What functionality of firewalld are you likely to use in your professional work?



### What is a firewall

https://www.fortinet.com/resources/cyberglossary/firewall

Can be both hardware and software
they inspect data packets and determine whether to allow or block them based
on a set of rules.

Prevent bad actors from overloading or infiltrating a private network.
Traditionally firewalls form a secure perimeter around a network or computer.
	- today we need a layered approach due to  cloud computing, hybrid work
	environments, etc.


### What is a Web Application Firewall

https://www.fortinet.com/products/web-application-firewall/fortiweb/what-is-waf

###

## Reflection Questions

1. What questions do you still have about this week?

2. How are you going to use what you’ve learned in your current role?

