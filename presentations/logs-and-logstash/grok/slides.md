!SLIDE transition=fade
# grok

!SLIDE transition=fade
regular expressions are pretty awesome

!SLIDE transition=fade
regular expressions are pretty awful

!SLIDE transition=fade
why do developers keep writing crappy log formats?

!SLIDE transition=fade
<pre style="word-wrap: break-word; font-size: 2em">
(?:(?:(?:\b(?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)\b) +(?:(?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9])) (?:(?!<[0-9])(?:(?:2[0123]|[01][0-9])):(?:(?:[0-5][0-9]))(?::(?:(?:(?:[0-5][0-9]|60)(?:[.,][0-9]+)?)))(?![0-9]))) (?:(?:(?:\b(?:[0-9A-Za-z][0-9A-Za-z-]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-]{0,62}))*(\.?|\b))|(?<a10>(?<![0-9])(?:(?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2}))(?![0-9])))) (?<a11>(?<a12>(?:[\w._/%-]+))(?:\[(?<a13>\b(?:[1-9][0-9]*)\b)\])?): (?<a14>(?<![0-9])(?:(?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2}))(?![0-9])):(?<a15>(?:[+-]?(?:[0-9]+))) \[(?<a16>(?<a17>(?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9]))/(?<a18>\b(?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)\b)/(?<a19>[0-9]+):(?<a20>(?!<[0-9])(?<a21>(?:2[0123]|[01][0-9])):(?<a22>(?:[0-5][0-9]))(?::(?<a23>(?:(?:[0-5][0-9]|60)(?:[.,][0-9]+)?)))(?![0-9])).(?<a24>(?:[+-]?(?:[0-9]+))))\] (?<a25>\S+) (?<a26>\S+)/(?<a27>\S+) (?<a28>(?:[+-]?(?:[0-9]+)))/(?<a29>(?:[+-]?(?:[0-9]+)))/(?<a30>\S+) (?<a31>\S+) (?<a32>\S+) (?<a33>(?:[+-]?(?:[0-9]+)))/(?<a34>(?:[+-]?(?:[0-9]+)))/(?<a35>(?:[+-]?(?:[0-9]+)))/(?<a36>(?:[+-]?(?:[0-9]+)))/(?<a37>\S+) (?<a38>(?:[+-]?(?:[0-9]+)))/(?<a39>(?:[+-]?(?:[0-9]+))))
</pre>

!SLIDE transition=fade bullet incremental
# haproxy tcp logs

* Could you write that?
* Could you debug what you wrote? 
* Could you maintain it?
* What about your coworkers?

<pre style="word-wrap: break-word; font-size: 0.5em">
(?:(?:(?:\b(?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)\b) +(?:(?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9])) (?:(?!<[0-9])(?:(?:2[0123]|[01][0-9])):(?:(?:[0-5][0-9]))(?::(?:(?:(?:[0-5][0-9]|60)(?:[.,][0-9]+)?)))(?![0-9]))) (?:(?:(?:\b(?:[0-9A-Za-z][0-9A-Za-z-]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-]{0,62}))*(\.?|\b))|(?<a10>(?<![0-9])(?:(?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2}))(?![0-9])))) (?<a11>(?<a12>(?:[\w._/%-]+))(?:\[(?<a13>\b(?:[1-9][0-9]*)\b)\])?): (?<a14>(?<![0-9])(?:(?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2}))(?![0-9])):(?<a15>(?:[+-]?(?:[0-9]+))) \[(?<a16>(?<a17>(?:(?:0[1-9])|(?:[12][0-9])|(?:3[01])|[1-9]))/(?<a18>\b(?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)\b)/(?<a19>[0-9]+):(?<a20>(?!<[0-9])(?<a21>(?:2[0123]|[01][0-9])):(?<a22>(?:[0-5][0-9]))(?::(?<a23>(?:(?:[0-5][0-9]|60)(?:[.,][0-9]+)?)))(?![0-9])).(?<a24>(?:[+-]?(?:[0-9]+))))\] (?<a25>\S+) (?<a26>\S+)/(?<a27>\S+) (?<a28>(?:[+-]?(?:[0-9]+)))/(?<a29>(?:[+-]?(?:[0-9]+)))/(?<a30>\S+) (?<a31>\S+) (?<a32>\S+) (?<a33>(?:[+-]?(?:[0-9]+)))/(?<a34>(?:[+-]?(?:[0-9]+)))/(?<a35>(?:[+-]?(?:[0-9]+)))/(?<a36>(?:[+-]?(?:[0-9]+)))/(?<a37>\S+) (?<a38>(?:[+-]?(?:[0-9]+)))/(?<a39>(?:[+-]?(?:[0-9]+))))
</pre>

!SLIDE transition=fade
# solution: grok

!SLIDE transition=fade incremental
# grok

* write patterns once
* name them
* test and verify
* reuse everywhere

!SLIDE transition=fade incremental
# grok

* IPORHOST = (?:%{HOSTNAME}|%{IP})
* HOSTNAME = \b(?:[0-9A-Za-z]...
* IP = (?<![0-9])(?:(?:25[0-5]|2[0-4][0-9]...

!SLIDE transition=fade incremental
# grok

* Ships with about 100 patterns
* It's easy to add new ones.

!SLIDE transition=fade incremental
# grok discovery

Logs -> Patterns for those logs

!SLIDE transition=fade incremental
# grok discovery

* Apr 20 00:53:46 rickastley roll: Never gonna give you up.
* %{SYSLOGBASE}\Q Never gonna give you up.\E

!SLIDE transition=fade incremental

%{SYSLOGBASE}\Q Never gonna give you up.\E

<pre style="word-wrap: break-word; font-size: 2em">
\Q\E(?<0000>(?<0001>(?<0002>\b(?:Jan(?:uary)?|Feb(?:ruary)?|Mar(?:ch)?|Apr(?:il)?|May|Jun(?:e)?|Jul(?:y)?|Aug(?:ust)?|Sep(?:tember)?|Oct(?:ober)?|Nov(?:ember)?|Dec(?:ember)?)\b) +(?<0003>(?:3[01]|[1-2]?[0-9]|0?[1-9])) (?<0004>(?!<[0-9])(?<0005>(?:2[0123]|[01][0-9])):(?<0006>(?:[0-5][0-9]))(?::(?<0007>(?:(?:[0-5][0-9]|60)(?:[.,][0-9]+)?)))(?![0-9]))) (?:(?<0008><(?<0009>\b(?:[0-9]+)\b).(?<000a>\b(?:[0-9]+)\b)>) )?(?<000b>(?<000c>(?:(?<000d>\b(?:[0-9A-Za-z][0-9A-Za-z-]{0,62})(?:\.(?:[0-9A-Za-z][0-9A-Za-z-]{0,62}))*(\.?|\b))|(?<000e>(?<![0-9])(?:(?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2})[.](?:25[0-5]|2[0-4][0-9]|[0-1]?[0-9]{1,2}))(?![0-9]))))) (?<000f>(?<0010>(?:[\w._/-]+))(?:\[(?<0011>\b(?:[0-9]+)\b)\])?):)\Q Never gonna give you up.\E
</pre>

!SLIDE transition=fade incremental

input:

* Aug 23 12:04:33 "hello world" 123.4.3.5 something something woo!

output:

* `%{SYSLOGTIMESTAMP} %{QS} %{IP} something something woo!`