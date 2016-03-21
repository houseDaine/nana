# nginx-log-analyzer
A tool written in shell and awk to analyze performance bottleneck for nginx. So far the following metrics are extracted:
* Requests count
* Unique ip count
* Total bytes transfered and average bytes per erquest
* transfer rate
* Requests count and unique ip count that response time is longer than specified limit(3 seconds in default)

## 0. Assumptions
It's assumed that your Nginx log file is in the following format:
`'$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent $request_time $upstream_response_time "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';`
And here is an example of the log file:
`183.63.28.26 - - [21/Mar/2016:03:53:41 +0800] "GET /weshares/index.html HTTP/1.1" 200 100759 0.255 0.105 "-" "Mozilla/5.0 (iPhone; CPU iPhone OS 9_1 like Mac OS X) AppleWebKit/601.1.46 (KHTML, like Gecko) Mobile/13B143" "-"`

 As '$remote_addr' is extracted as column 10 in awk,`$body_bytes_sent` as column 10 and `$request_time` as column 11.
 
## 1. Usage
`./nanalyze.sh <logfile> [timeLimitInSeconds]`

Where <logfile> is the absolute location of nginx log file; [timeLImitInSeconds] is optional with a default value of 3 seconds

For example: 
`./nanalyze.sh /var/log/nginx/main.log`

And here is an exmample output
`---------nginx summary---------
requests: total 78970, slow 178
unique ip: total 4229, slow 53
time: average 0.205734s, max 73.528s
traffic: total 1158.1MB, max 1417.18KB, average 72.9927KB/req
rate: average 4186.4KB/s, max 757.32KB/s, min 0KB/s
`

## 2. TBD
* List requests that have a lower response time than specific value
* List requests that have a bigger response size than specific value
* Analyze and alert when nginx or app is slow(note that sometimes user has a poor network bandwith on mobile phones)
