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
  ```sh
  /var/log/suricata
  ```
  
  <img src="https://drive.google.com/file/d/1eO4aMVnqS_r5Uc1X8OOVfT5gVVWCTrrE/view?usp=drive_link">
  <br/>
  
+ Suricata rule files
  ```sh
  ls -lah /etc/suricata/rules/
  ```
   <img src="/Image/">
  <br/>
+ Show rule files
  ```sh
  more /etc/suricata/rules/emerging-trojan.rules
  ```
+ Variables are defined inside the configuration
  ```sh
  more /etc/suricata/suricata.yaml
  ```
+ default-rule-path
  ```sh
  $ cat /etc/suricata/suricata.yaml
  default-rule-path: from /etc/suricata/rules
  ```
# Basic Commands

+ Suricata with Offline Input Pcap file
  ```sh
  sudo suricata -r file.pcap
  ```
As already mentioned, Suricata outputs various logs inside the /var/log/suricata directory,
by default. You need root-level access to edit or use them.
1. eve.json <- Suricata’s recommended output
2. fast.log
3. stats.log <- Human-readable statistics log

+ Suricata’s (Live) LibPCAP mode
  ```sh
  ifconfig <- To identify the network interface Suricata will listen on
  sudo suricata --pcap=eth0 -vv
  ```



# Analyis Json file using jq

+ Search about alerts
  ```sh
  cat eve.json| jq -c 'select(.event_type == "alert")'
  ```
+ Protocol Filter (dns)
  ```sh
  cat eve.json| jq -c 'select(.event_type == "dns")'
  ```






















