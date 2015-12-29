LAN-Cache v1.0
==============
Based off work of https://gitlab.com/frag-o-matic/lan-cache
By Bruno Gysels

OS: Ubuntu 14.04
Install following packages
apt-get install libreadline-dev libncurses5-dev libpcre3-dev libssl-dev perl make

sudo adduser --system --no-create-home lancache
sudo addgroup --system lancache
sudo usermod -aG lancache lancache

1)      Install BIND
        sudo apt-get install bind9
        Copy configuration files from bind/ folder to /etc/bind change the IP addresses if necessary

2)      Copy hosts file to /etc/hosts
        Copy interfaces file to /etc/network/interfaces change the IP addresses if necessary

3)      Configure and compile LuaJIT
        ./configure
        make
        sudo make install

4)      Configure and compile openresty
        ./configure --prefix=/opt/openresty
        make
        sudo make install
        
5)      Copy limits.conf to /etc/security/limits.conf

6)	Run nginx /opt/openresty/nginx/sbin/nginx

