## Synopsis
This program will take a series of named regexes and periodically print out the bandwidth used for all matching flows. Regexes are applied to the output of `tcpdump -nnql`.

So far only tested on various flavors of Linux. 

## Usage
```
 ratesniff [options] "name1=regex1" "name2=regex2"...

 Options
    -h              Print usage and quit
    -m              Print full manpage and quit
    -V              Print version and quit
    -i <dev>        The device to pass to tcpdump, defaults to eth0
    -n <interval>   The seconds between rate dumps, defaults to 10
    -t              Print a timestamp at the end of each line
    -d              Print debug info, for testing regexes
    -C <command>    Override the F<tcpdump> command
    -A <args>       Override the options passed to tcpdump
    -E <expression> Matching expression passed to tcpdump
    -b              Disable output buffering
    -r <scale>      Set displayed rate scale
```

## Example
```
%> ./ratesniff -i eth0 -n60 -t \
    "mysqlin=\.3306 > 10\.0\.0\.1" \
    "mysqlout=> .*?\.3306" \
    "httpout=\.80 >" \
    "httpin=> .*?\.80" \
    "totalout=10\.0\.0\.1\..*? >" \
    "totalin=> 10\.0\.0\.1"
Reading from tcpdump...
tcpdump: listening on eth0
       httpin       httpout       mysqlin      mysqlout       totalin      totalout         Time
  450.81 Kib/s    4.19 Mib/s    5.43 Mib/s  277.80 Kib/s    5.81 Mib/s    4.51 Mib/s   Feb  4 13:29:28
  463.18 Kib/s    4.22 Mib/s    8.50 Mib/s  777.13 Kib/s    8.89 Mib/s    5.02 Mib/s   Feb  4 13:30:28
```
