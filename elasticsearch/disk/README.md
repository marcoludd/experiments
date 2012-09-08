# logstash+elasticsearch storage experiments

Problem: Many users observe a 5x inflation of storage data from "raw logs" vs
logstash data stored in elasticsearch.

Hypothesis: There are likely small optimizations we can make on the
elasticsearch side to occupy less physical disk space.

Constraints: Data loss is not acceptable (can't just stop storing the logs)

Options:

* Compression ([LZF](http://www.elasticsearch.org/blog/2012/06/25/0.19.5-released.html) and
  [Snappy](http://www.elasticsearch.org/blog/2012/08/23/0.19.9-released.html)
* Disable the ['_all' field](http://www.elasticsearch.org/guide/reference/mapping/all-field.html)
* For well-parsed logs, remove the '@message' field.

## Discussion

The compression features really need no discussion. 

The purpose of the '_all' field is documented in the link above. In logstash,
users have reported success in disabling this feature without losing
functionality.

In this scenario, I am parsing apache logs. Logstash reads lines from a file and
sets the '@message' field to the contents of that line. After grok parses it
and produces a nice structure, making fields like 'bytes', 'response', and
'clientip' available in the event, we no longer need the original log line, so
it is quite safe to delete the @message (original log line) in this case. Doing
this saves us much duplicate data in the event itself.

## Test scenarios

* 0: test defaults
* 1: disable _all
* 2: store compress + disable _all
* 3: store compress w/ snappy + disable _all
* 4: remove superfluous things (@message and @source) (simulate 'apache logs in json')

## Test data

One million apache logs from semicomplete.com:

    % du -hs /data/jls/million.apache.logs 
    218M    /data/jls/million.apache.logs
    % wc -l /data/jls/million.apache.logs
    1000000 /data/jls/million.apache.logs

## Environment

This should be unrelated to the experiment, but including for posterity if the
run-time of these tests is of interest to you.

* CPU: Xeon E31230 (4-core)
* Memory: 16GB
* Disk: Unknown spinning variety, 1TB

## Results

<table>
  <tr>
    <td> run </td>
    <td> space usage </td>
    <td> elasticsearch/original ratio  </td>
    <td> run time (wall clock) </td>
  </tr>
  <tr> 
    <td> ORIGIN </td>
    <td> 218M    /data/jls/million.apache.logs </td>
    <td> N/A </td>
    <td> N/A </td>
  </tr>
  <tr>
    <td> 0 </td>
    <td> 1358M    /data/jls/millionlogstest/0.yml </td>
    <td> 6.23x </td>
    <td> 6m47.343s </td>
  </tr>
  <tr>
    <td> 1 </td>
    <td> 1183M    /data/jls/millionlogstest/1.yml </td>
    <td> 5.47x </5d>
    <td> 6m13.339s </td>
  </tr>
  <tr>
    <td> 2 </td>
    <td> 539M    /data/jls/millionlogstest/2.yml </td>
    <td> 2.47x </td>
    <td> 6m17.103s </td>
  </tr>
  <tr>
    <td> 3 </td>
    <td> 537M    /data/jls/millionlogstest/3.yml </td>
    <td> 2.47x </td>
    <td> 6m15.382s </td>
  </tr>
  <tr>
    <td> 4 </td>
    <td> 395M    /data/jls/millionlogstest/4.yml </td>
    <td> 1.81x </td>
    <td> 6m39.278s </td>
  </tr>
</table>

## Conclusion

This test confirms what many logstash users have already reported: it is easy
to achieve a 5-6x increase in storage from raw logs caused by common logstash
filter uses, for example grok.

* Enabling store compression uses 55% less storage
* Removing the @message and @source fields save you 26% of storage.
* Disabling the '_all' field saves you 13% in storage.