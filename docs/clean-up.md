# Clean up
Congratulations ! You have made it. We will now perform the clean-up. Cleanup will  involve shutting down the services. We will use docker-compose command for this.

```
PS C:\Users\aniru\workspace\github\python-nginx-microservice> docker compose down
[+] Running 6/6
 - Container python-nginx-microservice-weather-1  Removed                                                                                                                                                                    3.9s 
 - Container python-nginx-microservice-news-1     Removed                                                                                                                                                                    2.7s 
 - Container nginx                                Removed                                                                                                                                                                    3.9s 
 - Container python-nginx-microservice-master-1   Removed                                                                                                                                                                    1.2s
 - Container python-nginx-microservice-master-2   Removed                                                                                                                                                                    1.0s 
 - Network python-nginx-microservice_default      Removed                              

```

Let's test one last time to see if all services are shutdown 
```
PS C:\Users\aniru\workspace\github\python-nginx-microservice> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

As you can see all the services are down. You made it !
