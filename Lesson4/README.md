## Практическое задание 4

### Задание:

1. Не трогая кабели и не добавляя/удаляя интерфейсы создать топологию, как на приложенной к ДЗ картинке
2. Подсети возьмите из картинки
3. Номера VLAN, если потребуется, можно брать любые, кроме vlan 1
4. У каждого роутера должен быть лупбек с IP адресов формата 1.1.1.1/32 2.2.2.2/32 и так далее до роутера 5
4. Тип интерфейса между R3 и R4 должен быть point-to-point
5. Трафик от VPC до VPC должен ходить по маршруту R1-R2-R4-R5 без балансировки нагрузки
6. В случае проблем со связностью между R2 и R4 трафик должен переключаться на маршрут r1-r3-r4-r5
7. Hello таймеры должны быть в 2 раза меньше стандартных в соседстве между R4 и R5

### Результат


### 1. show ip ospf int с каждого роутера

#### 1.1 **R1**

<pre>
R1#show ip ospf int
GigabitEthernet0/1 is up, line protocol is up
  Internet Address 10.1.30.1/24, Area 0, Attached via Network Statement
  Process ID 1, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 1.1.1.1, Interface address 10.1.30.1
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:05
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/2/2, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 0, maximum is 0
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)
GigabitEthernet0/0.101 is up, line protocol is up
  Internet Address 192.168.10.1/28, Area 0, Attached via Network Statement
  Process ID 1, Router ID 1.1.1.1, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 1.1.1.1, Interface address 192.168.10.1
  Backup Designated router (ID) 2.2.2.2, Interface address 192.168.10.2
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:08
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/1, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 0, maximum is 5
  Last flood scan time is 0 msec, maximum is 4 msec
  Neighbor Count is 2, Adjacent neighbor count is 2
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
    Adjacent with neighbor 3.3.3.3
  Suppress hello for 0 neighbor(s)
</pre>

#### 1.2 **R2**

<pre>
R2#show ip ospf int
GigabitEthernet0/0.101 is up, line protocol is up
  Internet Address 192.168.10.2/28, Area 0, Attached via Network Statement
  Process ID 1, Router ID 2.2.2.2, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State BDR, Priority 1
  Designated Router (ID) 1.1.1.1, Interface address 192.168.10.1
  Backup Designated router (ID) 2.2.2.2, Interface address 192.168.10.2
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:03
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/1, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 4, maximum is 4
  Last flood scan time is 1 msec, maximum is 2 msec
  Neighbor Count is 2, Adjacent neighbor count is 2
    Adjacent with neighbor 1.1.1.1  (Designated Router)
    Adjacent with neighbor 3.3.3.3
  Suppress hello for 0 neighbor(s)
GigabitEthernet0/0.102 is up, line protocol is up
  Internet Address 192.168.20.1/30, Area 1, Attached via Network Statement
  Process ID 1, Router ID 2.2.2.2, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State BDR, Priority 1
  Designated Router (ID) 4.4.4.4, Interface address 192.168.20.2
  Backup Designated router (ID) 2.2.2.2, Interface address 192.168.20.1
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:00
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/2, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 2, maximum is 2
  Last flood scan time is 0 msec, maximum is 1 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 4.4.4.4  (Designated Router)
  Suppress hello for 0 neighbor(s)
</pre>

#### 1.3 **R3**

<pre>
show ip ospf int
GigabitEthernet0/0.101 is up, line protocol is up
  Internet Address 192.168.10.3/28, Area 0, Attached via Network Statement
  Process ID 1, Router ID 3.3.3.3, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State DROTHER, Priority 1
  Designated Router (ID) 1.1.1.1, Interface address 192.168.10.1
  Backup Designated router (ID) 2.2.2.2, Interface address 192.168.10.2
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:07
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/1, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 0, maximum is 5
  Last flood scan time is 1 msec, maximum is 2 msec
  Neighbor Count is 2, Adjacent neighbor count is 2
    Adjacent with neighbor 1.1.1.1  (Designated Router)
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
GigabitEthernet0/0.103 is up, line protocol is up
  Internet Address 192.168.20.5/30, Area 1, Attached via Network Statement
  Process ID 1, Router ID 3.3.3.3, Network Type BROADCAST, Cost: 10
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           10        no          no            Base
  Transmit Delay is 1 sec, State BDR, Priority 1
  Designated Router (ID) 4.4.4.4, Interface address 192.168.20.6
  Backup Designated router (ID) 3.3.3.3, Interface address 192.168.20.5
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:07
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/2, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 2, maximum is 2
  Last flood scan time is 0 msec, maximum is 1 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 4.4.4.4  (Designated Router)
  Suppress hello for 0 neighbor(s)
</pre>

#### 1.4 **R4**

<pre>
R4#show ip ospf int
GigabitEthernet0/0.105 is up, line protocol is up
  Internet Address 192.168.30.1/30, Area 1, Attached via Network Statement
  Process ID 1, Router ID 4.4.4.4, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State BDR, Priority 1
  Designated Router (ID) 5.5.5.5, Interface address 192.168.30.2
  Backup Designated router (ID) 4.4.4.4, Interface address 192.168.30.1
  Timer intervals configured, Hello 5, Dead 20, Wait 20, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:00
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/3/3, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 2, maximum is 3
  Last flood scan time is 1 msec, maximum is 3 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 5.5.5.5  (Designated Router)
  Suppress hello for 0 neighbor(s)
GigabitEthernet0/0.103 is up, line protocol is up
  Internet Address 192.168.20.6/30, Area 1, Attached via Network Statement
  Process ID 1, Router ID 4.4.4.4, Network Type BROADCAST, Cost: 10
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           10        no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 4.4.4.4, Interface address 192.168.20.6
  Backup Designated router (ID) 3.3.3.3, Interface address 192.168.20.5
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:05
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/2/2, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 2, maximum is 3
  Last flood scan time is 0 msec, maximum is 3 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 3.3.3.3  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
GigabitEthernet0/0.102 is up, line protocol is up
  Internet Address 192.168.20.2/30, Area 1, Attached via Network Statement
  Process ID 1, Router ID 4.4.4.4, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 4.4.4.4, Interface address 192.168.20.2
  Backup Designated router (ID) 2.2.2.2, Interface address 192.168.20.1
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:03
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/1, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 2, maximum is 3
  Last flood scan time is 0 msec, maximum is 3 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 2.2.2.2  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
</pre>

#### 1.5 **R5**

<pre>
R5#show ip ospf int
GigabitEthernet0/1 is up, line protocol is up
  Internet Address 10.4.30.1/24, Area 1, Attached via Network Statement
  Process ID 1, Router ID 5.5.5.5, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 5.5.5.5, Interface address 10.4.30.1
  No backup designated router on this network
  Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:07
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/2/2, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 0, maximum is 0
  Last flood scan time is 0 msec, maximum is 0 msec
  Neighbor Count is 0, Adjacent neighbor count is 0
  Suppress hello for 0 neighbor(s)
GigabitEthernet0/0.105 is up, line protocol is up
  Internet Address 192.168.30.2/30, Area 1, Attached via Network Statement
  Process ID 1, Router ID 5.5.5.5, Network Type BROADCAST, Cost: 1
  Topology-MTID    Cost    Disabled    Shutdown      Topology Name
        0           1         no          no            Base
  Transmit Delay is 1 sec, State DR, Priority 1
  Designated Router (ID) 5.5.5.5, Interface address 192.168.30.2
  Backup Designated router (ID) 4.4.4.4, Interface address 192.168.30.1
  Timer intervals configured, Hello 5, Dead 20, Wait 20, Retransmit 5
    oob-resync timeout 40
    Hello due in 00:00:03
  Supports Link-local Signaling (LLS)
  Cisco NSF helper support enabled
  IETF NSF helper support enabled
  Index 1/1/1, flood queue length 0
  Next 0x0(0)/0x0(0)/0x0(0)
  Last flood scan length is 2, maximum is 2
  Last flood scan time is 0 msec, maximum is 3 msec
  Neighbor Count is 1, Adjacent neighbor count is 1
    Adjacent with neighbor 4.4.4.4  (Backup Designated Router)
  Suppress hello for 0 neighbor(s)
</pre>

### 2. Результат трассировки между VPC

Так и не понял почему последний **хоп** с ошибкой, поэтому добавил результат **ping**.

<pre>
 5   *10.4.30.8   13.560 ms <b>(ICMP type:3, code:3, Destination port unreachable)</b>
</pre>

Причем это происходит при трассировки до любого узла.

#### 2.1 VPC7

<pre>
<b>VPC7> ping 10.4.30.8</b>

84 bytes from 10.4.30.8 icmp_seq=1 ttl=60 time=16.624 ms
84 bytes from 10.4.30.8 icmp_seq=2 ttl=60 time=13.113 ms
84 bytes from 10.4.30.8 icmp_seq=3 ttl=60 time=12.202 ms
84 bytes from 10.4.30.8 icmp_seq=4 ttl=60 time=15.335 ms
84 bytes from 10.4.30.8 icmp_seq=5 ttl=60 time=18.009 ms

<b>VPC7> trace 10.4.30.8</b>
trace to 10.4.30.8, 8 hops max, press Ctrl+C to stop
 1   10.1.30.1   1.408 ms  1.639 ms  1.621 ms
 2   192.168.10.2   9.558 ms  8.673 ms  6.592 ms
 3   192.168.20.2   12.982 ms  12.625 ms  9.379 ms
 4   192.168.30.2   19.284 ms  15.968 ms  14.629 ms
 5   *10.4.30.8   13.560 ms (ICMP type:3, code:3, Destination port unreachable)
</pre>

#### 2.2 VPC8

<pre>
VPC8> ping 10.1.30.7

84 bytes from 10.1.30.7 icmp_seq=1 ttl=60 time=13.437 ms
84 bytes from 10.1.30.7 icmp_seq=2 ttl=60 time=25.559 ms
84 bytes from 10.1.30.7 icmp_seq=3 ttl=60 time=13.049 ms
84 bytes from 10.1.30.7 icmp_seq=4 ttl=60 time=9.371 ms
84 bytes from 10.1.30.7 icmp_seq=5 ttl=60 time=21.688 ms

VPC8> trace 10.1.30.7
trace to 10.1.30.7, 8 hops max, press Ctrl+C to stop
 1   10.4.30.1   2.867 ms  1.649 ms  1.330 ms
 2   192.168.30.1   5.830 ms  6.090 ms  5.100 ms
 3   192.168.20.1   9.404 ms  10.169 ms  7.386 ms
 4   192.168.10.1   11.276 ms  11.364 ms  13.033 ms
 5   *10.1.30.7   9.703 ms (ICMP type:3, code:3, Destination port unreachable)
</pre>