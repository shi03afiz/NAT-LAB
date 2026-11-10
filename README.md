NAT LAB REPORT  
LAN to Internet (NAT + DHCP)
Date: 10 Nov 2026

Goal: To make 7 PCs on LAN reach the internet and web server through NAT

Topology:
 [PCs] - [Switch] - [MyRouter] - [ISP Router] - [Internet / Web server]

IP Plan Table:
Device    Port    IP    Gateway
MyRouter    Gi0/0   192.168.1.1    
MyRouter    Gi0/1    209.165.200.1    
ISP              Gi0/0    209.165.200.2    
ISP              Gi0/1    8.8.8.1
PCs             DHCP  192.168.1.1
Internet / Server       8.8.8.1    8.8.8.2

What I Did:
I configured NAT (Network Address Translation) on MyRouter to allow devices on the private LAN 192.168.1.0/24 to access the internet. I configured the inside interface G0/0 with IP 192.168.1.1, the outside interface G0/1 with IP 209.165.200.1, and created an access-list to permit the LAN traffic. I also configured a default route pointing to the ISP router 209.165.200.2 and set up NAT overload so all internal hosts could share one public IP.
On the ISP router, I configured G0/0 with 209.165.200.2 and G0/1 with 8.8.8.1 to connect to the Internet Server 8.8.8.2.

Commands I Used:
Access-list 1 permit 192.168.1.0   0.0.0.255
Ip nat inside source list 1 interface Gi0/1 overload 
Ip route 0.0.0.0  0.0.0.0 
Ip nat inside / ip nat outside on Gi0/0 and Gi0/1

 How I knew it worked:
I tested connectivity step by step from the PCs:
1. Ping to Gateway 192.168.1.1 - Successful
2. Ping to WAN 209.165.200.1 and 209.165.200.2 - Successful
3. Ping to ISP Outside 8.8.8.1 and Internet Server 8.8.8.2 - Successful
4. Web access to http://8.8.8.2 - Successful, the Cisco Packet Tracer page loaded
This proves that NAT translation was working and private hosts could reach the public internet.

 Problem I Faced:
First ping failed because i forgot default route, so i added ip route 0.0.0.0 0.0.0.0 209.165.200.2 and it worked.
 What I Learned.
NAT enable all PCs in the topology to share one public IP to reach the internet.

## Screenshots

### Network Topology
![Topology](Network%20Topology%20showing%20LAN,%20my%20router,%20ISP%20router%20and%20internet%20server.png)

### Ping to Default Gateway
![Ping Gateway](ping%20to%20default%20gateway%20192.168.1.1%20from%20pc.png)

### Ping to ISP Router
![Ping ISP](Successful%20ping%20to%20ISP%20router%20209.165.200.2%20from%20PC%203.png)

### Ping to WAN Interface
![Ping WAN](Successful%20ping%20to%20WAN%20interface%20209.165.200.1.png)

### Web Access to Internet Server
![Web Access](Successful%20Web%20Access%20to%20Internet%20Server%20(8.8.8.2)%20from%20PC5%20via%20NAT.png)



