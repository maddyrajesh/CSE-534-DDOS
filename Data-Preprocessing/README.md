# Synthetic Workload

## Combining legitimate traces
cd ~/p4sec/ddosm-p4/datasets/caida/2016\ anonymized

mergecap -a -w legitimate.pcap \\
equinix-chicago.dirA.20160406-130300.UTC.anon.pcap.gz \\
equinix-chicago.dirA.20160406-130400.UTC.anon.pcap.gz \\
equinix-chicago.dirA.20160406-130500.UTC.anon.pcap.gz \\
equinix-chicago.dirA.20160406-130600.UTC.anon.pcap.gz \\
equinix-chicago.dirA.20160406-130700.UTC.anon.pcap.gz \\
equinix-chicago.dirA.20160406-130800.UTC.anon.pcap.gz \\
equinix-chicago.dirA.20160406-130900.UTC.anon.pcap.gz 

madhavrajesh@asu-server:~/p4sec/ddosm-p4/datasets/caida/2016 anonymized$ capinfos -m legitimate.pcap

File name:           legitimate.pcap
File type:           Wireshark/... - pcapng
File encapsulation:  Raw IP
File timestamp precision:  microseconds (6)
Packet size limit:   file hdr: (not set)
Packet size limit:   inferred: 20 bytes - 64 bytes (range)
Number of packets:   221 M
File size:           17 GB
Data size:           157 GB
Capture duration:    419,428234 seconds
First packet time:   2016-04-06 10:03:00,000000
Last packet time:    2016-04-06 10:09:59,428234
Data byte rate:      375 MBps
Data bit rate:       3.004 Mbps
Average packet size: 711,86 bytes
Average packet rate: 527 kpackets/s
SHA256:              7e1b5490d52de6ff1e838c3dc095313b26c8da771ef34360d97cd9ace8c8763b
RIPEMD160:           84325aafdce20dbc46d8e082ff29990343b06d84
SHA1:                b36a48c1116a9aeb7ea0cee8eff0c59f9b853a07
Strict time order:   True
Capture oper-sys:    Linux 4.15.0-66-generic
Capture application: mergecap
Number of interfaces in file: 1
Interface #0 info:
                 Encapsulation = Raw IP (7 - rawip)
                 Capture length = 65536
                 Time precision = microseconds (6)
                 Time ticks per second = 1000000
                 Number of stat entries = 0
                 Number of packets = 221285596

**Moved to ~/p4sec/ddosm-p4/workloads/preprocessed/legitimate.pcap**

## Combining attack traces
cd ~/p4sec/ddosm-p4/datasets/caida/2007\ ddostrace

mergecap -w ddostrace.20070804_141436.pcap \\
from-victim/ddostrace.from-victim.20070804_141436.pcap.gz \\
to-victim/ddostrace.to-victim.20070804_141436.pcap.gz 

madhavrajesh@asu-server:~/p4sec/ddosm-p4/datasets/caida/2007 ddostrace$ capinfos ddostrace.20070804_141436.pcap
File name:           ddostrace.20070804_141436.pcap
File type:           Wireshark/... - pcapng
File encapsulation:  Raw IP
File timestamp precision:  microseconds (6)
Packet size limit:   file hdr: (not set)
Packet size limit:   inferred: 20 bytes - 52 bytes (range)
Number of packets:   26 M
File size:           1.650 MB
Data size:           1.622 MB
Capture duration:    299,999612 seconds
First packet time:   2007-08-04 18:14:36,485318
Last packet time:    2007-08-04 18:19:36,484930
Data byte rate:      5.407 kBps
Data bit rate:       43 Mbps
Average packet size: 60,63 bytes
Average packet rate: 89 kpackets/s
SHA256:              1859afd9bd3f75a0917aed8dcaa284066d3b433a7b99eafe73e2de7592c6ab44
RIPEMD160:           c3caa94c9ef80d8e3954436ff1f20672c17f718b
SHA1:                25d672990405805ab50d3f68e73ae6a8372dce5d
Strict time order:   True
Capture oper-sys:    Linux 4.15.0-66-generic
Capture application: mergecap
Number of interfaces in file: 1
Interface #0 info:
                     Encapsulation = Raw IP (7 - rawip)
                     Capture length = 65536
                     Time precision = microseconds (6)
                     Time ticks per second = 1000000
                     Number of stat entries = 0
                     Number of packets = 26760675

Moved to ~/p4sec/ddosm-p4/workloads/preprocessed/malicious.pcap

#Generating the Synthetic Workload
madhavrajesh@asu-server:~/p4sec/ddosd-cpp/build$ ~/p4sec/ddosd-cpp/bin/trafg -n 131072000 -a 0.2 \
>  ~/p4sec/ddosm-p4/datasets/caida/2016\ anonymized/legitimate.pcap \
>  ~/p4sec/ddosm-p4/datasets/caida/2007\ ddostrace/ddostrace.20070804_141436.pcap \
>  ~/p4sec/ddosm-p4/datasets/synthetic-ilha-ddos20-full/ddos20-full.pcap

1459947961456402
1459948064019593

capinfos -m ~/p4sec/ddosm-p4/datasets/synthetic-ilha-ddos20-full/ddos20-full.pcap
    File name:           /home/ilha/p4sec/ddosm-p4/datasets/synthetic-ilha-ddos20-full/ddos20-full.pcap
    File type:           Wireshark/tcpdump/... - pcap
    File encapsulation:  Ethernet
    File timestamp precision:  microseconds (6)
    Packet size limit:   file hdr: 1500 bytes
    Number of packets:   196 M
    File size:           13 GB
    Data size:           10 GB
    Capture duration:    348,322107 seconds
    First packet time:   2016-04-06 10:03:00,000000
    Last packet time:    2016-04-06 10:08:48,322107
    Data byte rate:      29 MBps
    Data bit rate:       234 Mbps
    Average packet size: 52,00 bytes
    Average packet rate: 564 kpackets/s
    SHA256:              8d328537636f6237f61a7e2e32fc6ea8728cf0d67c58e08ae635ea1da2bb5697
    RIPEMD160:           1f0254e652455d25eb54f223b3c0b033cad2955e
    SHA1:                fd172e442b1c11ae7f68f06ab22ef6a8ab541241
    Strict time order:   True
    Number of interfaces in file: 1
    Interface #0 info:
                         Encapsulation = Ethernet (1 - ether)
                         Capture length = 1500
                         Time precision = microseconds (6)
                         Time ticks per second = 1000000
                         Number of stat entries = 0
                         Number of packets = 196608000
