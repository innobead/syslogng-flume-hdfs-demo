#FROM probablyfine/flume
FROM openjdk:8-jre

RUN mkdir /opt/flume
RUN wget -qO- http://archive.apache.org/dist/flume/1.6.0/apache-flume-1.6.0-bin.tar.gz \
  | tar zxvf - -C /opt/flume --strip 1

ADD start-flume.sh /opt/flume/bin/start-flume
ADD hadoop/libs/* /opt/hadoop/libs/

ENV PATH /opt/flume/bin:$PATH
ENV FLUME_CONF_DIR /opt/flume/conf
ENV FLUME_CONF_FILE ${FLUME_CONF_DIR}/flume-conf.properties

CMD [ "start-flume" ]
