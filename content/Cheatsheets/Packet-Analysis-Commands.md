---
dg-publish: true
---
# Packet Analysis Commands

```bash
Tshark is a command line tool created by the Wireshark team, sharing the same parsing engine as Wireshark, but with the "from command line" advantage — ideal for batch analysis, offline processing, and routine automation of traffic analysis tasks.
tshark -v
tshark -D
tshark -i eth0
tshark -r HTTP_traffic.pcap
tshark -r HTTP_traffic.pcap | wc -l
tshark -r HTTP_traffic.pcap -z io,phs -q
tshark -Y 'http' -r HTTP_traffic.pcap
tshark -r HTTP_traffic.pcap -Y 'ip.src==192.168.252.128 && ip.dst==52.32.74.91'
tshark -r HTTP_traffic.pcap -Y 'http.request.method==GET'
tshark -r HTTP_traffic.pcap -Y "http.request.method==GET" -Tfields -e frame.time -e ip.src -e http.request.full_uri
tshark -r HTTP_traffic.pcap -Y "http contains password"
arpspoof -i eth1 -t 10.100.13.37 -r 10.100.13.36
```
