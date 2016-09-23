FROM probablyfine/flume

ENV FLUME_CONF_DIR /opt/flume/conf/
ENV FLUME_CONF_FILE ${FLUME_CONF_DIR}/flume-conf.properties

ADD flume-conf.properties ${FLUME_CONF_DIR}/ 
ADD flume-env.sh ${FLUME_CONF_DIR}/
ADD hadoop/libs/* /opt/hadoop/libs/

