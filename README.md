# TEST_JAVA_MAVEN

FROM openjdk:17-jdk-slim

# Set environment variables untuk Java, Maven, dan Tomcat

ENV JAVA_HOME=/usr/local/openjdk-17
ENV MAVEN_VERSION=3.9.8
ENV MAVEN_HOME=/usr/share/maven
ENV TOMCAT_VERSION=7.0.109
ENV CATALINA_HOME=/usr/local/tomcat

echo "# TEST_JAVA_MAVEN" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/adijewel97/TEST_JAVA_MAVEN.git
git push -u origin main
