FROM centos:7

COPY /files/wandisco-svn.repo /etc/yum.repos.d/wandisco-svn.repo
COPY /files/authorized_keys /home/jenkins/.ssh/authorized_keys
COPY /data/ifort/USE_SERVER.lic /usr/local/vol/intel/
COPY /data/ifort/ /usr/local/vol/intel/ifort_16.0.4/

# Install Packages
RUN yum -y update && \
    yum -y groupinstall "Development Tools" && \ 
    yum -y provides ld-linux.so.2 && \
    yum -y install git \
        wget \
        libX11-devel \
        sshpass \
        bc \
        which \
        subversion \
        openssh-server \
        java-1.8.0-openjdk-devel \
        glibc.i686 libfreetype.so.6 libfontconfig.so.1 libstdc++.so.6 bgfortran-4.8.5-28.el7_5.1.i686 compat-libgfortran-41-4.1.2-44.el7.i686 \
        sudo && \
    /usr/bin/ssh-keygen -A && \
    sed -ri 's/^session\s+required\s+pam_loginuid.so$/session optional pam_loginuid.so/' /etc/pam.d/sshd && \
    useradd jenkins -m -s /bin/bash && \
    chown -R jenkins /home/jenkins && \
    chgrp -R jenkins /home/jenkins && \
    chmod 600 /home/jenkins/.ssh/authorized_keys && \
    chmod 700 /home/jenkins/.ssh && \
    mkdir -p /usr/local/vol/intel && \
    echo "jenkins  ALL=(ALL)  ALL" >> etc/sudoers && \
    wget --no-verbose -O /tmp/apache-maven-3.3.9.tar.gz http://archive.apache.org/dist/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz && \
    echo "516923b3955b6035ba6b0a5b031fbd8b /tmp/apache-maven-3.3.9.tar.gz" | md5sum -c && \
    tar xzf /tmp/apache-maven-3.3.9.tar.gz -C /opt/ && \
    ln -s /opt/apache-maven-3.3.9 /opt/maven && \
    ln -s /opt/maven/bin/mvn /usr/local/bin && \
    rm -f /tmp/apache-maven-3.3.9.tar.gz

ENV MAVEN_HOME /opt/maven
ENV JAVA_HOME /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.151-1.b12.el7_4.x86_64

RUN ln -fs /usr/local/vol/intel/ifort_16.0.4 /usr/local/vol/intel/_16.0.4 && \
    chown -R jenkins:jenkins /usr/local/vol/intel && \
    chmod +x /usr/local/vol/intel/ifort_16.0.4/bin/compilervars.sh
