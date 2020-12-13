## Практическое задание 2

\* - Пункт 5.2 добавил уже после срока сдачи, т.к. посчитал что так будет правильнее.

### 1. Настроить все VPCS в соответсвии с подсетями для каждого vlan. GW для компьютеров - роутер R1. Пример команды (VPCS> ip 10.10.0.2/24 10.10.0.1)
### 2. Порты коммутаторов и роутеров уже имеют  некую преднастройку
### 3. Настройте MST с именем конфиугурации GEEK, номером ревизии 8 и добавьте 2 инстанса, в первый добавьте вланы 100,110,120,130 а во второй 200,210,220,230

<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=16.966 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=18.597 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=16.577 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=28.594 ms
84 bytes from 10.100.0.5 icmp_seq=5 ttl=63 time=14.501 ms
</pre>

<pre>
<b>SW2#sh spanning-tree</b>

MST0
  Spanning tree enabled protocol mstp
  Root ID    Priority    32768
             Address     5000.0001.0000
             Cost        0
             Port        2 (GigabitEthernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32768  (priority 32768 sys-id-ext 0)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 20000     128.1    P2p
Gi0/1               Root FWD 20000     128.2    P2p
Gi0/2               Desg FWD 20000     128.3    P2p
Gi0/3               Desg FWD 20000     128.4    P2p
Gi1/0               Desg FWD 20000     128.5    P2p
Gi1/1               Desg FWD 20000     128.6    P2p
Gi1/2               Desg FWD 20000     128.7    P2p
Gi1/3               Desg FWD 20000     128.8    P2p



MST1
  Spanning tree enabled protocol mstp
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        60000
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 20000     128.1    P2p
Gi0/2               Root FWD 20000     128.3    P2p
Gi0/3               Altn BLK 20000     128.4    P2p



MST2
  Spanning tree enabled protocol mstp
  Root ID    Priority    32770
             Address     5000.0001.0000
             Cost        60000
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32770  (priority 32768 sys-id-ext 2)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Root FWD 20000     128.3    P2p
Gi0/3               Altn BLK 20000     128.4    P2p
</pre>

### 4. Необходимо, чтобы коммутатор Switch1 был root коммутатором для инстанса 1. Коммутатор Switch4 был root коммутатором для инстанса 2

<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=40.305 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=16.918 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=16.357 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=17.996 ms
84 bytes from 10.100.0.5 icmp_seq=5 ttl=63 time=17.443 ms
</pre>

<pre>
<b>SW2#sh spanning-tree</b>

MST0
  Spanning tree enabled protocol mstp
  Root ID    Priority    32768
             Address     5000.0001.0000
             Cost        0
             Port        2 (GigabitEthernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32768  (priority 32768 sys-id-ext 0)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 20000     128.1    P2p
Gi0/1               Root FWD 20000     128.2    P2p
Gi0/2               Desg FWD 20000     128.3    P2p
Gi0/3               Desg FWD 20000     128.4    P2p
Gi1/0               Desg FWD 20000     128.5    P2p
Gi1/1               Desg FWD 20000     128.6    P2p
Gi1/2               Desg FWD 20000     128.7    P2p
Gi1/3               Desg FWD 20000     128.8    P2p



MST1
  Spanning tree enabled protocol mstp
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        60000
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 20000     128.1    P2p
Gi0/2               Root FWD 20000     128.3    P2p
Gi0/3               Altn BLK 20000     128.4    P2p



MST2
  Spanning tree enabled protocol mstp
  Root ID    Priority    8194
             Address     5000.0008.0000
             Cost        40000
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32770  (priority 32768 sys-id-ext 2)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/2               Root FWD 20000     128.3    P2p
Gi0/3               Altn BLK 20000     128.4    P2p
</pre>

### 5. На коммутаторе Switch2 рут портом для инстанса 2 должен быть интерфейс Gi0/2

#### 5.1 Без изменения режима линка между Switch1 и Switch2 на trunk

В процессе выполнения задания 4 так и вышло.

#### 5.2 После изменения режима линка между Switch1 и Switch2 на trunk

<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=48.585 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=13.536 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=9.172 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=12.969 ms
84 bytes from 10.100.0.5 icmp_seq=5 ttl=63 time=8.286 ms
</pre>

<pre>
<b>SW2#sh spanning-tree</b>


MST0
  Spanning tree enabled protocol mstp
  Root ID    Priority    32768
             Address     5000.0001.0000
             Cost        0
             Port        2 (GigabitEthernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32768  (priority 32768 sys-id-ext 0)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 20000     128.1    P2p
Gi0/1               Root FWD 20000     128.2    P2p
Gi0/2               Desg FWD 20000     128.3    P2p
Gi0/3               Desg FWD 20000     128.4    P2p
Gi1/0               Desg FWD 20000     128.5    P2p
Gi1/1               Desg FWD 20000     128.6    P2p
Gi1/2               Desg FWD 20000     128.7    P2p
Gi1/3               Desg FWD 20000     128.8    P2p



MST1
  Spanning tree enabled protocol mstp
  Root ID    Priority    32769
             Address     5000.0001.0000
             Cost        20000
             Port        2 (GigabitEthernet0/1)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/0               Desg FWD 20000     128.1    P2p
Gi0/1               Root FWD 20000     128.2    P2p
Gi0/2               Desg FWD 20000     128.3    P2p
Gi0/3               Desg FWD 20000     128.4    P2p



MST2
  Spanning tree enabled protocol mstp
  Root ID    Priority    8194
             Address     5000.0008.0000
             Cost        40000
             Port        3 (GigabitEthernet0/2)
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32770  (priority 32768 sys-id-ext 2)
             Address     5000.0002.0000
             Hello Time   2 sec  Max Age 20 sec  Forward Delay 15 sec

Interface           Role Sts Cost      Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1               Altn BLK 80000     128.2    P2p
Gi0/2               Root FWD 20000     128.3    P2p
Gi0/3               Altn BLK 20000     128.4    P2p
</pre>


### 6. На роутерах Router1 и Router2 создайте две VRRP группы (для влана 100 и 200). Мастером для обоих групп долежн быть Router1. В качестве VIP используйте IP 10.x.0.254

\* - На VPC заменен шлюз по умолчанию на P 10.x.0.254

<pre>
<b>Router1#sh vrrp</b>

GigabitEthernet0/0.100 - Group 100
  State is Master
  Virtual IP address is 10.100.0.254
  Virtual MAC address is 0000.5e00.0164
  Advertisement interval is 1.000 sec
  Preemption enabled
  Priority is 150
  Master Router is 10.100.0.1 (local), priority is 150
  Master Advertisement interval is 1.000 sec
  Master Down interval is 3.414 sec

GigabitEthernet0/0.200 - Group 200
  State is Master
  Virtual IP address is 10.200.0.254
  Virtual MAC address is 0000.5e00.01c8
  Advertisement interval is 1.000 sec
  Preemption enabled
  Priority is 150
  Master Router is 10.200.0.1 (local), priority is 150
  Master Advertisement interval is 1.000 sec
  Master Down interval is 3.414 sec
</pre>


<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=39.354 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=15.823 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=16.483 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=15.074 ms
84 bytes from 10.100.0.5 icmp_seq=5 ttl=63 time=16.048 ms

<b>VPC4> ping 10.200.0.1</b>

84 bytes from 10.200.0.1 icmp_seq=1 ttl=255 time=3.214 ms
84 bytes from 10.200.0.1 icmp_seq=2 ttl=255 time=9.569 ms
84 bytes from 10.200.0.1 icmp_seq=3 ttl=255 time=5.473 ms
84 bytes from 10.200.0.1 icmp_seq=4 ttl=255 time=3.421 ms
84 bytes from 10.200.0.1 icmp_seq=5 ttl=255 time=4.564 ms

<b>VPC4> ping 10.200.0.254</b>

84 bytes from 10.200.0.254 icmp_seq=1 ttl=255 time=5.213 ms
84 bytes from 10.200.0.254 icmp_seq=2 ttl=255 time=6.768 ms
84 bytes from 10.200.0.254 icmp_seq=3 ttl=255 time=4.011 ms
84 bytes from 10.200.0.254 icmp_seq=4 ttl=255 time=4.519 ms
84 bytes from 10.200.0.254 icmp_seq=5 ttl=255 time=5.005 ms
</pre>

### 7. Убедитесь, что VPC5 и VPC6 могут пинговать VPC4 как в случае, когда роутер Router1 работает, так и в случае, когда VIP переезжает на Router2

<pre>
<b>VPC4> ping 10.100.0.5</b>

84 bytes from 10.100.0.5 icmp_seq=1 ttl=63 time=51.004 ms
84 bytes from 10.100.0.5 icmp_seq=2 ttl=63 time=15.538 ms
84 bytes from 10.100.0.5 icmp_seq=3 ttl=63 time=14.257 ms
84 bytes from 10.100.0.5 icmp_seq=4 ttl=63 time=22.179 ms
84 bytes from 10.100.0.5 icmp_seq=5 ttl=63 time=14.750 ms


<b>VPC4> ping 10.200.0.1</b>

host (10.200.0.1) not reachable


<b>VPC4> ping 10.200.0.254</b>

84 bytes from 10.200.0.254 icmp_seq=1 ttl=255 time=8.068 ms
84 bytes from 10.200.0.254 icmp_seq=2 ttl=255 time=7.698 ms
84 bytes from 10.200.0.254 icmp_seq=3 ttl=255 time=7.548 ms
84 bytes from 10.200.0.254 icmp_seq=4 ttl=255 time=11.767 ms
84 bytes from 10.200.0.254 icmp_seq=5 ttl=255 time=7.483 ms
</pre>
