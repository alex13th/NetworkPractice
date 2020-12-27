## Практическое задание 6

Домашка основана на топологии предыдущего задания.
В качестве сдачи показываем show ip bgp на каждом роутере

### Задание:

1. Настроить IBGP пиринги между R3<->R4 и R7<->R6
2. Проанносмровать все префиксы из OSPF процесса в BGP процесс на всех BGP роутерах
3. Сделать так, чтобы трафик из ASN300 в AS700 ходил через пиринг R3<->R6, а траффик из ASN700 в ASN300 через пиринг R4<->R7
4. Убедиться, что ASN700 и ASN300 не являются транзитными (то есть автономная система аннонсирует наружу только принадлежищие ей префиксы, что делает невозможным транзитный трафик через нее)


### Результат:

#### 1.  Роутер R3

<pre>
R3#show ip bgp
BGP table version is 61, local router ID is 10.3.3.3
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 * i  10.0.0.0/30      10.0.0.18                3    100      0 ?
 *>                    10.0.0.9                 2         32768 ?
 * i  10.0.0.4/30      10.0.0.18                2    100      0 ?
 *>                    10.0.0.9                 3         32768 ?
 * i  10.0.0.8/30      10.0.0.18                4    100      0 ?
 *>                    0.0.0.0                  0         32768 ?
 * i  10.0.0.12/30     10.0.0.18                0    100      0 ?
 *>                    10.0.0.9                 4         32768 ?
 * i  10.0.0.16/30     10.0.0.18                0    100      0 ?
 *>                    0.0.0.0                  0         32768 ?
 * i  10.1.1.1/32      10.0.0.18                4    100      0 ?
 *>                    10.0.0.9                 2         32768 ?
 * i  10.2.2.2/32      10.0.0.18                2    100      0 ?
     Network          Next Hop            Metric LocPrf Weight Path
 *>                    10.0.0.9                 4         32768 ?
 * i  10.3.3.3/32      10.0.0.18                2    100      0 ?
 *>                    0.0.0.0                  0         32768 ?
 * i  10.4.4.4/32      10.0.0.18                0    100      0 ?
 *>                    10.0.0.18                2         32768 ?
 * i  10.5.5.5/32      10.0.0.18                3    100      0 ?
 *>                    10.0.0.9                 3         32768 ?
 *>   10.6.6.6/32      188.24.0.2               0             0 700 ?
 *>   10.7.7.7/32      188.24.0.2               2             0 700 ?
 *>   10.8.8.8/32      188.24.0.2               2             0 700 ?
 *    13.201.255.4/30  188.24.0.2                             0 700 i
 *>i                   10.0.0.18                0    100      0 i
 *>   172.16.0.0/30    188.24.0.2               0             0 700 ?
 *>   172.16.0.4/30    188.24.0.2               0             0 700 ?
 *>   172.16.0.8/30    188.24.0.2               2             0 700 ?
 *    188.24.0.0/30    188.24.0.2               0             0 700 i
 *>                    0.0.0.0                  0         32768 i

</pre>

#### 2.  Роутер R4

<pre>
R4#sh ip bgp
BGP table version is 85, local router ID is 10.4.4.4
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.0.0.0/30      10.0.0.13                3         32768 ?
 * i                   10.0.0.17                2    100  32768 ?
 *>   10.0.0.4/30      10.0.0.13                2         32768 ?
 * i                   10.0.0.17                3    100  32768 ?
 *>   10.0.0.8/30      10.0.0.13                4         32768 ?
 * i                   10.0.0.17                0    100  32768 ?
 *>   10.0.0.12/30     0.0.0.0                  0         32768 ?
 * i                   10.0.0.17                4    100  32768 ?
 *>   10.0.0.16/30     0.0.0.0                  0         32768 ?
 * i                   10.0.0.17                0    100  32768 ?
 *>   10.1.1.1/32      10.0.0.13                4         32768 ?
 * i                   10.0.0.17                2    100  32768 ?
 *>   10.2.2.2/32      10.0.0.13                2         32768 ?
     Network          Next Hop            Metric LocPrf Weight Path
 * i                   10.0.0.17                4    100  32768 ?
 *>   10.3.3.3/32      10.0.0.17                2         32768 ?
 * i                   10.0.0.17                0    100  32768 ?
 * i  10.4.4.4/32      10.0.0.17                2    100  32768 ?
 *>                    0.0.0.0                  0         32768 ?
 *>   10.5.5.5/32      10.0.0.13                3         32768 ?
 * i                   10.0.0.17                3    100  32768 ?
 *>i  10.6.6.6/32      10.0.0.17                0    100  32768 700 ?
 *>i  10.7.7.7/32      10.0.0.17                2    100  32768 700 ?
 *>i  10.8.8.8/32      10.0.0.17                2    100  32768 700 ?
 *>   13.201.255.4/30  0.0.0.0                  0         32768 i
 *>i  172.16.0.0/30    10.0.0.17                0    100  32768 700 ?
 *                     13.201.255.6             0             0 700 i
 *>i  172.16.0.4/30    10.0.0.17                0    100  32768 700 ?
 *                     13.201.255.6             2             0 700 i
 *>i  172.16.0.8/30    10.0.0.17                2    100  32768 700 ?
 *                     13.201.255.6             0             0 700 i
 *>i  188.24.0.0/30    10.0.0.17                0    100  32768 i
</pre>

#### 3.  Роутер R6

<pre>
R6#sh ip bgp 13.201.255.4
BGP routing table entry for 13.201.255.4/30, version 73
Paths: (1 available, best #1, table default)
  Advertised to update-groups:
     3         
  Refresh Epoch 12
  Local
    172.16.0.2 from 172.16.0.2 (10.7.7.7)
      Origin IGP, metric 0, localpref 100, weight 32768, valid, internal, best
      rx pathid: 0, tx pathid: 0x0
R6#sh ip bgp
BGP table version is 94, local router ID is 10.6.6.6
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>i  10.0.0.0/30      172.16.0.2               3    100  32768 300 i
 *                     188.24.0.1               2             0 300 i
 *    10.0.0.4/30      188.24.0.1               3             0 300 i
 *>i                   172.16.0.2               2    100  32768 300 i
 *>i  10.0.0.8/30      172.16.0.2               4    100  32768 300 i
 *                     188.24.0.1               0             0 300 i
 *    10.0.0.12/30     188.24.0.1               4             0 300 i
 *>i                   172.16.0.2               0    100  32768 300 i
 *    10.0.0.16/30     188.24.0.1               0             0 300 i
 *>i                   172.16.0.2               0    100  32768 300 i
 *>i  10.1.1.1/32      172.16.0.2               4    100  32768 300 i
 *                     188.24.0.1               2             0 300 i
 *    10.2.2.2/32      188.24.0.1               4             0 300 i
     Network          Next Hop            Metric LocPrf Weight Path
 *>i                   172.16.0.2               2    100  32768 300 i
 *>i  10.3.3.3/32      172.16.0.2               2    100  32768 300 i
 *                     188.24.0.1               0             0 300 i
 *    10.4.4.4/32      188.24.0.1               2             0 300 i
 *>i                   172.16.0.2               0    100  32768 300 i
 *    10.5.5.5/32      188.24.0.1               3             0 300 i
 *>i                   172.16.0.2               3    100  32768 300 i
 * i  10.6.6.6/32      172.16.0.2               2    100  32768 ?
 *>                    0.0.0.0                  0         32768 ?
 * i  10.7.7.7/32      172.16.0.2               0    100  32768 ?
 *>                    172.16.0.2               2         32768 ?
 * i  10.8.8.8/32      172.16.0.2               2    100  32768 ?
 *>                    172.16.0.6               2         32768 ?
 *>i  13.201.255.4/30  172.16.0.2               0    100  32768 i
 * i  172.16.0.0/30    172.16.0.2               0    100  32768 ?
 *>                    0.0.0.0                  0         32768 ?
 * i  172.16.0.4/30    172.16.0.2               2    100  32768 ?
 *>                    0.0.0.0                  0         32768 ?
 * i  172.16.0.8/30    172.16.0.2               0    100  32768 ?
 *>                    172.16.0.6               2         32768 ?
 *>   188.24.0.0/30    0.0.0.0                  0         32768 i
</pre>

#### 4.  Роутер R7

<pre>
R7#sh ip bgp
BGP table version is 72, local router ID is 10.7.7.7
Status codes: s suppressed, d damped, h history, * valid, > best, i - internal, 
              r RIB-failure, S Stale, m multipath, b backup-path, f RT-Filter, 
              x best-external, a additional-path, c RIB-compressed, 
              t secondary path, 
Origin codes: i - IGP, e - EGP, ? - incomplete
RPKI validation codes: V valid, I invalid, N Not found

     Network          Next Hop            Metric LocPrf Weight Path
 *>   10.0.0.0/30      13.201.255.5             3             0 300 i
 *>   10.0.0.4/30      13.201.255.5             2             0 300 i
 *>   10.0.0.8/30      13.201.255.5             4             0 300 i
 *>   10.0.0.12/30     13.201.255.5             0             0 300 i
 *>   10.0.0.16/30     13.201.255.5             0             0 300 i
 *>   10.1.1.1/32      13.201.255.5             4             0 300 i
 *>   10.2.2.2/32      13.201.255.5             2             0 300 i
 *>   10.3.3.3/32      13.201.255.5             2             0 300 i
 *>   10.4.4.4/32      13.201.255.5             0             0 300 i
 *>   10.5.5.5/32      13.201.255.5             3             0 300 i
 *>   10.6.6.6/32      172.16.0.1               2         32768 ?
 * i                   172.16.0.1               0    100      0 ?
 * i  10.7.7.7/32      172.16.0.1               2    100      0 ?
     Network          Next Hop            Metric LocPrf Weight Path
 *>                    0.0.0.0                  0         32768 ?
 *>   10.8.8.8/32      172.16.0.10              2         32768 ?
 * i                   172.16.0.1               2    100      0 ?
 *>   13.201.255.4/30  0.0.0.0                  0         32768 i
 *>   172.16.0.0/30    0.0.0.0                  0         32768 ?
 * i                   172.16.0.1               0    100      0 ?
 *>   172.16.0.4/30    172.16.0.10              2         32768 ?
 * i                   172.16.0.1               0    100      0 ?
 *>   172.16.0.8/30    0.0.0.0                  0         32768 ?
 * i                   172.16.0.1               2    100      0 ?
 *>i  188.24.0.0/30    172.16.0.1               0    100      0 i
</pre>
