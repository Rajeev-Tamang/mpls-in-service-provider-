# MPLS VPLS POINT TO MULTIPOINT

# IP,IGP,MPLS IN CORE

# BRIDGE-DOMAIN IS LOCALLY SIGNIFICANT

# VPN ID ID GLOBALLY SIGNIFICANT

#R4
>int f 3/0
>
>no shut
>
>int f 3/0.1
>
>encapsulation dot1Q 100
>
>ip add 192.168.100.4 255.255.255.0

#R5
>int f 3/0
>
>no shut
>
>int f 3/0.1
>
>encapsulation dot1Q 100
>
>ip add 192.168.100.5 255.255.255.0


#R6
>int f 3/0
>
>no shut
>
>int f 3/0.1
>
>encapsulation dot1Q 100
>
>ip add 192.168.100.6 255.255.255.0


#R1
>int f 3/0
>
>no shut
>
>service instance 5 ethernet
>
>encapsulation dot1Q 100
>
>bridge-domain 123
>
>l2 vfi CUST-A manual
>
>vpn id 111
>
>bridge-domain 123
>
>neighbor 2.2.2.2 encapsulation mpls
>
>neighbor 3.3.3.3 encapsulation mpls



#R2
>int f 3/0
>
>no shut
>
>service instance 5 ethernet
>
>encapsulation dot1Q 100
>
>bridge-domain 123
>
>l2 vfi CUST-A manual
>
>vpn id 111
>
>bridge-domain 123
>
>neighbor 1.1.1.1 encapsulation mpls
>
>neighbor 3.3.3.3 encapsulation mpls


#R3
>int f 3/0
>
>no shut
>
>service instance 5 ethernet
>
>encapsulation dot1Q 100
>
>bridge-domain 123
>
>l2 vfi CUST-A manual
>
>vpn id 111
>
>bridge-domain 123
>
>neighbor 2.2.2.2 encapsulation mpls
>
>neighbor 1.1.1.1 encapsulation mpls
