# Gunakan image dasar OpenJDK 17
FROM openjdk:17-jdk-slim

# Set environment variables untuk Java, Maven, dan Tomcat
ENV JAVA_HOME=/usr/local/openjdk-17
ENV MAVEN_VERSION=3.9.8
ENV MAVEN_HOME=/usr/share/maven
ENV TOMCAT_VERSION=7.0.109
ENV CATALINA_HOME=/usr/local/tomcat

# Instalasi dependencies curl, unzip, lalu unduh Maven dan Tomcat
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    unzip && \
    curl -fsSL https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.zip -o maven.zip && \
    unzip maven.zip -d /usr/share && \
    mv /usr/share/apache-maven-${MAVEN_VERSION} $MAVEN_HOME && \
    ln -s $MAVEN_HOME/bin/mvn /usr/bin/mvn && \
    rm -rf maven.zip && \
    curl -fsSL https://archive.apache.org/dist/tomcat/tomcat-7/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz | tar xz -C /usr/local/ && \
    mv /usr/local/apache-tomcat-${TOMCAT_VERSION} $CATALINA_HOME && \
    apt-get purge -y unzip && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Konfigurasi file tomcat-users.xml untuk akses ke Tomcat Manager
RUN echo '<?xml version="1.0" encoding="UTF-8"?>' > $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '<tomcat-users>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '  <role rolename="admin"/>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '  <role rolename="manager-gui"/>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '  <role rolename="manager-script"/>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '  <role rolename="manager-jmx"/>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '  <role rolename="manager-status"/>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '  <role rolename="manager"/>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '  <user username="admin" password="admin" roles="admin,manager,manager-gui,manager-script,manager-jmx,manager-status"/>' >> $CATALINA_HOME/conf/tomcat-users.xml && \
    echo '</tomcat-users>' >> $CATALINA_HOME/conf/tomcat-users.xml

# Izinkan akses dari semua alamat IP di Manager
# RUN sed -i 's|<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow="[^"]*" />|<Valve className="org.apache.catalina.valves.RemoteAddrValve" allow=".*" />|' $CATALINA_HOME/webapps/manager/META-INF/context.xml
RUN sed -i '/<Valve className="org.apache.catalina.valves.RemoteAddrValve"/d' $CATALINA_HOME/webapps/manager/META-INF/context.xml

# Atur izin pada folder manager ke 755
RUN chmod -R 755 $CATALINA_HOME/webapps/manager

# Salin file demo.war ke dalam direktori webapps Tomcat
COPY ./demo.war $CATALINA_HOME/webapps/

# Set working directory untuk aplikasi Java
WORKDIR /app

# Expose port 8080 untuk Tomcat
EXPOSE 8080

# Set entrypoint untuk menjalankan Tomcat
CMD ["sh", "/usr/local/tomcat/bin/catalina.sh", "run"]
