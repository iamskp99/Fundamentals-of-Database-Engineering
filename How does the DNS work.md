### Suppose you are asked to make a DNS system ?

So the naive idea that comes to our mind is to keep a centralized database and index it , make partition ,shard it and many other things but the thing is our internet hosts billion of websites and
it would take finite amount of time to run the query and get the ip address.


So how does it currently work ?

So currently we have three moving parts named Root level servers , TLD (Top Level Domain Servers) servers and Auth NS (Authorative Name Servers).
So for example we are hitting www.google.com. Now, we know that its top level domain is .com , we will go to the .com TLD server. Now, we will search
for the domain name in it, it will then point to Auth NS, that server will give us our ip address. DNS doesn't use TCP , it uses UDP with query id and
that makes it fast. Godaddy is a service that provides Auth Ns for your domain. 

### I want to ask that suppose we have two servers, those two will have different ip address.
### Auth NS maps domain to server ip address, in this case which ip will it point to ?

```
In a typical DNS setup, when you have two servers with different IP addresses, you can configure your Authoritative Name Server (Auth NS) to point to both IP addresses using DNS records. This setup is known as DNS Round Robin.

Here's how it works:

1. You have two servers: Server A with IP address 1.2.3.4 and Server B with IP address 5.6.7.8.

2. You configure your Auth NS to create DNS records for your domain that map to both server IP addresses. This can be done by creating two A records (Address records) with the same hostname (e.g., www) but different IP addresses:

   - www.example.com IN A 1.2.3.4
   - www.example.com IN A 5.6.7.8

3. When a client requests the IP address of www.example.com, the DNS resolver may return one of the IP addresses in a round-robin fashion. For example, the first request might return 1.2.3.4, and the next request might return 5.6.7.8. Subsequent requests continue to cycle through the IP addresses in the order they are listed.

This setup allows you to distribute incoming traffic between the two servers, providing a basic form of load balancing. However, it's important to note that this method doesn't consider server health or availability. If one of the servers becomes unavailable, DNS Round Robin won't automatically route traffic away from it. More advanced load balancing solutions, like dedicated load balancers or DNS-based load balancing services, can provide better control and failover mechanisms.

In summary, the Auth NS can map a domain to multiple server IP addresses, and DNS Round Robin is one way to achieve basic load balancing between those servers.
```
