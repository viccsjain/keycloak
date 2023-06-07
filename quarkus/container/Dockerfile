FROM sdartifact.td.teradata.com:7001/bcm/alpine:3.18
RUN apk update --no-cache
RUN apk upgrade --available
RUN wget --quiet https://cdn.azul.com/public_keys/alpine-signing@azul.com-5d5dc44c.rsa.pub -P /etc/apk/keys/ && \
    apk --repository https://repos.azul.com/zulu/alpine --no-cache add zulu17-jre
ENV KEYCLOAK_VERSION 999.0.0-SNAPSHOT 
ARG KEYCLOAK_DIST=keycloak-$KEYCLOAK_VERSION.tar.gz
RUN apk add bash
RUN apk add tar gzip
ADD $KEYCLOAK_DIST /tmp/keycloak/
RUN (cd /tmp/keycloak && \
    tar -xvf /tmp/keycloak/keycloak-*.tar.gz && \
    rm /tmp/keycloak/keycloak-*.tar.gz) || true
RUN mv /tmp/keycloak/keycloak-* /opt/keycloak && mkdir -p /opt/keycloak/data
RUN chmod -R g+rwX /opt/keycloak
RUN chown -R 1000:0 /opt/keycloak
RUN apk del tar gzip
RUN echo "keycloak:x:0:root" >> /etc/group && \
   echo "keycloak:x:1000:0:keycloak user:/opt/keycloak:/sbin/nologin" >> /etc/passwd
USER 1000

EXPOSE 8080
EXPOSE 8443

ENTRYPOINT [ "/opt/keycloak/bin/kc.sh" ]

#FROM registry.access.redhat.com/ubi9 AS ubi-micro-build
#
#ENV KEYCLOAK_VERSION 999.0.0-SNAPSHOT
#ARG KEYCLOAK_DIST=keycloak-$KEYCLOAK_VERSION.tar.gz
#
#RUN dnf install -y tar gzip
#
#ADD $KEYCLOAK_DIST /tmp/keycloak/
#
## The next step makes it uniform for local development and upstream built.
## If it is a local tar archive then it is unpacked, if from remote is just downloaded.
#RUN (cd /tmp/keycloak && \
#    tar -xvf /tmp/keycloak/keycloak-*.tar.gz && \
#    rm /tmp/keycloak/keycloak-*.tar.gz) || true
#
#RUN mv /tmp/keycloak/keycloak-* /opt/keycloak && mkdir -p /opt/keycloak/data
#RUN chmod -R g+rwX /opt/keycloak
#
#ADD ubi-null.sh /tmp/
#RUN yum install -y https://cdn.azul.com/zulu/bin/zulu-repo-1.0.0-1.noarch.rpm
##RUN yum install -y zulu17-jdk
#RUN bash /tmp/ubi-null.sh zulu17-jdk
#
#FROM registry.access.redhat.com/ubi9-micro
#ENV LANG en_US.UTF-8
#
#COPY --from=ubi-micro-build /tmp/null/rootfs/ /
#COPY --from=ubi-micro-build --chown=1000:0 /opt/keycloak /opt/keycloak
#RUN echo "keycloak:x:0:root" >> /etc/group && \
#    echo "keycloak:x:1000:0:keycloak user:/opt/keycloak:/sbin/nologin" >> /etc/passwd
#
#USER 1000
#
#EXPOSE 8080
#EXPOSE 8443

# ENTRYPOINT [ "/opt/keycloak/bin/kc.sh" ]
