![MPLS-IN-SERVICE-PROVIDER](mpls%20in%20service%20provider.jpg)

# mpls-in-service-provider-
# IP-CONFIGURATION

**# R1**

##int f 0/0

##no shutdown

##ip add 192.168.12.1 255.255.255.0

##mpls ip

##exit

##int f 1/0

##no shutdown

##ip add 192.168.16.1 255.255.255.0

##exit

##int loop 0 

#ip add 1.1.1.1 255.255.255.255

#exit


# OSPF

#router ospf 1

 #network 1.1.1.1 0.0.0.0 area 0
 
 #network 192.168.12.0 0.0.0.255 area 0


# MPLS

#ip cef    

#mpls ip

#mpls ldp router-id Loopback0


# BGP

#router bgp 100
 
 #bgp log-neighbor-changes
 
 #network 1.1.1.1 mask 255.255.255.255
 
 #neighbor 4.4.4.4 remote-as 100
 
 #neighbor 4.4.4.4 update-source Loopback0
 
 #neighbor 4.4.4.4 next-hop-self
 
 #neighbor 192.168.16.6 remote-as 600



# R2

#int f 0/0

#no shut

#ip add 192.168.12.2 255.255.255.0

#mpls ip 

#exit

#int f 1/0

#no shut

#ip add 192.168.23.2 255.255.255.0

#mpls ip

#exit

#int loop 0 

#ip add 2.2.2.2 255.255.255.255

#exit


# OSPF

#router ospf 1

 #network 2.2.2.2 0.0.0.0 area 0
 
 #network 192.168.12.0 0.0.0.255 area 0
 
 #network 192.168.23.0 0.0.0.255 area 0


# MPLS

#ip cef

#mpls ip

#mpls ldp router-id Loopback0



# R3

#int f 1/0

#no shut

#ip add 192.168.23.3 255.255.255.0

#mpls ip


#exit

#int f 0/0

#ip add 192.168.34.3 255.255.255.0

#mpls ip

#no shut

#exit

#int loop 0

#ip add 3.3.3.3 255.255.255.255

#exit

# OPSF

#router ospf 1
 
 #network 3.3.3.3 0.0.0.0 area 0
 
 #network 192.168.23.0 0.0.0.255 area 0
 
 #network 192.168.34.0 0.0.0.255 area 0


# MPLS

#ip cef

#mpls ip

#mpls ldp router-id Loopback0




# R4

#int f 0/0

#no shut

#ip add 192.168.34.4 255.255.255.0

#mpls ip


#exit

#int f 1/0

#no shut

#ip add 192.168.45.4 255.255.255.0

#exit

#int loop 0 

#ip add 4.4.4.4 255.255.255.255

#exit

# OSPF

#router ospf 1
 
 #network 4.4.4.4 0.0.0.0 area 0
 
 #network 192.168.34.0 0.0.0.255 area 0


# MPLS

#ip cef

#mpls ip

#mpls ldp router-id Loopback0


# R5

#int f 1/0

#no shut

#ip add 192.168.45.5 255.255.255.0

#exit

#int loop 0

#ip add 5.5.5.5 255.255.255.255

#exit


# BGP

#router bgp 500
 
 #bgp log-neighbor-changes
 
 #network 5.5.5.5 mask 255.255.255.255
 
 #neighbor 192.168.45.4 remote-as 100


# R6

#int f 1/0

#no shut

#ip add 192.168.16.6 255.255.255.0

#exit

#int loop 0

#ip add 6.6.6.6 255.255.255.255

#exit

# BGP

#router bgp 600
 
 #bgp log-neighbor-changes
 
 #network 6.6.6.6 mask 255.255.255.255
 
 #neighbor 192.168.16.1 remote-as 100





