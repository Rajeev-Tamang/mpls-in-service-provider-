# INTER-AS-MPLS-VPN-OPTION-A-B-AND-C

# Option A (treating each SP as a CE)
#R4
>int f 1/0
>
> vrf forwarding CLIENT-A
>
> ip add 192.168.48.4 255.255.255.0
>
> no shut

> router bgp 100
>
> address-family ipv4 vrf CLIENT-A
> 
> neighbor 192.168.48.8 remote-as 200
>
> exit
>
> exit


#R8
>int f 1/0
>
> vrf forwarding CLIENT-A
>
> ip add 192.168.48.8 255.255.255.0
>
> no shut

> router bgp 200
>
> address-family ipv4 vrf CLIENT-A
> 
> neighbor 192.168.48.4 remote-as 100
>
> exit


# Option B (ASBRs forming MP-BGP peering with huge vpnv4 tables)
#R4
>router bgp 100
>
>neighbor 192.168.48.8 remote-as 200
>
>neighbor 3.3.3.3 next-hop-self
>
>address-family vpnv4
>
>neighbor 192.168.48.8 activate
>
>exit
>
>exit
>
>vrf definition CLIENT-A
>
>address-family ipv4
>
>route-target import 200:2 
>
>exit
>
>exit

#R1
>vrf definition CLIENT-A
>
>address-family ipv4
>
>route-target import 200:2 
>
>exit
>
>exit

#R2
>vrf definition CLIENT-A
>
>address-family ipv4
>
>route-target import 200:2 
>
>exit
>
>exit



#R8
>router bgp 200
>
>neighbor 192.168.48.4 remote-as 100
>
>neighbor 7.7.7.7 next-hop-self
>
>address-family vpnv4
>
>neighbor 192.168.48.4 activate
>
>exit
>
>exit
>
>vrf definition CLIENT-A
>
>address-family ipv4
>
>route-target import 100:1
>
>exit
>
>exit

#R5
>vrf definition CLIENT-A
>
>address-family ipv4
>
>route-target import 100:1
>
>exit
>
>exit

#R6
>vrf definition CLIENT-A
>
>address-family ipv4
>
>route-target import 100:1
>
>exit
>
>exit
>


# Option C (RRs forming MP-BGP peering separating Control Plane from Data Plane)**

#R4
>access-list 1 permit 3.3.3.3 0.0.0.0
>
>access-list 1 permit 1.1.1.1 0.0.0.0
>
>access-list 1 permit 2.2.2.2 0.0.0.0
>
>access-list 1 permit 4.4.4.4 0.0.0.0


>route-map O2B
>
>match ip address 1
>
>exit
>
>access-list 2 permit 7.7.7.7 0.0.0.0
>
>access-list 2 permit 8.8.8.8 0.0.0.0
>
>access-list 2 permit 6.6.6.6 0.0.0.0
>
>access-list 2 permit 5.5.5.5 0.0.0.0
>
>route-map B2O
>
>match ip address 2
>
>exit
>
>router ospf 1
>
>redistribute bgp 100 route-map B2O subnets
>
>exit
>
>router bgp 100
>
>neighbor 192.168.48.8 remote-as 200
>
>neighbor 192.168.48.8 send-label
>
>redistibute ospf 1 route-map O2B
>


#R8
>access-list 1 permit 7.7.7.7 0.0.0.0
>
>access-list 1 permit 6.6.6.6 0.0.0.0
>
>access-list 1 permit 5.5.5.5 0.0.0.0
>
>access-list 1 permit 8.8.8.8 0.0.0.0
>
>route-map R2B
>
>match ip address 1
>
>exit
>
>access-list 2 permit 3.3.3.3 0.0.0.0
>
>access-list 2 permit 1.1.1.1 0.0.0.0
>
>access-list 2 permit 2.2.2.2 0.0.0.0
>
>access-list 2 permit 4.4.4.4 0.0.0.0
>
>route-map B2R
>
>match ip address 2
>
>exit
>
>router rip
>
>redistribute bgp 100 route-map B2R 
>
>exit
>
>router bgp 200
>
>neighbor 192.168.48.4 remote-as 100
>
>>neighbor 192.168.48.4 send-label

>redistibute rip route-map R2B
>

#R3
>ip prefic-list DENY deny 0.0.0.0/0 le 32
>
>router bgp 100
>
>neighbor 7.7.7.7 remote-as 200
>
>neighbor 7.7.7.7 update-source loopback 0
>
>neighbor 7.7.7.7 ebgp-multihop
>
>neighbor 7.7.7.7 prefix-list deny out
>
>address-family vpnv4
>
>neighbor 7.7.7.7 activate
>
>neighbor 7.7.7.7 next-hop-unchange

#R7
>ip prefic-list DENY deny 0.0.0.0/0 le 32
>
>router bgp 200
>
>neighbor 3.3.3.3 remote-as 100
>
>neighbor 3.3.3.3update-source loopback 0
>
>neighbor 3.3.3.3 ebgp-multihop
>
>neighbor 3.3.3.3 prefix-list deny out
>
>address-family vpnv4
>
>neighbor 3.3.3.3 activate
>
>>neighbor 3.3.3.3 next-hop-unchange




# BASIC CONFIG IP , MPLS , IGP 
#R1
>vrf definition CLIENT-A
>
>rd 100:1
>
>address-family ipv4
>
>route-target both 100:1
>
>exit
>
>int f 0/0
>
>ip add 192.168.13.1 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 3/0
>
>ip add 192.168.100.1 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 1/0
>
>vrf forwarding CLINET-A
>
>ip add 192.168.19.1 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 1.1.1.1 255.255.255.255
>
>no shut
>
>exit
>
> router ospf 1
>
> network 192.168.13.0 0.0.0.255 area 0
>
> network 192.168.100.0 0.0.0.255 area 0
>
> network 1.1.1.1 0.0.0.0 area 0
>
> exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 100
>
>neighbor 3.3.3.3 remote-as 100
>
>neighbor 3.3.3.3 update-source loopback 0
>
>network 1.1.1.1 mask 255.255.255.255
>
>address-family  vpnv4
>
>neighbor 3.3.3.3 activate
>
>address-family ipv4 vrf CLIENT-A
>
>neighbor 192.168.19.9 remote-as 65001
>
>exit
>

  

#R2
>vrf definition CLIENT-A
>
>rd 100:1
>
>address-family ipv4
>
>route-target both 100:1
>
>exit
>
>int f 0/0
>
>ip add 192.168.100.2 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 1/0
>
>ip add 192.168.23.2 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 3/0
>
>vrf forwarding CLIENT-A
>
>ip add 192.168.20.2 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 2.2.2.2 255.255.255.255
>
>exit
>
>  router ospf 1
>
> network 192.168.23.0 0.0.0.255 area 0
>
> network 192.168.100.0 0.0.0.255 area 0
>
> network 2.2.2.2 0.0.0.0 area 0
>
> exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 100
>
>neighbor 3.3.3.3 remote-as 100
>
>neighbor 3.3.3.3 update-source loopback 0
>
>network 2.2.2.2 mask 255.255.255.255
>
>address-family  vpnv4
>
>neighbor 3.3.3.3 activate
>
>address-family ipv4 vrf CLIENT-A
>
>neighbor 192.168.20.10 remote-as 65002
>
>exit
>



#R3
>int f 0/0
>
>ip add 192.168.13.3 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int fa 1/0
>
>ip add 192.168.23.3 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 2/0
>
>ip add 192.168.34.3 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 3.3.3.3 255.255.255.255
>
>no shut
>
>exit
>
> router ospf 1
>
> network 192.168.23.0 0.0.0.255 area 0
>
> network 192.168.13.0 0.0.0.255 area 0
>
> network 192.168.34.0 0.0.0.255 area 0
>  
> network 3.3.3.3 0.0.0.0 area 0
>
> exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 100
>
>bgp log-neighbor-changes
>
>network 3.3.3.3 mask 255.255.255.255
>
>neighbor PG peer-group
>
>neighbor PG remote-as 100
>
>neighbor PG update-source Loopback0
>
>neighbor 1.1.1.1 peer-group PG
>
>neighbor 2.2.2.2 peer-group PG
>
>neighbor 4.4.4.4 peer-group PG
>
>address-family  vpnv4
>
>neighbor 1.1.1.1 activate
>
>neighbor 2.2.2.2 activate
>
>neighbor 4.4.4.4 activate
>
>neighbor PG route-reflector-client





#R4
>vrf definition CLIENT-A
>
>rd 100:1
>
>address-family ipv4
>
>route-target both 100:1
>
>exit
>
>int f 2/0
>
>ip add 192.168.34.4 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 0/0
>
>ip add 192.168.100.4 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 3/0
>
>vrf forwarding CLIENT-A
>
>ip add 192.168.43.4 255.255.255.0
>
>no shut
>
>exit
>
>int fa 1/0
>
>ip add 192.168.48.4 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 4.4.4.4 255.255.255.255
>
>exit
>
>
>router ospf 1
>
> network 192.168.34.0 0.0.0.255 area 0
>
> network 192.168.100.0 0.0.0.255 area 0
>
> network 4.4.4.4 0.0.0.0 area 0
>
> exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 100
>
>neighbor 3.3.3.3 remote-as 100
>
>neighbor 3.3.3.3 update-source loopback 0
>
>network 4.4.4.4 mask 255.255.255.255
>
>address-family  vpnv4
>
>neighbor 3.3.3.3 activate
>
>>address-family ipv4 vrf CLIENT-A
>
>neighbor 192.168.43.13 remote-as 65003
>
>exit
>




#R5
>vrf definition CLIENT-A
>
>rd 200:2
>
>address-family ipv4
>
>route-target both 200:2
>
>exit
>
>int f 3/0
>
>vrf forwarding CLIENT-A
>
>ip add 192.168.51.5 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 5.5.5.5 255.255.255.255
>
>no shut
>
>int f 0/0
>
>ip add 192.168.57.5 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 1/0
>
>no shut
>
>ip add 192.168.200.5 255.255.255.0
>
>mpls ip
>
>exit
>
>router rip
>
>version 2
>
>no auto
>
>network 192.168.57.0
>
>network 192.168.200.0
>
>network 5.0.0.0
>
>exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 200
>
>neighbor 7.7.7.7 remote-as 200
>
>neighbor 7.7.7.7 update-source loopback 0
>
>network 5.5.5.5 mask 255.255.255.255
>
>address-family ipv4 vrf CLIENT-A
>
>neighbor 192.168.51.11 remote-as 65004
>
>address-family vpnv4
>
>neighbor 7.7.7.7 activate
>
>exit


#R6
>vrf definition CLIENT-A
>
>rd 200:2
>
>address-family ipv4
>
>route-target both 200:2
>
>exit
>
>int f 3/0
>
>vrf forwarding CLIENT-A
>
>ip add 192.168.62.6 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 6.6.6.6 255.255.255.255
>
>no shut
>
>int f 0/0
>
>ip add 192.168.200.6 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 1/0
>
>no shut
>
>ip add 192.168.67.6 255.255.255.0
>
>mpls ip
>
>exit
>
>router rip
>
>version 2
>
>no auto
>
>network 192.168.67.0
>
>network 192.168.200.0
>
>network 6.0.0.0
>
>exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 200
>
>neighbor 7.7.7.7 remote-as 200
>
>neighbor 7.7.7.7 update-source loopback 0
>
>network 6.6.6.6 mask 255.255.255.255
>
>address-family ipv4 vrf CLIENT-A
>
>neighbor 192.168.62.12 remote-as 65505
>
>>ddress-family vpnv4
>
>neighbor 7.7.7.7 activate
>
>exit 


#R7
>int f 0/0
>
>ip add 192.168.57.7 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 7.7.7.7 255.255.255.255
>
>no shut
>
>int f 1/0
>
>ip add 192.168.67.7 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 2/0
>
>no shut
>
>ip add 192.168.78.7 255.255.255.0
>
>mpls ip
>
>exit
>
>router rip
>
>version 2
>
>no auto
>
>network 192.168.57.0
>
>network 192.168.67.0
>
>network 192.168.78.0
>
>network 7.0.0.0
>
>exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 200
>
>bgp log-neighbor-changes
>
>network 7.7.7.7 mask 255.255.255.255
>
>neighbor PG peer-group
>
>neighbor PG remote-as 200
>
>neighbor PG update-source Loopback0
>
>>
>neighbor 5.5.5.5 peer-group PG
>
>neighbor 6.6.6.6 peer-group PG
>
>neighbor 8.8.8.8 peer-group PG
>
>address-family  vpnv4
>
>neighbor 5.5.5.5 activate
>
>neighbor 6.6.6.6 activate
>
>neighbor 8.8.8.8 activate
>
>neighbor PG route-reflector-client


#R8
>vrf definition CLIENT-A
>
>rd 200:2
>
>address-family ipv4
>
>route-target both 200:2
>
>exit
>
>int f 1/0
>
>ip add 192.168.48.8 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 8.8.8.8 255.255.255.255
>
>no shut
>
>int f 2/0
>
>ip add 192.168.78.8 255.255.255.0
>
>mpls ip
>
>no shut
>
>exit
>
>int f 0/0
>
>no shut
>
>ip add 192.168.200.8 255.255.255.0
>
>mpls ip
>
>exit
>
>int fa 3/0
>
>no shut
>
>vrf forwarding CLIENT-A
>
>ip add 192.168.84.8 255.255.255.0
>
>exit
>
>router rip
>
>version 2
>
>no auto
>
>network 192.168.78.0
>
>network 192.168.200.0
>
>network 8.0.0.0
>
>exit
>
>ip cef
>
>mpls ldp router-id loopback 0
>
>mpls ip
>
>router bgp 200
>
>neighbor 7.7.7.7 remote-as 200
>
>neighbor 7.7.7.7 update-source loopback 0
>
>network 8.8.8.8 mask 255.255.255.255
>
>address-family ipv4 vrf CLIENT-A
>
>neighbor 192.168.84.14 remote-as 65006
>
>address-family vpnv4
>
>neighbor 7.7.7.7 activate
>
>exit
>
>


#R9
>int f 1/0
>
>ip add 192.168.19.9 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 9.9.9.9 255.255.255.255
>
>no shut
>
>exit
>
>router bgp 65001
>
>network 9.9.9.9 mask 255.255.255.255
>
>neighbor 192.168.19.1 remote-as 100
>
>exit
>
>

#R10
>int f 3/0
>
>ip add 192.168.20.10 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 10.10.10.10 255.255.255.255
>
>no shut
>
>router bgp 65002
>
>network 10.10.10.10 mask 255.255.255.255
>
>neighbor 192.168.20.2 remote-as 100
>
>exit

#R11
>int f 3/0
>
>ip add 192.168.51.11 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 11.11.11.11 255.255.255.255
>
>no shut
>
>router bgp 65004
>
>neighbor 192.168.51.5 remote-as 100
>
>network 11.11.11.11 mask 255.255.255.255
>
>exit
>


#R12
>int f 3/0
>
>ip add 192.168.62.12 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 12.12.12.12 255.255.255.255
>
>no shut
>
>router bgp 65005
>
>neighbor 192.168.62.6 remote-as 100
>
>network 12.12.12.12 mask 255.255.255.255
>
>exit


#R13
>int f 3/0
>
>ip add 192.168.43.13 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 13.13.13.13 255.255.255.255
>
>no shut
>
>router bgp 65003
>
>network 13.13.13.13 mask 255.255.255.255
>
>neighbor 192.168.43.4 remote-as 100
>
>exit

#R14
>int f 3/0
>
>ip add 192.168.84.14 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 14.14.14.14 255.255.255.255
>
>no shut
>
>>router bgp 65006
>
>neighbor 192.168.84.8 remote-as 100
>
>network 14.14.14.14 mask 255.255.255.255
>
>exit
