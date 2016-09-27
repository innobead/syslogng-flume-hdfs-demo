# Syslog-ng, Flume & HDFS integration
A syslog log aggregation sample, integrate syslog, Flume and HDFS.

## Configurations

***syslog-ng.conf***

    source s_file {
        file("/var/log/input" follow-freq(1) flags(no-parse));
    };
    
    destination d_file {
        file("/var/log/output");
    };
    
    destination d_flume {
        network("flume" port(7777));
    };
    
    log {
        source(s_file);
        destination(d_file);
        destination(d_flume);
    };

***flume-conf.properties***

    a1.sources = syslog
    a1.channels = memory
    a1.sinks = hdfs
    
    a1.sources.syslog.type = syslogtcp
    a1.sources.syslog.host = flume
    a1.sources.syslog.port = 7777
    a1.sources.syslog.channels = memory
    
    a1.sinks.hdfs.type = hdfs
    a1.sinks.hdfs.channel = memory
    a1.sinks.hdfs.hdfs.path = hdfs://hadoop:9000/tmp/syslog/
    a1.sinks.hdfs.hdfs.fileType = DataStream
    
    a1.channels.memory.type = memory
    a1.channels.memory.capacity = 1000
    a1.channels.memory.transactionCapacity = 100

## Usage
1. Up docker containers

> docker-compose up

2. Input message to syslog log file, to see it redirects to HDFS

> docker exec syslogngflumehdfs_syslog_1 /bin/bash -c "echo 'hellow world' \>\> /var/log/input"
> docker exec syslogngflumehdfs_hadoop_1 /usr/local/hadoop/bin/hadoop fs -cat /tmp/syslog/*

## Notes
1. Need to copy hadoop common libraries (version the same from the hadoop instance) to Flume classpath to make HDFS sink work

> <hadoop distribution>/share/hadoop/common/*.jar including files under libs too

