## Практическое задание 7

### Задвание

1. В AS 700 сделать редистрибуцию BGP в OSPF и OSPF в BGP.<br>
Убедиться, что префиксы из BGP добавились в OSPF домен, а также исключена возможность закольцовки трафика.
2. Настроить iBGP в ASN 300. Префиксы из BGP должны находиться на всех маршрутизаторах в ASN 300.
3. Трафик из ASN 300 в ASN 700 должен проходить по стыку R3-R6.
4. Трафик из ASN 700 в ASN 300 должен проходить по стыку R7-R4.
5. Убедиться в этом сделав трассировку от R5 до лупбека роутера R8 и трассировку о R8 до лупбека роутера R5.
