# <center>Forward Proxy & Reverse Proxy</center>

> A proxy is an entity that has the authority to act on behalf of another.

<p>Proxies and reverse proxies are servers that sit between clients and servers to improve security, privacy and performance.</p>

A **Proxy server** (sometimes called Forward Proxy) acts on behalf of clients, while a **Reverse Proxy** acts on behalf of servers. 

![proxy](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fwqces5nwe4hb4bydyd13.png)

---

# 1. Proxy Server / Forward Proxy 

When you send a request, like opening a webpage, the proxy intercepts it, forwards it to the target server, and then relays the server's response back to you. 
**Think of proxy server as a middleman that sits between the private network and the public internet.**
<ol>
<li>The user types a website URL into their browser. The request is intercepted by the proxy server instead of going directly to the website.</li>
<li>The proxy server examines the request to decide if it should forward it, deny it, or serve a cached copy.</li>
<li>If the proxy decides to forward the request, it contacts the target website. The website sees only the proxy server’s IP, not the user’s.</li>
<li>When the target website responds, the proxy receives the response and relays it to the user.</li>
</ol>

### Key Benefits of Proxy Servers:
1. **Privacy and Anonymity**: Proxy servers hide your IP address by using their own, so the destination server cannot know your real location or identity. 
2. **Access Control**: Organizations use proxies to enforce content restrictions, monitor internet usage. 
3. **Security**: Proxies can filter out malicious content and block suspicious sites, providing an additional layer of security. 
4. **Improved Performance**: Proxies cache frequently accessed content, reducing latency and improving load times for websites. 

```code
Is a VPN the same as a Proxy?

No. While both hide your IP, a VPN encrypts all your internet traffic, making it more secure. 
A proxy only forwards specific requests without necessarily encrypting them.
```

### Real Work Applications of Proxy Server
1. **Bypassing Geographic Restrictions**
        * One of the most common uses of proxy servers is bypassing geographic restrictions on websites and content.
        * Streaming services, for instance, often offer different content based on a user’s location. With a proxy server based in the target region, you can access that region’s content library as if you were a local user. **Similar to VPN**

2. **Speed and Performance Optimization**
        * Proxies can store cached versions of frequently accessed content, enabling faster load times and reducing bandwidth usage. 

---
# 2. Reverse Proxy Server (`nginx/haproxy`)

<p>A **reverse proxy** is a server positioned between clients (like browsers) and one or more backend web servers. Instead of clients connecting directly to the origin server, all requests first go through the reverse proxy, which then forwards them to the appropriate backend and returns the response to the client.</p>

Think of a reverse proxy as a gatekeeper. Instead of hiding clients from the server, it hides servers from clients. Allowing direct access to servers can pose security risks, exposing them to threats like hackers and DDoS attacks.

<p>A reverse proxy mitigates these risks by creating a single, controlled point of entry that filters and regulates incoming traffic all while keeping server IP addresses hidden.With a reverse proxy in place, clients no longer interact directly with the servers.</p>

1. A user types a website URL into their browser, which sends a request to the server.
2. The reverse proxy server receives the request before it reaches the backend servers.
3. Based on predefined rules (like load balancing or server availability), the reverse proxy forwards the request to the appropriate backend server.
4. The backend server processes the request and sends a response back to the reverse proxy.
5. The reverse proxy relays the response to the client, with the client never directly interacting with the backend servers.

#### Key Benefits of Reverse Proxy
1. **Enhanced Security**: By acting as a protective layer, a reverse proxy hides backend servers from clients, reducing the risk of attacks directly targeting backend infrastructure.
2. **Load Balancing**: A reverse proxy can distribute incoming requests evenly across multiple backend servers, improving system reliability and preventing server overload.
3. **Caching Static Content**: Reverse proxies can cache static assets like images, CSS, and JavaScript, reducing the need to fetch these files from the backend repeatedly.
4. **SSL Termination**: Reverse proxies can handle SSL encryption, offloading this work from backend servers.
5. **Web Application Firewall (WAF)**: Reverse proxies can inspect incoming requests, acting as a firewall to detect and block malicious traffic.

```code
What is WEB APPLICATION FIREWALL (WAF) ?

A WAF protects your web app by inspecting HTTP/HTTPS traffic and filtering out malicious requests. 
Example: SafeLIne, AWS WAF

* Block common Attacks like SQL injection
* Rate Limiting and bot detection
* Access control and IP reptation checks
* Logging and alerting for suspicious activity

Many open source or commercial solutions now combine both roles into a single component, reducing deployment complexity and improving performance.
```
#### Real-World Example of a Reverse Proxy
**Cloudflare’s** reverse proxy is widely used by global websites and applications to boost speed, security, and reliability.

- It’s `Web Application Firewall (WAF)` and `DDoS`protection blocks malicious traffic before it reaches the site’s servers, safeguarding against attacks and improving uptime.
- Cloudflare’s global content *caching* caches static and dynamic content at over 200 data centers around the world, storing frequently accessed files (like images, CSS, and JavaScript) closer to users. This significantly reduces load times and latency, as requests don’t always need to travel to the origin server.

```code
What is CloudFlare ?

Cloudflare is a widely used content delivery network (CDN) and internet security service designed to improve your website’s speed, protect it from cyber threats, and ensure uptime even during high traffic periods. It is beneficial for running an e-commerce store, blog, or corporate website, Cloudflare helps optimize loading speeds and block malicious activity like DDoS attacks, all without compromising user experience.
```
---
# What’s the Difference Between Load Balancer/Reverse Proxy/API Gateway?

- A `reverse proxy` accepts a request from a client, forwards it to a server that can fulfill it, and returns the server’s response to the client. Whereas deploying a `load balancer` makes sense only when you have multiple servers, it often makes sense to deploy a reverse proxy even with just one web server or application server. You can think of the reverse proxy as a website’s *“public face.”*
- `API Gateway` acts as a single-entry point for all API requests and provides features such as request routing, rate limiting, authentication, and API versioning and also hide the complexities of the underlying microservices from the client applications.
- Solutions such as `NGINX` and `HAProxy` can support both layer 7 reverse proxying and load balancing.

![cal](https://miro.medium.com/v2/resize:fit:720/format:webp/1*IyQTbwVHSh359sSsEIxWFQ.jpeg
)
## Disadvantages of Reverse Proxy:
- Introducing a reverse proxy results in increased complexity.
- A single reverse proxy is a `single point of failure`, configuring multiple reverse proxies (ie a failover) further increases complexity.