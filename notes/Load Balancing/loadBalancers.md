# <center> What is a Load Balancer ?</center>
A load balancer is a device or software application that distributes network or application traffic across multiple servers.

Load balancers are effective at:
* Preventing requests from going to unhealthy servers scales Applications
* Preventing overloading resources and imprves performance
* Helping to eliminate a single point of failure ensuring Availability and Reliabilty

![lb](https://assets.bytebytego.com/diagrams/0261-what-is-a-load-balancer.png)


#### Additional benefits include:
1. **Traffic Distribution**: To keep any one server from becoming overburdened, load balancers divide incoming requests evenly among several servers.
2. **High Availability**: Applications' reliability and availability are improved by load balancers, which divide traffic among several servers. The load balancer reroutes traffic to servers that are in good condition in the event that one fails.
3. **Scalability**: By making it simple to add servers or resources to meet growing traffic demands, load balancers enable horizontal scaling.
4. **Optimization**: Load balancers optimize resource utilization, ensuring efficient use of server capacity and preventing bottlenecks.
5. **Health Monitoring**: Load balancers often monitor the health of servers, directing traffic away from servers experiencing issues or downtime.
6. **SSL termination**: Some load balancers can handle SSL/TLS encryption and decryption, offloading this resource-intensive task from servers.. Decrypt incoming requests and encrypt server responses so backend servers do not have to perform these potentially expensive operations. Removes the need to install `X.509` certificates on each server
7. **Session persistence**: Issue cookies and route a specific client's requests to same instance if the web apps do not keep track of sessions.

![lb2](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgs7yGseA2JblV-s_vw5HoRkfsQ4WM9fBOprJByra9VPXiUxXCYf-D4NWqqVdJKJrYd3xDUgOiPIccV9JNc_32ph9MnmgDKjeAlceNKsR3JT7kiYFwWW6i2ySTa8Kj3FwU2ceWhd4P04CGA/s400/p1.png)

In this model, there is a dispatcher that determines which worker instance will handle the request based on different policies. The application should best be "stateless" so any worker instance can handle the request. 
<p>This pattern is deployed in almost every medium to large web site setup.</p>

![lb3](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgFqHcngyxv_0yf4AnQhxkS96UlsM-W48GnLe8KYRKDO6QgkSqK9GhnpuzrG8Fthn870OerRpY-p9L2pl180pr8ohCrnvd_R5rUhQp_DKuMxb0GENhqWxM1yqBBnWN8mOA9Q7z07SPPTl4x/s400/P3.png)

In this model, the dispatcher will first lookup if the request has been made before and try to find the previous result to return, in order to save the actual execution.
<p>This pattern is commonly used in large enterprise application. Memcached is a very commonly deployed cache server.</p>

> To protect against failures, it's common to set up multiple load balancers, either in `active-passive` or `active-active`mode.

---
Load balancers can route traffic based on various metrics/Algorithms, including:

![lb4](https://assets.bytebytego.com/diagrams/0251-lb-algorithms.png)

### Static Algorithms

1. **Round Robin**: The client requests are sent to different service instances in sequential order. The services are usually required to be stateless. 
2. **Sticky round robin**: This is an improvement of round robin, If a persons request goes to service A, all other following requests from the client will go to service A. 
3. **Weighted round robin**: The admin can specify the weight for each service. The ones with a higher weight handle more requests than others. 
4. **HASH**: This algorithm applies a hash function on the incoming requests’ IP or URL. The requests are routed to relevant instances based on the hash function result.

### Dynamic Algorithms

1. **Least connections**: A new request is sent to the service instance with the least concurrent connections. 
2. **Weighted Least Connections**: Lke Lest Connections, but takes **server capacity** into account. 
3. **Least Response Time**: A new reqest is sent to the service instance with the fastest response time. 
4. **Resource Based**: Balancing decisions are based on metrics like **CPU, memory consumption, queue length**.

---

## Types of Load Balancers
































#### Comparing the Types

<table>
<thead>
<tr>
<th>Type</th>
<th>OSI Layer</th>
<th>Flexibility</th>
<th>Speed</th>
<th>Cost</th>
<th>Example Tools</th>
</tr>
</thead>
<tbody>
<tr>
<td>Layer 4 (Transport)</td>
<td>L4</td>
<td>Low</td>
<td>Very High</td>
<td>Medium</td>
<td>LVS, HAProxy</td>
</tr>
<tr>
<td>Layer 7 (Application)</td>
<td>L7</td>
<td>Very High</td>
<td>Moderate</td>
<td>Medium</td>
<td>Nginx, Envoy</td>
</tr>
<tr>
<td>Hardware Appliance</td>
<td>L4/L7</td>
<td>Medium</td>
<td>Very High</td>
<td>High</td>
<td>F5, Citrix</td>
</tr>
<tr>
<td>Software Load Balancer</td>
<td>L4/L7</td>
<td>High</td>
<td>High</td>
<td>Low</td>
<td>HAProxy, Nginx</td>
</tr>
<tr>
<td>Cloud Managed LB</td>
<td>L4/L7/DNS</td>
<td>Very High</td>
<td>High</td>
<td>Pay-as-you-go</td>
<td>AWS ELB, GCP LB</td>
</tr>
<tr>
<td>DNS Load Balancer</td>
<td>DNS</td>
<td>Medium</td>
<td>High</td>
<td>Low</td>
<td>Route53, Cloudflare</td>
</tr>
</tbody>
</table>
















![cloudLB](https://assets.bytebytego.com/diagrams/0094-cloud-load-balancer-cheatsheet.gif)