FROM ubuntu:latest 

RUN apt update && \
    apt upgrade -y && \
    apt install nginx -y  
    
RUN rm -f /usr/share/nginx/html/* && \
    echo "This is nginx container created from Dockerfile" > /usr/share/nginx/html/index.html

EXPOSE 80 

CMD ["nginx", "-g", "daemon off;"]      # start nginx in foreground
