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

1. As you’re looking this up, what terms and concepts are new to you?

2. What are the ports that you need to expose? How did you find the answer?

3. What are you going to do to fix this on your firewall?

### 2


Scenario:

A manager heard you were the one that saved the new application by fixing the firewall.
They get your manager to approach you with a request to review some documentation 
from a vendor that is pushing them hard to run a WAF in front of their web application. 
You are “the firewall” guy now, and they’re asking you to give them a review of the 
differences between the firewalls you set up (which they think should be enough to protect them) 
and what a WAF is doing.

1. What do you know about the differences now?

2. What are you going to do to figure out more?

3. Prepare a report for them comparing it to the firewall you did in the first discussion.

## Definitions

Firewall:

Zone:

Service:

DMZ:

Proxy:

Stateful packet filtering:

Stateless packet filtering:

WAF:

NGFW:

## Digging Deeper

1. Read https://docs.rockylinux.org/zh/guides/security/firewalld-beginners/
	- What new things did you learn that you didn’t learn in the lab?
	- What functionality of firewalld are you likely to use in your professional work?


## Reflection Questions

1. What questions do you still have about this week?

2. How are you going to use what you’ve learned in your current role?

