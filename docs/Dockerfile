FROM ubuntu 
MAINTAINER Jeganathan Swaminathan <jegan@tektutor.org> <http://www.tektutor.org>

# Let's ensure that all package repositories are up to date 
RUN apt-get update

# We need ssh server so that Jenkins Master can connect to slave jenkins docker image
RUN apt-get install -y openssh-server
RUN sed -i 's|session required pam_loginuid.so|session optional pam_loginuid.so|g' /etc/pam.d/sshd
RUN mkdir -p /var/run/sshd

# Create an user named jenkins with admin privileges
RUN adduser --quiet jenkins
RUN adduser jenkins sudo

# Set password for the jenkins user as jenkins
RUN echo "jenkins:jenkins" | chpasswd

RUN mkdir -p /home/jenkins/CRM
RUN chown jenkins:jenkins /home/jenkins/CRM

WORKDIR /home/jenkins/CRM 

#COPY apache-maven-3.3.9-bin.tar.gz /home/jenkins/CRM
#COPY jdk-8u112-linux-x64.tar.gz /home/jenkins/CRM
#COPY src /home/jenkins/CRM
COPY . /home/jenkins/CRM

# Create Maven Local Repository directory and ensure jenkins user is the owner
RUN mkdir /home/jenkins/.m2
RUN chown -R jenkins:jenkins /home/jenkins/.m2/


# Open SSH Port for jenkins master to ssh into this slave machine
EXPOSE 22

# Lauch SSH Server once the container is booted as a daemon
CMD ["/usr/sbin/sshd", "-D"]
