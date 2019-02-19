## 用法

新建一个文件夹用于永久存储数据
```
mkdir -p /opt/portainer/data
mkdir -p /opt/portainer/local-certs
```

SSL证书保护
```
mv ~/1_domain.crt /opt/portainer/local-certs/portainer.crt
mv ~/2_domain.key /opt/portainer/local-certs/portainer.key
```
或者自签证书
```
cd /opt/portainer/local-certs
openssl genrsa -out portainer.key 2048
openssl ecparam -genkey -name secp384r1 -out portainer.key
openssl req -new -x509 -sha256 -key portainer.key -out portainer.crt -days 3650
```

```
docker run -d -p 9000:9000 \
-v /opt/portainer/data:/data \
-v /opt/portainer/local-certs:/certs \
-v /var/run/docker.sock:/var/run/docker.sock \
--label owner=portainer \
--name ui \
--restart=always \
vank3f3/portainer -l owner=portainer \
--ssl --sslcert /certs/portainer.crt --sslkey /certs/portainer.key
```
```
docker run -d   --name watchtower   -v /var/run/docker.sock:/var/run/docker.sock   v2tec/watchtower --cleanup ui
```

快速部署

```
docker run -d -p 9000:9000 --label owner=portainer \
--restart=always --name=ui \
-v /var/run/docker.sock:/var/run/docker.sock \
portainer/portainer -l owner=portainer
```

更多参考请访问：

http://bit.ly/2FMu0YX