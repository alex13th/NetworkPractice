## Практическое задание 7

### Задание

1. В AS 700 сделать редистрибуцию BGP в OSPF и OSPF в BGP.<br>
Убедиться, что префиксы из BGP добавились в OSPF домен, а также исключена возможность закольцовки трафика.
2. Настроить iBGP в ASN 300. Префиксы из BGP должны находиться на всех маршрутизаторах в ASN 300.
3. Трафик из ASN 300 в ASN 700 должен проходить по стыку R3-R6.
4. Трафик из ASN 700 в ASN 300 должен проходить по стыку R7-R4.
5. Убедиться в этом сделав трассировку от R5 до лупбека роутера R8 и трассировку о R8 до лупбека роутера R5.

### Результат

#### 1. Результаты трассировок из задания

##### 1.1 Маршрутизатор R5

<pre>
R5#traceroute ip 10.8.8.8
Type escape sequence to abort.
Tracing the route to 10.8.8.8
VRF info: (vrf in name/id, vrf out name/id)
  1 10.0.0.1 3 msec
    10.0.0.5 2 msec
    10.0.0.1 1 msec
  2 10.0.0.14 3 msec
    10.0.0.10 3 msec
    10.0.0.14 3 msec
  3 188.24.0.2 5 msec
    10.0.0.17 3 msec
    188.24.0.2 6 msec
  4 188.24.0.2 6 msec
    172.16.0.6 [AS 700] 4 msec
    188.24.0.2 7 msec
</pre>

##### 1.2 Маршрутизатор R8

<pre>
R8#traceroute ip 10.5.5.5
Type escape sequence to abort.
Tracing the route to 10.5.5.5
VRF info: (vrf in name/id, vrf out name/id)
  1 172.16.0.9 4 msec 2 msec 2 msec
  2 13.201.255.5 6 msec 3 msec 2 msec
  3 10.0.0.13 4 msec 5 msec 4 msec
  4 10.0.0.6 8 msec *  12 msec
</pre>


#### 2. show run с роутеров R6 и R7

##### 2.1 Маршрутизатор R6

<pre>
R6#sh run
Building configuration...

Current configuration : 4382 bytes
!
! Last configuration change at 05:36:32 UTC Sun Jan 10 2021
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R6
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
!
!
!
!
!
!
no ip domain lookup
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
redundancy
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 10.6.6.6 255.255.255.255
!
interface GigabitEthernet0/0
 ip address 188.24.0.2 255.255.255.252
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/1
 ip address 172.16.0.1 255.255.255.252
 ip ospf network point-to-point
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 ip address 172.16.0.5 255.255.255.252
 ip ospf network point-to-point
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
router ospf 1
 redistribute bgp 700 subnets route-map rm_BGP-TO-OSPF
 network 10.6.6.6 0.0.0.0 area 0
 network 172.16.0.0 0.0.0.3 area 0
 network 172.16.0.4 0.0.0.3 area 0
!
router bgp 700
 bgp log-neighbor-changes
 bgp redistribute-internal
 network 188.24.0.0 mask 255.255.255.252
 redistribute ospf 1 match internal external 1 external 2 route-map rm_OSPF-TO-BGP
 neighbor 172.16.0.2 remote-as 700
 neighbor 172.16.0.2 next-hop-self
 neighbor 172.16.0.2 send-community
 neighbor 172.16.0.2 weight 48000
 neighbor 188.24.0.1 remote-as 300
 neighbor 188.24.0.1 weight 30000
 neighbor 188.24.0.1 route-map rm_BGP-OUT out
!
ip forward-protocol nd
!
ip community-list 70 permit 700
!         
no ip http server
no ip http secure-server
!
!
ip prefix-list pl_BGP-OUT-FILTER seq 5 permit 172.16.0.0/12 le 30
ip prefix-list pl_BGP-OUT-FILTER seq 10 permit 10.6.6.6/32
ip prefix-list pl_BGP-OUT-FILTER seq 15 permit 10.7.7.7/32
ip prefix-list pl_BGP-OUT-FILTER seq 20 permit 10.8.8.8/32
ipv6 ioam timestamp
!
route-map rm_BGP-TO-OSPF deny 10
 match community 70
!
route-map rm_BGP-TO-OSPF permit 20
 set metric 10
 set tag 300
!
route-map rm_OSPF-TO-BGP deny 10
 match tag 300
!
route-map rm_OSPF-TO-BGP permit 20
 set weight 60000
 set community 700
!
route-map rm_BGP-OUT permit 10
 match ip address prefix-list pl_BGP-OUT-FILTER
 set origin igp
!
route-map rm_BGP-OUT deny 20
!
!
!
control-plane
!
banner exec ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner incoming ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner login ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
!
line con 0
line aux 0
line vty 0 4
 login
 transport input none
!
no scheduler allocate
!
end

</pre>

##### 2.2 Маршрутизатор R7

<pre>
R7#sh run
Building configuration...

Current configuration : 4355 bytes
!
! Last configuration change at 05:38:32 UTC Sun Jan 10 2021
!
version 15.9
service timestamps debug datetime msec
service timestamps log datetime msec
no service password-encryption
!
hostname R7
!
boot-start-marker
boot-end-marker
!
!
!
no aaa new-model
!
!
!
mmi polling-interval 60
no mmi auto-configure
no mmi pvc
mmi snmp-timeout 180
!
!
!
!
!
!
!
!
!
!
!
no ip domain lookup
ip cef
no ipv6 cef
!
multilink bundle-name authenticated
!
!
!
!
!
redundancy
!
!
! 
!
!
!
!
!
!
!
!
!
!
!
!
interface Loopback0
 ip address 10.7.7.7 255.255.255.255
!
interface GigabitEthernet0/0
 ip address 13.201.255.6 255.255.255.252
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/1
 ip address 172.16.0.2 255.255.255.252
 ip ospf network point-to-point
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/2
 ip address 172.16.0.9 255.255.255.252
 ip ospf network point-to-point
 duplex auto
 speed auto
 media-type rj45
!
interface GigabitEthernet0/3
 no ip address
 shutdown
 duplex auto
 speed auto
 media-type rj45
!
router ospf 1
 redistribute bgp 700 subnets route-map rm_BGP-TO-OSPF
 network 10.7.7.7 0.0.0.0 area 0
 network 172.16.0.0 0.0.0.3 area 0
 network 172.16.0.8 0.0.0.3 area 0
!
router bgp 700
 bgp log-neighbor-changes
 bgp redistribute-internal
 network 13.201.255.4 mask 255.255.255.252
 redistribute ospf 1 match internal external 1 external 2 route-map rm_OSPF-TO-BGP
 neighbor 13.201.255.5 remote-as 300
 neighbor 13.201.255.5 weight 48000
 neighbor 13.201.255.5 route-map rm_BGP-OUT out
 neighbor 172.16.0.1 remote-as 700
 neighbor 172.16.0.1 next-hop-self
 neighbor 172.16.0.1 weight 30000
!
ip forward-protocol nd
!
ip community-list 70 permit 700
!
no ip http server
no ip http secure-server
!
!
ip prefix-list pl_BGP-OUT-FILTER seq 5 permit 172.16.0.0/12 le 30
ip prefix-list pl_BGP-OUT-FILTER seq 10 permit 10.6.6.6/32
ip prefix-list pl_BGP-OUT-FILTER seq 15 permit 10.7.7.7/32
ip prefix-list pl_BGP-OUT-FILTER seq 20 permit 10.8.8.8/32
ipv6 ioam timestamp
!
route-map rm_OSPF-TO-BGP deny 10
 match tag 300
!
route-map rm_OSPF-TO-BGP permit 20
 set weight 32000
 set community 700
!
route-map rm_BGP-TO-OSPF deny 10
 match community 70
!
route-map rm_BGP-TO-OSPF permit 20
 set metric 5
 set tag 300
!         
route-map rm_BGP-OUT permit 10
 match ip address prefix-list pl_BGP-OUT-FILTER
 set origin igp
!
route-map rm_BGP-OUT deny 20
!
!
!
control-plane
!
banner exec ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner incoming ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
banner login ^C
**************************************************************************
* IOSv is strictly limited to use for evaluation, demonstration and IOS  *
* education. IOSv is provided as-is and is not supported by Cisco's      *
* Technical Advisory Center. Any use or disclosure, in whole or in part, *
* of the IOSv Software or Documentation to any third party for any       *
* purposes is expressly prohibited except as otherwise authorized by     *
* Cisco in writing.                                                      *
**************************************************************************^C
!
line con 0
line aux 0
line vty 0 4
 login
 transport input none
!
no scheduler allocate
!         
end
</pre>

#### 3. show ip bgp на роутере R3, R4, R5, R6, R7

##### 3.1 Маршрутизатор R3

<pre>
R3#show ip bgp
BGP table version is 133, local router ID is 10.3.3.3
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.0.0.0/30      10.0.0.9                 2         32768 ?
 *>   10.0.0.4/30      10.0.0.9                 3         32768 ?
 *>   10.0.0.8/30      0.0.0.0                  0         32768 ?
 *>   10.0.0.12/30     10.0.0.9                 4         32768 ?
 *>   10.0.0.16/30     0.0.0.0                  0         32768 ?
 *>   10.1.1.1/32      10.0.0.9                 2         32768 ?
 *>   10.2.2.2/32      10.0.0.9                 4         32768 ?
 *>   10.3.3.3/32      0.0.0.0                  0         32768 ?
 *>   10.4.4.4/32      10.0.0.18                2         32768 ?
 *>   10.5.5.5/32      10.0.0.9                 3         32768 ?
 *>   10.6.6.6/32      188.24.0.2               0         60000 700 i
 *>   10.7.7.7/32      188.24.0.2               2         60000 700 i
 *>   10.8.8.8/32      188.24.0.2               2         60000 700 i
     Network          Next Hop            Metric LocPrf Weight Path
 *>i  13.201.255.4/30  10.0.0.18                0    100      0 i
 *>   172.16.0.0/30    188.24.0.2               0         60000 700 i
 *>   172.16.0.4/30    188.24.0.2               0         60000 700 i
 *>   172.16.0.8/30    188.24.0.2               2         60000 700 i
 *>   188.24.0.0/30    0.0.0.0                  0         32768 i
</pre>

##### 3.2 Маршрутизатор R4

<pre>
R4#show ip bgp
BGP table version is 234, local router ID is 10.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 r>i  10.0.0.0/30      10.0.0.17                2    100  48000 ?
 r                     10.0.0.13                3         32768 ?
 r>i  10.0.0.4/30      10.0.0.17                3    100  48000 ?
 r                     10.0.0.13                2         32768 ?
 r>i  10.0.0.8/30      10.0.0.17                0    100  48000 ?
 r                     10.0.0.13                4         32768 ?
 r>i  10.0.0.12/30     10.0.0.17                4    100  48000 ?
 r                     0.0.0.0                  0         32768 ?
 r>i  10.0.0.16/30     10.0.0.17                0    100  48000 ?
 r                     0.0.0.0                  0         32768 ?
 r>i  10.1.1.1/32      10.0.0.17                2    100  48000 ?
 r                     10.0.0.13                4         32768 ?
 r>i  10.2.2.2/32      10.0.0.17                4    100  48000 ?
     Network          Next Hop            Metric LocPrf Weight Path
 r                     10.0.0.13                2         32768 ?
 r>i  10.3.3.3/32      10.0.0.17                0    100  48000 ?
 r                     10.0.0.17                2         32768 ?
 r>i  10.4.4.4/32      10.0.0.17                2    100  48000 ?
 r                     0.0.0.0                  0         32768 ?
 r>i  10.5.5.5/32      10.0.0.17                3    100  48000 ?
 r                     10.0.0.13                3         32768 ?
 *>i  10.6.6.6/32      10.0.0.17                0    100  48000 700 i
 *                     13.201.255.6             2         30000 700 i
 *>i  10.7.7.7/32      10.0.0.17                2    100  48000 700 i
 *                     13.201.255.6             0         30000 700 i
 *>i  10.8.8.8/32      10.0.0.17                2    100  48000 700 i
 *                     13.201.255.6             2         30000 700 i
 *>   13.201.255.4/30  0.0.0.0                  0         32768 i
 *>i  172.16.0.0/30    10.0.0.17                0    100  48000 700 i
 *                     13.201.255.6             0         30000 700 i
 *>i  172.16.0.4/30    10.0.0.17                0    100  48000 700 i
 *                     13.201.255.6             2         30000 700 i
 *>i  172.16.0.8/30    10.0.0.17                2    100  48000 700 i
 *                     13.201.255.6             0         30000 700 i
 *>i  188.24.0.0/30    10.0.0.17                0    100  48000 i
</pre>

##### 3.3 Маршрутизатор R5

<pre>
R5#show ip bgp
BGP table version is 65, local router ID is 10.5.5.5
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 r i  10.0.0.0/30      10.0.0.17                2    100      0 ?
 r>i                   10.0.0.9                 2    100      0 ?
 r i  10.0.0.4/30      10.0.0.17                3    100      0 ?
 r>i                   10.0.0.9                 3    100      0 ?
 r>i  10.0.0.8/30      10.0.0.17                0    100      0 ?
 r i                   10.3.3.3                 0    100      0 ?
 r i  10.0.0.12/30     10.0.0.17                4    100      0 ?
 r>i                   10.0.0.9                 4    100      0 ?
 r>i  10.0.0.16/30     10.0.0.17                0    100      0 ?
 r i                   10.3.3.3                 0    100      0 ?
 r i  10.1.1.1/32      10.0.0.17                2    100      0 ?
 r>i                   10.0.0.9                 2    100      0 ?
 r i  10.2.2.2/32      10.0.0.17                4    100      0 ?
     Network          Next Hop            Metric LocPrf Weight Path
 r>i                   10.0.0.9                 4    100      0 ?
 r>i  10.3.3.3/32      10.0.0.17                0    100      0 ?
 r i                   10.3.3.3                 0    100      0 ?
 r i  10.4.4.4/32      10.0.0.17                2    100      0 ?
 r>i                   10.0.0.18                2    100      0 ?
 r i  10.5.5.5/32      10.0.0.17                3    100      0 ?
 r>i                   10.0.0.9                 3    100      0 ?
 * i  10.6.6.6/32      10.0.0.17                0    100      0 700 i
 *>i                   188.24.0.2               0    100      0 700 i
 * i  10.7.7.7/32      10.0.0.17                2    100      0 700 i
 *>i                   188.24.0.2               2    100      0 700 i
 * i  10.8.8.8/32      10.0.0.17                2    100      0 700 i
 *>i                   188.24.0.2               2    100      0 700 i
 *>i  13.201.255.4/30  10.4.4.4                 0    100      0 i
 * i                   10.0.0.18                0    100      0 i
 * i  172.16.0.0/30    10.0.0.17                0    100      0 700 i
 *>i                   188.24.0.2               0    100      0 700 i
 * i  172.16.0.4/30    10.0.0.17                0    100      0 700 i
 *>i                   188.24.0.2               0    100      0 700 i
 * i  172.16.0.8/30    10.0.0.17                2    100      0 700 i
 *>i                   188.24.0.2               2    100      0 700 i
 *>i  188.24.0.0/30    10.0.0.17                0    100      0 i
     Network          Next Hop            Metric LocPrf Weight Path
 * i                   10.3.3.3                 0    100      0 i
</pre>

##### 3.4 Маршрутизатор R6

<pre>
R6#show ip bgp
BGP table version is 3277, local router ID is 10.6.6.6
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 r>i  10.0.0.0/30      172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               2         30000 300 i
 r>i  10.0.0.4/30      172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               3         30000 300 i
 r>i  10.0.0.8/30      172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               0         30000 300 i
 r>i  10.0.0.12/30     172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               4         30000 300 i
 r>i  10.0.0.16/30     172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               0         30000 300 i
 r>i  10.1.1.1/32      172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               2         30000 300 i
 r>i  10.2.2.2/32      172.16.0.2               0    100  48000 300 i
     Network          Next Hop            Metric LocPrf Weight Path
 r                     188.24.0.1               4         30000 300 i
 r>i  10.3.3.3/32      172.16.0.2               2    100  48000 300 i
 r                     188.24.0.1               0         30000 300 i
 r>i  10.4.4.4/32      172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               2         30000 300 i
 r>i  10.5.5.5/32      172.16.0.2               0    100  48000 300 i
 r                     188.24.0.1               3         30000 300 i
 * i  10.6.6.6/32      172.16.0.2               2    100  48000 ?
 *>                    0.0.0.0                  0         60000 ?
 *>   10.7.7.7/32      172.16.0.2               2         60000 ?
 * i                   172.16.0.2               0    100  48000 ?
 *>   10.8.8.8/32      172.16.0.6               2         60000 ?
 * i                   172.16.0.2               2    100  48000 ?
 r>i  13.201.255.4/30  172.16.0.2               0    100  48000 i
 * i  172.16.0.0/30    172.16.0.2               0    100  48000 ?
 *>                    0.0.0.0                  0         60000 ?
 * i  172.16.0.4/30    172.16.0.2               2    100  48000 ?
 *>                    0.0.0.0                  0         60000 ?
 *>   172.16.0.8/30    172.16.0.6               2         60000 ?
 * i                   172.16.0.2               0    100  48000 ?
 *>   188.24.0.0/30    0.0.0.0                  0         32768 i
</pre>

##### 3.5 Маршрутизатор R7

<pre>
R7#show ip bgp
BGP table version is 3667, local router ID is 10.7.7.7
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.0.0.0/30      13.201.255.5                       48000 300 i
 *>   10.0.0.4/30      13.201.255.5                       48000 300 i
 *>   10.0.0.8/30      13.201.255.5                       48000 300 i
 *>   10.0.0.12/30     13.201.255.5                       48000 300 i
 *>   10.0.0.16/30     13.201.255.5                       48000 300 i
 *>   10.1.1.1/32      13.201.255.5                       48000 300 i
 *>   10.2.2.2/32      13.201.255.5                       48000 300 i
 *>   10.3.3.3/32      13.201.255.5             2         48000 300 i
 *>   10.4.4.4/32      13.201.255.5                       48000 300 i
 *>   10.5.5.5/32      13.201.255.5                       48000 300 i
 *>   10.6.6.6/32      172.16.0.1               2         32000 ?
 * i                   172.16.0.1               0    100  30000 ?
 * i  10.7.7.7/32      172.16.0.1               2    100  30000 ?
     Network          Next Hop            Metric LocPrf Weight Path
 *>                    0.0.0.0                  0         32000 ?
 * i  10.8.8.8/32      172.16.0.1               2    100  30000 ?
 *>                    172.16.0.10              2         32000 ?
 *>   13.201.255.4/30  0.0.0.0                  0         32768 i
 *>   172.16.0.0/30    0.0.0.0                  0         32000 ?
 * i                   172.16.0.1               0    100  30000 ?
 *>   172.16.0.4/30    172.16.0.10              2         32000 ?
 * i                   172.16.0.1               0    100  30000 ?
 * i  172.16.0.8/30    172.16.0.1               2    100  30000 ?
 *>                    0.0.0.0                  0         32000 ?
 r>i  188.24.0.0/30    172.16.0.1               0    100  30000 i
</pre>

