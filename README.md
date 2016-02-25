LAN-Cache v1.1.2
==============

Based off work of https://gitlab.com/frag-o-matic/lan-cache
So Credits go to Bruno Gysels and MultiPlay.co.uk for the base they made!

Credits for this work of art in Development Branch as of 24 Febr. 2016 is to NexusofDoom !!!

OS: Ubuntu 14.04 x64


	1) sudo apt-get install build-essential libpcre3 libpcre3-dev zlib1g-dev libreadline-dev libncurses5-dev libpcre3-dev git
	2) sudo nano /etc/dhcp/dhclient.conf
	- 2.1 Add the lines: prepend domain-name-servers 8.8.8.8, 8.8.4.4;
	3) git clone -b development http://github.com/bntjah/lancache
	4) curl http://nginx.org/download/nginx-1.9.9.tar.gz | tar zx
	5) ./configure --with-http_slice_module
	6) sudo make
	7) sudo make install
	8) *grab a coffee right here*
	9) sudo nano /etc/network/interfaces
	- 9.1 Add the following:
		auto eth1:1
		iface eth1:1 inet static
        	address 192.168.1.91
        	netmask 255.255.255.0

		auto eth1:2
		iface eth1:2 inet static
        	address 192.168.1.92
        	netmask 255.255.255.0

		auto eth1:3
		iface eth1:3 inet static
        	address 192.168.1.93
        	netmask 255.255.255.0

		auto eth1:4
		iface eth1:4 inet static
        	address 192.168.1.94
        	netmask 255.255.255.0
	
		auto eth1:5
		iface eth1:5 inet static
        	address 192.168.1.95
        	netmask 255.255.255.0

		auto eth1:6
		iface eth1:6 inet static
        	address 192.168.1.96
        	netmask 255.255.255.0

		auto eth1:7
		iface eth1:7 inet static
        	address 192.168.1.97
        	netmask 255.255.255.0

		auto eth1:8
		iface eth1:8 inet static
        	address 192.168.1.98
        	netmask 255.255.255.0

		auto eth1:9
		iface eth1:9 inet static
        	address 192.168.1.99
        	netmask 255.255.255.0

		auto eth1:10
		iface eth1:10 inet static
        	address 192.168.1.90
        	netmask 255.255.255.0

		auto eth1:11
		iface eth1:11 inet static
        	address 192.168.1.70
        	netmask 255.255.255.0

	Note to self: Should make a script for the Step 10)
	10) Just create the folders: /srv/lancache/ 
	
		sudo mkdir -p /srv/lancache/blizzard
		sudo mkdir -p /srv/lancache/data/installs
		sudo mkdir -p /srv/lancache/data/other
		sudo mkdir -p /srv/lancache/data/tmp
		sudo mkdir -p /srv/lancache/hirez/
		sudo mkdir -p /srv/lancache/origin/
		sudo mkdir -p /srv/lancache/riot/
		sudo mkdir -p /srv/lancache/sony/
		sudo mkdir -p /srv/lancache/steam/
		sudo mkdir -p /srv/lancache/logs
		sudo mkdir -p /srv/lancache/wargaming
		sudo mkdir -p /srv/lancache/tera
		sudo mkdir -p /srv/lancache/arenanetworks
		
	- 10.1 chowning can be achieved by: 
		sudo chmod -R 755 /srv/lancache

	11) Copy the conf folder and contents (where you originally git cloned it to in step 4) to /usr/local/nginx/conf/
	12) echo Start Nginx with the following
	- 12.1 cd /usr/local/nginx/sbin/
	- 12.2 sudo ./nginx

	Optional A) Monitor Through nload
		-A.1 sudo apt-get install nload -y
		-A.2 sudo nload -U G - u M -i 102400 -o 102400
	Optional B) Monitor Network Usage Through iftop
		-B.1 sudo apt-get install iftop -y
		-B.2 sudo iftop -i eth1
		Note ETH1 is the Interface I've defined for Lancache to use
		

