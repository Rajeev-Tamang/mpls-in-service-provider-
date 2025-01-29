
![MPLS L2VPN VPWS](MPLS%20L2VPN%20VPWS.jpg)

# MPLS-L2VPN-VPWS

#R1

>ip cef
>
>mpls ldp router-id loop 0
>
>mpls ip
>
> int f 0/0
>
> ip add 192.168.12.1 255.255.255.0
>
>mpls ip
>
> no shut
>
> exit
>
> int loop 0
>
> ip add 1.1.1.1 255.255.255.255
>
> exit
>
> router ospf 1
>
> network 192.168.12.0 0.0.0.255 area 0
> 
> network 1.1.1.1 0.0.0.0 area 0
>
>exit




#R2

>ip cef
>
>mpls ldp router-id loop 0
>
>mpls ip
>
> int f 0/0
>
> ip add 192.168.12.2 255.255.255.0
>
>mpls ip
>
> no shut
>
>
>int f 1/0
>
>>no shut
>
>ip add 192.168.23.2 255.255.255.0
>
>mpls ip
>
>
> int loop 0
>
> ip add 2.2.2.2 255.255.255.255
>
> router ospf 1
>
> network 192.168.12.0 0.0.0.255 area 0
>
> network 192.168.23.0 0.0.0.255 area 0
>
> network 2.2.2.2 0.0.0.0 area 0
>
>exit

#R3

>ip cef
>
>mpls ldp router-id loop 0
>
>mpls ip
>
> int f 1/0
>
> ip add 192.168.23.3 255.255.255.0
>
>mpls ip
>
> no shut
>
> exit
>
> int loop 0
>
> ip add 3.3.3.3 255.255.255.255
>
> exit
>
> router ospf 1
>
> network 192.168.23.0 0.0.0.255 area 0
> 
> network 3.3.3.3 0.0.0.0 area 0
>
>exit

! ATOM-PHYSICAL-INTERFACES

#R4
>int f 1/0
>
>ip add 192.168.45.4 255.255.255.0
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
>router eigrp 100
>
>no auto
>
>netwokr 192.168.45.0
>
>network 4.0.0.0

#R5
>int f 0/0
>
>ip add 192.168.45.5 255.255.255.0
>
>no shut
>
>exit
>
>int loop 0
>
>ip add 5.5.5.5 255.255.255.255
>
>exit
>
>router eigrp 100
>
>no auto
>
>netwokr 192.168.45.0
>
>network 5.0.0.0
>

#R1

>int f 1/0
>
>no shut
>
>xconnect 3.3.3.3 100 encapsulation mpls
>

#R3

#R1

>int f 0/0
>
>no shut
>
>xconnect 1.1.1.1 100 encapsulation mpls
>
!



! ATOM IN SUB INTEFACES !

#R4
> interface fastEthernet 1/0
> 
> no shut
>
> int fa 1/0.1
> 
> encapsulation dot1Q 100
> 
> ip address 192.168.45.4 255.255.255.0
> 
> exit


#R5
> interface fastEthernet 0/0
> 
> no shut
>
> int fa 0/0.1
> 
> encapsulation dot1Q 100
> 
> ip address 192.168.45.5 255.255.255.0
> 
> exit

#R1

>int fa 1/0
>
>no shut
>
>int fa 1/0.1
>
> encapsulation dot1Q 100
>
>xconnect 3.3.3.3 111 encapsulation mpls


#R3

>int fa 0/0
>
>no shut
>
>int fa 0/0.1
>
> encapsulation dot1Q 100
>
>xconnect 1.1.1.1 111 encapsulation mpls


!ATOM-INTERNETWORK [ETHERNET AND SERIAL]

#R3

> pseudowire-class XYZ
>
> encapsulaiton mpls
>
> interworking ip
>
> int f 0/0
>
> xconnect 1.1.1.1 110 pw-class XYZ
>
> exit

#R1

> pseudowire-class XYZ
>
> encapsulaiton mpls
>
> interworking ip
>
> int f 1/0
>
> xconnect 3.3.3.3 110 pw-class XYZ
>
> exit

! WHEN CORE DOES NOT HAVE MPLS , WE USE L2TPV3 

#R1
>pseudowire-class ABC
>
>encapsulation l2tpv3
>
>ip local interface loopback 0

>int f 1/0
>
>no shut
>
>int f 1/0.1
>
>encapsulation dot1q 100
>
>xconnect 3.3.3.3 encapsualtion l2tpv3 pw-class ABC


#R3
>pseudowire-class ABC
>
>encapsulation l2tpv3
>
>ip local interface loopback 0

>int f 0/0
>
>no shut
>
>int f 0/0.1
>
>encapsulation dot1q 100
>
>xconnect 1.1.1.1 encapsualtion l2tpv3 pw-class ABC

