<h1 align="center">Suricata Study Notes<p>
  
<p align="center">
<img src="https://suricata.io/wp-content/uploads/2021/01/Logo-FINAL_Vertical_Color_Whitetext.png" height=400 >
</p>
  
# Introduction To Suricata
Suricata is a high-performance Network IDS, IPS, and Network Security Monitoring engine.
It is open source and owned (as well as developed) by a community-run non-profit
foundation, the Open Information Security Foundation (OISF).

# Suricata Directory

+ Suricata Logs
  ```css
  /var/log/suricata
  ```
  
  <img src="https://drive.google.com/file/d/1eO4aMVnqS_r5Uc1X8OOVfT5gVVWCTrrE/view?usp=drive_link">
  <br/>
  
+ Suricata rule files
  ```css
  ls -lah /etc/suricata/rules/
  ```
   <img src="/Image/">
  <br/>
+ cssow rule files
  ```css
  more /etc/suricata/rules/emerging-trojan.rules
  ```
+ Variables are defined inside the configuration
  ```css
  more /etc/suricata/suricata.yaml
  ```
+ default-rule-path
  ```css
  $ cat /etc/suricata/suricata.yaml
  default-rule-path: from /etc/suricata/rules
  ```
# Basic Commands

+ Suricata with Offline Input Pcap file
  ```css
  sudo suricata -r file.pcap
  ```
As already mentioned, Suricata outputs various logs inside the /var/log/suricata directory,
by default. You need root-level access to edit or use them.
1. eve.json <- Suricata’s recommended output
2. fast.log
3. stats.log <- Human-readable statistics log

+ Suricata’s (Live) LibPCAP mode
  ```css
  ifconfig <- To identify the network interface Suricata will listen on
  sudo suricata --pcap=eth0 -vv
  ```



# Analyis Json file using jq

+ Search about alerts
  ```css
  cat eve.json| jq -c 'select(.event_type == "alert")'
  ```
+ Protocol Filter (dns)
  ```css
  cat eve.json| jq -c 'select(.event_type == "dns")'
  ```
If you want a prettier representation, don't write ``-c``

# Suricata Rules Syntax

```css
action protocol from_ip port -> to_ip port (msg:"we are under attack by X"; content:"something"; content:"something else"; sid:10000000; rev:1;)
```

## Rule Sections 

1. Header : ( action, protocol, src.ip, src.port, direction of the rule, dst.ip, dst.port  )
   ```css
   alert tcp $EXTERNAL_NET any -> 10.200.0.0/24 80
    ```
2. Rule message & Contents : ( msg, content )
   ```css
   (msg:"Malicious"; content:"POST"; content:"/trigger.php"; ...)
   ```
3. Rule metadata : ( reference, sid, rev )
   ```css
   sid:10000000; rev:5; reference:url,securingtomorrow.mcafee.com/2017-11-20-dridex;
   ```

### Actoins
Valid actions are:
+ alert - generate an alert
+ pass : stop further inspection of the packet
+ drop : drop packet and generate alert
+ reject : send RST/ICMP unreach error to the sender of the matching packet.
+ Log : Log traffic (no alert)

>**Note**
> In IPS mode, using any of the reject actions also enables drop.

<br/>

### Protocol
This keyword in a signature tells Suricata which protocol it concerns. You can choose between four basic protocols:

+ tcp (for tcp-traffic)
+ udp
+ icmp
+ ip (ip stands for ‘all’ or ‘any’)

<br/> There are also a few so-called application layer protocols, or layer 7 protocols you can pick from. These are:


+ http
+ ftp
+ tls (this includes ssl)
+ smb
+ dns
+ dcerpc
+ scss
+ smtp
+ imap
+ modbus (disabled by default)
+ dnp3 (disabled by default)
+ enip (disabled by default)
+ nfs
+ ikev2
+ krb5
+ ntp
+ dhcp
+ rfb
+ rdp
+ snmp
+ tftp
+ sip
+ http2
+ 
> **Note**
> The availability of these protocols depends on whether the protocol is enabled in the configuration file suricata.yaml.
> If you have a signature with for instance a http protocol, Suricata makes sure the signature can only match if it concerns http-traffic.


### Rule Direction
+ Outbound traffic : $HOME_NET any -> $EXTERNAL_NET any
+ Inbound traffic : $EXTENRAL_NET any -> $HOME_NET any
+  Bidirectional traffic : $EXTENRAL_NET any <> $HOME_NET any


# Rules Example

+ Rule 1
  ```css
  alert dns $HOME_NET any -> any any (msg:"TROJAN X Rogue DNS Query Observed" dns_query; content:"default27061330-a.akamaihd.net"; isdataat:!1,relative; reference:url,threatintelprovider.com/trojanx; classtype:trojan-activity; sid:1; rev:1;)
  ```
+ Rule 2
  ```cs
  alert tls $EXTERNAL_NET any -> $HOME_NET any (msg:"TROJAN Z malicious SSL Cert"; flow:established,to_client; tls_cert_subject; content:"CN=uniquestring"; classtype:trojan-activity; sid:1; rev:1;)
  ```

+ Rule 3
  ```css
  alert dns $HOME_NET any -> any any (msg:"TROJAN Activity Detected DNS Query to Known Sofacy Domain 1"; dns_query; content:"drivres-update.info"; nocase; isdataat:!1,relative; sid:1; rev:1;)
  ```

+ Rule 4
  ```css
  alert dns $HOME_NET any -> any any (msg:"TROJAN Activity Detected DNS Query to Known Sofacy Domain 2"; dns_query; content:"softupdates.info"; nocase; isdataat:!1,relative; sid:2; rev:1;)
  ```
+ Rule 5
  ```css
  alert dns $HOME_NET any -> any any (msg:"Possible Citi Phishing Attempt Observed in DNS Query "; dns_query; content:"online.citi.com"; nocase; isdataat:1,relative; sid:3; rev:1;)
  ```

+ Rule 6
  ```css
  alert tls $EXTERNAL_NET any -> $HOME_NET any (msg:"TROJAN Activity Observed Malicious SSL Cert (Qadars CnC)"; flow:established,to_client; tls_cert_subject; content:"CN=susana24.com"; classtype:trojanactivity; sid:4; rev:1;)
  ```





































