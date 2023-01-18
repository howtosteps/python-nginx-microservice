# Create Nginx Loadbalancer
The purpose of the Nginx Loadblancer is to accept all requests and load-balance it across 2 instances of the master application. The scaling of the master service will be defined during the container start-up. 

## Create nginx.conf
Load balancing with Nginx uses a round-robin algorithm by default if no other method is defined. With round-robin scheme each server is selected in turns according to the order we set them in the `nginx.conf` file. This balances the number of requests equally between our to two master instances. 

***Please notice that the master application will have 2 instances and are refernced by their container-names. The container-name is usually :

* [project-directory]-[image-name-from-docker-compose.yml]-[instance-count]
* Project directory from the start page is : `python-nginx-microservice`

Now create file `nginx.conf` and copy the following contents :
```
events {}
# Define which servers to include in the load balancing scheme.
http {
    upstream app {
        server master;
        server python-nginx-microservice-master-1:3001;
        server python-nginx-microservice-master-2:3001;
     }
# This server accepts all traffic to port 80 and passes it to the upstream.
server {
         listen 80;
         server_name app.com;
         
         location / {
              proxy_pass http://app;
          }
     }
}
```


## Create Dockerfile.nginx

```
# using Nginx base image
FROM nginx

# delete nginx default .conf .file
RUN rm /etc/nginx/conf.d/default.conf

# add the .conf file we have created
COPY nginx.conf /etc/nginx/nginx.conf

#start the nginx server
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
```

This tells Docker to:

* Build an image starting with the nginx base image
* Remove the old configuration file
* Replace it with the newly updated nginx.conf
* Start the nginx server