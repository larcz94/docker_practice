#Generate a secure root password for image

#Create a projecct folder
mkdir sshd
cd sshd/

#Create a Dockerfile
vi Dockerfile

#Add following to Dockerfile ( without ###### and Replace your Password to 'myPass@123')
###############
FROM ubuntu:16.04

RUN apt-get update && apt-get install -y openssh-server
RUN mkdir /var/run/sshd
RUN echo 'root:myPass@123' | chpasswd
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile

EXPOSE 22
CMD ["/usr/sbin/sshd", "-D"]
###############



#Build docker image
docker image build -t sshd_server:v1 .

#Check docker image
docker image ls

#Run docker image
docker container run -d -P --name test-sshd-server sshd_server:v1

#Check docker container
docker container ps

docker container port test-sshd-server



#connect to ssh server (Replace your server IP)
ssh root@ServerIP -p 32769


#####
ssh root@ServerIP -p 32769
root@ServerIP's password:
Welcome to Ubuntu 16.04.7 LTS (GNU/Linux 4.18.0-513.el8.x86_64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage
Last login: Thu Oct 19 19:39:26 2023 from xxx.xxx.xx.xx



