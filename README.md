LAN-Cache v1.3-unbound
==============

Based off work of https://gitlab.com/frag-o-matic/lan-cache
So credits go to Bruno Gysels and MultiPlay.co.uk for the base they made!

Come visit us @ www.cu-lan.be | www.gunsnbits.de | www.discoverpc.net/ | https://lanfest.intel.com/netwar

Facebook @ https://www.facebook.com/groups/434599530274923/?notif_id=1509019697563943

Twitter @ https://twitter.com/search?q=%40LAN_CACHE&src=typd

OS: Debian 8.6 amd64 (Jessie)

## Short Changelog
* 6-30-2017 saambd
    * Added missing } on line 56 of Microsoft conf    
* 8-03-2017 bn_
    * Added 2 Steam URLs to Unbound.Conf as posted in Multiplay Github
* 10-17-2017 bn_    
    * Added Sony DNS to Unbound.Conf as posted by Muffeee in issues
    * Added Glyph Support (Still needs testing)
* 10-25-2017 Nagilum99 & Nexusofdoom
    * Merged the pull request from nagilum99 to correct / standardize the layout (Issue #65)
    * Changed Steamconfig as problem and solution posted by Nexusofdoom (Issue #61)
* 10-28-2017 nexusofdoom
    * Added 2 Battlenet URLs to Unbound.Conf
    * Added New Origins URLs to Unbound.Conf
    * Did some magic in lancache-origin.conf to make it work :-)
* 12-4-2017 nexusofdoom
    * Added Warframe and ESO/Elder Scrolls Online to Config.

## Important!
If you already have an installation of nginx installed via apt-get install nginx, it is necessary that you remove it, the configuration files and all recommended packagaes via:
apt-get purge nginx
apt-get autoremove

Otherwise nginx may not start with lancache start script, instead run as wrong user and load /etc/nginx/nginx.conf.
It results in not proxying and leaving entries in /var/log/nginx/...

## Installation

### Quick Installation on a clean Debian
This repository contains instructions and configuration details for the a service, which is used for caching binary Gaming content for lan parties.
It is part of the repository https://github.com/bntjah/lc-installer and is used as submodule of it. Of course it can be used standalone as well,
although please be aware that the configuration files are mostly templated and therefore have to be changed manually for the use in different environments.

The quickest and easiest way ot get lancache up and running is the use of:
https://github.com/bntjah/lc-installer (warning might contain bugs! So proceed on your own accord!)

### Manual installation

If you want to install it manually, please follow the instructions below:

    1) Install the required utilities
	   sudo apt-get install curl git unbound build-essential libpcre3 libpcre3-dev zlib1g-dev libreadline-dev libncurses5-dev git libssl-dev

	2) Clone the git repo
	   git clone -b master http://github.com/bntjah/lancache

	3) Install nginx
	   curl http://nginx.org/download/nginx-1.13.4.tar.gz | tar zx
	   cd ngnix-1.13.4
	   ./configure --with-http_ssl_module --with-http_slice_module --with-file-aio --with-threads
	   make
	   sudo make install

	4) Add the virtual interfaces (used for caching in nginx) to /etc/network/interfaces

```
# Ip used for STEAM caching
auto eth0:1
iface eth0:1 inet static
address 10.0.1.11
netmask 255.255.0.0

# Ip used for RIOT caching
auto eth0:2
iface eth0:2 inet static
address 10.0.1.12
netmask 255.255.0.0

# Ip used for Blizzard caching
auto eth0:3
iface eth0:3 inet static
address 10.0.1.13
netmask 255.255.0.0

# Ip used for Hirez caching
auto eth0:4
iface eth0:4 inet static
address 10.0.1.14
netmask 255.255.0.0

# Ip used for Origin caching
auto eth0:5
iface eth0:5 inet static
address 10.0.1.15
netmask 255.255.0.0

# Ip used for Sony caching
auto eth0:6
iface eth0:6 inet static
address 10.0.1.16
netmask 255.255.0.0

# Ip used for Microsoft caching
auto eth0:7
iface eth0:7 inet static
address 10.0.1.17
netmask 255.255.0.0

# Ip used for Tera caching
auto eth0:8
iface eth0:8 inet static
address 10.0.1.18
netmask 255.255.0.0

# Ip used for GOG caching
auto eth0:9
iface eth0:9 inet static
address 10.0.1.19
netmask 255.255.0.0

# Ip used for ArenaNetworks caching
auto eth0:10
iface eth0:10 inet static
address 10.0.1.20
netmask 255.255.0.0

# IP used for Apple caching
auto eth0:11
iface eth0:11 inet static
address 10.0.1.21
netmask 255.255.0.0

# IP used for WarGaming caching
auto eth0:12
iface eth0:12 inet static
address 10.0.1.22
netmask 255.255.0.0

# IP used for Uplay caching
auto eth0:13
iface eth0:13 inet static
address 10.0.1.23
netmask 255.255.0.0

# Ip used for Glyph caching
auto eth0:14
iface eth0:14 inet static
address 10.0.1.24
netmask 255.255.0.0
```

	5) Create the user lancache
		sudo adduser --system --no-create-home lancache
		sudo addgroup --system lancache
		sudo usermod -aG lancache lancache
	
	6) Just create the folders:
		sudo mkdir -p /srv/lancache/data/blizzard/
		sudo mkdir -p /srv/lancache/data/microsoft/
		sudo mkdir -p /srv/lancache/data/installs/
		sudo mkdir -p /srv/lancache/data/other/
		sudo mkdir -p /srv/lancache/data/tmp/
		sudo mkdir -p /srv/lancache/data/hirez/
		sudo mkdir -p /srv/lancache/data/origin/
		sudo mkdir -p /srv/lancache/data/riot/
		sudo mkdir -p /srv/lancache/data/gog/
		sudo mkdir -p /srv/lancache/data/sony/
		sudo mkdir -p /srv/lancache/data/steam/
		sudo mkdir -p /srv/lancache/data/wargaming
		sudo mkdir -p /srv/lancache/data/tera
		sudo mkdir -p /srv/lancache/data/arenanetworks
		sudo mkdir -p /srv/lancache/data/uplay
		sudo mkdir -p /srv/lancache/data/glyph
		sudo mkdir -p /srv/lancache/logs/Errors
		sudo mkdir -p /srv/lancache/logs/Keys
		sudo mkdir -p /srv/lancache/logs/Access

	6.1) chown the folder:
		sudo chown -R lancache:lancache /srv/lancache

	7) Copy the conf folder and contents (where you originally git cloned it to in step 4) to /usr/local/nginx/conf/
		sudo cp -R ~/lancache/conf /usr/local/nginx/
    7.1) Replace the proxy_bind variable with your primary IP address (not one of the virtual ones)

	8) Copy the Lancache file from init.d/ to /etc/init.d/ by:
		sudo cp -R lancache /etc/init.d/lancache

	9) Make it an executable:
		sudo chmod +x /etc/init.d/lancache

	10) Put it in the standard Boot:
		sudo update-rc.d lancache defaults

	11) Copy limits.conf to /etc/security/limits.conf

    12) Copy hosts to /etc/hosts
	12.1) Replace the hostnames with the virtual IPs

	13) Disable IPv6
	    sudo echo "net.ipv6.conf.all.disable_ipv6=1" >/etc/sysctl.d/disable-ipv6.conf
        sudo sysctl -p /etc/sysctl.d/disable-ipv6.conf

	14) Install sniproxy for passing through HTTPS traffic (cannot be cached)
		14.1) git clone https://github.com/dlundquist/sniproxy
		14.2) sudo curl https://raw.githubusercontent.com/OpenSourceLAN/origin-docker/master/sniproxy/sniproxy.conf -o /etc/sniproxy.conf
		19.3) cd sniproxy
		19.4) ./autogen.sh && ./configure && make check && sudo make install
		19.5) Start sniproxy with ./sniproxy -c /etc/sniproxy.conf

	15) Copy the unbound configuration from unbound/unbound.conf to /etc/unbound/unbound.conf
	15.1) Replace the interfaces: section with the normal ip (not the virtual ones)
    15.2) Replace all A records with the appropriate IPs (the virtual IPs for the appropriate caching service)

## Traffic Monitoring on CLI

	A) Monitor through nload
	   sudo apt-get install nload -y
	   sudo nload -U G -u M -i 102400 -o 102400 (shows bandwith in MByte/s)
	   or
	   sudo nload -U G -u m -i 1024000 -o 1024000 (shows bandwith in Mbit/s, scales graph to 1 Gbit/s)
	   
	B) Monitor network usage through iftop
	   sudo apt-get install iftop -y
	   sudo iftop -i eth0
	   (Instead of eth0 just use your physical interface)

## Optionals
### DHCPd

If your lancache server is a DHCP client, please add public resolvers (not the ones who you use for your LAN party guests) to dhclient.conf
to avoid resolving issues for the cache server. This can be achieved like this:
	- Add to /etc/dhclient.conf: prepend domain-name-servers 8.8.8.8, 8.8.4.4;

Alternatively you can use one of those mechanism to avoid resolv.conf being overwritten by DHCP or other services:
https://www.cyberciti.biz/faq/dhclient-etcresolvconf-hooks/

