FROM centos:7

ARG PROXY
ARG JENKINS_VERSION
ARG JENKINS_SHA
ARG JAVA_OPTS
ARG JENKINS_OPTS

ENV http_proxy=${PROXY} https_proxy=${PROXY}

RUN yum -y update && \
    yum -y install \
        wget \
        zip \
        openssh-client \
        unzip \
        java-1.8.0-openjdk \
        git
    

# SET Jenkins Environment Variables
ENV JENKINS_HOME /var/jenkins_home
ENV JENKINS_SLAVE_AGENT_PORT 50000
ENV JENKINS_VERSION ${JENKINS_VERSION}
ENV JENKINS_SHA ${JENKINS_SHA}
ENV JENKINS_UC https://updates.jenkins-ci.org
ENV COPY_REFERENCE_FILE_LOG $JENKINS_HOME/copy_reference_file.log
ENV JAVA_OPTS ${JAVA_OPTS}
ENV JENKINS_OPTS ${JENKINS_OPTS}

# Jenkins is run with user `jenkins`, uid = 1000
# If you bind mount a volume from the host or a data container,
# ensure you use the same uid
RUN useradd -d "$JENKINS_HOME" -u 1000 -m -s /bin/bash jenkins


# `/usr/share/jenkins/ref/` contains all reference configuration we want
# to set on a fresh new installation. Use it to bundle additional plugins
# or config file with your custom jenkins Docker image.
RUN mkdir -p /usr/share/jenkins/ref/init.groovy.d

# Install Jenkins
RUN curl -fL http://mirrors.jenkins-ci.org/war-stable/$JENKINS_VERSION/jenkins.war -o /usr/share/jenkins/jenkins.war \
    && echo "$JENKINS_SHA /usr/share/jenkins/jenkins.war" | sha256sum -c -

# Prep Jenkins Directories
RUN chown -R jenkins "$JENKINS_HOME" /usr/share/jenkins/ref
RUN mkdir /var/log/jenkins
RUN mkdir /var/cache/jenkins
RUN chown -R jenkins:jenkins /var/log/jenkins
RUN chown -R jenkins:jenkins /var/cache/jenkins
#RUN chown -R jenkins:jenkins /usr/local/etc/jenkins

# Switch to the jenkins user
USER jenkins


