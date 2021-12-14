FROM openjdk:11
# define activemq version and workdir
ENV ACTIVEMQ_VERSION 5.16.3
ENV ACTIVEMQ_WORKDIR /app

# create the work dir
RUN mkdir -p $ACTIVEMQ_WORKDIR

#create user group and user permission

RUN groupadd -r activemq && useradd -g activemq activemq && chown -R activemq:activemq /app
USER activemq

# copy activemq
COPY apache-activemq-5.16.3-bin.zip $ACTIVEMQ_WORKDIR
RUN unzip $ACTIVEMQ_WORKDIR/apache-activemq-5.16.3-bin.zip -d $ACTIVEMQ_WORKDIR \
&& rm $ACTIVEMQ_WORKDIR/apache-activemq-5.16.3-bin.zip

RUN ls -ltrh $ACTIVEMQ_WORKDIR

COPY conf/jetty.xml $ACTIVEMQ_WORKDIR/apache-activemq-5.16.3/conf/jetty.xml
# change work dir
WORKDIR $ACTIVEMQ_WORKDIR/apache-activemq-5.16.3

# EXPOSE PORTS
# ACTIVEMQ UI
EXPOSE 8161
# ACTIVEMQ TCP
EXPOSE 61616

# start
CMD ["/bin/sh", "-c", "bin/activemq console"]