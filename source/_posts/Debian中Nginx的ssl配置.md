---
title: Debian中Nginx的ssl配置
tags: [Linux]
categories: [Linux]
date: 2018-04-12 17:46:19
---




* Debian 中 Nginx 的 ssl 配置
* 1.服务器的配置

```
Linux glb-gz-debian 3.16.0-4-amd64 #1 SMP Debian 3.16.43-2+deb8u5 (2017-09-19) x86_64
nginx/1.6.2

```

* 2.参考:
	* 1.[How To Create a Self-Signed SSL Certificate for Nginx on Debian 8](https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-on-debian-8)



<!-- more -->


### 1.GLB 公司的服务器,参考了 ‘参考1’ 后，已经可以外部 HTTPS 访问到网站(用自签名的 key ).

### 2.用 腾讯云的 key,[腾讯云提供的 ATS 页面检测](https://cloud.tencent.com/product/ssl#userDefined10).

### 3.china Server 集成 腾讯云 的 SSL 证书.

* 1.申请一个 '域名型免费版(DV)' 证书
* 2.填写的相关的内容 -> 文件验证 -> 按照要求，在指定的位置写入文件
* 3.证书审核通过，然后就开始[证书的部署](https://cloud.tencent.com/document/product/400/4143)



* 4.把证书(.key 和 .crt)放到 `/etc/nginx/china.tianlovezhen.site_ssl_conf` 目录里面.

* 5. 在 `/etc/nginx/sites-available/default` 中，增加了以下的代码:

```
    listen 443;
        server_name china.tianlovezhen.site; #Bind the host
        ssl on;
        ssl_certificate /etc/nginx/china.tianlovezhen.site_ssl_conf/1_china.tianlovezhen.site_bundle.crt;
        ssl_certificate_key /etc/nginx/china.tianlovezhen.site_ssl_conf/2_china.tianlovezhen.site.key;
        ssl_session_timeout 5m;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #Use this Protocols Setting
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#Use this Utils Setting
        ssl_prefer_server_ciphers on;
        
```

* 6.成功在 nginx 中加入 HTTPS  ！！！相关的图片:
![](/assets/imgs/web/Snip20180427_1.png)
![](/assets/imgs/web/Snip20180427_3.png)

* 7.(请看👇下面的 8 ,这个 7 有错误 !!!)`https` 访问正常，但 `http` 访问出现错误 `400 The plain HTTP request was sent to HTTPS port`
>[解决方法:](https://www.centos.bz/2018/01/nginx%E5%A6%82%E4%BD%95%E8%A7%A3%E5%86%B3the-plain-http-request-was-sent-to-https-port%E9%94%99%E8%AF%AF/)将上面配置文中的“ ssl on ; ” 注释掉或者修改成 “ ssl off ;”，这样，Nginx就可以同时处理HTTP请求和HTTPS请求了。
>发现 这样做的话， https 请求不能正常返回值.

* 8.HTTP 请求会转为 HTTPS 请求.
	* 1.参考[How To Secure Nginx with Let's Encrypt on Debian 8](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-debian-8) 
	* 2.下面是我的配置:
		* 1.增加 `/etc/nginx/snippets/ssl-example.com.conf` 文件,内容为:
		
```
ssl_certificate /etc/nginx/china.tianlovezhen.site_ssl_conf/1_china.tianlovezhen.site_bundle.crt;
ssl_certificate_key /etc/nginx/china.tianlovezhen.site_ssl_conf/2_china.tianlovezhen.site.key;
```
	
* 2.增加 `/etc/nginx/snippets/ssl-params.conf` 文件，内容为:
		
```
# from https://cipherli.st/
# and https://raymii.org/s/tutorials/Strong_SSL_Security_On_nginx.html

ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
ssl_prefer_server_ciphers on;
ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
ssl_ecdh_curve secp384r1;
ssl_session_cache shared:SSL:10m;
ssl_session_tickets off;
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
resolver_timeout 5s;
# Disable preloading HSTS for now.  You can use the commented out header line that includes
# the "preload" directive if you understand the implications.
#add_header Strict-Transport-Security "max-age=63072000; includeSubdomains; preload";
add_header Strict-Transport-Security "max-age=63072000; includeSubdomains";
add_header X-Frame-Options DENY;
add_header X-Content-Type-Options nosniff;

#ssl_dhparam /etc/ssl/certs/dhparam.pem;
```
	
* 3.修改 `/etc/nginx/sites-available/default` 文件，把前面的 

```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	server_name _
	# SSL configuration
	#
	#listen 443 ssl default_server;
	#listen [::]:443 ssl default_server;
	
	#
	# Note: You should disable gzip for SSL traffic.
	# See: https://bugs.debian.org/773332
   ...
```

修改为:

```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	server_name  china.tianlovezhen.site; #Bind the host
        return 301 https://$server_name$request_uri;
}
server {
	# SSL configuration
	#
	 listen 443 ssl default_server;
	 listen [::]:443 ssl default_server;
	include snippets/ssl-example.com.conf;
	include snippets/ssl-params.conf;
	#
	# Note: You should disable gzip for SSL traffic.
	# See: https://bugs.debian.org/773332
	...
```
	
* 3.如果需要同时支持 HTTP 和 HTTPS 的访问，只需把 `default` 文件改为:

```
server {
	listen 80 default_server;
	listen [::]:80 default_server;
	#	server_name japan.tianlovezhen.site; #Bind the host
        #return 301 https://$server_name$request_uri;
#}
#server {
	# SSL configuration
	#
	 listen 443 ssl default_server;
	 listen [::]:443 ssl default_server;
server_name japan.tianlovezhen.site; #Bind the host
	include snippets/ssl-example.com.conf;
	include snippets/ssl-params.conf;
	...
```





