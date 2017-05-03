FROM jenkins:2.19.1
ARG gituser=changeme
ARG gitpass=changeme

MAINTAINER erdaoya

COPY init/init.groovy /usr/share/jenkins/ref/init.groovy
COPY init/settings.xml /usr/share/jenkins/settings.xml
COPY init/gradle.properties /usr/share/jenkins/gradle.properties
COPY init/plugins.txt /usr/share/jenkins/plugins.txt

USER root

# Generated via `start.sh`. If you don't want to provide it just put empty
# files there
RUN printf "%s" "${gituser}" > /usr/share/jenkins/gituser
RUN printf "%s" "${gitpass}" > /usr/share/jenkins/gitpass

USER root

ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false \
              -Djava.awt.headless=true \
              -Dhudson.model.ParametersAction.keepUndefinedParameters=true

RUN apt-get update && \
    apt-get install -y ruby curl && \
    apt-get clean

USER jenkins
RUN mkdir -p ~/.ssh
COPY init/id_rsa.pub ~/.ssh/id_rsa.pub

# You can use Jenkins API to generate the list of plugins from a running
# Jenkins instance:
#
#  $ JENKINS_URL="http://user:pass@localhost:8080"
#  $ curl -sSL "${JENKINS_URL}/pluginManager/api/json?depth=1" | \
#    jq -r '.plugins[] | .shortName +":"+ .version' | sort > plugins.txt
#
RUN install-plugins.sh $( paste -sd' ' /usr/share/jenkins/plugins.txt )
