# NGINX

### What is it Nginx ?

Open source web server written in C.

One of the uniqueness of Nginx that not common is to be both web server & reverse proxy.

Nginx is capable doing :

- Serves web content.
- Proxy
  - Load Balancing
  - Backed Routing
  - Caching



### Let's talk about Layer 4 vs Layer 7 

Nginx can operate as layer 7 (HTTP) or Layer 4 (TCP).

in layer 4 we have little information like ip address and port.

in layer 7 we have which protocol header body and type of request , etc...



### Nginx as web server

I'm going to install my Nginx on docker container.

```ba
docker run -d --name nginx2 --mount  source=/home/aviad/workspace/tutoirals/nginx,target=/etc/nginx -p 80:80 nginx
```

I've mount the Nginx folder , now i can play with Nginx  easily.



you can notice that we've file called `nginx.conf`  that have the configuration of Nginx server.

```nginx

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```



let's play with this config

we'll Nginx behave like web server , he will serve HTML files for us.

we have to tell him , which port to listen , what file to serve.

```nginx
http {
    # context , i can create here many web servers 
    # when i specify http this is explictly layer 7
    
    server { # this called block directive
        # block derective to define a server
        listen 80; # tell what port to listen
        root /etc/nginx # tell nginx from where serve files
        
    }
    
    events { # you have to include this to run the server
        
    }
    
}
```

I've create file in path `~/workspace/tutoirals/nginx/files` there i put all `index.html`

we can notice that Nginx serve us the required HTML file.

```bash
aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx/files$ curl localhost:80
#output
<html>
    <body>
        <h1>This is Aviad, learning nginx!!</h1>
    </body>
</html>
```



that approach works great if you have 



now let's create directory `site1` `site2` in path `nginx/files` 

in this directory I've put 2 HTML files called index.html.



now when i 

```bash
root@aviad-Inspiron-7373:/home/aviad/workspace/tutoirals/nginx/images# curl http://localhost/site2/
<html>
    <body>
        <h1>This is Aviad, King of nginx!!</h1>
	<h2>Site 2 </h2>
    </body>
</html>
```

whoooo! we get index of site 2.



now we are going to serve images.

so I've create directory  in path `/home/aviad/workspace/tutoirals/nginx/images` and put there images of Elon Musk.

http {
    # context , i can create here many web servers 
    # when i specify http this is explictly layer 7
    
```nginx
http {
    server { # this called block directive
        # block derective to define a server
        listen 80; # tell what port to listen
        root /etc/nginx # tell nginx from where serve files

            location /images {
                root  /etc/nginx/;
        }
    }
}
events { 
	# you have to include this to run the server    
}
```



`nginx -s reload` will reload change of config file.

```bash
aviad@aviad-Inspiron-7373:~$ curl http://localhost/images/elon.jpg
Warning: Binary output can mess up your terminal. Use "--output -" to tell 
Warning: curl to output it to your terminal anyway, or consider "--output 
Warning: <FILE>" to save to a file.

```



now let's forbidden client access to `jpg` images , user can access any other images in this folder.



```nginx
http {
    server { # this called block directive
        # block derective to define a server
        listen 80; # tell what port to listen
        root /etc/nginx # tell nginx from where serve files

        location /images {
            root  /etc/nginx/;
        }
        
        location ~ .jpg$ { # if someone try to access to file that ending with jpg
            return 403; #this page will be returend
        }
    }
}
events { 
	# you have to include this to run the server    
}
```



let's create another server that listen to port `8888`

we also running this with publishing new port

```bash
docker run -d --name nginx2 --mount  source=/home/aviad/workspace/tutoirals/nginx,target=/etc/nginx -p 80:80 -p 8888:8888 nginx
```



```nginx
http {
    server {
        listen 80;
        root /etc/nginx 

        location /images {
            root  /etc/nginx/;
        }
        
        location ~ .jpg$ { 
            return 403; 
        }
    }
    
     server { 
      
        listen 8888;

        location / {
            proxy_pass http://localhost:80/; 
            # this is layer 7 reverse proxy , that navigate to diffrent server
        }
    }
}
events { 
	# you have to include this to run the server    
}
```



### let's let Nginx be load balancer of 4 NodeJS servers.

```bash
docker create network learn-nginx
docker run -p 2222:9999 -e APPID=2222 --name nodeapp1 -d nodeapp
docker run -p 3333:9999 -e APPID=3333 --name nodeapp2 -d nodeapp
docker run -p 4444:9999 -e APPID=4444 --name nodeapp3 -d nodeapp
docker run -p 5555:9999 -e APPID=5555 --name nodeapp4 -d nodeapp

docker network connect learn-nginx nginx1
docker network connect learn-nginx nginx2
docker network connect learn-nginx nginx3
docker network connect learn-nginx nginx3

```



let's do round robin between all nodeapp containers

```nginx
http {
        upstream allbackend { 
            server nodeapp1:9999;
            server nodeapp2:9999;
            server nodeapp3:9999;
            server nodeapp4:9999;
        }

        server { 
            listen 80;
            location / {
                proxy_pass http://allbackend/;
            } 
        }
    }
}
events { 
}
```





let's check if working

```bash
aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl localhost
appid: 3333 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl localhost
appid: 4444 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl localhost
appid: 5555 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl localhost
appid: 2222 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl localhost
appid: 3333 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl localhost
appid: 4444 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ 
```





now this is using consistent hashing algorithm;

it takes the ip address and hash it 

when client fetch again he will get same node app.

this is state full connection

```nginx
http {
        upstream allbackend { 
        	ip_hash;
            server nodeapp1:9999;
            server nodeapp2:9999;
            server nodeapp3:9999;
            server nodeapp4:9999;
        }

        server { 
            listen 80;
            location / {
                proxy_pass http://allbackend/;
            } 
        }
    }
}
events { 
}
```



let's check if working

```bash
aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl http://localhost/
appid: 5555 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl http://localhost/
appid: 5555 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl http://localhost/
appid: 5555 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl http://localhost/
appid: 5555 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl http://localhost/
appid: 5555 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ curl http://localhost/
appid: 5555 home page: says hello!aviad@aviad-Inspiron-7373:~/workspace/tutoirals/nginx-node-proj/javascript_playground/docker$ 
```



let's split the load.

when client fetch localhost/app1 he will get either appnode1 or appnode2.

when client fetch localhost/app2 he will get either appnode3 or appnode4.

```nginx
http {
        upstream allbackend {
            server nodeapp1:9999;
            server nodeapp2:9999;
            server nodeapp3:9999;
            server nodeapp4:9999;
        }
        upstream app1backend {
            server nodeapp1:9999;
            server nodeapp2:9999;
        }
        upstream app2backend {
            server nodeapp3:9999;
            server nodeapp4:9999;
        }
        
        server { 
            listen 80;
            location / {
                proxy_pass http://allbackend/;
            } 

            location /app1 {
                proxy_pass http://app1backend/;
            }

            location /app2 {
                proxy_pass http://app2backend/;
            }
        }
    }

events { 
}
```



now let's block route `GET /admin` from port 80 this will be only accessible from port 2222.

```nginx
http {
        upstr eam allbackend {
            server nodeapp1:9999;
            server nodeapp2:9999;
            server nodeapp3:9999;
            server nodeapp4:9999;
        }
        upstream app1backend {
            server nodeapp1:9999;
            server nodeapp2:9999;
        }
        upstream app2backend {
            server nodeapp3:9999;
            server nodeapp4:9999;
        }
        
        server { 
            listen 80;
            location / {
                proxy_pass http://allbackend/;
            } 

            location /app1 {
                proxy_pass http://app1backend/;
            }

            location /app2 {
                proxy_pass http://app2backend/;
            }

            location /admin {
                return 403;
            }
        }
    
      server { 
            listen 2222;
            location / {
                proxy_pass http://allbackend/;
            } 

            location /app1 {
                proxy_pass http://app1backend/;
            }

            location /app2 {
                proxy_pass http://app2backend/;
            }
        }
    }

events {}
```



> Todo : add here caching

 

so we have dig in reverse proxy layer 7.

 let's talk about layer 4.



# Nginx Layer 4

the proxy stream the data to required server , the browser will be connected to the proxy.

just change `http` to `stream`

we don't have `location` because we are in layer 4.

`ws` , `smtp` , `http`  everything will works because we work here we tcp connection.

```nginx
stream {
        upstream allbackend {
            server nodeapp1:9999;
            server nodeapp2:9999;
            server nodeapp3:9999;
            server nodeapp4:9999;
        }
        
        server { 
            listen 80;
        	proxy_pass allbackend;
        }
    }

events { 
}
```

