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
+ Suricata rule files
  ```sh
  ls -lah /etc/suricata/rules/
  ```
+ Show rule files
  ```sh
  more /etc/suricata/rules/emerging-trojan.rules
  ```
+ Variables are defined inside the configuration
  ```sh
  more /etc/suricata/suricata.yaml
  ```
+  default-rule-path
  ```sh
  $ cat /etc/suricata/suricata.yaml
  default-rule-path: from /etc/suricata/rules
  ```
# Basic Commands

+ Suricata with Offline Input
  ```sh
  sudo suricata -r file.pcap
  ```
