# Metro Valparaíso - Migración a EtherChannel

Versión 4 - 9 de octubre de 2026. Estado validado del proyecto Metro_Valparaiso_Sandbox.

## Resultado

Las 21 estaciones forman un anillo enrutado con 21 conexiones dobles. Cada conexión agrupa dos enlaces FastEthernet en un EtherChannel estático (mode on). Hay 42 Port-channel, dos por switch de distribución, y 84 puertos miembros. Se mantienen 66 equipos y 89 enlaces físicos.

Las direcciones de administración SSH, los gateways de usuarios y las IP de los PC se conservan. No se utiliza DHCP. Los equipos mantienen las credenciales anteriores y acceso administrativo SSH.

## Puertos por estación

| Puertos de Distribución | Función |
|---|---|
| Fa1/1 y Fa1/2 | Port-channel1 hacia la estación anterior |
| Fa1/3 y Fa1/4 | Port-channel2 hacia la estación siguiente |
| Fa1/5 | Trunk principal hacia Fa1/15 del switch de acceso |
| Fa1/6 en Puerto | Trunk de respaldo hacia Fa1/14 del switch de acceso |
| Fa1/0 | Reserva apagada |
| Fa0/0 y Fa0/1 | Enlaces originales retirados; interfaces apagadas para eventual reversión |

Anterior y siguiente siguen el orden Puerto, Bellavista, Francia, Barón, Portales, Recreo, Miramar, Viña, Hospital, Chorrillos, El Salto, Valencia, Quilpué, El Sol, Belloto, Las Américas, La Concepción, Villa Alemana, Sargento Aldea, Peñablanca, Limache y cierre a Puerto.

Los enlaces de Puerto y Limache con SW-SERVIDORES, SW-SERVIDORES con R-SALIDA, y los clientes conservan sus conexiones originales.

## VLAN y enrutamiento

Cada estación mantiene VLAN 10 Operaciones, 20 Seguridad electrónica, 30 Medio de pago, 40 WiFi y 99 Administración. Distribución añade dos VLAN de tránsito, una por vecino: siete VLAN creadas por nosotros, además de VLAN 1 por defecto. Acceso conserva las cinco VLAN base, además de VLAN 1.

VLAN 201 a 221 se usan exclusivamente en el Port-channel de la pareja correspondiente. Los puertos físicos son de capa 2, modo access en su VLAN de tránsito, sin IP. La IP /30 está en la SVI Vlan correspondiente. OSPF área 0 usa las SVI como redes punto a punto, coste 1, sin modo passive.

Los trunks de Distribución hacia Acceso excluyen explícitamente VLAN 201-221. Permiten 1-200 y 222-4094; las VLAN activas del laboratorio son 1,10,20,30,40,99. Esta configuración real no equivale a una lista permitida limitada únicamente a las cinco VLAN base.

STP permanece activo. En cada VLAN de tránsito la estación anterior tiene prioridad 4096 y la siguiente 32768; el único puerto de tránsito en STP es el Port-channel. En Puerto el trunk de acceso principal tiene coste STP 19 y el respaldo 100; se conserva la prueba de redundancia de acceso.

## Direcciones de tránsito

| VLAN | Estación anterior: Po2 / SVI | IP /30 | Estación siguiente: Po1 / SVI | IP /30 |
|---|---|---|---|---|
| 201 | SW-PUERTO | 10.253.0.1 | SW-BELLAVISTA | 10.253.0.2 |
| 202 | SW-BELLAVISTA | 10.253.0.5 | SW-FRANCIA | 10.253.0.6 |
| 203 | SW-FRANCIA | 10.253.0.9 | SW-BARON | 10.253.0.10 |
| 204 | SW-BARON | 10.253.0.13 | SW-PORTALES | 10.253.0.14 |
| 205 | SW-PORTALES | 10.253.0.17 | SW-RECREO | 10.253.0.18 |
| 206 | SW-RECREO | 10.253.0.21 | SW-MIRAMAR | 10.253.0.22 |
| 207 | SW-MIRAMAR | 10.253.0.25 | SW-VINA | 10.253.0.26 |
| 208 | SW-VINA | 10.253.0.29 | SW-HOSPITAL | 10.253.0.30 |
| 209 | SW-HOSPITAL | 10.253.0.33 | SW-CHORRILLOS | 10.253.0.34 |
| 210 | SW-CHORRILLOS | 10.253.0.37 | SW-EL-SALTO | 10.253.0.38 |
| 211 | SW-EL-SALTO | 10.253.0.41 | SW-VALENCIA | 10.253.0.42 |
| 212 | SW-VALENCIA | 10.253.0.45 | SW-QUILPUE | 10.253.0.46 |
| 213 | SW-QUILPUE | 10.253.0.49 | SW-EL-SOL | 10.253.0.50 |
| 214 | SW-EL-SOL | 10.253.0.53 | SW-BELLOTO | 10.253.0.54 |
| 215 | SW-BELLOTO | 10.253.0.57 | SW-LAS-AMERICAS | 10.253.0.58 |
| 216 | SW-LAS-AMERICAS | 10.253.0.61 | SW-LA-CONCEPCION | 10.253.0.62 |
| 217 | SW-LA-CONCEPCION | 10.253.0.65 | SW-VILLA-ALEMANA | 10.253.0.66 |
| 218 | SW-VILLA-ALEMANA | 10.253.0.69 | SW-SARGENTO-ALDEA | 10.253.0.70 |
| 219 | SW-SARGENTO-ALDEA | 10.253.0.73 | SW-PENABLANCA | 10.253.0.74 |
| 220 | SW-PENABLANCA | 10.253.0.77 | SW-LIMACHE | 10.253.0.78 |
| 221 | SW-LIMACHE | 10.253.0.81 | SW-PUERTO | 10.253.0.82 |

## Validación realizada

- Las 21 estaciones tienen Po1 y Po2 en SU, con cuatro miembros P por equipo.
- Hay 42 adyacencias OSPF FULL sobre las nuevas SVI, dos por estación.
- Los 22 PC comprobaron comunicación remota: PC de estaciones hacia 10.100.50.10; PC-SERVIDORES hacia 10.1.10.10.
- Se probaron fallos de un miembro y del grupo completo en Puerto-Bellavista, Quilpué-El Sol y Limache-Puerto, con los enlaces antiguos apagados.
- Con un miembro apagado el EtherChannel sigue funcionando; con los dos apagados OSPF utiliza otra ruta disponible.
- Todos los miembros quedaron habilitados después de las pruebas y las configuraciones fueron guardadas en startup-config.
- Tras exportar y reiniciar los 66 equipos, se verificaron de nuevo los 42 grupos, los 22 PC y las 44 IP de administración desde SW-SERVIDORES. Esta última prueba comprueba conectividad IP; no constituye una nueva sesión SSH autenticada.

La prueba simula fallos mediante shutdown en ambos extremos del cable. No mide duración exacta de interrupción ni rendimiento agregado. Puerto y Limache también tienen un recorrido alternativo mediante SW-SERVIDORES; no se garantiza que todo desvío utilice exclusivamente las otras 20 conexiones del anillo.

## Consideraciones para el bot

Registrar tanto Port-channel como sus interfaces físicas. Dos miembros activos indican estado normal; uno indica degradación; cero indica pérdida del grupo. Usar show etherchannel summary, show ip ospf neighbor, show ip interface brief, show interfaces trunk, show spanning-tree vlan y show cdp neighbors detail para contrastar la configuración y el estado observado.

CDP puede mostrar interfaces físicas y lógicas: deduplicar con la pertenencia real al EtherChannel. Este IOS solo soporta agrupación estática mode on; no se usa negociación LACP. Los switches NM-16ESW no soportan port-security sticky ni BPDU Guard por interfaz en esta imagen; no se han habilitado.

Los antiguos /30 10.254.0.x siguen configurados en interfaces Fa0/0-Fa0/1 apagadas, con coste OSPF 100, para reversión. Sus cables ya no forman parte del proyecto y esas subredes no deben considerarse enlaces operativos.

## Evidencias

EtherChannel_anillo_validacion.json contiene comprobaciones de grupos, interfaces, trunks, OSPF, CDP, pings de clientes y pruebas de fallo. Configuraciones completas con credenciales y claves permanecen en el respaldo local, separado de los documentos públicos.
