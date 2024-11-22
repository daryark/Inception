FROM	alpine:3.20
#update and install NGINX, clears the local repo of retrieved package files
RUN	apk update && apk upgrade && apk add --no-cache nginx

#COPY ./index.html /var/www
EXPOSE	443
CMD	["nginx", "-g", "daemon off;"]
#COPY	./conf/nginx.conf /etc/nginx/nginx.conf
