LAN-Cache v1.1.3
==============

Based off work of https://gitlab.com/frag-o-matic/lan-cache
So Credits go to Bruno Gysels and MultiPlay.co.uk for the base they made!

Credits for this work of art as of 24 Febr. 2016 is to NexusofDoom !!!

OS: Ubuntu 14.04 x64

If you are to lazy to read below you can use the script I created for this: [https://github.com/bntjah/lc-installer](My LC-Installer) (warning might contain bugs!)

	1) sudo apt-get install build-essential libpcre3 libpcre3-dev zlib1g-dev libreadline-dev libncurses5-dev git libssl-dev
	2) sudo nano /etc/dhcp/dhclient.conf
	- 2.1 Add the lines: prepend domain-name-servers 8.8.8.8, 8.8.4.4;
	3) git clone -b master http://github.com/bntjah/lancache
	4) curl http://nginx.org/download/nginx-1.9.9.tar.gz | tar zx
	5) ./configure --with-http_ssl_module --with-http_slice_module
	6) sudo make
	7) sudo make install
	8) *grab a coffee right here*
	9) Creating the necessary ip's
		9.1) Paste the following in ~/addips.sh
		# Ip used for STEAM caching
		sudo /sbin/ip addr add 10.0.1.11/16 dev eth0
		# Ip used for RIOT caching
		sudo /sbin/ip addr add  10.0.1.12/16 dev eth0
		# Ip used for Blizzard caching
		sudo /sbin/ip addr add  10.0.1.13/16 dev eth0
		# Ip used for Hirez caching
		sudo /sbin/ip addr add  10.0.1.14/16 dev eth0
		# Ip used for Origin caching
		sudo /sbin/ip addr add  10.0.1.15/16 dev eth0
		# Ip used for Sony caching
		sudo /sbin/ip addr add  10.0.1.16/16 dev eth0
		# Ip used for Microsoft caching
		sudo /sbin/ip addr add  10.0.1.17/16 dev eth0
		# Ip used for Tera caching
		sudo /sbin/ip addr add  10.0.1.18/16 dev eth0
		# Ip used for GOG caching
		sudo /sbin/ip addr add  10.0.1.19/16 dev eth0
		# Ip used for ArenaNetworks caching
		sudo /sbin/ip addr add  10.0.1.20/16 dev eth0
		# Ip used for WarGaming caching
		sudo /sbin/ip addr add  10.0.1.21/16 dev eth0
		9.2) sh ~/addips.sh
		9.3) Check if all ips are there with
		ip addr show

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
