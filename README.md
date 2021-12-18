<div id="top"></div>


<!-- PROJECT LOGO -->
<br />
<div align="center">
    <img src="images/spreedsheet icon.jpg" alt="Logo" width="500" height="400">
  
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
        <li><a href="#Introduction">introduction</a></li>
        <li><a href="#Part-I">Functionality</a></li>
        <li><a href="#Part-II">SpreedSheet Header</a></li>
        <li><a href="#Part-III">SpreedSheet cpp</a></li>
  </ol>
</details>

# Introduction:

<p align="center">
     <img src="images/Network-arch.png">
   </p>

# Part-I

**Setup ISC DHCP server**
First we install the DHCP server.

## Installing the DHCP server
`sudo apt-get install isc-dhcp-server`

<!-- DHCP configuration -->
# Part-II

### Network architecture DHCP

<p align="center">
     <img src="images/DHCP-arch.png">
   </p>
   
## Configuring the DHCP server
First we have to define for which network interface the ISC DHCP server should work. To do this, we open the following configuration file.

`sudo nano /etc/default/isc-dhcp-server`

Here we add the interface name in the line provided. Here we choose “eth1”. Depending on the application, it can also be “wlan0”.
```sh
INTERFACESv4="eth1"
INTERFACESv6=""
```
### Subnets declaration
`sudo nano /etc/dhcp/dhcpd.conf`

Then the editor opens with an empty file, in which we enter the following configuration.
```sh
authoritative;
default-lease-time 600;
max-lease-time 7200;

subnet 192.168.1.0 netmask 255.255.255.0
{
        range 192.168.1.50 192.168.1.80;
        option routers 192.168.1.1;
        interface eth1;
}

subnet 192.168.2.0 netmask 255.255.255.0
{
        range 192.168.2.50 192.168.2.80;
        option routers 192.168.2.1;
        interface eth0;
}
```
## Interfaces
`sudo nano /etc/network/interfaces`

```sh
auto lo
iface lo inet loopback

allow-hotplug eth0
iface eth0 inet static
        address 10.1.1.1
        gateway 10.1.1.1
        network 10.1.1.0
        netmask 255.255.255.0

allow-hotplug eth1
iface eth1 inet static
        address 192.168.1.1
        gateway 192.168.1.1
```
### firewall_disabled
`sudo nano /etc/selinux/config`

<p align="center">
     <img src="images/firewall_disabled.png">
   </p>
   
 ### Routing
`sudo nano route -n`

<p align="center">
     <img src="images/routing_dhcp.png">
   </p>  

### Then the DHCP server has to be started.
`sudo systemctl start isc-dhcp-server`

If the command prompt returns without a message, then it worked.

### Server status

If the start of the ISC DHCP server was aborted with an error, then you should perform a status query to get to the bottom of the error.

`sudo systemctl status isc-dhcp-server`

<p align="center">
     <img src="images/status-DHCP.png">
   </p>
   
In case of error, the messages are unfortunately not always clear. Unless you have made a configuration error. Very popular are typos or missing semicolons (“;”) at the end of a directive.

<p align="right">(<a href="#top">back to top</a>)</p>


<!-- Relay agent configuration -->
# Part-III

### Network architecture RELAY-AGENT

<p align="center">
     <img src="images/RELAY_agent-arch.png">
   </p>

**Setup ISC DHCP RELAY**
### First we install the DHCP relay agent.
`apt install isc-dhcp-relay`

First we have to define for which network interface the ISC DHCP server should work. To do this, we open the following configuration file.

### Configuring ther DHCP Relay
`sudo nano /etc/default/isc-dhcp-relay`

Here we add the ADDRESS of dhcp and interfaces names.
```sh
# What servers should the DHCP relay forward requests to?
SERVERS="10.1.1.1"

# On what interfaces should the DHCP relay (dhrelay) serve DHCP requests?
INTERFACES="eth0 eth1"

# Additional options that are passed to the DHCP relay daemon?
OPTIONS=""
```
### Configuring the interfaces
`sudo nano /etc/network/interfaces`


```sh
auto lo
iface lo inet loopback

allow-hotplug eth1
iface eth1 inet static
        address 10.1.1.2
        gateway 10.1.1.2
        network 10.1.1.0
        netmask 255.255.255.0

allow-hotplug eth0
iface eth0 inet static
        address 192.168.2.1
        gateway 192.168.2.1
        network 192.168.2.0
        netmask 255.255.255.0

```
### Activating packet forwarding
`etc/sysctl.conf`

```sh
net.ipv4.ip_forward=1
```

### firewall_disabled
`sudo nano /etc/selinux/config`

<p align="center">
     <img src="images/firewall_disabled.png">
   </p>
   
### Routing
`sudo nano route -n`

<p align="center">
     <img src="images/relay_routing.png">
   </p>     

### Then the DHCP relay has to be started
`sudo systemctl start isc-dhcp-relay`

If the command prompt returns without a message, then it worked.

### DHCP relay status



<p align="center">
     <img src="images/status-RELAY.png">
   </p>


<p align="right">(<a href="#top">back to top</a>)</p>


<!-- Demonstration -->
# Part-IV



<p align="right">(<a href="#top">back to top</a>)</p>


<p align="right">(<a href="#top">back to top</a>)</p>
