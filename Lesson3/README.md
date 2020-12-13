## Практическое задание 3

Изменил формат сдачи задания.

## Задания:

1. Необходимо между всеми роутерами включить проктокол маршрутизации RIP version2, и отключить auto-summary
2. Трафик между VPC6 и VPC7 должен ходить только по маршруту R1<->R2<->R5
3. В случае проблем с интерфейсами между роутерами R1 и R2, либо между R2 и R5, трафик между VPC6 и VPC7 должен переключиться на маршрут R1<->R4<->R5
4. Роутер R1 должен анонсировать IP адреса интерфейсов lo0 и lo1 в RIP домен
5. На роутерах R2, R3, R4 в таблице маршрутизации должны присуствовать оба адреса лупбеков 1.1.1.1/32 и 1.1.1.2/32 в качестве RIP маршрута
6. На маршрутизаторе R5 в таблице маршрутизации должна находится только одна подсеть из лупбеков - 1.1.1.2/32 в качестве RIP маршрута

## Результат:

### 1. show ip route на всех руотерах

#### 1.1 **R1**
<pre>
<b>R1#sh ip route</b>
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

      1.0.0.0/32 is subnetted, 2 subnets
C        1.1.1.1 is directly connected, Loopback0
C        1.1.1.2 is directly connected, Loopback1
      10.0.0.0/8 is variably subnetted, 9 subnets, 2 masks
C        10.1.2.0/24 is directly connected, GigabitEthernet0/1
L        10.1.2.1/32 is directly connected, GigabitEthernet0/1
C        10.1.3.0/24 is directly connected, GigabitEthernet0/2
L        10.1.3.1/32 is directly connected, GigabitEthernet0/2
C        10.1.4.0/24 is directly connected, GigabitEthernet0/3
L        10.1.4.1/32 is directly connected, GigabitEthernet0/3
R        10.2.5.0/24 [120/1] via 10.1.2.2, 00:00:22, GigabitEthernet0/1
R        10.3.5.0/24 [120/1] via 10.1.3.3, 00:00:21, GigabitEthernet0/2
R        10.4.5.0/24 [120/1] via 10.1.4.4, 00:00:23, GigabitEthernet0/3
      172.16.0.0/16 is variably subnetted, 2 subnets, 2 masks
C        172.16.0.0/24 is directly connected, GigabitEthernet0/0
L        172.16.0.1/32 is directly connected, GigabitEthernet0/0
R     192.168.0.0/24 [120/2] via 10.1.2.2, 00:00:22, GigabitEthernet0/1
</pre>

#### 1.2 **R2**

<pre>
<b>R2#sh ip route</b>
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

      1.0.0.0/32 is subnetted, 2 subnets
R        1.1.1.1 [120/1] via 10.1.2.1, 00:00:14, GigabitEthernet0/0
R        1.1.1.2 [120/1] via 10.1.2.1, 00:00:14, GigabitEthernet0/0
      10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
C        10.1.2.0/24 is directly connected, GigabitEthernet0/0
L        10.1.2.2/32 is directly connected, GigabitEthernet0/0
R        10.1.3.0/24 [120/1] via 10.1.2.1, 00:00:14, GigabitEthernet0/0
R        10.1.4.0/24 [120/1] via 10.1.2.1, 00:00:14, GigabitEthernet0/0
C        10.2.5.0/24 is directly connected, GigabitEthernet0/1
L        10.2.5.2/32 is directly connected, GigabitEthernet0/1
R        10.3.5.0/24 [120/1] via 10.2.5.5, 00:00:12, GigabitEthernet0/1
R        10.4.5.0/24 [120/1] via 10.2.5.5, 00:00:12, GigabitEthernet0/1
      172.16.0.0/24 is subnetted, 1 subnets
R        172.16.0.0 [120/1] via 10.1.2.1, 00:00:14, GigabitEthernet0/0
R     192.168.0.0/24 [120/1] via 10.2.5.5, 00:00:12, GigabitEthernet0/1
</pre>

#### 1.3 **R3**

<pre>
<b>R3#sh ip route</b>
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

      1.0.0.0/32 is subnetted, 2 subnets
R        1.1.1.1 [120/5] via 10.1.3.1, 00:00:08, GigabitEthernet0/0
R        1.1.1.2 [120/5] via 10.1.3.1, 00:00:08, GigabitEthernet0/0
      10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
R        10.1.2.0/24 [120/5] via 10.1.3.1, 00:00:08, GigabitEthernet0/0
C        10.1.3.0/24 is directly connected, GigabitEthernet0/0
L        10.1.3.3/32 is directly connected, GigabitEthernet0/0
R        10.1.4.0/24 [120/5] via 10.1.3.1, 00:00:08, GigabitEthernet0/0
R        10.2.5.0/24 [120/5] via 10.3.5.5, 00:00:13, GigabitEthernet0/1
C        10.3.5.0/24 is directly connected, GigabitEthernet0/1
L        10.3.5.3/32 is directly connected, GigabitEthernet0/1
R        10.4.5.0/24 [120/5] via 10.3.5.5, 00:00:13, GigabitEthernet0/1
      172.16.0.0/24 is subnetted, 1 subnets
R        172.16.0.0 [120/5] via 10.1.3.1, 00:00:08, GigabitEthernet0/0
R     192.168.0.0/24 [120/5] via 10.3.5.5, 00:00:13, GigabitEthernet0/1
</pre>

#### 1.4 **R4**

<pre>
<b>R4#sh ip route</b>
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

      1.0.0.0/32 is subnetted, 2 subnets
R        1.1.1.1 [120/3] via 10.1.4.1, 00:00:27, GigabitEthernet0/0
R        1.1.1.2 [120/3] via 10.1.4.1, 00:00:27, GigabitEthernet0/0
      10.0.0.0/8 is variably subnetted, 8 subnets, 2 masks
R        10.1.2.0/24 [120/3] via 10.1.4.1, 00:00:27, GigabitEthernet0/0
R        10.1.3.0/24 [120/3] via 10.1.4.1, 00:00:27, GigabitEthernet0/0
C        10.1.4.0/24 is directly connected, GigabitEthernet0/0
L        10.1.4.4/32 is directly connected, GigabitEthernet0/0
R        10.2.5.0/24 [120/3] via 10.4.5.5, 00:00:04, GigabitEthernet0/1
R        10.3.5.0/24 [120/3] via 10.4.5.5, 00:00:04, GigabitEthernet0/1
C        10.4.5.0/24 is directly connected, GigabitEthernet0/1
L        10.4.5.4/32 is directly connected, GigabitEthernet0/1
      172.16.0.0/24 is subnetted, 1 subnets
R        172.16.0.0 [120/3] via 10.1.4.1, 00:00:27, GigabitEthernet0/0
R     192.168.0.0/24 [120/3] via 10.4.5.5, 00:00:04, GigabitEthernet0/1
</pre>

#### 1.5 **R5**

<pre>
<b>R5#sh ip route</b>
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

      1.0.0.0/32 is subnetted, 1 subnets
R        1.1.1.1 [120/2] via 10.2.5.2, 00:00:01, GigabitEthernet0/1
      10.0.0.0/8 is variably subnetted, 9 subnets, 2 masks
R        10.1.2.0/24 [120/1] via 10.2.5.2, 00:00:01, GigabitEthernet0/1
R        10.1.3.0/24 [120/1] via 10.3.5.3, 00:00:13, GigabitEthernet0/2
R        10.1.4.0/24 [120/1] via 10.4.5.4, 00:00:20, GigabitEthernet0/3
C        10.2.5.0/24 is directly connected, GigabitEthernet0/1
L        10.2.5.5/32 is directly connected, GigabitEthernet0/1
C        10.3.5.0/24 is directly connected, GigabitEthernet0/2
L        10.3.5.5/32 is directly connected, GigabitEthernet0/2
C        10.4.5.0/24 is directly connected, GigabitEthernet0/3
L        10.4.5.5/32 is directly connected, GigabitEthernet0/3
      172.16.0.0/24 is subnetted, 1 subnets
R        172.16.0.0 [120/2] via 10.2.5.2, 00:00:01, GigabitEthernet0/1
      192.168.0.0/24 is variably subnetted, 2 subnets, 2 masks
C        192.168.0.0/24 is directly connected, GigabitEthernet0/0
L        192.168.0.1/32 is directly connected, GigabitEthernet0/0
</pre>

### 2. Трассировка между VPC

- **VPC6**
<pre>
<b>VPC6> ping 192.168.0.7</b>

84 bytes from 192.168.0.7 icmp_seq=1 ttl=61 time=5.211 ms
84 bytes from 192.168.0.7 icmp_seq=2 ttl=61 time=4.003 ms
84 bytes from 192.168.0.7 icmp_seq=3 ttl=61 time=3.528 ms
84 bytes from 192.168.0.7 icmp_seq=4 ttl=61 time=4.469 ms
84 bytes from 192.168.0.7 icmp_seq=5 ttl=61 time=3.261

<b>VPC6> trace 192.168.0.7</b>
trace to 192.168.0.7, 8 hops max, press Ctrl+C to stop
 1   172.16.0.1   2.471 ms  1.374 ms  1.264 ms
 2   10.1.2.2   3.382 ms  1.671 ms  2.240 ms
 3   10.2.5.5   5.197 ms  3.204 ms  3.179 ms
 4   *192.168.0.7   6.352 ms (ICMP type:3, code:3, Destination port unreachable)
</pre>

- **VPC7**
<pre>
<b>VPC7> ping 172.16.0.6</b>

84 bytes from 172.16.0.6 icmp_seq=1 ttl=61 time=5.370 ms
84 bytes from 172.16.0.6 icmp_seq=2 ttl=61 time=4.376 ms
84 bytes from 172.16.0.6 icmp_seq=3 ttl=61 time=4.569 ms
84 bytes from 172.16.0.6 icmp_seq=4 ttl=61 time=4.501 ms
84 bytes from 172.16.0.6 icmp_seq=5 ttl=61 time=3.889 ms

<b>VPC7> trace 172.16.0.6</b>
trace to 172.16.0.6, 8 hops max, press Ctrl+C to stop
 1   192.168.0.1   1.867 ms  1.882 ms  1.782 ms
 2   10.2.5.2   2.554 ms  2.941 ms  3.896 ms
 3   10.1.2.1   6.416 ms  3.758 ms  4.509 ms
 4   *172.16.0.6   5.183 ms (ICMP type:3, code:3, Destination port unreachable)
</pre>

### 3.  на роутере R5

<pre>
<b>R5#show ip rip database</b>
1.0.0.0/8    auto-summary
1.1.1.1/32
    [2] via 10.2.5.2, 00:00:19, GigabitEthernet0/1
10.0.0.0/8    auto-summary
10.1.2.0/24
    [1] via 10.2.5.2, 00:00:19, GigabitEthernet0/1
10.1.3.0/24
    [1] via 10.3.5.3, 00:00:06, GigabitEthernet0/2
10.1.4.0/24
    [1] via 10.4.5.4, 00:00:09, GigabitEthernet0/3
10.2.5.0/24    directly connected, GigabitEthernet0/1
10.3.5.0/24    directly connected, GigabitEthernet0/2
10.4.5.0/24    directly connected, GigabitEthernet0/3
172.16.0.0/16    auto-summary
172.16.0.0/24
    [2] via 10.2.5.2, 00:00:19, GigabitEthernet0/1
192.168.0.0/24    auto-summary
192.168.0.0/24    directly connected, GigabitEthernet0/0
</pre>