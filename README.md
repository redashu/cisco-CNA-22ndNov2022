# cisco-CNA-22ndNov2022

## traning plan 

<img src="plan.png">

### login and create folder in Docker server 

```
fire@ashutoshhs-MacBook-Air ~ % ssh ashu@54.145.245.225
ashu@54.145.245.225's password: 
Last login: Tue Nov 22 11:48:42 2022 from 103.59.75.139

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
1 package(s) needed for security, out of 1 available
Run "sudo yum update" to apply all updates.
-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory
[ashu@ip-172-31-16-246 ~]$ 
[ashu@ip-172-31-16-246 ~]$ 
[ashu@ip-172-31-16-246 ~]$ docker  ps 
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[ashu@ip-172-31-16-246 ~]$ ls  /home
ankit  ashu  atul  ec2-user  leni  manash  teja  vijay  vijayk
[ashu@ip-172-31-16-246 ~]$ who
ashu     pts/0        Nov 23 04:24 (103.59.75.139)
vijayk   pts/1        Nov 23 04:24 (72.163.220.6)
atul     pts/2        Nov 23 04:24 (72.163.220.1)
manash   pts/3        Nov 23 04:24 (72.163.220.25)
[ashu@ip-172-31-16-246 ~]$ whoami
ashu
[ashu@ip-172-31-16-246 ~]$ pwd
/home/ashu
[ashu@ip-172-31-16-246 ~]$ mkdir ashu-container-apps
[ashu@ip-172-31-16-246 ~]$ ls
ashu-container-apps  project-website-template
[ashu@ip-172-31-16-246 ~]$ 

```

### app to container image using dockerfile 

```
git clone https://github.com/ShaifArfan/one-page-website-html-css-project.git

```

### Dockerfile 

```
FROM oraclelinux:8.4 
# taking base os library 
LABEL name=ashutoshh
LABEL email=ashutoshh@linux.com
# optional field but you can share details of image developer
RUN yum install httpd -y 
COPY . /var/www/html/
CMD ["httpd","-DFOREGROUND"]

```

### lets build it 

```
[ashu@ip-172-31-16-246 ashu-container-apps]$ ls
one-page-website-html-css-project  project-html-website
[ashu@ip-172-31-16-246 ashu-container-apps]$ cd one-page-website-html-css-project/
[ashu@ip-172-31-16-246 one-page-website-html-css-project]$ ls
app.js  Dockerfile  img  index.html  LICENSE  README.md  style.css
[ashu@ip-172-31-16-246 one-page-website-html-css-project]$ docker build -t  ashuapp:v1  . 
Sending build context to Docker daemon  793.6kB
Step 1/6 : FROM oraclelinux:8.4
8.4: Pulling from library/oraclelinux
a4df6f21af84: Pull complete 
Digest: sha256:b81d5b0638bb67030b207d28586d0e714a811cc612396dbe3410db406998b3ad
Status: Downloaded newer image for oraclelinux:8.4
 ---> 97e22ab49eea
Step 2/6 : LABEL name=ashutoshh
 ---> Running in c8347d7f5cb8
Removing intermediate container c8347d7f5cb8
 ---> c944ba9ec483
Step 3/6 : LABEL email=ashutoshh@linux.com
 ---> Running in 3b2c0e9c3d13
Removing intermediate container 3b2c0e9c3d13
 ---> 693774e81fba
Step 4/6 : RUN yum install httpd -y
 ---> Running in 16be39a07e1a
Oracle Linux 8 BaseOS Latest (x86_64)    
```

### lets check the images 

```
[ashu@ip-172-31-16-246 one-page-website-html-css-project]$ docker  images
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
<none>        <none>    758535f922f3   33 seconds ago   246MB
vijaypapp     v1        5ccb531b3861   2 minutes ago    456MB
tejuapp       v1        504b45cd0abd   3 minutes ago    456MB
leniapp       v1        e3577d96eaf1   4 minutes ago    456MB
atulapp       v1        a0a5e78726d0   4 minutes ago    456MB
ashuapp       v1        71ad8d760ee9   5 minutes ago    456MB
nginx         latest    88736fe82739   7 days ago       142MB
oraclelinux   8.4       97e22ab49eea   12 months ago    246MB
[ashu@ip-172-31-16-246 one-page-website-html-css-project]$ 
```

### creating container from this image 

```
[ashu@ip-172-31-16-246 one-page-website-html-css-project]$ docker run -itd --name ashuwebapp1 -p 1234:80 ashuapp:v1  
34b393c7c67249d20829c24a3634abbe8b4e0d38f5ddd3a54296f84a8aae83f0
[ashu@ip-172-31-16-246 one-page-website-html-css-project]$ docker ps
CONTAINER ID   IMAGE        COMMAND                CREATED         STATUS         PORTS                                   NAMES
34b393c7c672   ashuapp:v1   "httpd -DFOREGROUND"   4 seconds ago   Up 3 seconds   0.0.0.0:1234->80/tcp, :::1234->80/tcp   ashuwebapp1
```

### Understanding one More CNA -- standard for container images -- OCI 

<img src="oci.png">

### journey of application Design and deployment 

<img src="appd.png">

## k8s and CLoud nativ kubernetes 

<img src="knativ.png">

### COntainer RUNTIME Interface 

<img src="cri.png">

### Pushing image to Registry server 

<img src="push.png">

### pushing image to container registry -Docker hub 

```
[ashu@ip-172-31-16-246 ashu-container-apps]$ docker  tag  ashuapp:v1   docker.io/dockerashu/ashuapp:v1  
[ashu@ip-172-31-16-246 ashu-container-apps]$ 
[ashu@ip-172-31-16-246 ashu-container-apps]$ 
[ashu@ip-172-31-16-246 ashu-container-apps]$ docker login 
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: dockerashu
Password: 
WARNING! Your password will be stored unencrypted in /home/ashu/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded
[ashu@ip-172-31-16-246 ashu-container-apps]$ docker push docker.io/dockerashu/ashuapp:v1 
The push refers to repository [docker.io/dockerashu/ashuapp]
7ce885c8fb74: Pushed 
8402448e869e: Pushed 
2d3586eacb61: Mounted from library/oraclelinux 
v1: digest: sha256:0b75085a73b2ec906b294827a51517b3c32024b4891dcf554f4f386e164ada80 size: 952
[ashu@ip-172-31-16-246 ashu-container-apps]$ 
[ashu@ip-172-31-16-246 ashu-container-apps]$ 
[ashu@ip-172-31-16-246 ashu-container-apps]$ docker logout 
Removing login credentials for https://index.docker.io/v1/
```



