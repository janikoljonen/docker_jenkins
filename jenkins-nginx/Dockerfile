FROM centos:7
ARG PROXY
ENV http_proxy=${PROXY} https_proxy=${PROXY}
RUN yum -y update && \
    yum -y install \
        http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm && \
    yum -y makecache && \
    yum -y install nginx-1.8.0 && \
    rm /etc/nginx/conf.d/default.conf && \
    rm /etc/nginx/conf.d/example_ssl.conf


