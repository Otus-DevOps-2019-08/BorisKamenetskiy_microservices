# BorisKamenetskiy_microservices
BorisKamenetskiy microservices repository

Homework docker-2 - what was done:
- docker engine installed;
- docker-machine installed;
- hello-world tried;
- tried following commands: docker run (run new container); docker ps (list of containers); docker start & attach (docker create + start + attach = run); docker exec (runs process inside the container); docker commit (creates container image) - here output of docker images is written to docker-monolith/docker-1.log;
- difference between docker inspect u_cotainer_id and docker inspect u_image_id described; 
- docker kill tried;
- docker system df tried;
- docker rm & rmi tried;
- project docker-262816 created;
- gcloud sdk installed and configured (corresponding project attached);
- authentication for gcloud configured;
- docker-host created using docker-machine;
- PID, net and user namespaces actions from demos repeated;
- output of docker run --rm -ti tehbilly/htop and docker run --rm --pid host -ti tehbilly/htop compared - in the latter case we have all processes from the local host;
- Dockerfile, mongod.conf, db_config and start.sh added in accordance with corresponding gists;
- docker build applied, images checked (necessary image was created);
- firewall rule for port 9292 added, so, that reddit app is available via corresponding IP address;
- application worked at http://35.205.32.231:9292/ before I have killed it;
- registered at Docker hub and successfully authenticated;
- image reddit pushed to docker hub;
- checks from last 3 slides didn't work for me, as not enough space on the disk of my machine (so, was not able to run reddit on local machine).

Issues:
- because of lack of space was not able to run reddit on local machine.

Homework docker-3 - what was done:
- re-created EC2 instance in Amazon, as previous was out of space;
- installed git and checked out my code;
- installed docker and docker-machine;
- logged in to DockerHub;
- copied archive into my repo;
- renamed unzipped archive to src;
- created Dockerfile with code from gist in post-py, comment, ui folders;
- in post-py I had to append the code with gcc installation using apk;
- downloaded the latest image of MongoDB;
- built images for post, comment, ui;
- created network reddit and run containers for post, ui, comment, mongodb with pre-defined aliases;
- checked http://35.205.32.231:9292/ - everything worked fine;
- checked image size - for ui it was more than 700 Mb;
- after modifying Dockerfile for UI I was not able to re-build the corresponding image, as no space left;
- killing all images didn't help.

Issues:
- EC2 t2.micro machine doesn't fit for homework purposes, as docker images are huge. And I don't have Linux installed on my local machine to try everything out;
- spent some time, trying to figure out how to install gcc in post module.

Howework Docker-4 - what was done:
- docker run with none module run - only loopback network interface present;
- docker run with host module run - docker0, loopback, bridge and external ethernet to connect with bridge are visible;
- docker-machine ssh docker-host ifconfig run - docker0, loopback and ens4 are visible;
- running "sudo docker run --network host -d nginx" provided one container only. Probably, that is the case because the same interface with the same service is used (the port for nginx is already occupied);
- running containers with none and host modules, sudo ip netns shows new net-namespace for none only;
- bridge network created;
- reddit project is started using bridge network;
   
