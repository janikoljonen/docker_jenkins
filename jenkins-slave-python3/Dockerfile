FROM centos:7
MAINTAINER thermocalc

COPY /files/wandisco-svn.repo /etc/yum.repos.d/wandisco-svn.repo
COPY /files/authorized_keys /home/jenkins/.ssh/authorized_keys
# Set Name Servers
COPY /files/resolv.conf /etc/resolv.conf

# Install Packages
RUN yum -y update  && \
         yum install -y git && \
         yum -y groupinstall "Development Tools" && \
         yum install -y subversion unzip && \
         yum install -y wget && \
         yum install -y openssh-server && \
         yum install -y java-1.8.0-openjdk && \
         yum install -y sudo && \
         yum install -y https://centos7.iuscommunity.org/ius-release.rpm && \
         yum install -y python36u python36u-libs python36u-devel python36u-pip && \
         yum install -y texlive-* && \
         yum clean all && \
         # pipenv installation
         pip3.6 install pipenv && \
         ln -s /usr/bin/pip3.6 /bin/pip && \
         ln -s /usr/bin/python3.6 /usr/bin/python3 && \
         # gen dummy keys, centos doesn't autogen them like ubuntu does
         /usr/bin/ssh-keygen -A && \
         # Set SSH Configuration to allow remote logins without /proc write access
         sed -ri 's/^session\s+required\s+pam_loginuid.so$/session optional pam_loginuid.so/' /etc/pam.d/sshd && \
         # Create Jenkins User
         useradd jenkins -m -s /bin/bash && \
         # Add public key for Jenkins login
         chown -R jenkins /home/jenkins && \
         chgrp -R jenkins /home/jenkins && \
         chmod 600 /home/jenkins/.ssh/authorized_keys && \
         chmod 700 /home/jenkins/.ssh && \
         # Add the jenkins user to sudoers
         echo "jenkins  ALL=(ALL)  ALL" >> etc/sudoers

# Expose SSH port and run SSHD
EXPOSE 22
CMD ["/usr/sbin/sshd","-D"]
