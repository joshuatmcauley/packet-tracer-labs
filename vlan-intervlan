Lab 01 - VLANs, inter-VLAN routing, DHCP, ACL, forced failure

Status: done
Program: Packet Tracer


Topology:
PC0 to switch Fa0/1 (VLAN 10)
PC1 to switch Fa0/2 (VLAN 20)
PC1 to switch Fa0/2 (VLAN 20)
Router Gi0/0 to switch Gi0/1 (trunk)

Addressing
- VLAN 10 Staff: 192.168.10.0/24, gateway 192.168.10.1, DHCP pool STAFF, exclude .1, DNS 8.8.8.8, PC0
- VLAN 20 IoT: 192.168.20.0/24, gateway 192.168.20.1, DHCP pool IOT, exclude .1, DNS 8.8.8.8, PC1
- VLAN 99: not used

What I built:
- Two user VLANs
- Router-on-a-stick (Gi0/0.10 and Gi0/0.20)
- DHCP per VLAN on the router
- DNS 8.8.8.8 in DHCP (does not need to resolve in PT)
- ACL BLOCK20TO10 inbound on Gi0/0.20: deny 20.0/24 to 10.0/24, permit any
- SSH: not done

Forced failure
What I broke: switchport Fa0/2 set to VLAN 10 instead of 20. Then ipconfig /renew on PC1.
What the user saw: IoT PC got a 192.168.10.x address (or kept 20.x and could not reach 192.168.20.1).
Ladder step that caught it: Physical/port (wrong VLAN) after IP looked “fine” or gateway 20.1 failed.
Fix: Fa0/2 back to VLAN 20, release/renew. PC1 is 192.168.20.x again.

Normal tests (after fix)
- PC0 ping 192.168.10.1: success
- PC1 ping 192.168.20.1: success
- PC1 ping 192.168.10.1: fail (ACL)

PC0 got:
PC1 got (correct VLAN):
PC1 got (wrong VLAN):

Config
Switch: Fa0/1 VLAN 10 access, Fa0/2 VLAN 20 access, Gi0/1 trunk
Router: Gi0/0.10 192.168.10.1, Gi0/0.20 192.168.20.1, DHCP pools STAFF and IOT
ACL: deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255 then permit ip any any
