FROM openjdk:17-jdk-slim

# Set environment variables untuk Java, Maven, dan Tomcat
ENV JAVA_HOME=/usr/local/openjdk-17
ENV MAVEN_VERSION=3.9.8
ENV MAVEN_HOME=/usr/share/maven
ENV TOMCAT_VERSION=7.0.109
ENV CATALINA_HOME=/usr/local/tomcat


1. 403 Access Denied - You are not authorized to view this page.
 -- /usr/local/tomcat/conf/tomcat-users.xml.
    <role rolename="manager-gui"/>
    <user username="admin" password="admin" roles="manager-gui"/>

2 jalan kan base
  - docker exec -it java_tomcat_container bash
  - chek java versi : java -version