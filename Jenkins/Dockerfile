FROM jenkins/jenkins 
LABEL "com.inspo.solutions"="Inspo solutions"
LABEL com.inspo.solutions.label-with-value="Himadri Talukder"
LABEL version="1.0"
LABEL description="This docker file is going to setup a jenkins container "

ENV TZ America/New_York
USER root

# Install gradle
ENV GRADLE_VERSION 7.3.1
ENV GRADLE_HOME /usr/share/gradle
ENV PATH ${PATH}:${GRADLE_HOME}/bin
ENV GRADLE_USER_HOME /usr/share/gradle/gradle
RUN mkdir /usr/share/gradle
WORKDIR /usr/share/gradle
RUN curl -L "https://services.gradle.org/distributions/gradle-7.3.1-bin.zip" -o /usr/share/gradle/gradle-7.3.1-bin.zip && chmod +x /usr/share/gradle/gradle-7.3.1-bin.zip
RUN unzip gradle-$GRADLE_VERSION-bin.zip && \
    rm -f gradle-$GRADLE_VERSION-bin.zip && \
    ln -s gradle-$GRADLE_VERSION gradle && \
    chown -R jenkins: /usr/share/gradle/ && \
    echo -ne "- with Gradle $GRADLE_VERSION\n" >> /root/.built


#ENV CASC_JENKINS_CONFIG /usr/share/jenkins/ref/casc.yaml
COPY wait-for-it.sh /wait-for-it.sh
COPY --chown=jenkins:jenkins config/* $JENKINS_HOME/init-configs/
#COPY --chown=jenkins:jenkins config/auth.yml /usr/share/jenkins/ref/casc.yaml
#ENV JENKINS_CONFIG_PATH $JENKINS_HOME/init-configs
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/plugins.txt
USER root
RUN apt-get update \
    && apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common 
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
RUN apt-get update  -qq \
    && apt-get install docker-ce -y
RUN usermod -aG docker jenkins
RUN apt-get clean
RUN curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
# Skip initial setup
ENV JAVA_OPTS -Djenkins.install.runSetupWizard=false
ENV JENKINS_CONFIG_PATH $JENKINS_HOME/init-configs
USER jenkins
WORKDIR ${JENKINS_HOME}