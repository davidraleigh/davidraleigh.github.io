I wanted to run TeamCity with HTTPS. I've written a docker-compose.yml file for running TeamCity server with HTTPS certification provided by nginx and LetsEncrpyt. This is inspired by the following three items:
 * https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion
 * https://github.com/jwilder/nginx-proxy
 * https://github.com/dataminelab/docker-jenkins-nginx-letsencrypt
 
You should be familiar TeamCity Server instructions for running their docker container:
https://hub.docker.com/r/jetbrains/teamcity-server/

The repo is https://github.com/davidraleigh/docker-teamcity-nginx-letsencrypt

You need a registered domain name. You need to update all the environment variables in the `.env`. After that it's as easy as `docker-compose up -d`. The `TEAMCITY_DIR` is the directory where all of your data persists after your container is shutdown or deleted.

You may need to restart your agents and change the SERVER_URL from `http` to `https` (ie  `SERVER_URL="https://yourfancyurlname.com/"`).
