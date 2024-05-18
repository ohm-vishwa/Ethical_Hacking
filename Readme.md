# Ethical Hacking Cheat Sheet
<details>
    <summary> Basic Information</summary>
These commands are works only on linux based OS system
An external Network Adapter is required, Your Network Adapter must support
{ Monitor Mode, packet injection } mode Here my network interface name is { wlan1 }, use these commands 

```sh
iwconfig 
```
 or 
 ```
 ifconfig
 ```
check your network interface name, your network interface name may be diffrent use accordingly.

First you need to switch root user

```
sudo su
```
</details>

# Table of Content
- [Network Manager Commands](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#network-manager-commands)
- [Network Adapter testing commands](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#network-adapter-testing-commands)
- [Changing MAC Address](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#changing-mac-address)
- [Network Hacking](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#network-hacking)
    * [1. Pre-Connection Attack](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#1-pre-connection-attack-1)
        * Packet Sniffing
        * De-Authentication Attack
    * [2. Ganing Access](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#2-gaining-access)
        * WEP Cracking
    * [3. Post-Connection Attack]()




---
## Network Manager Commands
 Kill Network Manager
```
airmon-ng check kill
```
 Enable Network Manager
```
service NetworkManager start
```
 Restart Network Manager
```
systemctl restart NetworkManager
```
 Check Status of Network Manager
```
systemctl status NetworkManager
```
---
## Network Adapter Testing Commands
 Enable Monitor Mode
<details>
    <summary>Why we need to enable monitor mode ?</summary>
we want is to be able to capture all the packets
that are within our range,
even if they are sent to the router
and even if they are sent to another device.
So to do this, we need to set our network adapter to the monitor mode
</details>

```
airmon-ng start wlan1
```
 Disable Monitor Mode
```
airmon-ng stop wlan1
```
 Packet Injection test
```
aireplay-ng --test wlan1
```
 Access Point Creation Test
you can put any fake MAC Address like this `00:01:02:03:04:05`
```
airbase-ng -a 00:01:02:03:04:05 --essid "test_01" -c 11 wlan1
```
---
# Network Hacking

### [1. Pre-Connection Attack](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#1-pre-connection-attack-1)
### [2. Ganing Access](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file#2-gaining-access)
### [3. Post-Connection Attack]()
<details>
    <summary>What is MAC Address ?</summary>
MAC address stands for Media Access Control,
it's a permanent, physical, and unique address
assigned to network interfaces
by the device manufacturer.
So whether you have a wireless card,
or a wired, or Ethernet card,
each one of these network cards
come with a specific address that is unique to this card.
So there is no two devices in the world
that would have the same MAC address.
And this address will always be the same
to this specific device,
even if you unplug it from your computer,
connect it to another computer,
then this network device will always have the same address.
So you might already know that the IP address
is used in the internet to identify computers,
and communicate between devices on the internet.
The MAC address is used within the network
to identify devices and transfer data between devices.
So each piece of data or packet
that is sent within the network
contains a source MAC and a destination MAC.
Therefore, this packet would flow
from the source MAC to the destination MAC.
So because this is a physical unique address
to each interface, to each network device,
and because it is used to identify devices,
then changing it will make you anonymous on the network.
Not only that, but the MAC address is often used
by filters to prevent or allow devices
to connect to networks,
and do specific tasks on the network.
So being able to change your MAC address
to another device's MAC address
will allow you to impersonate this device
and allow you to do things
that you might not be able to do.
So you'd be able to bypass filters,
or connect to networks that only specific devices
with specific MAC addresses can connect to,
and you will also be able to hide your identity.
</details>

### Changing MAC Address
1. step
```
ifconfig
```
2. step
```
ifconfig wlan1 down
```
3. step ( you can assign any MAC Address, just make sure your address starts with 00 ) 
```
ifconfig wlan1 ether 00:11:22:33:44:55
```
4. step
```
ifconfig wlan1 up
```

the MAC address will revert back
to the original one once you restart the computer,
because we're only changing the MAC address in memory,
we're not really changing the physical MAC address.

```
ifconfig wlan1 down
ifconfig wlan1 ether 00:11:22:33:44:55
ifconfig wlan1 up
```
---
# 1. Pre-Connection Attack
<details>
    <summary>What is Pre-Connection Attack ?</summary>
A pre-connection attack is like a sneak attack on a computer or network before any proper connection is established. It's when a hacker tries to break into a system without actually logging in or getting permission. They might do this by scanning for vulnerabilities or trying to guess passwords. It's a bit like trying to break into a house before you even knock on the door.
</details>

### Packet Sniffing
<details>
    <summary>What is Packet Sniffing ?</summary>
Now that we have enabled monitor mode
on our wireless interface,
we are able to capture all the wifi packets
sent within our range,
even if the packet is not directed to our computer,
even if we're not connected to the target network,
and even without knowing the key
or the password to the target network.
So all we need right now is a program
that can capture these packets for us.
The program that we're going to use is called { airodump-ng }.
It's part of the a { aircrack-ng } suit,
and it's a packet-sniffer,
so it's basically a program designed
to capture packets while you're in monitor mode.
So it will allow us to see
all the wireless networks around us,
and show us detailed information about it's MAC address,
it's channel, it's encryption,
the clients connected to this network, and so on.
</details>

 Sniff Network around `2.4 GHz`
```
airodump-ng wlan1
```
 Sniff Network Both `2.4 GHz and 5 GHz`
```
airodump-ng --band a wlan1
```
 Capture data both `2.4 GHz and 5 GHz`
```
airodump-ng --band abg wlan1
```
 Capture data in a file `test`
```
airodump-ng --bssid @1 --channel @2 --write test wlan1
```
@1 → `BSSID` of target

@2 → Channel no. of target

 Open wireshark application to read captured data, captured in `test.cap`, 
```
wireshark
```
### DE-Authentication Attack 
<details>
    <summary>What is de-authentication attack ?</summary>
This attack allow us to disconnect any device,
from any network, before connecting to any of these networks
and without the need to know the password for the network.
To do this, we're going to pretend to be the client
that we want to disconnect, by changing our MAC address
to the MAC address of that client, and tell the router
that I want to disconnect from you.
Then, we're going to pretend to be the router, again,
by changing our MAC address to the router's MAC address
and tell the client that you're requested
to be disconnected, so I'm going to disconnect you.
This will allow us to successfully disconnect,
or de-authenticate any client from any network.
Now, we're actually not going to do this manually,
we're gonna use a tool called { aireplay-ng }to do that.
</details>

```
aireplay-ng --deauth @1 -a @2 -c @3 wlan1
```
@1 → no. of packets you want to send 

@2 → BSSID of router

@3 → MAC Address of the Client



if it`s fails then, target router on specfic channel
```
airodump-ng --bssid @1 --channel @2 wlan1
```
@1 → BSSID of target

@2 → Channel number

---

# 2. Gaining Access
<details>
    <summary>What is Gaining Acess ?</summary>
Once`s we connect to the network, we can do so many cool things. we will able to gather so much more info, we will be able to intercept the connection and see every things that the people sends whether it user name, passward, url and anything.
And we will be able to modify data. 
&nbsp;

if your target does not use encryption then you just connect to it. if your target is wired network  then just use cable and connect to it.
&nbsp;

The only problem is if your target using encryption.

</details>

### WEP Cracking
<details>
    <summary>Know about WEP ?</summary>
● Wired Equivalent Privacy. _
● Old encryption.
● Uses an algorithm called RC4.
● Still used in some networks. 
● Can be cracked easily.  

● Client encrypts data using a key.
● Encrypted packet sent in the air.
● Router decrypts packet using the key.

● Each packet is encrypted using a unique key stream.
● Random initialization vector (IV) is used to generate the keys streams.
● The initialization vector is only 24 bits!
● IV + Key (password) = Key stream.

● IV is too small (only 24 bits).
● IV is sent in plain text.

Result:
● IV’s will repeat on busy networks.
● This makes WEP vulnerable to statistical attacks.
● Repeated IVs can be used to determine the key stream;
● And break the encryption

Conclusion:
To crack WEP we need to:
1. Capture a large number of packets/IVs. → using airodump-ng
2. Analyse the captured IVs and crack the key. → using aircrack-ng

Problem:
● If network is not busy.
● It would take some time to capture enough IVs.

Solution:
→ Force the AP to generate new IVs.

Fake Authentication
Problem:
● APs only communicate with connected clients.
→ We can’t communicate with it.
→ We can’t even start the attack.
Solution:
→ Associate with the AP before launching the attack.

ARP Request Replay
● Wait for an ARP packet.
● Capture it, and replay it (retransmit it).
● This causes the AP to produce another packet with a new IV.
● Keep doing this till we have enough IVs to crack the key.
</details>


<details>
    <summary></summary>
    
</details>



























# 3. Post-Connection Attack









<details>
    <summary></summary>

</details>

# ===}> [Keep Supporting me on YouTube](https://www.youtube.com/@ohm_vishwa)