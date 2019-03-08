yum -y install epel-release

yum -y update

yum install python-setuptools python-pip supervisor python-meld3 wget -y

### easy_install pip


yum install git

yum -y groupinstall "Development Tools"

wget https://github.com/jedisct1/libsodium/releases/download/1.0.16/libsodium-1.0.16.tar.gz
tar xf libsodium-1.0.16.tar.gz && cd libsodium-1.0.16
./configure && make -j2 && make install
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig
cd ../ && rm -rf libsodium*

docker version > /dev/null || curl -fsSL get.docker.com | bash
chkconfig docker on
service docker start
service docker stop
service docker restart

docker run -d --name=ssr -e NODE_ID=40 -e API_INTERFACE=modwebapi -e WEBAPI_URL=https://xxx -e WEBAPI_TOKEN=xxx -e MU_SUFFIX=jd.hk --network=host --log-opt max-size=50m --log-opt max-file=3 --restart=always alliswell2day/v3ssr:ssr


cat >> /etc/security/limits.conf << EOF
* soft nofile 51200
* hard nofile 51200
EOF

ulimit -n 51200


cat >> /etc/sysctl.conf << EOF
fs.file-max = 51200
net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.core.netdev_max_backlog = 250000
net.core.somaxconn = 4096
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1
EOF

sysctl -p

pip install supervisor==3.1

chkconfig supervisord on

wget https://raw.githubusercontent.com/alliswell2day/panel-download/master/supervisord.conf -O /etc/supervisord.conf
wget https://raw.githubusercontent.com/alliswell2day/panel-download/master/supervisord -O /etc/init.d/supervisord

chmod -R 777 /etc/init.d/supervisord
service supervisord start


systemctl stop firewalld
systemctl mask firewalld


yum install iptables-services -y
systemctl status iptables
systemctl enable iptables
systemctl start iptables
systemctl status iptables
systemctl stop iptables
systemctl restart iptables
