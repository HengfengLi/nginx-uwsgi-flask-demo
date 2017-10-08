# A demo of nginx + uwsgi + flask
Make a demo for putting nginx and uwsgi into two containers. 

## How to run

1. Run the commands

```bash
# run docker-compose
docker-compose up
```

2. Visit endpoint `localhost:8000/`. 

## Solutions

1. Using a shared file as socket between nginx and uwsgi

```bash
# uwsgi.ini
socket = /tmp/uwsgi.sock

# nginx/conf.d/default.conf
include uwsgi_params;
uwsgi_pass unix:///tmp/uwsgi.sock;
```

2. Using ip + port. But note that `0.0.0.0` should be used instead of `127.0.0.1`, because if we use `127.0.0.1`, it will not allow to access from the host. 

```bash
# uwsgi.ini
socket = 0.0.0.0:9000

# nginx/conf.d/default.conf
upstream uwsgi-upstream { server uwsgi:9000; }

server {
    ... 
    
    location / {
        include uwsgi_params;
        uwsgi_pass uwsgi-upstream;
    }
}
```