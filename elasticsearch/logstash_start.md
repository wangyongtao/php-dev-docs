
检测 java 版本
$ java -version
java version "1.8.0"
Java(TM) SE Runtime Environment (build 1.8.0-b132)
Java HotSpot(TM) 64-Bit Server VM (build 25.0-b70, mixed mode)



wget https://artifacts.elastic.co/downloads/logstash/logstash-5.3.0.tar.gz

tar zxf logstash-5.3.0.tar.gz

cd logstash-5.3.0

vi logstash-simple.conf

input { stdin { } }
output {
  elasticsearch { hosts => ["localhost:9200"] }
  stdout { codec => rubydebug }
}

bin/logstash -f logstash-simple.conf

http://localhost:9600/

{
    host: "YONG-TEST.local",
    version: "5.3.0",
    http_address: "127.0.0.1:9600",
    id: "6bd6fa7c-1559-42fc-9864-7275a2da3081",
    name: "YONG-TEST.local",
    build_date: "2017-03-23T03:56:19Z",
    build_sha: "da58f0946883e10b4472f6ade80d38b6e02947be",
    build_snapshot: false
}

To enable automatic config reloading, start Logstash with the --config.reload.automatic (or -r) command-line option specified. For example:

bin/logstash –f apache.config --config.reload.automatic