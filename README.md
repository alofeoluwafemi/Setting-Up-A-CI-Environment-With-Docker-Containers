# Setting-Up-A-CI-Environment-With-Docker-Containers
Setting Up Gitlab, Redmine, MySQL , Sonarqube, Phabricator &amp; Gerrit  Containers

> To Follow This Steps Have The Latest Docker Installed On Your Machine  
> Am Using Version: Docker version 1.12.6, build 3a094bd/1.12.6
```
//https://github.com/sameersbn @repository for docker images
//Using Postgre SQL
//Using: https://hub.docker.com/r/sameersbn/redmine
docker run --name=sql-redmine -d   --env='DB_NAME=redmine_production'   --env='DB_USER=redmine' --env='DB_PASS=password'   --volume=/srv/docker/redmine/postgresql:/var/lib/postgresql   sameersbn/postgresql:9.6-2

docker run --name=redmine -d   --link=postgresql-redmine:postgresql --publish=9091:80 --env='REDMINE_PORT=9091' --volume=/docker/redmine/redmine:/home/redmine/data sameersbn/redmine:3.2.5-1


//Using MySQL
docker run --name=mysql-redmine -d --env='DB_NAME=redmine_production' --env='DB_USER=redmine' --env='DB_PASS=powerRanger2014' --volume=/docker/redmine/mysql:/var/lib/mysql sameersbn/mysql:latest

docker run --name=redmine -d --link=mysql-redmine:mysql --publish=9091:80 --env='REDMINE_PORT=9091' --volume=/docker/redmine/redmine:/home/redmine/data sameersbn/redmine:3.2.5-1

//Setup With SMTP
docker run --name=redmine -d --link=mysql-redmine:mysql --publish=9091:80 --env='REDMINE_PORT=9091' --env='SMTP_AUTHENTICATION=:login' --env='SMTP_TLS=false' --env='SMTP_METHOD=smtp' --env='SMTP_PASS=proteux123' --env='SMTP_USER=redmine@proteuxtechnologies.net' --env='SMTP_PORT=587' --env='SMTP_HOST=a2plcpnl0360.prod.iad2.secureserver.net' --env='SMTP_DOMAIN=proteuxtechnologies.net' --volume=/docker/redmine/redmine:/home/redmine/data sameersbn/redmine:3.2.5-1 app:backup:create

docker run --name=gitlab -d --publish9095:80  docker.io/store/gitlab/gitlab-ee:8.14.0-ee.0

//Setup Gitlab
docker run --name redis-gitlab -d --volume /docker/gitlab/redis:/var/lib/redis sameersbn/redis:latest

docker run --name=mysql-gitlab -d --env='DB_NAME=gitlabhq_production' --env='DB_USER=gitlab' --env='DB_PASS=powerRanger2014' --volume=/docker/gitlab/mysql:/var/lib/mysql sameersbn/mysql:latest

docker run --name gitlab -d --link mysql-gitlab:mysql --link redis-gitlab:redisio --publish=9095:80 --expose=80 --expose=443 --expose=22 --expose=9095 --env 'GITLAB_PORT=9095' --env 'GITLAB_HOST=192.168.1.254' --env='SMTP_ENABLED=true' --env='SMTP_DOMAIN=proteuxtechnologies.net' --env='SMTP_HOST=a2plcpnl0360.prod.iad2.secureserver.net' --env='SMTP_PORT=465' --env='SMTP_USER=redmine@proteuxtechnologies.net' --env='SMTP_PASS=proteux123' --env='SMTP_TLS=true' --env='SMTP_AUTHENTICATION=login' --env='SMTP_STARTTLS=true' --env='GITLAB_SECRETS_OTP_KEY_BASE=8rzCrH8s6RMBHzq8Jr83vCgS8DJdLH3JSJhHlglQKPxR3mtkDGD8QWSmgRpSF7mQ' --env='GITLAB_SECRETS_DB_KEY_BASE=8rzCrH8s6RMBHzq8Jr83vCgS8DJdLH3JSJhHlglQKPxR3mtkDGD8QWSmgRpSF7mQ' --env='GITLAB_SECRETS_SECRET_KEY_BASE=8rzCrH8s6RMBHzq8Jr83vCgS8DJdLH3JSJhHlglQKPxR3mtkDGD8QWSmgRpSF7mQ' --volume /docker/gitlab/gitlab:/home/git/data --volume /docker/gitlab/gitlab/log:/var/log/gitlab sameersbn/gitlab:9.1.4

//Setup Jenkins
docker run --name=jenkins -p 9093:8080 -p 50000:50000 --expose=80 --expose=443 --expose=22 -d -v /docker/jenkins:/var/ jenkins

//Setup Gerrit
docker run --name=gerrit -d -v /docker/gerrit:/var/gerrit/review_site -p 9099:8080 -p 29418:29418 openfrontier/gerrit

//SonarQube
docker run -d --name sonarqube -p 9097:9000 -p 9092:9092 sonarqube

//Installing Phabricator
//Option I (No mounting of volume)
docker run --name=mysql-phabricator yesnault/docker-phabricator-mysql:latest
docker run --name=phabricator-container -p 9097:80 --link mysql-phabricator:database  -d yesnault/docker-phabricator:latest 

//Option II (Mount Volume)
docker run --name=mysql-phabricator --env='MYSQL_ROOT_PASSWORD=mysql-phabricator-password' -d mysql:5.7

//Run command: docker inspect mysql-phabricator
//Grab the ip address and replace the MYSQL_HOST env flag
docker run --name=phabricator-container --rm -p 8081:80  --env PHABRICATOR_HOST={machine-host}:{port_to_serve_phabricator} --env MYSQL_HOST={mysql_container_host} --env MYSQL_USER=root --env MYSQL_PASS=mysql-phabricator-password --env PHABRICATOR_REPOSITORY_PATH=/repos -v /docker_containers:/repos redpointgames/phabricator

//Run: docker exec -it phabricator-container bash
//You would be right in the container run
//$ /srv/phabricator/phabricator/bin/config  set phabricator.base-uri 'http://{machine-host}:{port_to_serve_phabricator}/
//If config file does'nt exist run
//$ find . -name config
//Above command should help you with locating file
//Run /srv/phabricator/phabricator/bin/config set <key> <value>
//For other Options ex. ./srv/phabricator/phabricator/bin/config  set phpmailer.smtp-host demo.smtp.com

```
