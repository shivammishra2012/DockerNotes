# DockerNotes

https://medium.com/fintechexplained/what-is-a-container-architecture-design-54826e93fc18

https://www.burwood.com/blog-archive/containerization-vs-virtualization


<----------------------------------------------docker commands---------------------------->
 1.To know the details about the conatiner 

     docker inspect conatinername

2.to run commands on container from host machine 
   docker exec conatinername cat /etc/os-release

3.docker ps 
   list all the conatiners which is in running and exited mode

4.to access our container from external world or browser

	this scenario is applicable to only webapp/app server conatiner

     notes :each container runs on certain port

   port mapping 
    
	1.automatically : docker run --name webe -d -P httpd
 	    In this case it will automatically pick the port and map to container and port can we see by docker ps.
	2.manual port binding :docker run --name web -d -p 8989:80 nginx
	     In this case we are mapping the port 8989(ec2 instance to conatiner port (80))
              when we call http://ec2ip:8989 -> the request goes to port 80 of conatiner because we have done the port binding of port of 8989 of conatiner to port 80 of httpd.
	
6.How to preserve the data of container on host machine

Conatiner is volatile in nature so once deleted so all the files of conatiner get deleted.so volume concept comes into 
conatiner can share among themselves through volume concept
   steps
 
   1.docker volume create myvolume
   2.docker volume inspect myvol
   3.docker run -it --name d1 -v myvolume:/temp ubuntu
         >cd /temp
	>touch file 1
    4.cd /var/lib/docker/volumes/myvolume/_data
       then the files is present in volume as well as inside the conatiner as well 
    5. docker run -it --name u2 -v myvolume:/temp ubuntu :since we can also send the file from volume to container as it is birectional .In otherword conatiners can share the data through volume . 
		

7. docker images- list all the images in docker

8.Volumes in docker types :
      1.named volume :we create a volume with a name in docker home directory (/var/Lib/docker)
      2.Bind mount:
 		1.On the host machine(ec2 instances in aws) in a directory called as /product we have a configuration file file.conf.
		2.we want this file to be available on containers directory(/etc).
		3.we will use bind mount volumes .
		4.In this case ,we have to inspect every container mount section to see where it is saving the data .

		example git clone 
			docker run --name webaptp1 -itd -P -v /root/ecomm:/usr/localapache2/htdcocs/ httpd
			docker exec -it webapp bash   ->get inside the conatiner 
			ls
			cd htdocs 
			your clone file is there in htdocs 
			once it copy to htdocs directory of httpd server it automatically code
9.docker rm -f $(docker ps -aq) -> delete all the container 
10. docker inspect conatinername-> list all the information about the conatiners 



###<------------Docker File-------------------------->############ 

 
11. docker build -t myimage01 .   -> this command gives you idea that the build the dockerfile which is in current directory(. repesent current directory) and convert into images (set of files).Always save there will be only one docker file in current directory.

12  docker run -itd --name base01 myimage01 ping -c 10 google.com

13.docker logs containerid  -> to log all the details on the server.

14 Docker history imagename -> gives the history of image.
