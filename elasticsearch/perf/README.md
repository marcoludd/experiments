# elasticsearch+logstash perf exploration

## Configuration

* [logstash.conf](https://github.com/jordansissel/experiments/tree/master/elasticsearch/perf/logstash.conf)
* [elasticsearch template](https://github.com/jordansissel/experiments/tree/master/elasticsearch/perf/template.sh) (6 shards, 0 replicas)
* logstash run: `timeout 300 java -XX:+UseParNewGC -XX:+UseConcMkSweepGC -Xss256k -jar logstash-1.1.7-monolithic.jar agent -f logstash.conf`
* elasticsearch run: `bin/elasticsearch -f`
* logstash run on same host as elasticsearch

## Data

* https://docs.google.com/spreadsheet/ccc?key=0Aq9liCTsAyzRdDBlMzJNQncxWVhucElIV19wR2NyOFE

## Results

* null output: 24179 events/sec
* tcp output: 16284 events/sec
* file output: 16399 events/sec
* 1 node ES w/ elasticsearch: 5265 events/sec
* 2 node ES w/ elasticsearch: 5326 events/sec
* 1 node ES w/ elasticsearch\_http: 4446 events/sec
* 2 node ES w/ elasticsearch\_http: 4233 events/sec

## Conclusions

* null output is 'best possible performance' right now
* tcp and file outputs perform the same; drop against 'null' is likely JSON
  serialization.
* Big loss in performance with elasticsearch logstash output (vs tcp/file).
* 20% loss in performance using elasticsearch\_http instead of elasticsearch
  plugin.
* No difference in performance with 2 ES nodes vs 1.

## Next steps:

* Try to get tcp and file outputs closer to null output performance
* Find out why 1-to-2 ES nodes had no performance improvement.
* Improve performance of elasticsearch\_http relative to elasticsearch..
* Improve performance elasticsearch and elasticsearch\_http relative to null,
  tcp, and file outputs.