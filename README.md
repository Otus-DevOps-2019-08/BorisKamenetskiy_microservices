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
- migrated to new large ec2 instance;
- conducted the experiment with mounted volume for docker-3 homework - post remained at http://34.77.21.114:9292/;
- tried to run containers without network aliases - problem encountered;
- network aliases set - everything is fine;
- two docker networks created - back_net and front_net;
- containers run, but the app didn't work;
- post and comment added to both networks using docker network connect - applicartion works now;
- found all network id's using docker network ls - for reddit, front_net, back_net;
- bridge interfaces for all the beforementioned networks found;
- one of bridge interfaces is analyzed using brctl show;
- iptables investigated - POSTROUTING table is not 100% clear for me - would ask to explain once again;
- found rule for ui container;
- found the process docker-proxy;
- docker compose installed;
- docker-compose.yml created - the file is clear;
- USERNAME exported, so, that docker-compose could work;
- docker-compose up -d && docker-compose ps -> applications works fine (after starting container manually, killing all the containers and then - docker-compose up -d !!!);
- parameterized docker-compose.yml: variables PUBLISHPORT, COMMENTVER, UIVER, POSTVER added (they are also added to .env). .env.example as copy of .env creted;
- aliases, front_net and back_net added to docker-compose.yml;
- after docker-compose up -d application works as expected;
- docker-compose adds prefix to all created resources (by default it is equal to the folder, where we are). This prefix could be overridden by using -p option;
- some changes to Dockerfile's in accordance with Hadolint recommendations added.

Issues:
- some really weird issues with starting the containers using docker-compose. After some manual actions (starting containers) and killing them docker-compose suddenly worked;
- was not able to build hadolint locally, so, I have used Hadolint from docker container. As the topic is docker containers, probably, not worst solution :).

2020-03-01
Gitlab-ci-1 homework - what was done:
- new gcloud machine with enough space created;
- docker installed manually;
- directories /srv/gitlab/config, /srv/gitlab/data/ /srv/gitlab/logs created, docker-compose.yml prepared;
- container with gitlab ci started using docker-compose up -d;
- gitlab ci was available at http://35.234.126.242;
- registered as root and registration of other users is switched off;
- new group and new project created;
- git connected with repo on the created host;
- template of .gitlab-ci.yml created and pushed to gitlab server;
- runner added and registered;
- pipeline passed;
- reddit src code added to the gitlab repo;
- test_unit_job in .gitlab-ci.yml changed, simpletest.rb added;
- library for testing added to reddit/Gemfile;
- now test runs on each code change;
- deploy_dev_job added, after pipeline passed new dev environment is visible;
- staging and production environments with manual run added;
- staging and production deployments are possible only for tagged commits;
- dynamic environment for each branch, except for master, defined.

2020-01-08 
Monitoring-1 homework - what was done:
- new machine in gcloud set up and configured properly;
- firewall rules prometheus-default and puma-default created;
- docker-host created, local environment is configured to work with docker-host;
- prometheus container run;
- prometheus works from web interface;
- prometheus_build_info metric checked;
- targets checked - there is prometheus only;
- host:port/metrics checked - saw the metrics, gathered by Prometheus;
- container prometheus stopped;
- directory structure inside repository root directory was amended (first - incorrectly, I have moved ui, post and comment to the docker folder directly and removed src folder; I had to re-create src folder later);
- build instructions deleted from docker-compose.yml;
- directory monitoring/prometheus created. Dockerfile with prometheus image created;
- prometheus.yml created (with jobs for monitoring of prometheus, ui, comment with 5s interval);
- prometheus image built;
- images of post, ui, comment built using docker_build.sh;
- prometheus service added to docker-compose.yml (metrics are stored for 1 day);
- networks section added for prometheus service;
- after docker-compose up -d application works fine, prometheus works as well;
- targets checked (they are in accordance with prometheus.yml);
- ui_health metric checked;
- post service stopped (for some reason ui_health is still one, though metric for ui_health_post_availability dropped to 0);
- started post service - ui_health_post_availability returned to 1. Repeated experiment with post_db (stopped this container and then started it);
- added node exporter service to docker-compose.yml (with network section);
- added 'node' job to Prometheus monitoring in prometheus.yml;
- docker image for prometheus re-built;
- after docker-compose down and docker-compose up -d node appeared among target endpoints in web gui for Prometheus;
- CPU usage checked (metric node_load1);
- stress test (via connection to docker host and creating artificial load) conducted - CPU usage curve changed as expected;
- created images pushed to DockerHub;
- docker-host removed.

My DockerHub link is below:
https://hub.docker.com/repository/docker/boriskamenetskiy/$SERVICE_NAME
 

Issues:
- after stopping post service ui_health was still equal to 1, though ui_health_post_availability dropped to 0.

Monitoring-2 homework - what was done:
- docker-host created and local environment configured to work with it;
- monitoring and application separated - monitoring migrated to docker-compose-monitoring.yml;
- cadvisor service added to docker-compose-monitoring.yml;
- job for cadvisor added to prometheus to gather metrics;
- prometheus re-built;
- cAdvisor works as expected - information about running containers visible;
- checked in Prometheus, that cAdvisor metrics are gathered;
- grafana service added to docker-compose-monitoring.yml;
- run grafana service, logged in;
- added relevant data source;
- on the grafana website chosen Docker and system monitoring dashboard, imported it into working grafana container on docker-host;
- graphs are visible;
- folder monitoring/grafana/dashboards created;
- dashboard saved as DockerMonitoring.json in monitoring/grafana/dashboards;
- job for post service added to prometheus.yml to gather corresponding metrics;
- prometheus re-built;
- docker infrastructure re-created;
- new dashboard created in Grafana;
- ui_request_count added (later function rate added to this metric);
- title and description for the graph added, dashboard saved;
- graph (with label and description) for HTTP error requests added and checked via incorrect http request;
- histogram (95% percentile) for request latency metric added;
- dashboard saved as UI_Service_Monitoring.json to monitoring/grafana/dashboards;
- new dashboard created;
- business metrics rate(post_count[1h]) and rate(comment_count[1h]) added to this dashboard;
- dashboard saved as Business_Logic_Monitoring.json to monitoring/grafana/dashboards;
- directory monitoring/alertmanager created with corresponding Dockerfile; 
- webhook integrated to slack channel, file config.yml created and webhook added there;
- alertmanager image built;
- alertmanager service added to docker-compose-monitoring.yml;
- file alerts.yml created in monitoring/prometheus folder (the rule is: if service is down for 1 minute, alert is triggered and sent to the slack channel);
- operation of copying alerts.yml added to the Dockerfile;
- information about alerting rules added to prometheus.yml
- prometheus image re-built;
- alerting checked for comment and posrt services. After stopping those services alerts were visible in Prometheus web-interface, Alertmanager interface and in the slack channel as well;
- all images pushed to DockerHub;
- docker-host removed. 

Issues:
- comment service was not working because in docker-compose.yml comment_db was not added to mongodb aliases;
- by mistake killed docker-host before checks passed, had to create dashboards again.

