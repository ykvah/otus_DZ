








R28 

настройка ip  sla

ip sla 1
icmp-echo 10.10.25.3 source-interface Ethernet0/3
frequency 5
ip sla schedule 1 life forever start-time now
track 1 ip sla 1 reachability
ip sla 2
icmp-echo 10.10.26.1 source-interface Ethernet0/0
frequency 5
ip sla schedule 2 life forever start-time now
track 2 ip sla 2 reachability

Policy-Based Routing ---
! Например: часть трафика через R25, часть через R26
access-list 101 permit ip host 10.0.0.25 any
access-list 102 permit ip host 10.0.0.26 any

route-map PBR-TO-R25 permit 10
 match ip address 101
 set ip next-hop verify-availability 10.10.25.3 track 1
 set ip next-hop 10.10.26.1

route-map PBR-TO-R26 permit 10
 match ip address 102
 set ip next-hop verify-availability 10.10.26.1 track 2
 set ip next-hop 10.10.25.3

! Применяем PBR к исходящему интерфейсу, где идёт трафик "из офиса"
interface Ethernet0/0
 ip policy route-map PBR-TO-R25
interface Ethernet0/3
 ip policy route-map PBR-TO-R26
