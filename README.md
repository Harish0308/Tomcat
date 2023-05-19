Using the Nginx as Front-end Server and Application Server as Back-end Server


Since there are two applications running on a single server , we are going to view the application server on the Nginx Web server using the following two concepts: Upstream and ProxyPass.
For this  the prerequisites are one application server running two different applications and One web server which uses Nginx.
The following conf file will do the process as mentioned above.


upstream tomcat {
    server 54.188.195.64:8080;

}
upstream java {
    server 54.188.195.64:7070;
}
server {
    listen 80;
    server_name testdomainjsp.com;

     location / {
       proxy_pass http://tomcat;
}

  location /myapp{
proxy_pass  http://java/test;
    }
}


Upstream it is used to connect or communicate with different applications or sites using different ports.
In the first location the Proxy pass will work directly since it's easily accessible. In the second location we have to give the root of the application on the proxy pass directive.
By using the server name we will access the first application and by the server name / myapp will access the second application running on the server.





                Using single Proxypass and Redirect rule

        upstream tomcat {
    server 54.188.195.64:8080;

}
server {
    listen 80;
    server_name testdomainjsp.com;
    location / {
    proxy_pass http://tomcat;
    }

    location /tomcat1 {
        return 301 http://54.188.195.64:7070;
    }
}




Using Proxypass on Different server block 

upstream tomcat {
    server 172.26.2.70:8080;

}
upstream tomcat1 {
    server 172.26.2.70:7070;
}
server {
    listen 80;
    server_name testdomainjsp.com;
    location / {
        proxy_pass http://tomcat;
    }
}
server {
    listen 80;
    server_name testdomainjsp1.com;
    location / {
        proxy_pass http://tomcat1;
    }

Using single upstream along with the Loadbalancer method

upstream tomcat {
ip_hash;
    server 54.188.195.64:8080 ;
        server 54.188.195.64:7070;
}

server {
    listen 80;
    server_name testdomainjsp.com;
     location / {
       proxy_pass http://tomcat;
}


}

Where ip_hash is the load balancer inorder to avoid the loss of a web page after idle time of a user.




