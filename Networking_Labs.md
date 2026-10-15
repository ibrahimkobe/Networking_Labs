## Networking Labs - Revision
>When I give an IP-Address in a LAN, The most important thing is every device in the same LAN to have the same Subnet.

🔹**To configure the Router you have 2 methods:**
1. From the Router itself (CLI).
2. To connect the Router with a (Console) wire with a new PC, This way will give you an access from the PC to the Router (==The name of the port that I will connect the PC with is "Rs-232"+console port)

#### Lab 1- Simple Router Configuration

🔹**Configuration terminal ( ==1) enable 2) conf t== ) contains the following**:

- Change hostname.
- Adding password to the console on user mode, exec mode, privileged exec mode.
- Add password to the remotely users.
- Enable password encryption.
- Add "motd" banner (Message of the day).
- Save running- config.


| **Command**                                                | **Usage**                                                                                                                                                         |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `enable`                                                   | Enter the Privileged exec mode (==user have all privileges==), بستخدم المود لما بدي اعرض اشي مخزن على الراوتر                                                     |
| `configure terminal` (==conf t==)                          | Enter the global configuration mode, اي اشي بدو تعديل او اضافة رح يكون بهاد المود                                                                                 |
| `hostname + name`                                          | To change hostName                                                                                                                                                |
| `line console 0`                                           | Enter the console 0 configuration (==CLI of the router==)                                                                                                         |
| `password + ThePassword`                                   | Add your console password                                                                                                                                         |
| `login`                                                    | Force anyone wants to use the CLI to use the password you assigned                                                                                                |
| `enable password + ThePassword`                            | Add password for the privileged exec mode but keep the password as a plain text in the settings                                                                   |
| `enable secret + ThePassword`                              | Add password for the privileged exec mode and encrypt the password in the settings                                                                                |
| `service password-encryption`                              | Any password in the configuration will be encrypted                                                                                                               |
| `line vty first point end point`                           | Enter the remote line access configuration with a maximum number of users that can access at the same time                                                        |
| `banner motd "Text" `                                      | A text will appear inside the CLI                                                                                                                                 |
| `interface`                                                | Enter the interface configuration mode                                                                                                                            |
| `copy running-config startup-config`                       | To save all of the configurations to the Router                                                                                                                   |
| `ip route <NID> <subnetmask> <Next Hop or exit interface>` | To configure static routes in the routing table of the Router (This is how you manually tell a router: If you want to reach Network X, send the data to Router Y) |
| `show ip protocols`                                        | it displays current state of all active IPv4 routing protocols (like OSPF, EIGRP, RIP, or BGP) running on a router.                                               |
| `wr`, `write memory`                                       | To save all of your work                                                                                                                                          |

🔹**Interface Configuration Steps**
- `enable`
- `configure terminal`
- `interface <type> <number>` , ==interface gigabitEthernet 0/0==
- `ip address <IpAdress>`
-  `no shutdown`

🔹**Remotely Users**
>All of these PCs are an example of a Remotely users.
![[0426EN_10_05.webp]]


🔹**Default Gateway**
>Its the "exit door" for a device. If a computer or switch wants to send data to an IP address that is **not** on its own local network, it sends that data to the Default Gateway (usually a Router).
Without a correctly configured Default Gateway, your devices can talk to their neighbors on the same switch, but they can never reach the Internet or another VLAN.

---

#### Lab 2 - Connecting 2 Routers
> when you make a ping message the first message will be a request time out because in this situation the first packet get lost to learn the path then it will give you a reply and the TTL (==Time To Live==) is going to be from 128 to 127

>**IMPORTANT:** Whenever I want to connect 2 Routers together I have to connect them with a "==Serial==" wire, They aren't in the same LAN to use Fast Ethernet 
>**You won't find a serial port so you have to add it** ==Port Name: NIM-2T==

![[Connecting-two-routers-in-Packet-Tracer-.png]]
>Now since there is a network I have to give an IP address, Subnetmask and the a configuration to the serial ports

🔹**How to config the serial ports (On each Router)**
1. `interface serial+<number>` (==0/1/0 , 0/1/1==)
2. `ip address <IP Address> + <subnetmask>`
3. `no shutdown`

---

#### Lab 3- Static Routing

> To let the routing table know that there is a network I need protocols.

🔹**Routing table protocols**
1. Directly
2. Static Routing
3. Dynamic Routing

- Router0 knows that there is 10.0.0.0 and 20.0.0.0 networks but it doesn't know about 30.0.0.0 , To add it:
>`ip route 30.0.0.0 255.0.0.0 <next hop or exit interface>` ==Exit interface is much easier==.
>	`ip route 30.0.0.0 255.0.0.0 se 0/1/0` , ==se 0/1/0 is the exit interface==

- **How to count hops?**
> How many  Routers do I have? 2 , then 2 hops, ==TTL will be 126 Because 128-2 = 126==

🔹**LoopBack**
>If I tried to ping an IP that isn't in my topology it will give me an unreachable message and the packets are going to get lost.
>The solution of this problem is ==Default route== which is the ISP (==Internet Service Provider==)

- ==Default route(LoopBack):== is a specific type of static route that acts as a "catch-all" or the **Gateway of Last Resort**. It tells the router:  If you don't have a specific path to a destination in your routing table, send it here. "ISP" 
- **Configuration**
1. Enter the Router
2. Config mode
3. `interface loopback <ID ,it could be any number for example 100>`
4. `ip address 200.0.0.1 255.0.0.0`
5. `no shutdown`
- Now to config the default route (using static protocol) because we don't know the IP of any network that I'm going to ping (it could be any IP)
1. `ip route 0.0.0.0  0.0.0.0 loopback 100`
2. `show ip route ` , s* in the routing table will be seen , that means static default route
>The default TTL for the loopback is 255.
- Now Router1 doesn't know about the default route, to make Router1 able to see the loopback ==I have to use RIP==
==The steps are in Router0 not 1==
1. `router rip`
2. `version 2`
3. `default-information originate`
4. `show ip route (in Router1)`, R* in the routing table will be seen, this means RIP default route
>TTL= 254 

---

#### Lab 4- Dynamic Routing

>Most important Disadvantage of Static routing that I have to add any new network manually so if I had 100 Routers it will be much harder to configure them manually.

- **Dynamic Routing**
1. RIP- Routing Information Protocol 
2. OSPF - Open Shortest Path first

🔹**RIP**
- The RIP was a first generation routing protocol.
- It's easy to configure & doesn't require any advanced knowledge, this makes it a good choice for ==small networks==
- RIP uses ==Bellman-ford== algorithm to get the remote route and calculate its metric.
- RIP version 1 usually consists of a periodic updates every 30 sec of the entire routing table that are sent only to directly connected routers and sent as a broadcast of 255.255.255.255 .
- In most current networking environments, RIP isn't the preferred choice of routing as its time to converge and scalability are poor compared to EIGRP, OSPF routing protocols (administrative distance is 120).

>the three main reasons why professional network engineers rarely use RIP in real-world modern environments:
>1. **Convergence Time:** RIP is slow. Because it waits for that 30-second timer (from your first note) to send updates, it can take minutes for a network to "heal" if a cable is cut. OSPF and EIGRP can recover in milliseconds.
   > 
>2. **Scalability:** RIP has a **maximum hop count of 15**. If a destination is 16 routers away, RIP considers it "unreachable." This makes it useless for large networks.
   > 
>3. **Administrative Distance (AD):** * **RIP AD = 120**
  >  
    - **OSPF AD = 110**
   >     
    - **EIGRP AD = 90**
   >     
   > 
    > **Note:** Lower AD is "more trusted." If a router learns about the same network from both RIP and OSPF, it will **ignore** the RIP route because 110 is better than 120.

🔹**RIP version 1 VS RIP version 2**
- **RIP 1**
1. Broadcast.
2. Classful (assumes default subnetmask).
- **RIP 2**
1. Multicast.
2. Classless (includes subnetmask in each route).
3. Increased efficiency (forwards updates to multicast address 224.0.0.9).
4. Reduced routing entries (manual route summarization).
5. Secure (It supports an authentication mechanism to secure routing table updates between neighbors).


| **Commands**                                               | **Usage**                                                                                       |
| ---------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| `no enable password`                                       | To delete Password                                                                              |
| `no enable secret`                                         | To delete secret                                                                                |
| `router rip`                                               | To Use RIP                                                                                      |
| `version 2`                                                | To use version 2                                                                                |
| `no auto-summary`                                          | Making sure of the classless feature in version 2                                               |
| `network <NIDs that are connected directlu to the router>` | To know the connected networks                                                                  |
| `passive interface gig0/0/0`                               | to delete any interface that doesn't take an update every 30 sec, now the traffic will be lower |

>- RIP depends on Hop Count , Maximum hop count is 15 only which means ==maximum routers are 15==.
>- more than 15 will give you an unreachable message.
>- When hop counts are lower its always better ==Best Path==

---

#### Lab 5- OSPF
>The main problem in RIP is Routers limitation.

🔹**OSPF configuration**
1. Enter the first Router
2. `router ospf + <proccess ID, it could be any number>`
3. `passive-interface (gig 0/0/0 , gig 0/0/1)`
4. `network + <NID connected directly to the router> + <wild card> + area 0 `
5. Enter the second Router
6. `router ospf + <another proccess ID, (it could be the same)>`
7. `passive-interface gig 0/0/0`
8. `network + <NID> + <wild card> + area 0` , The area must be the same 
9. `show ip route`, O in the routing table will be seen, which means OSPF

---

#### Lab 6 - HTTPS, FTP, DNS, Email Servers
>**IMPORTANT** The IP for the Servers must be always Static, It can't be dynamic.

🔹**HTTPS**
1. Enter the server then go to services.
2. Choose the service.
3. To turn on HTTPS, turn off HTTP and on the HTTPS.
4. On the same page you can go to index.html, and edit the banner message.
5. To access the web from another PC you have to enter web browser first then type in URL the ip address of the server Because we still don't have a domain name like google for example.

🔹**DNS**
>It translates human-readable domain names (like `google.com`) into the machine-readable IP addresses (like `142.251.46.238` or an IPv6 address) that computers use to identify each other on a network. Without DNS servers, you would have to memorize complex strings of numbers every time you wanted to visit a website.

- **Note:*You have to give every end device the IP address of the DNS, from IP configurations inside the device (==an end device is "blind" to domain names by default, Because devices only understand IP addresses== ).
- DNS Configuration
1. Give DNS an IP and a Subnetmask.
2. Give the end devices the IP address of the DNS.
3. Go to DNS then services.
4. Press DNS service ON.
5. Type Domain Name in the name section <Networking.com>
6. Put the address of this Domain Name <For example the IP of the HTTPS, You have to put the IP of the end device that you want to access>
7. Add.

🔹**File Transfer Protocol (FTP)**
>Transfers Data from a specific Source to a specific Destination.
>Port Number: "20, Transfers Data" , "21, Controls Data "

- FTP Configuration
1. Give the server an IP and a Subnetmask.
2. Then access the server services, then FTP.
3. Make it ON, Add usernames & passwords of the clients.
4. Add permissions (write, read...)

- **How Pc0 makes a file and uploads it to FTP server then PC1 can download it?**
1. Enter text editor on PC0 , write on it then save it.
2. **Additional** ( go to command prompt write `dir` to see all folders ).
3. Enter command prompt write  `ftp + <ip address for ftp>` to enter the server.
4. Write the username + password.
5. `Put + <file name.txt>` to upload the file to the server.
6. Enter PC1 then CMD.
7. `ftp + <ip address for the ftp>` then enter username & password.
8. `get + <fileName.txt>

🔹**Email server**
>Port Number: "25 (SMTP), The protocol for sending" , "110 (POP3), The protocol for receiving"
- **Email Server Configuration**
1. Enter Email service.
2. Add domain Name `<htu.edu.jo>` Then set.
3. Username & Password.
4. Enter Pc0, Enter Email.
5. Configure mail `<Name, Email, user@DomainName>`
6. Add incoming server `<IP address for email server>`
7. Add outcoming server `<IP address for email server>`
8. Add username and password 
a) Repeat on Pc1 but different user.
b) Press compos to send email.

🔹**Dynamic Host Configuration protocol (DHCP)**
>Dynamically assigns IP address to attached hosts, **Uses UDP**
>Port Number: "67, for the Server" , Port Number: "68, for the Client"

![[DHCP server to client image.webp]]
- DHCP Configuration:
>**You should  configure the OSPF first**

1. configure a static IP address to Router interfaces. (gateway, serial) `interface <g0/0/1>,` `ip address <Gateway_IP_for_Server_Network> <Subnet_Mask>`
   `no shutdown`
2. configure a static IP address to DHCP server.
3. configure DHCP pools on DHCP servers.
4. configure a routing Protocols on routers.
5. configure IP helper address (DHCP relay agent). 

>**APIPA IP address**: is a networking feature that allows a device to automatically assign itself an IP address when it cannot find a DHCP server.

- When you try to request for a DHCP IP to a host that is outside the Lan (where the DHCP is) it will give you failed, The reason is when an end device requests an IP it uses a broadcast to request it and this method won't pass the switch so the message will never reach.
- **Solution:** IP helper "DHCP relay agent"

🔹**IP helper configuration**
1. Enter Router of the LAN that doesn't contain DHCP
2. `interface <gig0/0/0>` gig0/0/0 because the message didn't pass from this interface.
3. `ip helper-address <IP of the DHCP>`

#### Lab 7- IOT Devices

>You must have a wired network to be able to add a wireless one.

- **Configuration**
1. Go to wireless devices.
2. Choose Ac-Pt (Access Point).
3. Connect the switch with the (Ac-Pt) Port.
4. Add an end device like laptop.
5. We want to make the Ac more secure by adding authentication (WAP2-PSK). 
6. Add PSK pass phrase (password to the connection). 
7. Enter the laptop to Desktop the pc wireless. 
8. You have to put the interface that makes the laptop connect to a wireless connection:
	a)power off laptop.  
	b) take the wired connection port from the pc. 
	c) Add wpc300N to the laptop.

9. Pc wireless in Desktop, go to connect button.