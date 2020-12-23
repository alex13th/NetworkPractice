## Практическое задание 5

### Задание:

1. Настроить подсети между роутерами согласно схемы
2. Настроить протоколы маршрутизации согласно схемы.
3. Протоколы IGP должны быть включены только на "серых" IP адресах (RFC1918)
4. Для OSPF тип интерфейсов везде должен быть point-to point
5. Назначить на все роутеры лупбеки согласхно схемы 10.x.x.x где x это порядковый номер роутера
6. Не меняя скорости интерфейса и парпметра cost, сделайте так, чтобы трафик от роутера R5
до лупбека роутера R3 ходил по маршруту R5<->R2<->R4<->R3.
7. Настроить BGP peering между R3 и R6, а так же между R4 и R7

### Результат

#### 1. Состояние BGP пирингов роутеров R6 R7
##### 1.1 Роутер R6

<pre>
R6#sh ip bgp summary
BGP router identifier 10.6.6.6, local AS number 700
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
188.24.0.1      4          300       4       4        1    0    0 00:01:47        0
</pre>

##### 1.2 Роутер R7

<pre>
R7#sh ip bgp summary
BGP router identifier 10.7.7.7, local AS number 700
BGP table version is 1, main routing table version 1

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
13.201.255.5    4          300       4       4        1    0    0 00:01:14        0
</pre>

#### 2. Вывод show ip ospf interface brief со всех роутеров автономной системы 300

##### 2.1 Роутер R1

<pre>
R1#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Lo0          1     1               10.1.1.1/32        1     LOOP  0/0
Gi0/1        1     1               10.0.0.9/30        1     P2P   1/1
Gi0/0        1     1               10.0.0.1/30        1     P2P   1/1
</pre>

##### 2.2 Роутер R2

<pre>
R2#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Lo0          1     1               10.2.2.2/32        1     LOOP  0/0
Gi0/1        1     1               10.0.0.13/30       1     P2P   1/1
Gi0/0        1     1               10.0.0.5/30        1     P2P   1/1
</pre>

##### 2.3 Роутер R3

<pre>
R3#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Lo0          1     0               10.3.3.3/32        1     LOOP  0/0
Gi0/1        1     0               10.0.0.17/30       1     P2P   1/1
Gi0/0        1     1               10.0.0.10/30       1     P2P   1/1
</pre>

##### 2.4 Роутер R4

<pre>
R4#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Lo0          1     0               10.4.4.4/32        1     LOOP  0/0
Gi0/1        1     0               10.0.0.18/30       1     P2P   1/1
Gi0/0        1     1               10.0.0.14/30       1     P2P   1/1
</pre>

##### 2.5 Роутер R5

<pre>
R5#show ip ospf interface brief
Interface    PID   Area            IP Address/Mask    Cost  State Nbrs F/C
Lo0          1     1               10.5.5.5/32        1     LOOP  0/0
Gi0/1        1     1               10.0.0.6/30        1     P2P   1/1
Gi0/0        1     1               10.0.0.2/30        1     P2P   1/1
</pre>

#### 3. Show ip route на роутере R5

<pre>
R5#show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is not set

      10.0.0.0/8 is variably subnetted, 12 subnets, 2 masks
C        10.0.0.0/30 is directly connected, GigabitEthernet0/0
L        10.0.0.2/32 is directly connected, GigabitEthernet0/0
C        10.0.0.4/30 is directly connected, GigabitEthernet0/1
L        10.0.0.6/32 is directly connected, GigabitEthernet0/1
O        10.0.0.8/30 [110/2] via 10.0.0.1, 00:20:34, GigabitEthernet0/0
O        10.0.0.12/30 [110/2] via 10.0.0.5, 00:20:34, GigabitEthernet0/1
O IA     10.0.0.16/30 [110/3] via 10.0.0.5, 00:20:34, GigabitEthernet0/1
O        10.1.1.1/32 [110/2] via 10.0.0.1, 00:20:34, GigabitEthernet0/0
O        10.2.2.2/32 [110/2] via 10.0.0.5, 00:20:34, GigabitEthernet0/1
O IA     10.3.3.3/32 [110/4] via 10.0.0.5, 00:20:34, GigabitEthernet0/1
O IA     10.4.4.4/32 [110/3] via 10.0.0.5, 00:20:34, GigabitEthernet0/1
C        10.5.5.5/32 is directly connected, Loopback0
</pre>