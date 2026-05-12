# <center> Domain Name Systems ~ DNS </center>

The `Domain Name System` (DNS) is a *distributed*, *hierarchical* system designed to translate human-readable domain names into IP addresses.

- Operates in a `hierarchical & distributed` manner.
- Provides name-to-address resolution using a hierarchical system to locate domain information. 
- Improves performance by using caching mechanisms, reducing lookup time. 

## Parts of URL (Uniform Resource Locator)
A URL is a unique address used to locate and access resources on the internet. 
> A URL is composed of multiple parts, each defining how and where a resource can be accessed. 

![url](https://media.geeksforgeeks.org/wp-content/uploads/20260121115340137993/url_parts.webp)

<ul><li value="1"><b><strong>Scheme:</strong></b><span> Defines the protocol used to access the resource (e.g., http, https, ftp).</span></li><li value="2"><b><strong>Subdomain:</strong></b><span> Specifies a subdivision of the main domain (e.g., www).</span></li><li value="3"><b><strong>Domain:</strong></b><span> Identifies the main website name (e.g., example).</span></li><li value="4"><b><strong>Top-Level Domain (TLD):</strong></b><span> Indicates the domain extension (e.g., .com, .org).</span></li><li value="5"><b><strong>Port Number:</strong></b><span> Specifies the server port (default: 80 for HTTP, 443 for HTTPS).</span></li><li value="6"><b><strong>Path:</strong></b><span> Points to the resource location on the server.</span></li><li value="7"><b><strong>Query String Separator:</strong></b><span> The ? symbol that starts query parameters.</span></li><li value="8"><b><strong>Query String / Parameters:</strong></b><span> Passes data as key–value pairs using &amp;.</span></li><li value="9"><b><strong>Fragment:</strong></b><span> Refers to a specific section within the resource using #.</span></li></ul>

---
## Working of DNS

1. **User Input**: The user enters a domain name (e.g., www.example.com) in the browser.
2. **Local Cache Check**: The browser or OS checks its cache for a stored IP address.
3. **DNS Resolver Query**: If not found, the request is sent to a DNS resolver (usually by ISP).
4. **Root Server Query**: The resolver queries a root server, which points to the correct `TLD` server.
5. **TLD Server Response**: The TLD server directs the resolver to the domain’s `authoritative` server.
6. **Authoritative Server Response:** The authoritative server returns the actual IP address.
7. **Final Response**: The resolver sends the IP back to the user, and the browser connects to the server.

![dns](https://media.geeksforgeeks.org/wp-content/uploads/20230731154431/How-DNS-Works-gif-(1).gif)

## DNS Record Types

Different DNS record types are used to store specific information about a domain.

1. `A Record`: Maps a domain name to its corresponding IPv4 address.
2. `CNAME Record`: Creates an alias that points one domain name to another domain name.
3. `MX Record`: Specifies the mail server responsible for receiving emails for a domain.
4. `TXT Record`: Stores text information used for verification and email authentication purposes.

![records](https://assets.bytebytego.com/diagrams/0175-dns-record-types-you-should-know.png)

We have more Record but this are the important onces.
---

## DNS Components and Their Roles 

#### 1. Root Name Server
Root servers handle requests for information about Top-level domains. The root servers won’t actually know where the domain is hosted. They will, however, be able to direct the requester to the name servers that handle the specifically requested top-level domain.
> There are 13 logical root servers (labeled A through M), operated by various organizations worldwide.

**Example**: if a request for “www.wikipedia.org” is made to the root server. It will find a record for the `org` TLD and give the requesting entity the address of the name server responsible for `org` addresses.

#### 2. Top-Level Domain (TLD) Servers
**These servers manage the next layer of the domain hierarchy. For instance, the `.com` TLD server directs queries for domains ending in `.com`.**

The requester then sends a new request to the IP address (given to it by the root server) that is responsible for the top-level domain of the request. 
- In our case, it would send a request to the name server responsible for knowing about “org” domains to see if it knows where “www.wikipedia.org” is located.
-  TDL server will provide the IP address of the *name server* responsible for “wikipedia.org”.

#### 3. Authoritative Name Servers or Domain-Level Name Servers
**These servers contain the definitive information about a domain, including its IP address and DNS records (such as A, AAAA, MX, etc.).**


- The name server returns the final answer to the requester.

#### 4. DNS Resolver
- What/who is the **requester** in this situation?

**`Resolvers`/`Resolving Name Server` (or recursive resolvers) are the intermediaries that handle DNS queries from clients. They communicate with root, TLD, and authoritative servers to retrieve the required information.**

- Basically, a user will usually have a few resolving name servers configured on their computer system.
- When you type a URL in the address bar of your browser, your computer first looks to see if it can find out locally where the resource is located. It checks the “hosts” file on the computer and a few other locations. 
- It then sends the request to the resolving name server and waits back to receive the IP address of the resource. The resolving name server then checks its cache for the answer. If it doesn’t find it, it goes through the steps outlined above.

<P>Resolving name servers basically compress the requesting process for the end user. The clients simply have to know to ask the resolving name servers where a resource is located and be confident that they will investigate and return the final answer.</P>

**ZONE FILES**: `Zone files` are the way that name servers store information about the domains they know about. Every domain that a name server knows about is stored in a zone file. Most requests coming to the average name server are not something that the server will have zone files for.

---

## DNS Caching 

<P>DNS caching is a temporary storage system that stores recent domain-to-IP mappings to speed up future lookups. Instead of querying DNS servers every time, the system first checks the cache to reduce lookup time.</P>

* **First Request**: When you visit a website, your system queries a DNS serer for its IP address.
* **Cache Storage**: The IP address is saved locally - on `device, router, ISP server` - for a set of duration `TTL`
* **Subsequent Requests**: Future visits to that URL use the cached IP, skipping the DNS lookup and making the connection faster. 

`TTL`(**Time to Live**) determines how long a DNS record remains valid in cache before it must be refreshed or discarded. After expiration, the cached record is discarded, triggering a fresh DNS lookup.

### DNS Cache Hierarchy / Layers

DNS caching occurs at multiple levels, forming a hierarchy that optimizes performance and reduces repeated DNS lookups. 

![cache](https://media.geeksforgeeks.org/wp-content/uploads/20260331102903202468/dns_3.webp)

* DNS Resolver Cache is maintained by ISP or a third-party DNS service. 

1. **Router-Level DNS Caching**: Routers cache DNS queries locally for connected devices, reducing external DNS requests and improving network efficiency.
2. **DNS Resolver (ISP/Third-Party) Caching**: DNS resolvers cache responses for multiple users, reducing the need to query authoritative servers repeatedly and improving resolution speed.
3. **Content Delivery Network (CDN) Caching**: CDNs cache content and help route users to the nearest server, reducing latency and improving load times.
4. **Host File Caching**: The hosts file acts as a manual override, allowing systems to resolve domain names before making any DNS query.

---

Services such as `CloudFlare` and `Route 53` provide managed DNS services. Some DNS services can route traffic through various methods:
- Weighted round robin
    - Prevent traffic from going to servers under maintenance
    - Balance between varying cluster sizes
    - A/B testing
- Latency-based
- Geolocation-based

[DNS Architecture](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197427(v=ws.10)?redirectedfrom=MSDN)

---

## Usage Methods 

1. `nslookup` / `dig` to resolve the domain names and get ipaddress.
2. `dig +trace -4 example.com` to Trace full DNS resolution path. 

> `dig` shows what your system uses; `dig +trace` shows how the internet finds it.

**system resolver (`127.0.0.53` = systemd-resolved).**

```code
dig → cache → instant
+trace → live internet → slow & verbose
```