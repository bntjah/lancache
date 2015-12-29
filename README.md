LAN-Cache v1.0
==============
Based off work of https://gitlab.com/frag-o-matic/lan-cache
By Bruno Gysels

OS: Ubuntu 14.04

1) 	Install following packages

		apt-get install libreadline-dev libncurses5-dev libpcre3-dev libssl-dev perl make

2) 	Create User Lancache
		sudo adduser --system --no-create-home lancache
		sudo addgroup --system lancache
		sudo usermod -aG lancache lancache

3)      Install BIND
        sudo apt-get install bind9
        Copy configuration files from bind/ folder to /etc/bind change the IP addresses if necessary

4)      Copy hosts file to /etc/hosts
        Copy interfaces file to /etc/network/interfaces change the IP addresses if necessary

5)      Configure and compile LuaJIT
        ./configure
        make
        sudo make install

6)      Configure and compile openresty
        ./configure --prefix=/opt/openresty
        make
        sudo make install
        
6)      Copy limits.conf to /etc/security/limits.conf

7)	Before starting make sure the following folders exist and are chowned by lancache
	Since all the data will be stored under /srv/ I suggest you make this a seperate drive
		/srv/lancache
		
		/srv/lancache/blizzard
		
		/srv/lancache/blizzard/logs
		
		/srv/lancache/blizzard/temp
		
		/srv/lancache/data/installs
		
		/srv/lancache/data/other
		
		/srv/lancache/data/tmp
		
		/srv/lancache/hirez/
		
		/srv/lancache/hirez/logs
		
		/srv/lancache/hirez/temp
		
		/srv/lancache/origin/
		
		/srv/lancache/origin/logs
		
		/srv/lancache/origin/temp
		
		/srv/lancache/riot/
		
		/srv/lancache/riot/logs
		
		/srv/lancache/riot/temp
		
		/srv/lancache/sony
		
		/srv/lancache/sony/logs
		
		/srv/lancache/sony/temp
		
		/srv/lancache/steam
		
		/srv/lancache/steam/logs
		
		/srv/lancache/steam/tmp
		
		
	7.1)	chowning can be achieved by: 
			sudo chmod -R 755 /srv/lancache
			sudo chown lancache:lancache /srv/lancache

8)	Run nginx /opt/openresty/nginx/sbin/nginx

Optional)	To monitor Traffic through Shell you can install nload

	Optional A)	sudo apt-get install nload
	
	Optional B)	sudo nload -U G - u M -i 102400 -o 102400
