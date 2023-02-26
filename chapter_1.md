# Chapter 1

```
docker run --detach --name web nginx:latest 
docker run -d --name mailer dockerinaction/ch2_mailer
docker run -i --tty --link web --name web_test busybox:latest /bin/sh
    wget -O - http://web:80/
    # exit: exit an stop
    # CTRL P+Q: detach
docker run -it --name agent --link web:insideweb --link mailer:insidemailer dockerinaction/ch2_agent

# pid namespace

docker run -d --name namespaceA busybox:latest /bin/sh -c "sleep 30000"
docker run -d --name namespaceB busybox:latest /bin/sh -c "nc -l 0.0.0.0 -p 80"

docker exec namespaceA ps
docker exec namespaceB ps

docker run --pid host busybox:1.29 ps 

docker rename webid webid-old

docker create --cidfile /tmp/web.cid nginx

docker ps --latest --quiet
docker ps -l -q --no-trunc
# quiet: Only display container IDs

docker run -d --read-only --name wp wordpress:5.0.0-php7.2-apache
docker inspect --format "{{.State.Running}}" wp
docker run -d --name wp_writable wordpress:5.0.0-php7.2-apache
docker container diff wp_writable  
docker run -d --read-only -v /run/apache2 --tmpfs /tmp --name wp wordpress:5.0.0-php7.2-apache
docker run -d --name wpdb -e MYSQL_ROOT_PASSWORD=ch2demo mysql:5.7
docker run -d --read-only -v /run/apache2 --tmpfs /tmp --link wpdb:mysql -p 8000:80 --name wp3 wordpress:5.0.0-php7.2-apache
docker run -e MY_ENV="this is a test" busybox env
docker run -d --name backoff_detector --restart always busybox date

docker run -d --name lamp-test -p 9999:80 tutum/lamp
docker exec lamp-test ps
docker run wordpress:5.0.0-php7.2-apache cat /usr/local/bin/docker-entrypoint.sh
docker run --entrypoint="cat" wordpress:5.0.0-php7.2-apache  /usr/local/bin/docker-entrypoint.sh

```
