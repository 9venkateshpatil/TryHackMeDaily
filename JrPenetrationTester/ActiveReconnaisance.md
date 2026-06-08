# ***Active Reconnaissance***

**Active Reconnaissance Room from the Jr Penetration Tester Path**

## **TASK 3**

We learned about one of the most common commands used for active recon: <u><i>ping</i></u>.  
The name 'ping' comes from the sound of a sonar pulse.

### **How ping works?**

<u><i>ping</i></u> uses the ICMP protocol. It sends an ICMP echo request packet, and if the target receives the packet and is permitted to answer, it sends back an ICMP echo reply.

We use <u><i>-c</i></u> to define the number of packets transmitted.  
If we do not do this, then the <u><i>ping</i></u> command will keep sending packets and will not stop unless we press <u><i>Ctrl+C</i></u>.

## **TASK 4**

<u><i>traceroute</i></u> is another important utility used for discovering the IP addresses of the routers (hops) along the path and determining how many of them sit between you and the destination.  
It is useful for understanding the network topology.

### **How it works?**

It is not a direct procedure. Instead, it uses the TTL, or Time To Live, field of an IP header to determine the number of hops.  
Each router along the path decrements the TTL by one.  
Once a router notices that the TTL is <u><i>0</i></u>, it sends an ICMP Time To Live Exceeded message back to the sender.

<u><i>traceroute &lt;ip address&gt;/&lt;domain name&gt;</i></u>  
is the default command, which uses UDP probes.

Some routers deny traceroute requests or are configured not to reply, so they appear as <u><i>*</i></u>.

To reduce this issue, we use <u><i>-T</i></u> to use TCP packets instead of the normal UDP ones.  
TCP-based traceroute is useful to avoid or bypass UDP filters on the router.

We also used <u><i>telnet</i></u> to connect to a TCP service and for banner grabbing.

### **Telnet syntax**

<u><i>telnet &lt;target_ip&gt; &lt;port&gt;</i></u>

Then we have to manually type:

<u><i>GET / HTTP/1.1</i></u>  
<u><i>Host: telnet</i></u>

Then we get the header.

The same applies to <u><i>netcat</i></u>.  
The command is:

<u><i>nc &lt;target_ip&gt; &lt;port&gt;</i></u>

While using <u><i>nc</i></u> and <u><i>telnet</i></u>, we have to manually type the <u><i>GET</i></u> and <u><i>Host</i></u> headers, but using <u><i>curl</i></u> we do not need that.

So <u><i>curl</i></u> is preferred for HTTP or web-based banner grabbing.

Also, <u><i>netcat</i></u> can be used as a client connecting to a listening port or to listen on a port as a server.

**That is it in this room.**
