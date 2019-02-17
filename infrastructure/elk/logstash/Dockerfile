FROM docker.elastic.co/logstash/logstash:6.6.0


RUN /usr/share/logstash/bin/logstash-plugin install logstash-input-jdbc
RUN /usr/share/logstash/bin/logstash-plugin install logstash-output-jdbc
RUN /usr/share/logstash/bin/logstash-plugin install logstash-output-google_cloud_storage
RUN /usr/share/logstash/bin/logstash-plugin install logstash-output-s3

RUN /usr/share/logstash/bin/logstash-plugin install logstash-filter-date_formatter