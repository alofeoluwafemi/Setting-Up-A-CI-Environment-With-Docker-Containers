# Setting-Up-A-CI-Environment-With-Docker-Containers
Setting Up Gitlab, Redmine, MySQL , Sonarqube &amp; Gerrit  Containers

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

//Phabricator
//Mysql Container For Phabricator
docker run --name=mysql-phabricator -d --volume=/docker_container/mysql-phabricator:/var/lib/mysql yesnault/docker-phabricator-mysql:latest
```
