# Stomp-Client

> Nginx Setting
```
worker_processes  1;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    sendfile        on;
    keepalive_timeout  65;
    fastcgi_read_timeout 300;
    proxy_read_timeout 300;


    server {
        listen       80;
        server_name  localhost;

        location / {
            root   stomp;
            index  index.html index.htm;
            try_files $uri /index.html;
        }

        location /user {
            proxy_pass http://localhost:8080/user;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $http_host;
        }


        location /ws {
            proxy_pass http://localhost:8080/ws;
            proxy_http_version      1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "Upgrade";
            proxy_set_header Host $http_host;
            proxy_set_header Origin "*";
        }


        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }


}
```
