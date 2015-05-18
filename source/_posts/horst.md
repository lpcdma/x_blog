title: "horst"
date: 2012-06-13 15:11:45
tags:
id: 41
comment: false
---

horst
“horst” is a small, lightweight IEEE802.11 wireless LAN analyzer with a text interface. Its basic function is similar to tcpdump, Wireshark or Kismet, but it’s much smaller and shows different, aggregated information which is not easily available from other tools. It is mainly targeted at debugging wireless LANs with a focus on ad-hoc (IBSS) mode in larger mesh networks. It can be useful to get a quick overview of what’s going on on all wireless LAN channels and to identify problems.
•Shows signal/noise values per station
•Calculates channel utilization (“usage”) by adding up the amount of time the packets actually occupy the medium
•“Spectrum Analyzer” shows signal levels and usage per channel
•Graphical packet history, with signal/noise, packet type and physical rate
•Shows all stations per ESSID and the live TSF per node as it is counting
•Detects IBSS “splits” (same ESSID but different BSSID – this is a common driver problem)
•Statistics of packets/bytes per physical rate and per packet type
•Has some support for mesh protocols (OLSR and batman)
•Can filter specific packet types source addresses or BSSIDs
•Client/server support for monitoring on remote nodes

“horst” is a Linux program and can be used on any wireless LAN monitor interface. The latest git version can also be compiled and used on Mac OSX.

[转载](http://br1.einfach.org/tech/horst/)