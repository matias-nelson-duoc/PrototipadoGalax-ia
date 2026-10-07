# Validación de Distribución y Acceso

66 nodos, 67 enlaces; 21 estaciones con un equipo de cada capa. Todos los clientes tienen IP estática.

## Estaciones

| Estación | Trunks y VLAN | OSPF FULL en Distribución | Cliente: gateway, servidores y gestión |
|---|---|---|---|
| PUERTO | Correctos | 3 | Correcto |
| BELLAVISTA | Correctos | 2 | Correcto |
| FRANCIA | Correctos | 2 | Correcto |
| BARON | Correctos | 2 | Correcto |
| PORTALES | Correctos | 2 | Correcto |
| RECREO | Correctos | 2 | Correcto |
| MIRAMAR | Correctos | 2 | Correcto |
| VINA | Correctos | 2 | Correcto |
| HOSPITAL | Correctos | 2 | Correcto |
| CHORRILLOS | Correctos | 2 | Correcto |
| EL-SALTO | Correctos | 2 | Correcto |
| VALENCIA | Correctos | 2 | Correcto |
| QUILPUE | Correctos | 2 | Correcto |
| EL-SOL | Correctos | 2 | Correcto |
| BELLOTO | Correctos | 2 | Correcto |
| LAS-AMERICAS | Correctos | 2 | Correcto |
| LA-CONCEPCION | Correctos | 2 | Correcto |
| VILLA-ALEMANA | Correctos | 2 | Correcto |
| SARGENTO-ALDEA | Correctos | 2 | Correcto |
| PENABLANCA | Correctos | 2 | Correcto |
| LIMACHE | Correctos | 3 | Correcto |

## SSH después del reinicio

| Equipo | IP SSH | Privilegio | Resultado |
|---|---|---|---|
| R-SALIDA | 10.255.0.254 | 15 | Login y modo de configuración correctos |
| SW-ACC-BARON | 10.4.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-BELLAVISTA | 10.2.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-BELLOTO | 10.15.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-CHORRILLOS | 10.10.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-EL-SALTO | 10.11.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-EL-SOL | 10.14.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-FRANCIA | 10.3.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-HOSPITAL | 10.9.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-LA-CONCEPCION | 10.17.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-LAS-AMERICAS | 10.16.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-LIMACHE | 10.21.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-MIRAMAR | 10.7.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-PENABLANCA | 10.20.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-PORTALES | 10.5.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-PUERTO | 10.1.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-QUILPUE | 10.13.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-RECREO | 10.6.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-SARGENTO-ALDEA | 10.19.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-VALENCIA | 10.12.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-VILLA-ALEMANA | 10.18.99.2 | 15 | Login y modo de configuración correctos |
| SW-ACC-VINA | 10.8.99.2 | 15 | Login y modo de configuración correctos |
| SW-BARON | 10.255.0.4 | 15 | Login y modo de configuración correctos |
| SW-BELLAVISTA | 10.255.0.2 | 15 | Login y modo de configuración correctos |
| SW-BELLOTO | 10.255.0.15 | 15 | Login y modo de configuración correctos |
| SW-CHORRILLOS | 10.255.0.10 | 15 | Login y modo de configuración correctos |
| SW-EL-SALTO | 10.255.0.11 | 15 | Login y modo de configuración correctos |
| SW-EL-SOL | 10.255.0.14 | 15 | Login y modo de configuración correctos |
| SW-FRANCIA | 10.255.0.3 | 15 | Login y modo de configuración correctos |
| SW-HOSPITAL | 10.255.0.9 | 15 | Login y modo de configuración correctos |
| SW-LA-CONCEPCION | 10.255.0.17 | 15 | Login y modo de configuración correctos |
| SW-LAS-AMERICAS | 10.255.0.16 | 15 | Login y modo de configuración correctos |
| SW-LIMACHE | 10.255.0.21 | 15 | Login y modo de configuración correctos |
| SW-MIRAMAR | 10.255.0.7 | 15 | Login y modo de configuración correctos |
| SW-PENABLANCA | 10.255.0.20 | 15 | Login y modo de configuración correctos |
| SW-PORTALES | 10.255.0.5 | 15 | Login y modo de configuración correctos |
| SW-PUERTO | 10.255.0.1 | 15 | Login y modo de configuración correctos |
| SW-QUILPUE | 10.255.0.13 | 15 | Login y modo de configuración correctos |
| SW-RECREO | 10.255.0.6 | 15 | Login y modo de configuración correctos |
| SW-SARGENTO-ALDEA | 10.255.0.19 | 15 | Login y modo de configuración correctos |
| SW-SERVIDORES | 10.255.0.100 | 15 | Login y modo de configuración correctos |
| SW-VALENCIA | 10.255.0.12 | 15 | Login y modo de configuración correctos |
| SW-VILLA-ALEMANA | 10.255.0.18 | 15 | Login y modo de configuración correctos |
| SW-VINA | 10.255.0.8 | 15 | Login y modo de configuración correctos |

## Piloto de cuatro VLAN

Puerto pasó VLAN 10, 20, 30 y 40 desde un cliente conectado a Acceso. Se restauró la IP estática original de Operaciones.

## Caída y recuperación de enlace

El PC de Bellavista detrás de Acceso mantuvo alcance a Servidores mediante Francia al apagar Puerto Fa0/1. Se restauró el enlace y la ruta original.

### Ruta inicial

```text
show ip route 10.100.50.0
Routing entry for 10.100.50.0/24
  Known via "ospf 1", distance 110, metric 12, type intra area
  Last update from 10.254.0.1 on FastEthernet0/0, 00:20:17 ago
  Routing Descriptor Blocks:
  * 10.254.0.1, from 10.255.0.100, 00:20:17 ago, via FastEthernet0/0
      Route metric is 12, traffic share count is 1

SW-BELLAVISTA#
```

### Ruta alternativa

```text
show ip route 10.100.50.0
Routing entry for 10.100.50.0/24
  Known via "ospf 1", distance 110, metric 192, type intra area
  Last update from 10.254.0.6 on FastEthernet0/1, 00:00:05 ago
  Routing Descriptor Blocks:
  * 10.254.0.6, from 10.255.0.100, 00:00:05 ago, via FastEthernet0/1
      Route metric is 192, traffic share count is 1

SW-BELLAVISTA#
```

### Ping durante la falla

```text
ping 10.100.50.10 -c 3 -w 3000
84 bytes from 10.100.50.10 icmp_seq=1 ttl=43 time=661.107 ms
84 bytes from 10.100.50.10 icmp_seq=2 ttl=43 time=618.116 ms
84 bytes from 10.100.50.10 icmp_seq=3 ttl=43 time=617.697 ms

VPCS>
```

### Ruta restaurada

```text
show ip route 10.100.50.0
Routing entry for 10.100.50.0/24
  Known via "ospf 1", distance 110, metric 12, type intra area
  Last update from 10.254.0.1 on FastEthernet0/0, 00:00:00 ago
  Routing Descriptor Blocks:
  * 10.254.0.1, from 10.255.0.100, 00:00:00 ago, via FastEthernet0/0
      Route metric is 12, traffic share count is 1

SW-BELLAVISTA#
```
