# Introduction to Monitoring Concepts
## Where We've Been
Monitoring -  We'll learn how to collect and display metric information, and how to automate problem detection through **alerting**  
Prometheus platform  
## What is monitoring?  
> "For those of you who run large distributed systems, no monitoring of the production system is as if you were driving a bus with the windshield covered."  
> Mikey Dickerson, Head of the U.S. Digital Service

On top of this on-demand, or ad hoc, debugging, monitoring data can also be used for **historical analysis**; it can spot trends, regressions, or plan capacity over time.  

Memory Leak
:    A problem that comes up when a computer program allocates some memory but never reclaims it  

Logging  

Mean time to repair (MTTR)
:    The speed of a fix for a given system  

Monitoring can give you the information you need about a system to make the decisions that make it more **reliable**.  
## Sources of Information - Metrics
Metrics
:    Information, often quantitative, about some aspect of a system 

Aggregation functions
:    Perform a calculation on all the metric data in a particular set over a period of time  

* Mean  
* Long-tail (extreme values)
* Distribution
:    Takes metric data and group them into buckets so the values of the outliers aren't masked  
* Counters
:    One of the most common types of metric, and are generally used to represent a numerical value that only increases
* Gauges
:    represent measured values which can go up or down
* Distributions  
..* Request durations  
..* Latencies  
..* File sizes  
..* Standard deviation
..* Median  
..* Mean  
## Collecting Metrics
Host level e.g.
* CPU Idle Time
* Disk Latency

CollectD Daemon  
Node exporter  
Powershell  

Application-level metrics  
These metrics could be things like **QPS** or **errors-per-second** for web applications.  For an IMAP mail server, it could be the **length of open connections**.  
```ruby
require 'prometheus/client'

histogram = Prometheus::Client::Histogram.new(...)

# Serve an HTTP request, and record the latency.
def serve_http(request)
    start = Time.now
    handle(request)    # process the request
    stop = Time.now
    elapsed_time = start - stop
    histogram.observe(elapsed_time)
end
```
4 Broad categories of signals:
* Latency
:    The time that it takes to service a request  
* Traffic
:    How much work your system is doing
* The rate of **errors** is also an important class of signal to collect.  The kinds of errors that a system will produce are usually dependent on the types of actions the system takes which can fail.  
* Level of saturation
:    Measured as a percentage of capacity, **saturation** can tell you which resources in your system are under the most pressure.
[Prometheus Ruby client](https://github.com/prometheus/client_ruby)
[Prometheus Ruby client gem](https://rubygems.org/gems/prometheus-client/versions/0.4.2)
## Metric Visualizations
Remember that the purpose of **monitoring** is to get insight into the state and behavior of the system being monitored.  
The x-axis represents the time series.  
The y-axis displays the value of the metric being measured.  
Common graphs:  
* Line graphs
* Heatmaps

Goals:
* Simplicity
* Graphs grouped into **Dashboards**

Most graphing systems give you the option to add some kind of **metadata** to your graphs, like axis labels, titles, and a short description of what information is displayed.  
[Datadog graphing](https://docs.datadoghq.com/graphing/)  
[Graphana panels](http://docs.grafana.org/features/panels/graph/)
## Summary
[Mikey Dickerson interview: How monitoring impacted the Healthcare.gov revamp](https://fcw.com/articles/2015/03/27/dickerson-at-sxsw.aspx)
# Alerting
## Alerting
Alert
:    A machine-generated notification of a problem, meant for a human to read and then act on  
Alerts usually take the form of some kind of **electronic communication** like emails, tickets, or pages.  
Instead, a better solution is to use automation, and have computers, instead of humans, watch out for the problems.  
## Alert Rules
Common Parameters:
* Expression
* Threshold
* Duration
* Severity

`Create an alert that fires off if the amount of available RAM on a host node falls below 500MB`  
```Prometheus
# Alert for any node that has low memory for more than 30 minutes.
ALERT LowNodeMemory
IF node_memory_available < 500000000
FOR 30m
LABELS { severity = "page" }
ANNOTATIONS {
    summary = "Node is running out of RAM.",
    description = "The node has had less than 500MG of memory for more than 30 minutes.",
}
```
Expression  
Duration
:    How long the expression has to evaluate to True in order for the alert to fire  
Presence alert  

Common severity:
* Page
* Ticket
* Email  

The purpose of the page is to bring attention to a **severe problem** that a human needs to fix in a noticeable way.
## Black box and White box Monitoring
Alerts generated from a black-box system are based on the symptoms of the problem, or the errors presented to a user at that moment.  
**Prober**
:    A piece of software that issues logical checks or probes against endpoints exposed by the monitored system  
```ruby
# Create a simple prober
#
# Poke the Google homepage HTTP endpoint and display the result.

require 'net/http'
require 'uri'

def main
    while true
        uri = URI.parse("http://www.google.com")
        response = Net::HTTP.get_response(uri)
        if response.code == "200"
            puts "Response OK!"
        else
            puts "Response of #{response.code} received!"
        end
    sleep(30)
    end
end

Signal.trap("INT") {
    # Exit on ctrl-C SIGINT
    puts "\nTime to exit!"
    exit
}

main()
```
[Prometheus - black box exporter](https://github.com/prometheus/blackbox_exporter)
## Alerting Philosophy
Monitoring solution should answer 2 main questions:
* What's broken (Symptom)
* and Why (Cause)

Generally, better to focus on catching *symptoms* rather than causes.
Symptoms usually indicate an imminent and real problem with the system.  
Setting an appropriate **alert severity** can be helpful to categorize issues.  
Remember that a good monitoring system will evolve and adapt alongside the systems it monitors, and that a VCS makes managing frequent changes easier and safer.  

| Symptom | Possible cause(s) |  
--- |--- |
Users cant' access their email. | The mail server is literally on fire
|Responses from a web application are really slow. | The cpu or memory is over-utilized and under pressure.  The network card of the host node is misconfigured and dropping packets.
|API request to your company's public HTTP endpoint are failing. | The load balancer isn't directing requests properly.
## Summary
Paging alerts should be actionable  

First, you want to choose a monitoring system that's **scalable**.  
Next, you'll want a monitoring system that's **reliable**  
Finally, a good monitoring system should be as **simple** as possible.  

Monitoring platforms
* [Datadog](https://www.datadoghq.com/)
* [Wavefront](https://www.wavefront.com/)
* [Prometheus](https://prometheus.io/)
* [Ganglia](http://ganglia.sourceforge.net/)
* [Nagios](https://www.nagios.com/)
* [Elastic Stack](https://www.elastic.co/products)

Tools to collect metrics
* [Sysdig](https://sysdig.com/product/monitor/)
* [collectD](https://collectd.org/)

Visualization
* [Grafana](https://grafana.com/)

Alert Delivery System
* [Pagerduty](https://www.pagerduty.com/)

[What makes a good page](https://community.spiceworks.com/topic/615275-what-makes-a-good-page-alerting-philosophy-from-google-site-operations)
# Monitoring by Example
## Monitoring Metrics with Prometheus
[Node exporter](https://github.com/prometheus/node_exporter)  
[Black box exporter](https://github.com/prometheus/blackbox_exporter)  
[WMI exporter](https://github.com/martinlindhe/wmi_exporter)  
Alertmanager 
## Prometheus Basics
Open-source White Box monitoring system.  
Jobs and Exporters  
[Docker](https://www.docker.com/)  
[Cookbook for installing Prometheus via Chef](https://github.com/elijah/chef-prometheus)  
[Download and Build](https://github.com/prometheus/prometheus)  
[Installation Documentations](https://prometheus.io/docs/prometheus/latest/installation/)  
## Collecting Host-Level Metrics
`node_exporter --collectors.enabled=cpu`
see full list at /metrics HTTP endpoint  
## Prometheus Targets
Configuration File - prometheus.yml  
```
scrape_configs:
    - job_name: "node"
      scrape_interval: "15s"
      static_configs:
      - targets:
        - "localhost:9100"
```
Start Prometheus Server  
`sudo prometheus -config.file=prometheus.yml`  
Metric notation  
`<metric name>{<label name>=<label value>, ...}`  

[Prometheus query language](https://prometheus.io/docs/prometheus/latest/querying/basics/)  
`sum(node_cpu{mode="iowait"})`  
## Metric Visualizations
How much memory is currently in use on our node?  
* node_memory_MemFree
* node_memory_Buffers
* node_memory_Cached
* node_memory_MemTotal metrics  

`node_memory_MemTotal - node_memory_MemFree - node_memory_Buffers - node_memory_Cached`  

[Memory ona Linux-based computer](http://man7.org/linux/man-pages/man5/proc.5.html)  
[tutorial on Installing Grafana and running it wih Prometheus](https://prometheus.io/docs/visualization/grafana/)
## Writing Alerts
Prometheus Alertmanager  
First, the Prometheus server reads the alerting rules you write and evaluates them, sending the alerts that trigger to the Alertmanager.  
Alertmanager then handles things like **grouping** similar alerts together, **silencing** them if the human respondent decides they're not relevant, and **sending notifications** to the alert destinations like pagers, email addresses and ticketing systems.  

create a rules file (we'll call it **alert.rules** in this example)  
```
ALERT filesystem_dangerously_high
    IF ((node_filesystem_size - node_filesystem_free{mountpoint='/'}) / node_filesystem_size) * 100 > 10
    ANNOTATIONS {
        summary = "Filesystem usage is dangerously high",
        description = "This device's filesystem usage has exceeded the threshold of 10% with a value of {{ $value}}.",
    }
```  
Modify prometheus.yml file to tell Prometheus about the rule.
```
rule_files:
    - 'alert.rules' # add pointer

scrape_configs:
    - job_name: "node"
      scrape_interval: "15s"
      static_configs:
      - targets:
        - "localhost:9100"
```
Then stop and restart Prometheus server  
alertmanager.yml - Alertmanager configuration file  
```
route:
    group_by: [Alertname]
    # Every alert is sent to the 'email' receiver.
    receiver: email

receivers:
    # Receiver is a named configuration of one or more notification integrations.
    # In this case, email.
- name: email
  email_configs:
  - to: "dotmatrix@gmail.com"
    from: "dotmatrix@gmail.com"
    smarthost: smtp.gmail.com:587
    html: '{{ template "email.default.html" . }}'
    auth_username: "dotmatrix@gmail.com"
    auth_identity: "dotmatrix@gmail.com"
    auth_password: "some_password_here"
```

Receiver
:    Notification mechanism that should get the alert  

Start Alertmanager  
`./alertmanager -config-file=../alertmanager.yml`
Tell Prometheus where Alertmanager is running  
`sudo ./prometheus -config-file=../prometheus.yml -alertmanager.url "http://localhost:9093"`  

[Prometheus Alertmanager code and documentation](https://github.com/prometheus/alertmanager)  
## Prometheus Black box Monitoring
Blackbox exporter prober  
For test purposes we'll set up a server to probe using Ruby  
```ruby
require "socket"

server = TCPServer.new("localhost", 2345)

while true
    socket = server.accept
    request = socket.gets
    puts request
    socket.print "HTTP/1.1 200\r\n"
    socket.print "Content-Type: text/html\r\n"
    socket.print "\r\n"
    socket.print "Hello there! the time is #{Time.now}"
    socket.close
end
```
Configure using a yml file  (blackbox.yml for our example)
```
modules:
    http_2xx:
    prober: http
    timeout: 5s
    http:
        valid_http_versions: ["HTTP/1.1", "HTTP/2"]
        valid_status_codes: [] # Defaults to 2xx
        method: GET
        headers:
            Host: localhost:2345
            Accept-Language: en-US
```
Start blackbox exporter  
`./blackbox_exporter -config.file=blackbox.yml`
Modify the scrape config section of Prometheus yml file  
```
rule_files:
    - 'alert.rules' # add pointer

scrape_configs:
    - job_name: "node"
      scrape_interval: "15s"
      static_configs:
      - targets:
        - "localhost:9100"
    - job_name: "blackbox"
      metrics_path: /probe
      params:
        module: [http_2xx] # Look for a HTTP 200 response.
      static_configs:
        - targets:
          - "localhost:2345"
      relabel_configs:
        - source_labels: [__address__]
          target_label: __param_target
        - source_labels: [__param_target]
          target_label: instance
        - target_label: __address__
          replacement: localhost:9115 # Blackbox exporter.

```
probe_success  
SIGHUP  signal  
Usually better to fire an alert after a number of probes have failed  
## Summary
Using a monitoring platform means you don't need to log into each machine on your network to collect disk usage and information, and you can be immediately notified of problems through alerts so you can fix them faster.