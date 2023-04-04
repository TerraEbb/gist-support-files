### Running NGINX in Docker as a layer 4 load balancer

### Create a nginx.conf file and save it

```
/etc/nginx.conf
```
```
worker_processes 4;
worker_rlimit_nofile 40000;

events {
    worker_connections 8192;
}

http {
    server {
        listen         80;
        return 301 https://$host$request_uri;
    }
}

stream {
    upstream servers {
        least_conn;
        server IP_NODE_1:443 max_fails=3 fail_timeout=5s;
        server IP_NODE_2:443 max_fails=3 fail_timeout=5s;
        server IP_NODE_3:443 max_fails=3 fail_timeout=5s;
    }
    server {
        listen     443;
        proxy_pass servers;
    }
}
```

### Run this docker command

'''
docker run -d --restart=unless-stopped \
  -p 80:80 -p 443:443 \
  -v /etc/nginx.conf:/etc/nginx/nginx.conf \
  nginx:1.14
'''

