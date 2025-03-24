Switch>en
Switch#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
Switch(config)#no ip domain-lookup
Switch(config)#hostname S1
S1(config)#enable secret class
S1(config)#line console 0
S1(config-line)#password cisco
S1(config-line)#exec-timeout 99 0
S1(config-line)#logging synchronous
S1(config-line)#exit
S1(config)#line vty 0 4
S1(config-line)#password cisco
S1(config-line)#exec-timeout 99 0
S1(config-line)#logging synchronous
S1(config-line)#exit
S1(config)#banner motd #Unauthorized access prohibited! Uhodi#
S1(config)#interface vlan 1
S1(config-if)#
*Mar 24 11:23:36.358: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to down
S1(config-if)#ip address 192.168.1.1 255.255.255.0
S1(config-if)#no shutdown
S1(config-if)#
*Mar 24 11:23:57.782: %LINK-3-UPDOWN: Interface Vlan1, changed state to up
*Mar 24 11:23:58.782: %LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan1, changed state to up
S1(config-if)#exit
S1(config)#exit
S1#
*Mar 24 11:24:07.538: %SYS-5-CONFIG_I: Configured from console by console
S1#copy running-config startup-config
Destination filename [startup-config]?
Building configuration...
Compressed configuration from 978 bytes to 698 bytes[OK]
S1#
