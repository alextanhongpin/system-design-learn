# system-design-learn
Learning system design basics


# Seven steps for system design interviews


- **Step 1: Requirements clarification:** Always ask a question to find the exact scope of the problem you are solving
- **Step 2: System interface definition:** Define what APIs are expected from the system. This will also ensure you haven't gotten any requirement wrong.
- **Step 3: Back-of-the-envelope estimation:** Also called the Fermi problem, it's good to always have a good estimate on the scale of thes system you are going to design
- **Step 4: Define data model:** This helps you decide on how data are partitioned.
- **Step 5: High-level design:** Block diagrams to represent core components of the system.
- **Step 6: Detailed component design:** Dig deeper into 2-3 components 
- **Step 7: Bottlenecks:** Try to discuss as many bottlenecks as possible - and find ways to mitigate them. Note that sometimes it is impossible to mitigate them, so you need to understand what is the tradeoff of the approach you choose.


## Gathering Requirements

According to wiki, the process of defining, maintaining and documenting requirements is called [requirements engineering](https://en.wikipedia.org/wiki/Requirements_engineering). To simplify things, we focus on gathering _functional_ and _[non-functional](https://en.wikipedia.org/wiki/Non-functional_requirement)_ requirements. We use the shortform **FR** for functional requirements and **NFR** for non-functional requirements:

Here's the difference:
- FR define _what a system is supposed to do_, NFR _how a system is supposed to be_.
- FR "system shall do <requirement>", NFR "system shall be <requirement>"

## Capacity Estimations and Constraints

- Traffic estimates. Is the system read or write heavy? e.g. 20/1 read to write ratio. To hit slightly more than 1 million requests per day, we only need 12 requests per second (1,000,000 / 60 / 60 / 24). 
- Storage estimates. 1 mil requests, each of size 1kb will require 1MB storage.
- Bandwidth estimates. 1 mil requests per second, each of size 100kb will result in 1MB/s.
- Memory estimates, usually for cache. 


## System Components

### Load balancer vs reverse proxy

A **reverse proxy** accepts a request from a client, forwards it to a server that can fulfil it, and returns the server's response to the client.

A **load balancer** distributes incoming client requests among a group of servers, in each case returning the response from the selected servers to the appropriate client.

Nginx is able to serve as both load balancer and reverse proxy.

Reference: https://www.nginx.com/resources/glossary/reverse-proxy-vs-load-balancer/

### Load balancing algorithm

- round robin
- weighted round robin - same as round robin, but some servers gets a larger share of the overall traffic
- source ip hash - connections are distributed to backend servers based on the source ip address. If a webnode fails and is taken out of service the distribution changes. As long as all servers are running a given client ip address will always go to the same web server.
- url hash. Much like source IP hash, except hashing is done on the URL of the request. Useful when load balancing in front of proxy caches, as requests for a given object will always go to just one backend cache. This avoids cache duplication, having the same object stored in several / all caches, and increases effective capacity of the backend caches.
- least connections, weighted least connections. The load balancer monitors the number of open connections for each server, and sends to the least busy server.
- least traffic, weighted least traffic. The load balancer monitors the bitrate from each server, and sends to the server that has the least outgoing traffic.
- leat latency. Perlbal makes a quick HTTP OPTIONS request to backend servers, and sends the request to the first server to answer.
