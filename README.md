LAN-Cache v1.4-unbound
==============

Based off work of https://gitlab.com/frag-o-matic/lan-cache
So credits go to Bruno Gysels and MultiPlay.co.uk for the base they made!

Come visit us @ www.cu-lan.be | www.gunsnbits.de | www.discoverpc.net/ | www.netwar.org

Facebook @ https://www.facebook.com/groups/434599530274923/?notif_id=1509019697563943

Twitter @ https://twitter.com/search?q=%40LAN_CACHE&src=typd

Optional Installer @ https://github.com/nexusofdoom/lancache-installer

## Short Changelog
* 10-25-2017 Nagilum99 & Nexusofdoom
    * Merged the pull request from nagilum99 to correct / standardize the layout (Issue #65)
    * Changed Steamconfig as problem and solution posted by Nexusofdoom (Issue #61)
* 10-28-2017 nexusofdoom
    * Added 2 Battlenet URLs to Unbound.Conf
    * Added New Origins URLs to Unbound.Conf
    * Did some magic in lancache-origin.conf to make it work :-)
* 12-4-2017 nexusofdoom
    * Added Warframe and ESO/Elder Scrolls Online to Config.
* 9-19-2018 bn_
    * Added the two CDN wich were posted by @Billthecat in Issue #111
* 9-26-2018 bn_
    * Adding CDN's wich are posted in Issues by @Billthecat & @Chong601
    * Adding Gaijin Support as requested by @Chong601 in Issue #94
    * Added some CDN's listed by Apple within https://support.apple.com/en-us/HT201999
* 10-20-2018 Nagilum99
    * Rewrote Readme
    * Fixed an issue where bn_ missed some Gaijin hosts
 * 08-18-2019 bn_
    * Added EPIC Games CDN's and config as Lancache.net & UKLANS have been in contact with EPIC Games
 * 10-24-2019 Nagilum99
    * Added MS DNS Entries that can be cached (See Issue #146)

## Important!
If you already have an installation of nginx installed via apt-get install nginx, it is necessary that you remove it, the configuration files and all recommended packagaes via:
apt-get purge nginx
apt-get autoremove

Otherwise nginx may not start with lancache start script, instead run as wrong user and load /etc/nginx/nginx.conf.
It results in not proxying and leaving entries in /var/log/nginx/...

### Manual installation

If you want to install it manually, please follow the instructions below:

	0) Configure proper network interface in your /etc/network/interfaces file, go for static IP address, take notes about all IPs you'll assign, as you need to refer to them during this installation by A LOT!

	    	1) Install the required utilities
		   	apt-get install curl git unbound build-essential libpcre3 zlib1g-dev libreadline-dev libncurses5-dev libssl-dev httpry libudns0 libudns-dev libev4 libev-dev devscripts automake libtool autoconf autotools-dev cdbs debhelper dh-autoreconf dpkg-dev gettext pkg-config fakeroot libpcre3-dev libgd2-xpm-dev libgeoip-dev tcpdump -y

	    	2) NGINX + Pre Req
			2.1) Get Nginx from web
				curl http://nginx.org/download/nginx-1.13.4.tar.gz | tar zx
		   		cd nginx-1.13.4
			2.2) Get Nginx Cache Purge from Frickle Labs:
				curl "http://labs.frickle.com/files/ngx_cache_purge-2.3.tar.gz" | tar zx	
	    		2.3) Get the Range Cache Plugin from Multiplay Github
				git clone https://github.com/multiplay/nginx-range-cache/ $PWD/nginx-range-cache
			2.4) Get Wandenberg NGINX Stream Module
				curl "https://codeload.github.com/wandenberg/nginx-push-stream-module/tar.gz/0.5.1?dummy=/wandenberg-nginx-push-stream-module-0.5.1_GH0.tar.gz" | tar zx
	
		3) Clone the git repo
			   	git clone -b master http://github.com/bntjah/lancache
		4) Install NGINX 
               		4.1) Patching NGINX for Range Cache from Multiplay
				patch -p1 <$PWD/nginx-range-cache/range_filter.patch
			4.2) Configure NGINX with the previously downloaded addons
	   			./configure --modules-path=$PWD --with-cc-opt='-I /usr/local/include' --with-ld-opt='-L /usr/local/lib' --conf-path=/usr/local/nginx/nginx.conf --sbin-path=/usr/local/sbin/nginx --pid-path=/var/run/nginx.pid --with-file-aio --add-module=$PWD/ngx_cache_purge-2.3 --with-http_flv_module --with-http_geoip_module=dynamic --with-http_gzip_static_module --with-http_image_filter_module=dynamic --with-http_mp4_module --add-module=$PWD/nginx-range-cache --with-http_realip_module --with-http_slice_module --with-http_stub_status_module --with-pcre --with-http_v2_module --with-stream=dynamic --with-stream_ssl_module --with-stream_ssl_preread_module --with-http_ssl_module --add-module=$PWD/nginx-push-stream-module-0.5.1 --with-threads
	   			make
	   			make install

		5) Add the virtual interfaces (used for caching in nginx) to /etc/network/interfaces		
	
		6) Create the user lancache
			adduser --system --no-create-home lancache
			addgroup --system lancache
			usermod -aG lancache lancache
	
		7) Just create the folders:
			mkdir -p /srv/lancache/data/{microsoft,installs,other,tmp,hirez,origin,riot,gog,sony,steam,wargaming,arenanetworks,uplay,glyph,zenimax,digitalextremes,pearlabyss,blizzard,apple,epicgames}
			mkdir -p /srv/lancache/logs/{Errors,Keys,Access}

		8) chown the folder:
			chown -R lancache:lancache /srv/lancache

		9) Copy the conf folder and contents (where you originally git cloned it to in step 3) to /usr/local/nginx/conf/
			cp -R ~/lancache/conf /usr/local/nginx/
    		
		10) Replace the proxy_bind variable with your primary IP address (not one of the virtual ones)

		11) Copy the Lancache file from ~/lancache/init.d/ to /etc/init.d/ by:
			cp ~/lancache/init.d/lancache /etc/init.d/

		12) Make it an executable:
			chmod +x /etc/init.d/lancache

		13) Put it in the standard Boot:
			update-rc.d lancache defaults

		14) cp ~/lancache/limits.conf /etc/security/

   		15) Edit ~/lancache/hosts to your needs, placing all your virtual IP's next to the appropriate caching service
			15.1) cp ~/lancache/hosts /etc/
			
		16) Disable IPv6
			echo "net.ipv6.conf.all.disable_ipv6=1" >/etc/sysctl.d/disable-ipv6.conf
        		sysctl -p /etc/sysctl.d/disable-ipv6.conf

		16) Install sniproxy for passing through HTTPS traffic (cannot be cached)
			16.1) git clone https://github.com/dlundquist/sniproxy
			16.2) curl https://raw.githubusercontent.com/OpenSourceLAN/origin-docker/master/sniproxy/sniproxy.conf -o /etc/sniproxy.conf
			16.3) cd sniproxy
			16.4) ./autogen.sh && ./configure && make check && make install
			# If there are problems during test procedures, you can try to skip the checks by leaving out "&&make check" 
			16.5) Start sniproxy either:
				16.5a) Via: /usr/local/sbin/sniproxy -c /etc/sniproxy.conf
				or
				16.5b.1) Copy the sniproxy file from ~/lancache/init.d/ to /etc/init.d/ by:
					cp ~/lancache/init.d/sniproxy /etc/init.d/

				16.5b.2) Make it an executable:
					chmod +x /etc/init.d/sniproxy

				16.5.b3) Put it in the standard Boot:
					update-rc.d sniproxy defaults
			
		17) Copy the unbound configuration from ~/lancache/unbound/unbound.conf to /etc/unbound/unbound.conf
			17.1) Replace the interfaces: section with the normal ip (not the virtual ones)
    			17.2) Replace all "A records" with the appropriate IPs (the virtual IPs for the appropriate caching service)

## Traffic Monitoring on CLI

	A) Monitor through nload
	   apt-get install nload -y
	   nload -U G -u M -i 102400 -o 102400 (shows bandwith in MByte/s)
	   or
	   nload -U G -u m -i 1024000 -o 1024000 (shows bandwith in Mbit/s, scales graph to 1 Gbit/s)
	   
	B) Monitor network usage through iftop
	   apt-get install iftop -y
	   iftop -i eth0
	   (Instead of eth0 just use your physical interface)

## Optionals
### DHCPd

If your lancache server is a DHCP client, please add public resolvers (not the ones who you use for your LAN party guests) to dhclient.conf
to avoid resolving issues for the cache server. This can be achieved like this:
	- Add to /etc/dhclient.conf: prepend domain-name-servers 8.8.8.8, 8.8.4.4;

Alternatively you can use one of those mechanism to avoid resolv.conf being overwritten by DHCP or other services:
https://www.cyberciti.biz/faq/dhclient-etcresolvconf-hooks/

