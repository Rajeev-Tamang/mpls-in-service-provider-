# mpls-in-service-provider-
#IP-CONFIGURATION

#R1
#int f 0/0
#no shutdown
#ip add 192.168.12.1 255.255.255.0
#exit

#int f 1/0
#no shutdown
#ip add 192.168.16.1 255.255.255.0
#exit

#int loop 0 
#ip add 1.1.1.1 255.255.255.255
#exit




#R2

#int f 0/0
#no shut
#ip add 192.168.12.2 255.255.255.0
#exit

#int f 1/0
#no shut
#ip add 192.168.23.2 255.255.255.0
#exit
 
#int loop 0 
#ip add 2.2.2.2 255.255.255.255
#exit


#R3
#int f 1/0
#no shut
#ip add 192.168.23.3 255.255.255.0
#exit
#int f 0/0
#ip add 192.168.34.3 255.255.255.0
#no shut
#exit
#int loop 0
#ip add 3.3.3.3 255.255.255.255
#exit

#R4
#int f 0/0
#no shut
#ip add 192.168.34.4 255.255.255.0
#exit
#int f 1/0
#no shut
#ip add 192.168.45.4 255.255.255.0
#exit
int loop 0 
ip add 4.4.4.4 255.255.255.255
exit


R5
 int f 1/0
no shut
ip add 192.168.45.5 255.255.255.0
exit
int loop 0
ip add 5.5.5.5 255.255.255.255
exit

R6
int f 1/0
no shut
ip add 192.168.16.6 255.255.255.0
exit

int loop 0
ip add 6.6.6.6 255.255.255.255
exit

