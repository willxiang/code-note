### nginx 配置
```

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

	include include/*.conf;
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }



        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    #server {
    #    listen       443 ssl;
    #    server_name  localhost;

    #    ssl_certificate      cert.pem;
    #    ssl_certificate_key  cert.key;

    #    ssl_session_cache    shared:SSL:1m;
    #    ssl_session_timeout  5m;

    #    ssl_ciphers  HIGH:!aNULL:!MD5;
    #    ssl_prefer_server_ciphers  on;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}

}

```
文件夹下载：
https://github.com/willxiang/code-note/raw/master/attachment/include.zip

**增加的文件夹 include**
![增加的文件夹 include](https://raw.githubusercontent.com/willxiang/code-note/master/image/2019/5/29/Snipaste_2019-05-29_22-28-27.jpg)


**include文件夹内部**
![include文件夹内部](https://raw.githubusercontent.com/willxiang/code-note/master/image/2019/5/29/Snipaste_2019-05-29_22-28-52.jpg)


### app.conf
```
upstream gzyipijx { 
 server 127.0.0.1:8080 max_fails=5 fail_timeout=15s;
 keepalive 500;
}

 # HTTPS server
server {
 listen       443 ssl;
 server_name  app.qs365.org;

 ssl_certificate      include/ssl/1_app.qs365.org_bundle.crt;
 ssl_certificate_key  include/ssl/2_app.qs365.org.key;

 ssl_session_cache    shared:SSL:1m;
 ssl_session_timeout  5m;

 ssl_ciphers  HIGH:!aNULL:!MD5;
 ssl_prefer_server_ciphers  on;

 location / {
  root html;
  index index.html;
 
 }

 location  /wxapi/ {
  #limit_conn addr 5;          
  proxy_pass http://gzyipijx/market-app/;
  proxy_http_version 1.1;
  proxy_set_header X-Forward-For $remote_addr ;
  proxy_set_header Connection "";
  add_header Access-Control-Allow-Origin *;
  add_header Access-Control-Allow-Headers "X-Requested-With" always; 
  add_header Access-Control-Allow-Methods "POST, GET, OPTIONS" always; 
 }
}


server {
 listen       80;
 server_name  gzyipijx.com;
 location  / {
  #limit_conn addr 5;          
  proxy_pass http://gzyipijx/market-app/;
  proxy_http_version 1.1;
  proxy_set_header X-Forward-For $remote_addr ;
  proxy_set_header Connection "";
  add_header Access-Control-Allow-Origin *;
  add_header Access-Control-Allow-Headers "X-Requested-With" always; 
  add_header Access-Control-Allow-Methods "POST, GET, OPTIONS" always; 
 }
}
```



### 修改hosts文件
```
C:\Windows\System32\drivers\etc\hosts
```
增加一行域名地址：
```
127.0.0.1 app.qs365.org
```