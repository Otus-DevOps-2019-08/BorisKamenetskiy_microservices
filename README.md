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
 
