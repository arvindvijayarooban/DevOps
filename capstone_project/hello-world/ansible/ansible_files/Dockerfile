#FROM ubuntu

#RUN apt-get update  -y \
#    && apt-get upgrade -y \ 
#   && apt-get install -y \
#    ssh \
#    sshpass \
#    sudo \
#    software-properties-common

#RUN add-apt-repository --yes --update ppa:ansible/ansible \
#    && apt-get install -y ansible 


#WORKDIR /home/ansible_controller



#RUN useradd -rm -d /home/ansible_controller -s /bin/bash -g root -G sudo -u 1001 ansible_controller
#RUN echo ansible_controller:12345 | chpasswd
#RUN echo "ansible_controller ALL=(ALL:ALL) NOPASSWD: ALL" |  EDITOR="tee -a"  visudo

#RUN mkdir -p /home/ansible_controller/.ssh



FROM ubuntu
FROM node:16 AS build

RUN mkdir -p /tmp/build
WORKDIR /tmp/build

COPY  ./docker_files /tmp/build

RUN npm i -g npm; npm i; npm run build



FROM nginx AS final

WORKDIR /usr/share/nginx/html

COPY --from=build /tmp/build/build .

#COPY ./startup.sh /home/ansible_controller
#CMD ["/bin/bash", "/home/ansible_controller/startup.sh"]

EXPOSE 80
EXPOSE 22
EXPOSE 8080
EXPOSE 443

#ENTRYPOINT service ssh restart && bash

RUN useradd -rm -d /home/ansible_controller -s /bin/bash -g root -G sudo -u 1001 ansible_controller
RUN echo ansible_controller:12345 | chpasswd
#RUN echo "ansible_controller ALL=(ALL:ALL) NOPASSWD: ALL" |  EDITOR="tee -a"  visudo
RUN apt-get update  -y
RUN apt-get install -y ssh 
RUN apt-get install -y python3 
RUN chmod -R 775 /usr/share/nginx/html/static/js/main.7fdb1a28.js 

ENTRYPOINT [ "/bin/bash", "service ssh start" ]
