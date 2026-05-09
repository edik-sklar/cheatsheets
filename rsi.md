OSI Model—the seven-layer map of how the internet actually functions.
The Big Picture: Where Everything Fits
Imagine your data as a letter being sent.

    The Application (SSH, HTTP, FTP): This is the content of the letter.
    The Security Wrapper (TLS, SSL): This is the envelope that hides the content.
    The Transport (TCP, UDP): This is the delivery truck that moves the envelope.

1. The Core Connectivity (The Foundation)
Before you can encrypt anything, you need a way to move packets.

    TCP (Transmission Control Protocol): The "Reliable" one. It ensures every piece of data arrives in order (used by SSH, TLS, and HTTP).
    UDP (User Datagram Protocol): The "Fast" one. It sends data without checking if it arrived (used for gaming and Zoom calls).

2. The Security Layers (The "Envelopes")
We’ve talked about TLS and SSH, but there are others:

    IPsec (Internet Protocol Security): Unlike TLS (which happens at the application level), IPsec encrypts everything at the Network Level. This is how VPNs work—it’s like building a private tunnel through the public road.
    QUIC: A new protocol (by Google) that combines the speed of UDP with the security of TLS 1.3 built directly into it. It’s what makes modern web browsing feel so fast.

3. Identity and Trust (The "ID Cards")
Encryption is useless if you're talking to a hacker.

    PKI (Public Key Infrastructure): The global system of "Trust" (Certificate Authorities) that tells your browser "Yes, this really is Google.com."
    OAuth2 / OpenID Connect: These aren't for encryption, but for Identity. When you see "Login with Google," that’s OAuth. It tells the server who you are after the TLS tunnel is already secure.

4. Application Protocols (The "Action")
Now that it’s secure, what are you doing?

    HTTPS: HTTP (Web data) + TLS.
    DNSSEC: Secures the "phonebook" of the internet so hackers can't redirect yourbank.com to their own IP address.
    SFTP: FTP (File Transfer) running over an SSH tunnel.

The "Zoomed Out" Summary

    SSH is for Managing a machine (Login + Control).
    TLS is for Browsing and Apps (Privacy + Identity).
    IPsec/VPN is for Networking (Connecting two offices/homes).
    ACID/CAP are for Data Storage (How the info inside the server stays organized once it arrives).

A Practical Example:
When you buy something on Amazon:

    DNS finds the IP.
    TLS creates the secure handshake (using Diffie-Hellman).
    HTTPS sends your credit card info.
    The server processes the transaction following ACID rules to ensure you aren't charged twice.
    The database uses CAP principles to make sure your order shows up even if a data center in Virginia goes offline.


The RED Method is a monitoring framework specifically designed for microservices and request-driven applications. It shifts the focus from hardware performance to the user experience by tracking how well a service handles incoming traffic. 
The Three RED Metrics

    Rate: The number of requests the service is handling per second. This helps you understand workload patterns and detect sudden spikes (traffic surges) or drops (upstream failures). 
    Errors: The number of those requests that are failing. This is often measured as a percentage of total traffic to provide a normalized view of reliability. 
    Duration: The amount of time it takes to process a request (latency). It is best measured using percentiles (like p95 or p99) rather than averages, which can hide the "tail latency" experienced by the unluckiest users



RED vs. Other Frameworks
RED is part of a broader set of observability philosophies. Most SRE teams use a combination of these to get a full picture of their system. 
Framework 	Metrics	Best Used For...
RED	Rate, Errors, Duration	Services & APIs: Focuses on what the user feels.
USE Method	Utilization, Saturation, Errors	Infrastructure: Focuses on hardware like CPU, RAM, and disks.
Four Golden Signals	Latency, Traffic, Errors, Saturation	Full-Stack: A broader standard from Google that includes RED + saturation.
Why use it?
The main benefit of RED is standardization. By using the same three metrics for every microservice, different teams can easily understand the health of each other's services without needing specialized knowledge of the underlying code. This makes it a great foundation for setting Service Level Objectives (SLOs). 

To implement RED metrics in a real-world dashboard like Grafana, you can use the following standard Prometheus (PromQL) queries. 
1. Rate (Total Requests per Second) 
This measures the volume of traffic flowing through your service. 

    Query:
    sum(rate(http_requests_total[1m])) by (service)
    What it does: It calculates the per-second average rate of increase for the request counter over the last minute, grouped by service name. 

2. Errors (Error Rate Percentage)
This measures the proportion of failed requests (typically those returning a 5xx status code). 

    Query:
    (sum(rate(http_requests_total{code=~"5.."}[1m])) by (service) / sum(rate(http_requests_total[1m])) by (service)) * 100
    What it does: It divides the rate of failed requests by the total request rate to give you a percentage. Using a regex (5..) captures all server-side errors. 

3. Duration (Latency Percentiles) 
This measures how long it takes to process requests. For dashboards, it is more accurate to use percentiles (like the 99th percentile) rather than averages to capture "tail latency". 

    Query (p99):
    histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket[1m])) by (le, service))
    What it does: It looks at the duration histogram buckets and tells you the maximum time it took for the fastest 99% of requests. 

Comparison at a Glance
Metric 	PromQL Function	Focus
Rate	rate() or irate()	Throughput / Load
Errors	sum() / sum()	Reliability / Failures
Duration	histogram_quantile()	Latency / User experience
Pro Tip: When graphing these in Grafana, use rate() for smooth lines showing long-term trends, and irate() if you need to see sharp, instant spikes in traffic. 