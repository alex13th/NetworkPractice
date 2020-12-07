## Практическое задание 1

### 1. Настроить все VPCS в соответсвии с подсетями для каждого vlan. GW для компьютеров - роутер R1. Пример команды (VPCS> ip 10.10.0.2/24 10.10.0.1)


<pre>
VPC4: 10.200.0.4/24 10.200.0.1
VPC5: 10.100.0.5/24 10.100.0.1
VPC6: 10.100.0.6/24 10.100.0.1
</pre>


### 2. Настроить все порты на коммутаторах исходя из их режима работы (trunk access)

#### 2.1 Switch1

<pre>
interface GigabitEthernet0/0
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport trunk native vlan 200
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/3
 negotiation auto
!
interface GigabitEthernet1/0
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
</pre>

#### 2.2 Switch2

<pre>
interface GigabitEthernet0/0
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface GigabitEthernet0/1
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/3
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
</pre>

#### 2.3 Switch3

<pre>
interface GigabitEthernet0/0
 switchport access vlan 100
 switchport mode access
 negotiation auto
!
interface GigabitEthernet0/2
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet0/3
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
interface GigabitEthernet1/0
 switchport trunk allowed vlan 1,100,200
 switchport trunk encapsulation dot1q
 switchport mode trunk
 negotiation auto
!
</pre>

### 3. На всех trunk (везде, где наисано TRUNK) интерфейсах разрешить vlan1 ,vlan100, vlan200. Все остальные vlan должны быть запрещены

См. пункт 2.

### 4. Необходимо, чтобы коммутатор Switch3 был root коммутатором для всех vlan. В случае, если коммутаторо switch3 перестает работать, то роль root коммутатора должен принять на себя коммутатор switch2

#### Настройка

- Switch2
<pre>
spanning-tree vlan 1-4094 priority 20480
</pre>

- Switch3
<pre>
spanning-tree vlan 1-4094 priority 12488
</pre>


#### Switch1

<pre>
<b>Switch1#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    12388
             Address     5000.0003.0000
             Cost        4
             Port        5 (GigabitEthernet1/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32868  (priority 32768 sys-id-ext 100)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Altn BLK 4         128.2    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi1/0               Root FWD 4         128.5    P2p 
</pre>

<pre>
<b>Switch1#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             Cost        4
             Port        5 (GigabitEthernet1/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32968  (priority 32768 sys-id-ext 200)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Altn BLK 4         128.2    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi1/0               Root FWD 4         128.5    P2p 
</pre>


#### Switch2
<pre>
<b>Switch2#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    12388
             Address     5000.0003.0000
             Cost        4
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    20580  (priority 20480 sys-id-ext 100)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Desg FWD 4         128.2    P2p 
Gi0/2               Root FWD 4         128.3    P2p 
Gi0/3               Altn BLK 4         128.4    P2p 
</pre>

<pre>
<b>Switch2#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             Cost        4
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    20680  (priority 20480 sys-id-ext 200)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Desg FWD 4         128.2    P2p 
Gi0/2               Root FWD 4         128.3    P2p 
Gi0/3               Altn BLK 4         128.4    P2p
</pre>

#### Switch3
<pre>
<b>Switch3#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    12388
             Address     5000.0003.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    12388  (priority 12288 sys-id-ext 100)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi0/3               Desg LRN 4         128.4    P2p 
Gi1/0               Desg FWD 4         128.5    P2p 
</pre>

<pre>
<b>Switch3#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    12488  (priority 12288 sys-id-ext 200)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  15  sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Desg FWD 4         128.3    P2p 
Gi0/3               Desg FWD 4         128.4    P2p 
Gi1/0               Desg FWD 4         128.5    P2p 
</pre>

#### VPC4
<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=17.364 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=11.456 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=10.060 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=8.769 ms
</pre>

<pre>
<b>VPC4> ping 10.100.0.6</b>

84 bytes from 10.100.0.6 icmp_seq=1 ttl=63 time=15.184 ms
84 bytes from 10.100.0.6 icmp_seq=2 ttl=63 time=5.435 ms
84 bytes from 10.100.0.6 icmp_seq=3 ttl=63 time=6.362 ms
84 bytes from 10.100.0.6 icmp_seq=4 ttl=63 time=5.464 ms
</pre>


### 5. Сделайте так, чтобы от комптютера VPC4 до компьютера VPC6 трафик ходил по пути Switch1->Switch2->Switch3

#### Настройка

- Switch2
<pre>
spanning-tree vlan 100 priority 8192
</pre>

#### Switch1

<pre>
<b>Switch1#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    8292
             Address     5000.0002.0000
             Cost        4
             Port        2 (GigabitEthernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32868  (priority 32768 sys-id-ext 100)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Root FWD 4         128.2    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi1/0               Altn BLK 4         128.5    P2p 
</pre>

<pre>
<b>Switch1#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             Cost        4
             Port        5 (GigabitEthernet1/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32968  (priority 32768 sys-id-ext 200)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Altn BLK 4         128.2    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi1/0               Root FWD 4         128.5    P2p 
</pre>

#### Switch2
<pre>
<b>Switch2#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    8292
             Address     5000.0002.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    8292   (priority 8192 sys-id-ext 100)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Desg FWD 4         128.2    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi0/3               Desg FWD 4         128.4    P2p
</pre>

<pre>
<b>Switch2#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             Cost        4
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    20680  (priority 20480 sys-id-ext 200)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Desg FWD 4         128.2    P2p 
Gi0/2               Root FWD 4         128.3    P2p 
Gi0/3               Altn BLK 4         128.4    P2p 
</pre>


#### Switch3
<pre>
<b>Switch3#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    8292
             Address     5000.0002.0000
             Cost        4
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    12388  (priority 12288 sys-id-ext 100)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/2               Root FWD 4         128.3    P2p 
Gi0/3               Altn BLK 4         128.4    P2p 
Gi1/0               Desg FWD 4         128.5    P2p 
</pre>

<pre>
<b>Switch3#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    12488  (priority 12288 sys-id-ext 200)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Desg FWD 4         128.3    P2p 
Gi0/3               Desg FWD 4         128.4    P2p 
Gi1/0               Desg FWD 4         128.5    P2p 
</pre>

#### VPC4
<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=9.009 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=6.858 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=6.030 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=8.770 ms
84 bytes from 10.100.0.5 icmp_seq=5 ttl=63 time=6.546 ms
</pre>

<pre>
<b>VPC4> ping 10.100.0.6</b>

84 bytes from 10.100.0.6 icmp_seq=1 ttl=63 time=18.953 ms
84 bytes from 10.100.0.6 icmp_seq=2 ttl=63 time=8.617 ms
84 bytes from 10.100.0.6 icmp_seq=3 ttl=63 time=8.520 ms
84 bytes from 10.100.0.6 icmp_seq=4 ttl=63 time=9.501 ms
84 bytes from 10.100.0.6 icmp_seq=5 ttl=63 time=8.329 ms
</pre>


### 6. Не изменяя атрибут cost, на коммутаторе Switch2 сделайте так, чтобы интерфейс Gi0/3 имел роль Root, а интерфейс Gi0/2 был бы заблокирован

#### Настройка

- Switch2

Удаляем настройку из предыдущего задания
<pre>
no spanning-tree vlan 100 priority 8192
spanning-tree vlan 1-4094 priority 20480 
</pre>

- Switch3
<pre>
int gi0/2 
spanning-tree port-priority 240
</pre>


#### Switch1

<pre>
<b>Switch1#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    12388
             Address     5000.0003.0000
             Cost        4
             Port        5 (GigabitEthernet1/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32868  (priority 32768 sys-id-ext 100)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Altn BLK 4         128.2    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi1/0               Root FWD 4         128.5    P2p 
</pre>

<pre>
<b>Switch1#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             Cost        4
             Port        5 (GigabitEthernet1/0)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32968  (priority 32768 sys-id-ext 200)
             Address     5000.0001.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Altn BLK 4         128.2    P2p 
Gi0/2               Desg FWD 4         128.3    P2p 
Gi1/0               Root FWD 4         128.5    P2p 
</pre>

#### Switch2
<pre>
<b>Switch2#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    12388
             Address     5000.0003.0000
             Cost        4
             Port        4 (GigabitEthernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    20580  (priority 20480 sys-id-ext 100)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/1               Desg FWD 4         128.2    P2p 
Gi0/2               Altn BLK 4         128.3    P2p 
Gi0/3               Root FWD 4         128.4    P2p 
</pre>

<pre>
<b>Switch2#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             Cost        4
             Port        4 (GigabitEthernet0/3)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    20680  (priority 20480 sys-id-ext 200)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Desg FWD 4         128.2    P2p 
Gi0/2               Altn BLK 4         128.3    P2p 
Gi0/3               Root FWD 4         128.4    P2p 
</pre>



#### Switch3
<pre>
<b>Switch3#sh spanning-tree vlan 100</b>

VLAN0100
  Spanning tree enabled protocol ieee
  Root ID    Priority    12388
             Address     5000.0003.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    12388  (priority 12288 sys-id-ext 100)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 4         128.1    P2p 
Gi0/2               Desg FWD 4         240.3    P2p 
Gi0/3               Desg FWD 4         128.4    P2p 
Gi1/0               Desg FWD 4         128.5    P2p 
</pre>

<pre>
<b>Switch3#sh spanning-tree vlan 200</b>

VLAN0200
  Spanning tree enabled protocol ieee
  Root ID    Priority    12488
             Address     5000.0003.0000
             This bridge is the root
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    12488  (priority 12288 sys-id-ext 200)
             Address     5000.0003.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  300 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Desg FWD 4         240.3    P2p 
Gi0/3               Desg FWD 4         128.4    P2p 
Gi1/0               Desg FWD 4         128.5    P2p 
</pre>

#### VPC4
<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=12.638 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=6.553 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=8.019 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=5.687 ms
</pre>

<pre>
<b>VPC4> ping 10.100.0.6</b>

84 bytes from 10.100.0.6 icmp_seq=1 ttl=63 time=7.723 ms
84 bytes from 10.100.0.6 icmp_seq=2 ttl=63 time=12.633 ms
84 bytes from 10.100.0.6 icmp_seq=3 ttl=63 time=11.794 ms
84 bytes from 10.100.0.6 icmp_seq=4 ttl=63 time=9.256 ms
</pre>
