# dyomite

apt-get install -y autoconf automake libtool gcc openssl-dev

cd /usr/local/src
git clone https://github.com/Netflix/dynomite.git
cd dynomite
	
autoreconf -fvi
./configure --enable-debug=yes
make && src/dynomite -h
	 
	 
mkdir -p /usr/share/dynomite
mkdir /var/run/dynomite
useradd -r -M -c "Dynomite server" -s /sbin/nologin -d /usr/share/dynomite dynomite
chown -R dynomite:dynomite /usr/share/dynomite
chown -R dynomite:dynomite /var/run/dynomite

cp init/systemd_environment__dynomite /etc/default/dynomite
cp init/systemd_service_ubuntu__dynomite.service /lib/systemd/system/dynomite.service
systemctl daemon-reload
systemctl enable dynomite
systemctl status dynomite

mkdir /etc/dynomite/
cp /usr/local/src/dynomite/conf/redis_node1.yml /etc/dynomite/dynomite.yml

cp /usr/local/src/dynomite/conf/redis_node2.yml /etc/dynomite/dynomite.yml

systemctl start dynomite.service
