---
layout: post
title: oneinstack+v2fly+ws+tls+CloudFlare教程
categories: [IT&数码]
description: 一直习惯了用oneinstack来搭建网站的环境，建站使用，之前一直用v2ray来翻墙，不带伪装的那种，已经被干掉好久了，于是自己找了些免费的节点来用。为什么不自己搭呢？一是懒，习惯了用一键包，涉及到伪装的话，担心会影响到自己的网站，网上能找到的一键包要么包含了Caddy，要么是包含了Nginx并且自动配置好，没法满足我的要求。刚好一直用的免费节点不行了，应该是刚坏吧。。。。。。于是开始尝试自己配置带伪装的服务器端，也就有了这篇文章。
keywords: v2ray, v2fly, web socket, tls, 伪装， CloudFlare, oneinstack, nginx
---

# 缘起 #

一直习惯了用oneinstack来搭建网站的环境，建站使用，之前一直用v2ray来翻墙，不带伪装的那种，已经被干掉好久了，于是自己找了些免费的节点来用。为什么不自己搭呢？一是懒，习惯了用一键包，涉及到伪装的话，担心会影响到自己的网站，网上能找到的一键包要么包含了Caddy，要么是包含了Nginx并且自动配置好，没法满足我的要求。刚好一直用的免费节点不行了，应该是刚坏吧。。。。。。于是开始尝试自己配置带伪装的服务器端，也就有了这篇文章。

如果你正好在用 oneinstack，又刚好有这种需求，苦于到处找不到教程，那么这篇文章可以帮助到你。

# V2Fly 简介 #

v2ray 下面的 v2ray-core 是原版，v2fly 里面也有 v2ray-core, 是原作者离开后的社区维护版，其实是一个东西，更多的相信不用我介绍大家也能自行脑补了。

# Oneinstack 安装 #

首先，我们需要安装 oneinstack, 我一直都是用的交互模式，请访问  [Oneinstack](https://oneinstack.com) 按说明进行。

之后我们需要准备一个域名，并且指向安装程序的服务器 IP 地址，以上工作完成后，开始建设网站。

![vhos.sh](https://static.oneinstack.com/images/vhost.png)

请记住图片结尾处的配置文件的地址。

网站的配置：

`/usr/local/nginx/conf/vhost.sh/demo.oneinstack.com.conf` 

Nginx 的配置：

`/usr/local/nginx/conf/nginx.conf`

oneinstack 的设置到这里就结束了，如果需要做网站，就可以直接在域名下操作了，网站文件的存放地址是

`/data/wwwroot/demo.oneinstack.com`

# V2Fly 安装 #

我是直接参照 Github 上的说明文档来的。[V2Fly](https://github.com/v2fly/fhs-install-v2ray)

一行命令：

```
// 安裝執行檔和 .dat 資料檔
# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

相应的文件位置在这里：

```
installed: /usr/local/bin/v2ray
installed: /usr/local/bin/v2ctl
installed: /usr/local/share/v2ray/geoip.dat
installed: /usr/local/share/v2ray/geosite.dat
installed: /usr/local/etc/v2ray/config.json
installed: /var/log/v2ray/
installed: /var/log/v2ray/access.log
installed: /var/log/v2ray/error.log
installed: /etc/systemd/system/v2ray.service
installed: /etc/systemd/system/v2ray@.service
```

到此，所需的文件就全部装好了，后面就是配置环节了。

# V2Ray 配置 #
一般来说都是如下的样子：
```
{
  "inbounds": [
    {
    "port":9000,
    "listen":"127.0.0.1",//只监听 127.0.0.1，避免除本机外的机器探测到开放了 9000 端口
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "◆◆◆◆◆◆◆◆◆◆◆◆",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/★★★★★★"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```
UUID可以从这个网站生成：[UUID Generator](https://www.uuidgenerator.net/)。只要打开或者刷新这个网页就可以得到一个UUID。

“随机字符串”就是你在键盘上胡乱敲打出来的东西，比如dsfhsdjfhref。推荐用[这个网站](https://pincong.rocks/url/link/aHR0cHM6Ly93d3cucmFuZG9tLm9yZy9zdHJpbmdzLz9udW09MSZsZW49NyZkaWdpdHM9b24mdXBwZXJhbHBoYT1vbiZsb3dlcmFscGhhPW9uJnVuaXF1ZT1vZmYmZm9ybWF0PWh0bWwmcm5kPW5ldw)生成一个，只要打开或刷新网页就可以得到一个随机字符串。

我用这个网站随机生成的字符串是mL7Gg8K

这个随机字符串就是WebSocket路径，不要抄我这里的例子，去自己生成一个！否则会被墙探测出来。建议WebSocket路径取得长一些（5个字符以上），过于简单，过于常见的路径（比如/ray，/v2，/v2ray之类的名称），很容易被墙探测出来。

还需要注意的就是，端口需要保持一致。

# Nginx配置 #

一键包的配置是下面这个样子的：
```
server {
    server_name demo.oneinstack.com;
    listen 80 reuseport fastopen=10;
    rewrite ^(.*) https://$server_name$1 permanent;
    if ($request_method  !~ ^(POST|GET)$) { return  501; }
    autoindex off;
    server_tokens off;
}
server {
    ssl_certificate /etc/letsencrypt/live/demo.oneinstack.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/demo.oneinstack.com/privkey.pem;
    location /★★★★★★★★
   {
        proxy_pass http://127.0.0.1:9000;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_requests 10000;
        keepalive_timeout 2h;
        proxy_buffering off;
    }
    listen 443 ssl reuseport fastopen=10;
    server_name $server_name;
    charset utf-8;
    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;
    keepalive_requests 10000;
    keepalive_timeout 2h;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-256-GCM-SHA384:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_ecdh_curve secp384r1;
    ssl_prefer_server_ciphers off;

    ssl_session_cache shared:SSL:60m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 10s;

    if ($request_method  !~ ^(POST|GET)$) { return 501; }
    add_header X-Frame-Options DENY;
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Content-Type-Options nosniff;
    add_header Strict-Transport-Security max-age=31536000 always;
    autoindex off;
    server_tokens off;
    index index.html index.htm  index.php;
    location ~ .*\.(js|jpg|JPG|jpeg|JPEG|css|bmp|gif|GIF|png)$ { access_log off; }
    location / { index index.html; }
}
```

用 vim 命令打开网站的配置文件
```
server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  ssl_certificate /usr/local/nginx/conf/ssl/demo.oneinstack.com.crt;
  ssl_certificate_key /usr/local/nginx/conf/ssl/demo.oneinstack.com.key;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
  ssl_ciphers TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
  ssl_prefer_server_ciphers on;
  ssl_session_timeout 10m;
  ssl_session_cache builtin:1000 shared:SSL:10m;
  ssl_buffer_size 1400;
  add_header Strict-Transport-Security max-age=15768000;
  ssl_stapling on;
  ssl_stapling_verify on;
  server_name demo.oneinstack.com
  access_log /data/wwwlogs/demo.oneinstack.com_nginx.log combined;
  index index.html index.htm index.php;
  root /data/wwwroot/demo.oneinstack.com;
  if ($ssl_protocol = "") { return 301 https://$host$request_uri; }
  if ($host != demo.oneinstack.com) {  return 301 $scheme://demo.oneinstack.com$request_uri;  }
  include /usr/local/nginx/conf/rewrite/wordpress.conf;
  #error_page 404 /404.html;
  #error_page 502 /502.html;
    location ~ .*\.(wma|wmv|asf|mp3|mmf|zip|rar|jpg|gif|png|swf|flv|mp4)$ {
    valid_referers none blocked *.oneinstack.com demo.oneinstack.com 
    if ($invalid_referer) {
        return 403;
    }
  }
  location ~ [^/]\.php(/|$) {
    #fastcgi_pass remote_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
  }
  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
    expires 30d;
    access_log off;
  }
  location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
  }
  location ~ /(\.user\.ini|\.ht|\.git|\.svn|\.project|LICENSE|README\.md) {
    deny all;
  }
}
```
你会发现其中不同的部分就是下面这一点：
```
    location /★★★★★★★★
   {
        proxy_pass http://127.0.0.1:9000;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_requests 10000;
        keepalive_timeout 2h;
        proxy_buffering off;
    }
```  
我们只要把它插入到网站实际的配置中就可以了。
把网站的配置变成最终的样子后如下：

```
    server {
  listen 443 ssl http2;
  listen [::]:443 ssl http2;
  ssl_certificate /usr/local/nginx/conf/ssl/demo.oneinstack.com.crt;
  ssl_certificate_key /usr/local/nginx/conf/ssl/demo.oneinstack.com.key;
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3;
  ssl_ciphers TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+AES128:RSA+AES128:EECDH+AES256:RSA+AES256:EECDH+3DES:RSA+3DES:!MD5;
  ssl_prefer_server_ciphers on;
  ssl_session_timeout 10m;
  ssl_session_cache builtin:1000 shared:SSL:10m;
  ssl_buffer_size 1400;
  add_header Strict-Transport-Security max-age=15768000;
  ssl_stapling on;
  ssl_stapling_verify on;
  server_name demo.oneinstack.com
  access_log /data/wwwlogs/demo.oneinstack.com_nginx.log combined;
  index index.html index.htm index.php;
  root /data/wwwroot/demo.oneinstack.com;
      location /★★★★★★★★
   {
        proxy_pass http://127.0.0.1:9000;
        proxy_redirect off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
        keepalive_requests 10000;
        keepalive_timeout 2h;
        proxy_buffering off;
    }
  if ($ssl_protocol = "") { return 301 https://$host$request_uri; }
  if ($host != demo.oneinstack.com) {  return 301 $scheme://demo.oneinstack.com$request_uri;  }
  include /usr/local/nginx/conf/rewrite/wordpress.conf;
  #error_page 404 /404.html;
  #error_page 502 /502.html;
    location ~ .*\.(wma|wmv|asf|mp3|mmf|zip|rar|jpg|gif|png|swf|flv|mp4)$ {
    valid_referers none blocked *.oneinstack.com demo.oneinstack.com 
    if ($invalid_referer) {
        return 403;
    }
  }
  location ~ [^/]\.php(/|$) {
    #fastcgi_pass remote_php_ip:9000;
    fastcgi_pass unix:/dev/shm/php-cgi.sock;
    fastcgi_index index.php;
    include fastcgi.conf;
  }
  location ~ .*\.(gif|jpg|jpeg|png|bmp|swf|flv|mp4|ico)$ {
    expires 30d;
    access_log off;
  }
  location ~ .*\.(js|css)?$ {
    expires 7d;
    access_log off;
  }
  location ~ /(\.user\.ini|\.ht|\.git|\.svn|\.project|LICENSE|README\.md) {
    deny all;
  }
}
```
就可以了。

# 配置 CloudFlare #
这个其实就很简单了，把域名指向服务器的 IP 地址，然后开启 CDN， 橙色小箭头穿过小云朵就好了，不多说了，一搜一大把。
# 客户端设置 #
到此其实就可以了，大家用手上的 GUI 的客户端，host 填域名，路径选择前面生成的随机字符串，就可以开心的科学上网了。
