# You'll get to know how the web works.
## Client Server Model and its types.
In client/server architecture, clients, or programs that represent users who need services, and servers, or programs that provide services, are separate logical objects that communicate over a network to perform tasks together. A client makes a request for a service and receives a reply to that request; a server receives and processes a request, and sends back the required response.
it has three types  
- 1-Tire  
- 2-Tire  
- 3-Tire

To learn more about client/server architecture and its types, visit <a href="https://docs.oracle.com/cd/E13203_01/tuxedo/tux71/html/intbas3.htm" target="_blank">this summary of the Client-Server architecture</a>

## DNS Servers
The Domain Name System (DNS) is the phonebook of the Internet. When users type domain names such as ‘google.com’ or ‘nytimes.com’ into web browsers, DNS is responsible for finding the correct IP address for those sites. Browsers then use those addresses to communicate with **origin servers** or **CDN edge servers** to access website information. This all happens thanks to DNS servers: machines dedicated to answering DNS queries.  

### How do DNS servers resolve a DNS query?
In a typical DNS query without any caching, four servers work together to deliver an IP address to the client:
- Recursive resolvers
- Root nameservers
- TLD nameservers
- Authoritative nameservers.

<img src="/images/dns_servers.png" width="800" height="400">

To learn more about client/server architecture and its types, visit <a href="https://www.cloudflare.com/en-gb/learning/dns/what-is-a-dns-server/" target="_blank">this summary of the DNS Servers</a>
