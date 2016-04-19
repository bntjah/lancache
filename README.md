LAN-Cache v1.1.3
==============

Based off work of https://gitlab.com/frag-o-matic/lan-cache
So Credits go to Bruno Gysels and MultiPlay.co.uk for the base they made!

Credits for this work of art as of 24 Febr. 2016 is to NexusofDoom !!!

OS: Ubuntu 14.04 x64

If you are to lazy to read below you can use the script I created for this: [https://github.com/bntjah/lc-installer](My LC-Installer) (warning might contain bugs!)

	1) sudo apt-get install build-essential libpcre3 libpcre3-dev zlib1g-dev libreadline-dev libncurses5-dev git
	2) sudo nano /etc/dhcp/dhclient.conf
	- 2.1 Add the lines: prepend domain-name-servers 8.8.8.8, 8.8.4.4;
	3) git clone -b master http://github.com/bntjah/lancache
	4) curl http://nginx.org/download/nginx-1.9.9.tar.gz | tar zx
	5) ./configure --with-http_ssl_module --with-http_slice_module
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
	hosts file, bind config and necessary individual edits to db.* files should be mentioned.
	10) Create the user lancache
		sudo adduser --system --no-create-home lancache
		sudo addgroup --system lancache
		sudo usermod -aG lancache lancache
	
	11) Just create the folders:
		sudo mkdir -p /srv/lancache/data/blizzard
		sudo mkdir -p /srv/lancache/data/microsoft
		sudo mkdir -p /srv/lancache/data/installs
		sudo mkdir -p /srv/lancache/data/other
		sudo mkdir -p /srv/lancache/data/tmp
		sudo mkdir -p /srv/lancache/data/hirez/
		sudo mkdir -p /srv/lancache/data/origin/
		sudo mkdir -p /srv/lancache/data/riot/
		sudo mkdir -p /srv/lancache/data/sony/
		sudo mkdir -p /srv/lancache/data/steam/
		sudo mkdir -p /srv/lancache/logs
		sudo mkdir -p /srv/lancache/data/wargaming
		sudo mkdir -p /srv/lancache/data/tera
		sudo mkdir -p /srv/lancache/data/arenanetworks
		
	- 11.1 chowning can be achieved by: 
		sudo chmod -R 755 /srv/lancache

	12) Copy the conf folder and contents (where you originally git cloned it to in step 4) to /usr/local/nginx/conf/
	13) Copy the Lancache file from init.d/ to /etc/init.d/ by:
		sudo cp lancache /etc/init.d/lancache
	14) Make it an executable:
		sudo chmod +x /etc/init.d/lancache
	15) Put it in the standard Boot:
		sudo update-rc.d lancache defaults
	16) Start Lancache / Nginx by:
		sudo /etc/init.d/lancache start
	

	Optional A) Monitor Through nload
		-A.1 sudo apt-get install nload -y
		-A.2 sudo nload -U G - u M -i 102400 -o 102400
	Optional B) Monitor Network Usage Through iftop
		-B.1 sudo apt-get install iftop -y
		-B.2 sudo iftop -i eth1
		Note ETH1 is the Interface I've defined for Lancache to use
		
Note for LOL:
"The latest released client has included an HTTP downgrade for RFC1918 (or local IP) addresses. But as this instance of lancache has never recommended route poisoning over DNS spoofing, I think we're ok without." -Stealthii
