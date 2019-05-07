1) Install siege CentOS
------------------------------------
```
cd /opt

wget http://download.joedog.org/siege/siege-3.1.4.tar.gz

tar -zxvf siege-3.1.4.tar.gz

cd /opt/siege-3.1.4/

yum install gcc

./configure

make && make install

siege -V
```
