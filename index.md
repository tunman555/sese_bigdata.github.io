---
layout: home
title: Surv data pipeline
---

 # OS and system installation

1. Download fedora image from https://getfedora.org/en/workstation/download/ and create USB installation via software ex. rufus for example ==> https://linuxhint.com/install-fedora-workstation-35-usb/

Config each of NIC in path /etc/sysconfig/network-scripts/ to suit your operation ==> https://www.tecmint.com/set-add-static-ip-address-in-linux/

Config static route ==> https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/networking_guide/sec-configuring_static_routes_with_ip_commands

install vsftpd to jail ftp if you want ==> https://phoenixnap.com/kb/how-to-setup-ftp-server-install-vsftpd-centos-7

config ntp server to get time syncing from time server
Allowing Firewall

Allowing multicast protocol : firewall-cmd --zone=zone-name --add-protocol=igmp
config sshd server


# Network configuration 

1. Wiring : 4 wires ~ 25 m from SE.SE. server to AO.AS. Server and 3 lines to MOPS  and service lan

2. Network device (TMCS switch):
	Port Gi2/0/14 on OPS and service lan to send data
	service lan : trunk allow 130 146
	ops lan :access port 18

3. Network configuarion and routing persistance
	Network interface card (NIC) :
	-	1st ensf40 interface 10.2.130.201 connect to tmcs service lan
	-	2nd enp10s0f1 interface 10.2.18.201 connect to tmcs operational lan
	-	3rd interface 10.2.18.201 connect to tmcs operational lan
	-	4th enp10s0f0 interface 10.96.100.11 connect to Ao.As. network

	Routing :
	-	0.0.0.0 gw 10.2.130.250 via ensf40
	-	10.2.18.0 gw 0.0.0.0 via enp10s0f1
	-	10.2.130.0 gw 0.0.0.0 via ensf40
	-	10.2.146.0 gw 0.0.0.0 via ensf40.146
	-	10.96.100.0 gw 10.96.100.11 via enp10s0f0
	-	172.16.129.0 gw 10.96.100.11 via enp10s0f0
	-	224.8.48.0 gw 10.2.18.250 via enp10s0f1
	https://phoenixnap.com/kb/how-to-setup-ftp-server-install-vsftpd-centos-7

Security 
	(VSFTP) :
	-	install vsftpd service 
	-	config vsftpd.conf jail in there root dir
	-	add user to get files (ftper/sesedtg)
	-	change permission to dir 
	Firewall (UFW) policy :
	-	allow incoming-outgoing from EOP03(10.2.130.18)
	-	allow incoming-outgoing from big dataserver 10.96.x.x
	-	deny incoming from any
	Allow  protocol on fedora
	-	sudo firewall-cmd --permanent --direct --add-rule ipv4 filter INPUT 0 --destination 224.0.0.0/4 -j ACCEPT
	-	sudo firewall-cmd --reload

	Time synchronization (NTP) :
	-	install ntpd service 
	-	ntp.conf to syncing time from cdp server

	ssh server
	-	by config sshd.conf to allow and blocking some user in list etc.

