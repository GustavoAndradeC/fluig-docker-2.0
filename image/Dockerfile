FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

WORKDIR /opt/fluig

EXPOSE 8080 7777 8888 8000 8983 9990

ENTRYPOINT ["start-container"]

COPY . /var/fluig

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get -y install fontconfig wget \
    && apt-get clean \
    && mv /var/fluig/start-container /usr/local/bin/start-container \
    && chmod +x /usr/local/bin/start-container \
    && ln -snf /usr/share/zoneinfo/UTC /etc/localtime && echo UTC > /etc/timezone \
    && echo "alias log='tail -f /opt/fluig/appserver/domain/servers/fluig1/log/server.log'" >> /root/.bashrc \
    && wget -q https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-j_8.1.0-1ubuntu22.04_all.deb -O /tmp/mysql-connector.deb \
    && dpkg -i /tmp/mysql-connector.deb \
    && rm /tmp/mysql-connector.deb \
    && cd /var/fluig/installer \
    && chmod +x ./jdk-64/bin/java \
    && ./jdk-64/bin/java -cp fluig-installer.jar com.fluig.install.ExecuteInstall /var/fluig/install.conf \
    && mv /var/fluig/domain.xml /opt/fluig/appserver/domain/configuration/ \
    && mv /var/fluig/standalone.xml /opt/fluig/appserver/standalone/configuration/ \
    && cd / \
    && rm -fr /var/fluig \
    && true