# Metro Valparaíso — Sandbox GNS3

## Objetivo y alcance

Proyecto universitario de prototipado: emular una red para un bot de análisis reactivo y proactivo y un dashboard. Matías prepara la red; Cristopher aloja la infraestructura definitiva; Alex desarrolla el bot y la automatización; César desarrolla el frontend.

Este sandbox comprende la topología, configuración de capa 2 y 3, tráfico de prueba, validación y material transferible. Ubuntu, bot, dashboard y conexión real a Internet quedan fuera de esta implementación local. Se habilita SSH para la administración futura del bot.

## Referencia y simplificación

Referencia: E154-ID-VAL-100-PLA-CD-011-0.PDF, topología proyectada con estación Valencia. Se incluyen las 21 estaciones. El plano contiene enlaces paralelos, ramificaciones y equipos de distintos fabricantes; el sandbox usa un anillo lógico simplificado con Cisco. Limache–Puerto representa el cierre lógico del laboratorio. Las direcciones impresas en el plano son datos de referencia; el laboratorio utiliza el direccionamiento propio documentado a continuación.

## Implementación construida

Proyecto: Metro_Valparaiso_Sandbox.

- 21 C3725 de Distribución, uno por estación.
- 21 C3725 de Acceso, uno por estación.
- 1 C3725 para SW-SERVIDORES.
- 1 C7200 para R-SALIDA.
- 21 VPCS, uno por estación, más un VPCS de servidores.
- Total: 66 nodos y 67 enlaces Ethernet.

Los C3725 son routers con módulo de switching, usados para representar equipos de estación: GT96100-FE en slot 0, NM-16ESW en slot 1 y NM-1FE-TX en slot 2; RAM 256 MB e Idle-PC 0x60c086a8. C7200: NPE-400, RAM 512 MB, C7200-IO-FE y PA-2FE-TX, Idle-PC 0x60189214.

Distribución conserva los gateways, OSPF y el anillo sobre Fa0/0 y Fa0/1. Fa1/0 enlaza con Fa1/15 de Acceso mediante un trunk 802.1Q que permite las VLAN 10, 20, 30, 40 y 99. Acceso conecta los clientes, tiene el enrutamiento deshabilitado y usa una IP estática en VLAN 99 para SSH. Los puertos Fa1/1-15 de Distribución quedan apagados como reserva.

## VLAN y direccionamiento

Todas las direcciones son estáticas, siguiendo el requisito del cliente. No se usan servidores ni relay DHCP. Cada estación tiene cuatro VLAN de usuarios y una VLAN de gestión, con una red /24 por VLAN:

| VLAN | Uso | Red por estación | Gateway |
|---|---|---|---|
| 10 | Operaciones | 10.ID.10.0/24 | 10.ID.10.1 |
| 20 | Seguridad electrónica | 10.ID.20.0/24 | 10.ID.20.1 |
| 30 | Medio de pago | 10.ID.30.0/24 | 10.ID.30.1 |
| 40 | WiFi | 10.ID.40.0/24 | 10.ID.40.1 |
| 99 | Gestión | 10.ID.99.0/24 | 10.ID.99.1 |

ID representa el número de estación: Puerto 1, Bellavista 2, Francia 3, hasta Limache 21. Los VPCS usan 10.ID.10.10/24 en Operaciones. En Acceso: Fa1/0-3 para VLAN 10; Fa1/4-7 para VLAN 20; Fa1/8-11 para VLAN 30; Fa1/12-14 para VLAN 40; Fa1/15 es el trunk. Distribución usa 10.ID.99.1 y Acceso 10.ID.99.2 en gestión. Los PC existentes se conservaron con sus IP y gateways originales.

La sala de servidores usa VLAN 50 con 10.100.50.0/24 y gateway 10.100.50.1. Los enlaces de tránsito usan subredes /30 de 10.254.0.0/24; las loopbacks, 10.255.0.ID/32. El documento de direccionamiento contiene todas las asignaciones.

## Inventario de equipos para el bot

Inventario de los 66 equipos existentes. En los 44 equipos Cisco, la dirección indicada es el destino recomendado para SSH y para ansible_host. Distribución, Servidores y R-SALIDA usan Loopback0; Acceso usa Vlan99. Los VPCS tienen IP para pruebas de conectividad y no ofrecen SSH ni ejecutan playbooks Cisco.

SSH usa TCP 22, usuario botlab y privilegio de administrador 15. La contraseña se entrega por separado en Credenciales_SSH_LAB.txt. Los puertos de consola de GNS3 no son puertos SSH. Todas las IP son estáticas.

| Equipo | Rol | IP para SSH / pruebas | SSH / puerto |
|---|---|---|---|
| R-SALIDA | Router | 10.255.0.254 | Sí / 22 |
| SW-ACC-BARON | Acceso | 10.4.99.2 | Sí / 22 |
| SW-ACC-BELLAVISTA | Acceso | 10.2.99.2 | Sí / 22 |
| SW-ACC-BELLOTO | Acceso | 10.15.99.2 | Sí / 22 |
| SW-ACC-CHORRILLOS | Acceso | 10.10.99.2 | Sí / 22 |
| SW-ACC-EL-SALTO | Acceso | 10.11.99.2 | Sí / 22 |
| SW-ACC-EL-SOL | Acceso | 10.14.99.2 | Sí / 22 |
| SW-ACC-FRANCIA | Acceso | 10.3.99.2 | Sí / 22 |
| SW-ACC-HOSPITAL | Acceso | 10.9.99.2 | Sí / 22 |
| SW-ACC-LA-CONCEPCION | Acceso | 10.17.99.2 | Sí / 22 |
| SW-ACC-LAS-AMERICAS | Acceso | 10.16.99.2 | Sí / 22 |
| SW-ACC-LIMACHE | Acceso | 10.21.99.2 | Sí / 22 |
| SW-ACC-MIRAMAR | Acceso | 10.7.99.2 | Sí / 22 |
| SW-ACC-PENABLANCA | Acceso | 10.20.99.2 | Sí / 22 |
| SW-ACC-PORTALES | Acceso | 10.5.99.2 | Sí / 22 |
| SW-ACC-PUERTO | Acceso | 10.1.99.2 | Sí / 22 |
| SW-ACC-QUILPUE | Acceso | 10.13.99.2 | Sí / 22 |
| SW-ACC-RECREO | Acceso | 10.6.99.2 | Sí / 22 |
| SW-ACC-SARGENTO-ALDEA | Acceso | 10.19.99.2 | Sí / 22 |
| SW-ACC-VALENCIA | Acceso | 10.12.99.2 | Sí / 22 |
| SW-ACC-VILLA-ALEMANA | Acceso | 10.18.99.2 | Sí / 22 |
| SW-ACC-VINA | Acceso | 10.8.99.2 | Sí / 22 |
| SW-BARON | Distribución | 10.255.0.4 | Sí / 22 |
| SW-BELLAVISTA | Distribución | 10.255.0.2 | Sí / 22 |
| SW-BELLOTO | Distribución | 10.255.0.15 | Sí / 22 |
| SW-CHORRILLOS | Distribución | 10.255.0.10 | Sí / 22 |
| SW-EL-SALTO | Distribución | 10.255.0.11 | Sí / 22 |
| SW-EL-SOL | Distribución | 10.255.0.14 | Sí / 22 |
| SW-FRANCIA | Distribución | 10.255.0.3 | Sí / 22 |
| SW-HOSPITAL | Distribución | 10.255.0.9 | Sí / 22 |
| SW-LA-CONCEPCION | Distribución | 10.255.0.17 | Sí / 22 |
| SW-LAS-AMERICAS | Distribución | 10.255.0.16 | Sí / 22 |
| SW-LIMACHE | Distribución | 10.255.0.21 | Sí / 22 |
| SW-MIRAMAR | Distribución | 10.255.0.7 | Sí / 22 |
| SW-PENABLANCA | Distribución | 10.255.0.20 | Sí / 22 |
| SW-PORTALES | Distribución | 10.255.0.5 | Sí / 22 |
| SW-PUERTO | Distribución | 10.255.0.1 | Sí / 22 |
| SW-QUILPUE | Distribución | 10.255.0.13 | Sí / 22 |
| SW-RECREO | Distribución | 10.255.0.6 | Sí / 22 |
| SW-SARGENTO-ALDEA | Distribución | 10.255.0.19 | Sí / 22 |
| SW-SERVIDORES | Servidores | 10.255.0.100 | Sí / 22 |
| SW-VALENCIA | Distribución | 10.255.0.12 | Sí / 22 |
| SW-VILLA-ALEMANA | Distribución | 10.255.0.18 | Sí / 22 |
| SW-VINA | Distribución | 10.255.0.8 | Sí / 22 |
| PC-BARON | VPCS | 10.4.10.10 | No |
| PC-BELLAVISTA | VPCS | 10.2.10.10 | No |
| PC-BELLOTO | VPCS | 10.15.10.10 | No |
| PC-CHORRILLOS | VPCS | 10.10.10.10 | No |
| PC-EL-SALTO | VPCS | 10.11.10.10 | No |
| PC-EL-SOL | VPCS | 10.14.10.10 | No |
| PC-FRANCIA | VPCS | 10.3.10.10 | No |
| PC-HOSPITAL | VPCS | 10.9.10.10 | No |
| PC-LA-CONCEPCION | VPCS | 10.17.10.10 | No |
| PC-LAS-AMERICAS | VPCS | 10.16.10.10 | No |
| PC-LIMACHE | VPCS | 10.21.10.10 | No |
| PC-MIRAMAR | VPCS | 10.7.10.10 | No |
| PC-PENABLANCA | VPCS | 10.20.10.10 | No |
| PC-PORTALES | VPCS | 10.5.10.10 | No |
| PC-PUERTO | VPCS | 10.1.10.10 | No |
| PC-QUILPUE | VPCS | 10.13.10.10 | No |
| PC-RECREO | VPCS | 10.6.10.10 | No |
| PC-SARGENTO-ALDEA | VPCS | 10.19.10.10 | No |
| PC-SERVIDORES | VPCS | 10.100.50.10 | No |
| PC-VALENCIA | VPCS | 10.12.10.10 | No |
| PC-VILLA-ALEMANA | VPCS | 10.18.10.10 | No |
| PC-VINA | VPCS | 10.8.10.10 | No |

El inventario permite preparar los playbooks. Para ejecutarlos desde el PC de Alex o Cristopher deberá existir una conexión y rutas hacia la red virtual de GNS3; esa conexión todavía no está configurada. La compatibilidad SSH de la biblioteca del bot con estos IOS debe verificarse al integrarla.

La IP 10.100.50.20/24, gateway 10.100.50.1, es una propuesta para la futura VM del bot. No corresponde a un equipo instalado y no se incluye como equipo activo.

## Detalle de todas las IP configuradas

Direcciones extraídas de las configuraciones de arranque guardadas y de los archivos de los VPCS. Incluye loopbacks /32, enlaces de tránsito /30 y VLAN /24. Gateway identifica las SVI de Distribución o Servidores; en VPCS se indica su gateway predeterminado. Las interfaces sin IP, incluidos los trunks, no aparecen.

| Equipo | Interfaz | IP / prefijo | Uso |
|---|---|---|---|
| R-SALIDA | Loopback0 | 10.255.0.254/32 | SSH |
| R-SALIDA | FastEthernet0/0 | 10.254.0.94/30 | Tránsito |
| SW-ACC-BARON | Vlan99 | 10.4.99.2/24 | SSH |
| SW-ACC-BELLAVISTA | Vlan99 | 10.2.99.2/24 | SSH |
| SW-ACC-BELLOTO | Vlan99 | 10.15.99.2/24 | SSH |
| SW-ACC-CHORRILLOS | Vlan99 | 10.10.99.2/24 | SSH |
| SW-ACC-EL-SALTO | Vlan99 | 10.11.99.2/24 | SSH |
| SW-ACC-EL-SOL | Vlan99 | 10.14.99.2/24 | SSH |
| SW-ACC-FRANCIA | Vlan99 | 10.3.99.2/24 | SSH |
| SW-ACC-HOSPITAL | Vlan99 | 10.9.99.2/24 | SSH |
| SW-ACC-LA-CONCEPCION | Vlan99 | 10.17.99.2/24 | SSH |
| SW-ACC-LAS-AMERICAS | Vlan99 | 10.16.99.2/24 | SSH |
| SW-ACC-LIMACHE | Vlan99 | 10.21.99.2/24 | SSH |
| SW-ACC-MIRAMAR | Vlan99 | 10.7.99.2/24 | SSH |
| SW-ACC-PENABLANCA | Vlan99 | 10.20.99.2/24 | SSH |
| SW-ACC-PORTALES | Vlan99 | 10.5.99.2/24 | SSH |
| SW-ACC-PUERTO | Vlan99 | 10.1.99.2/24 | SSH |
| SW-ACC-QUILPUE | Vlan99 | 10.13.99.2/24 | SSH |
| SW-ACC-RECREO | Vlan99 | 10.6.99.2/24 | SSH |
| SW-ACC-SARGENTO-ALDEA | Vlan99 | 10.19.99.2/24 | SSH |
| SW-ACC-VALENCIA | Vlan99 | 10.12.99.2/24 | SSH |
| SW-ACC-VILLA-ALEMANA | Vlan99 | 10.18.99.2/24 | SSH |
| SW-ACC-VINA | Vlan99 | 10.8.99.2/24 | SSH |
| SW-BARON | Loopback0 | 10.255.0.4/32 | SSH |
| SW-BARON | FastEthernet0/0 | 10.254.0.10/30 | Tránsito |
| SW-BARON | FastEthernet0/1 | 10.254.0.13/30 | Tránsito |
| SW-BARON | Vlan10 | 10.4.10.1/24 | Gateway |
| SW-BARON | Vlan20 | 10.4.20.1/24 | Gateway |
| SW-BARON | Vlan30 | 10.4.30.1/24 | Gateway |
| SW-BARON | Vlan40 | 10.4.40.1/24 | Gateway |
| SW-BARON | Vlan99 | 10.4.99.1/24 | Gateway |
| SW-BELLAVISTA | Loopback0 | 10.255.0.2/32 | SSH |
| SW-BELLAVISTA | FastEthernet0/0 | 10.254.0.2/30 | Tránsito |
| SW-BELLAVISTA | FastEthernet0/1 | 10.254.0.5/30 | Tránsito |
| SW-BELLAVISTA | Vlan10 | 10.2.10.1/24 | Gateway |
| SW-BELLAVISTA | Vlan20 | 10.2.20.1/24 | Gateway |
| SW-BELLAVISTA | Vlan30 | 10.2.30.1/24 | Gateway |
| SW-BELLAVISTA | Vlan40 | 10.2.40.1/24 | Gateway |
| SW-BELLAVISTA | Vlan99 | 10.2.99.1/24 | Gateway |
| SW-BELLOTO | Loopback0 | 10.255.0.15/32 | SSH |
| SW-BELLOTO | FastEthernet0/0 | 10.254.0.54/30 | Tránsito |
| SW-BELLOTO | FastEthernet0/1 | 10.254.0.57/30 | Tránsito |
| SW-BELLOTO | Vlan10 | 10.15.10.1/24 | Gateway |
| SW-BELLOTO | Vlan20 | 10.15.20.1/24 | Gateway |
| SW-BELLOTO | Vlan30 | 10.15.30.1/24 | Gateway |
| SW-BELLOTO | Vlan40 | 10.15.40.1/24 | Gateway |
| SW-BELLOTO | Vlan99 | 10.15.99.1/24 | Gateway |
| SW-CHORRILLOS | Loopback0 | 10.255.0.10/32 | SSH |
| SW-CHORRILLOS | FastEthernet0/0 | 10.254.0.34/30 | Tránsito |
| SW-CHORRILLOS | FastEthernet0/1 | 10.254.0.37/30 | Tránsito |
| SW-CHORRILLOS | Vlan10 | 10.10.10.1/24 | Gateway |
| SW-CHORRILLOS | Vlan20 | 10.10.20.1/24 | Gateway |
| SW-CHORRILLOS | Vlan30 | 10.10.30.1/24 | Gateway |
| SW-CHORRILLOS | Vlan40 | 10.10.40.1/24 | Gateway |
| SW-CHORRILLOS | Vlan99 | 10.10.99.1/24 | Gateway |
| SW-EL-SALTO | Loopback0 | 10.255.0.11/32 | SSH |
| SW-EL-SALTO | FastEthernet0/0 | 10.254.0.38/30 | Tránsito |
| SW-EL-SALTO | FastEthernet0/1 | 10.254.0.41/30 | Tránsito |
| SW-EL-SALTO | Vlan10 | 10.11.10.1/24 | Gateway |
| SW-EL-SALTO | Vlan20 | 10.11.20.1/24 | Gateway |
| SW-EL-SALTO | Vlan30 | 10.11.30.1/24 | Gateway |
| SW-EL-SALTO | Vlan40 | 10.11.40.1/24 | Gateway |
| SW-EL-SALTO | Vlan99 | 10.11.99.1/24 | Gateway |
| SW-EL-SOL | Loopback0 | 10.255.0.14/32 | SSH |
| SW-EL-SOL | FastEthernet0/0 | 10.254.0.50/30 | Tránsito |
| SW-EL-SOL | FastEthernet0/1 | 10.254.0.53/30 | Tránsito |
| SW-EL-SOL | Vlan10 | 10.14.10.1/24 | Gateway |
| SW-EL-SOL | Vlan20 | 10.14.20.1/24 | Gateway |
| SW-EL-SOL | Vlan30 | 10.14.30.1/24 | Gateway |
| SW-EL-SOL | Vlan40 | 10.14.40.1/24 | Gateway |
| SW-EL-SOL | Vlan99 | 10.14.99.1/24 | Gateway |
| SW-FRANCIA | Loopback0 | 10.255.0.3/32 | SSH |
| SW-FRANCIA | FastEthernet0/0 | 10.254.0.6/30 | Tránsito |
| SW-FRANCIA | FastEthernet0/1 | 10.254.0.9/30 | Tránsito |
| SW-FRANCIA | Vlan10 | 10.3.10.1/24 | Gateway |
| SW-FRANCIA | Vlan20 | 10.3.20.1/24 | Gateway |
| SW-FRANCIA | Vlan30 | 10.3.30.1/24 | Gateway |
| SW-FRANCIA | Vlan40 | 10.3.40.1/24 | Gateway |
| SW-FRANCIA | Vlan99 | 10.3.99.1/24 | Gateway |
| SW-HOSPITAL | Loopback0 | 10.255.0.9/32 | SSH |
| SW-HOSPITAL | FastEthernet0/0 | 10.254.0.30/30 | Tránsito |
| SW-HOSPITAL | FastEthernet0/1 | 10.254.0.33/30 | Tránsito |
| SW-HOSPITAL | Vlan10 | 10.9.10.1/24 | Gateway |
| SW-HOSPITAL | Vlan20 | 10.9.20.1/24 | Gateway |
| SW-HOSPITAL | Vlan30 | 10.9.30.1/24 | Gateway |
| SW-HOSPITAL | Vlan40 | 10.9.40.1/24 | Gateway |
| SW-HOSPITAL | Vlan99 | 10.9.99.1/24 | Gateway |
| SW-LA-CONCEPCION | Loopback0 | 10.255.0.17/32 | SSH |
| SW-LA-CONCEPCION | FastEthernet0/0 | 10.254.0.62/30 | Tránsito |
| SW-LA-CONCEPCION | FastEthernet0/1 | 10.254.0.65/30 | Tránsito |
| SW-LA-CONCEPCION | Vlan10 | 10.17.10.1/24 | Gateway |
| SW-LA-CONCEPCION | Vlan20 | 10.17.20.1/24 | Gateway |
| SW-LA-CONCEPCION | Vlan30 | 10.17.30.1/24 | Gateway |
| SW-LA-CONCEPCION | Vlan40 | 10.17.40.1/24 | Gateway |
| SW-LA-CONCEPCION | Vlan99 | 10.17.99.1/24 | Gateway |
| SW-LAS-AMERICAS | Loopback0 | 10.255.0.16/32 | SSH |
| SW-LAS-AMERICAS | FastEthernet0/0 | 10.254.0.58/30 | Tránsito |
| SW-LAS-AMERICAS | FastEthernet0/1 | 10.254.0.61/30 | Tránsito |
| SW-LAS-AMERICAS | Vlan10 | 10.16.10.1/24 | Gateway |
| SW-LAS-AMERICAS | Vlan20 | 10.16.20.1/24 | Gateway |
| SW-LAS-AMERICAS | Vlan30 | 10.16.30.1/24 | Gateway |
| SW-LAS-AMERICAS | Vlan40 | 10.16.40.1/24 | Gateway |
| SW-LAS-AMERICAS | Vlan99 | 10.16.99.1/24 | Gateway |
| SW-LIMACHE | Loopback0 | 10.255.0.21/32 | SSH |
| SW-LIMACHE | FastEthernet0/0 | 10.254.0.78/30 | Tránsito |
| SW-LIMACHE | FastEthernet0/1 | 10.254.0.81/30 | Tránsito |
| SW-LIMACHE | FastEthernet2/0 | 10.254.0.89/30 | Tránsito |
| SW-LIMACHE | Vlan10 | 10.21.10.1/24 | Gateway |
| SW-LIMACHE | Vlan20 | 10.21.20.1/24 | Gateway |
| SW-LIMACHE | Vlan30 | 10.21.30.1/24 | Gateway |
| SW-LIMACHE | Vlan40 | 10.21.40.1/24 | Gateway |
| SW-LIMACHE | Vlan99 | 10.21.99.1/24 | Gateway |
| SW-MIRAMAR | Loopback0 | 10.255.0.7/32 | SSH |
| SW-MIRAMAR | FastEthernet0/0 | 10.254.0.22/30 | Tránsito |
| SW-MIRAMAR | FastEthernet0/1 | 10.254.0.25/30 | Tránsito |
| SW-MIRAMAR | Vlan10 | 10.7.10.1/24 | Gateway |
| SW-MIRAMAR | Vlan20 | 10.7.20.1/24 | Gateway |
| SW-MIRAMAR | Vlan30 | 10.7.30.1/24 | Gateway |
| SW-MIRAMAR | Vlan40 | 10.7.40.1/24 | Gateway |
| SW-MIRAMAR | Vlan99 | 10.7.99.1/24 | Gateway |
| SW-PENABLANCA | Loopback0 | 10.255.0.20/32 | SSH |
| SW-PENABLANCA | FastEthernet0/0 | 10.254.0.74/30 | Tránsito |
| SW-PENABLANCA | FastEthernet0/1 | 10.254.0.77/30 | Tránsito |
| SW-PENABLANCA | Vlan10 | 10.20.10.1/24 | Gateway |
| SW-PENABLANCA | Vlan20 | 10.20.20.1/24 | Gateway |
| SW-PENABLANCA | Vlan30 | 10.20.30.1/24 | Gateway |
| SW-PENABLANCA | Vlan40 | 10.20.40.1/24 | Gateway |
| SW-PENABLANCA | Vlan99 | 10.20.99.1/24 | Gateway |
| SW-PORTALES | Loopback0 | 10.255.0.5/32 | SSH |
| SW-PORTALES | FastEthernet0/0 | 10.254.0.14/30 | Tránsito |
| SW-PORTALES | FastEthernet0/1 | 10.254.0.17/30 | Tránsito |
| SW-PORTALES | Vlan10 | 10.5.10.1/24 | Gateway |
| SW-PORTALES | Vlan20 | 10.5.20.1/24 | Gateway |
| SW-PORTALES | Vlan30 | 10.5.30.1/24 | Gateway |
| SW-PORTALES | Vlan40 | 10.5.40.1/24 | Gateway |
| SW-PORTALES | Vlan99 | 10.5.99.1/24 | Gateway |
| SW-PUERTO | Loopback0 | 10.255.0.1/32 | SSH |
| SW-PUERTO | FastEthernet0/0 | 10.254.0.82/30 | Tránsito |
| SW-PUERTO | FastEthernet0/1 | 10.254.0.1/30 | Tránsito |
| SW-PUERTO | FastEthernet2/0 | 10.254.0.85/30 | Tránsito |
| SW-PUERTO | Vlan10 | 10.1.10.1/24 | Gateway |
| SW-PUERTO | Vlan20 | 10.1.20.1/24 | Gateway |
| SW-PUERTO | Vlan30 | 10.1.30.1/24 | Gateway |
| SW-PUERTO | Vlan40 | 10.1.40.1/24 | Gateway |
| SW-PUERTO | Vlan99 | 10.1.99.1/24 | Gateway |
| SW-QUILPUE | Loopback0 | 10.255.0.13/32 | SSH |
| SW-QUILPUE | FastEthernet0/0 | 10.254.0.46/30 | Tránsito |
| SW-QUILPUE | FastEthernet0/1 | 10.254.0.49/30 | Tránsito |
| SW-QUILPUE | Vlan10 | 10.13.10.1/24 | Gateway |
| SW-QUILPUE | Vlan20 | 10.13.20.1/24 | Gateway |
| SW-QUILPUE | Vlan30 | 10.13.30.1/24 | Gateway |
| SW-QUILPUE | Vlan40 | 10.13.40.1/24 | Gateway |
| SW-QUILPUE | Vlan99 | 10.13.99.1/24 | Gateway |
| SW-RECREO | Loopback0 | 10.255.0.6/32 | SSH |
| SW-RECREO | FastEthernet0/0 | 10.254.0.18/30 | Tránsito |
| SW-RECREO | FastEthernet0/1 | 10.254.0.21/30 | Tránsito |
| SW-RECREO | Vlan10 | 10.6.10.1/24 | Gateway |
| SW-RECREO | Vlan20 | 10.6.20.1/24 | Gateway |
| SW-RECREO | Vlan30 | 10.6.30.1/24 | Gateway |
| SW-RECREO | Vlan40 | 10.6.40.1/24 | Gateway |
| SW-RECREO | Vlan99 | 10.6.99.1/24 | Gateway |
| SW-SARGENTO-ALDEA | Loopback0 | 10.255.0.19/32 | SSH |
| SW-SARGENTO-ALDEA | FastEthernet0/0 | 10.254.0.70/30 | Tránsito |
| SW-SARGENTO-ALDEA | FastEthernet0/1 | 10.254.0.73/30 | Tránsito |
| SW-SARGENTO-ALDEA | Vlan10 | 10.19.10.1/24 | Gateway |
| SW-SARGENTO-ALDEA | Vlan20 | 10.19.20.1/24 | Gateway |
| SW-SARGENTO-ALDEA | Vlan30 | 10.19.30.1/24 | Gateway |
| SW-SARGENTO-ALDEA | Vlan40 | 10.19.40.1/24 | Gateway |
| SW-SARGENTO-ALDEA | Vlan99 | 10.19.99.1/24 | Gateway |
| SW-SERVIDORES | Loopback0 | 10.255.0.100/32 | SSH |
| SW-SERVIDORES | FastEthernet0/0 | 10.254.0.86/30 | Tránsito |
| SW-SERVIDORES | FastEthernet0/1 | 10.254.0.90/30 | Tránsito |
| SW-SERVIDORES | FastEthernet2/0 | 10.254.0.93/30 | Tránsito |
| SW-SERVIDORES | Vlan50 | 10.100.50.1/24 | Gateway |
| SW-VALENCIA | Loopback0 | 10.255.0.12/32 | SSH |
| SW-VALENCIA | FastEthernet0/0 | 10.254.0.42/30 | Tránsito |
| SW-VALENCIA | FastEthernet0/1 | 10.254.0.45/30 | Tránsito |
| SW-VALENCIA | Vlan10 | 10.12.10.1/24 | Gateway |
| SW-VALENCIA | Vlan20 | 10.12.20.1/24 | Gateway |
| SW-VALENCIA | Vlan30 | 10.12.30.1/24 | Gateway |
| SW-VALENCIA | Vlan40 | 10.12.40.1/24 | Gateway |
| SW-VALENCIA | Vlan99 | 10.12.99.1/24 | Gateway |
| SW-VILLA-ALEMANA | Loopback0 | 10.255.0.18/32 | SSH |
| SW-VILLA-ALEMANA | FastEthernet0/0 | 10.254.0.66/30 | Tránsito |
| SW-VILLA-ALEMANA | FastEthernet0/1 | 10.254.0.69/30 | Tránsito |
| SW-VILLA-ALEMANA | Vlan10 | 10.18.10.1/24 | Gateway |
| SW-VILLA-ALEMANA | Vlan20 | 10.18.20.1/24 | Gateway |
| SW-VILLA-ALEMANA | Vlan30 | 10.18.30.1/24 | Gateway |
| SW-VILLA-ALEMANA | Vlan40 | 10.18.40.1/24 | Gateway |
| SW-VILLA-ALEMANA | Vlan99 | 10.18.99.1/24 | Gateway |
| SW-VINA | Loopback0 | 10.255.0.8/32 | SSH |
| SW-VINA | FastEthernet0/0 | 10.254.0.26/30 | Tránsito |
| SW-VINA | FastEthernet0/1 | 10.254.0.29/30 | Tránsito |
| SW-VINA | Vlan10 | 10.8.10.1/24 | Gateway |
| SW-VINA | Vlan20 | 10.8.20.1/24 | Gateway |
| SW-VINA | Vlan30 | 10.8.30.1/24 | Gateway |
| SW-VINA | Vlan40 | 10.8.40.1/24 | Gateway |
| SW-VINA | Vlan99 | 10.8.99.1/24 | Gateway |
| PC-BARON | Ethernet0 | 10.4.10.10/24 | GW 10.4.10.1 |
| PC-BELLAVISTA | Ethernet0 | 10.2.10.10/24 | GW 10.2.10.1 |
| PC-BELLOTO | Ethernet0 | 10.15.10.10/24 | GW 10.15.10.1 |
| PC-CHORRILLOS | Ethernet0 | 10.10.10.10/24 | GW 10.10.10.1 |
| PC-EL-SALTO | Ethernet0 | 10.11.10.10/24 | GW 10.11.10.1 |
| PC-EL-SOL | Ethernet0 | 10.14.10.10/24 | GW 10.14.10.1 |
| PC-FRANCIA | Ethernet0 | 10.3.10.10/24 | GW 10.3.10.1 |
| PC-HOSPITAL | Ethernet0 | 10.9.10.10/24 | GW 10.9.10.1 |
| PC-LA-CONCEPCION | Ethernet0 | 10.17.10.10/24 | GW 10.17.10.1 |
| PC-LAS-AMERICAS | Ethernet0 | 10.16.10.10/24 | GW 10.16.10.1 |
| PC-LIMACHE | Ethernet0 | 10.21.10.10/24 | GW 10.21.10.1 |
| PC-MIRAMAR | Ethernet0 | 10.7.10.10/24 | GW 10.7.10.1 |
| PC-PENABLANCA | Ethernet0 | 10.20.10.10/24 | GW 10.20.10.1 |
| PC-PORTALES | Ethernet0 | 10.5.10.10/24 | GW 10.5.10.1 |
| PC-PUERTO | Ethernet0 | 10.1.10.10/24 | GW 10.1.10.1 |
| PC-QUILPUE | Ethernet0 | 10.13.10.10/24 | GW 10.13.10.1 |
| PC-RECREO | Ethernet0 | 10.6.10.10/24 | GW 10.6.10.1 |
| PC-SARGENTO-ALDEA | Ethernet0 | 10.19.10.10/24 | GW 10.19.10.1 |
| PC-SERVIDORES | Ethernet0 | 10.100.50.10/24 | GW 10.100.50.1 |
| PC-VALENCIA | Ethernet0 | 10.12.10.10/24 | GW 10.12.10.1 |
| PC-VILLA-ALEMANA | Ethernet0 | 10.18.10.10/24 | GW 10.18.10.1 |
| PC-VINA | Ethernet0 | 10.8.10.10/24 | GW 10.8.10.1 |

## Tabla de enlaces

| Equipo A | Puerto A | Equipo B | Puerto B |
|---|---|---|---|
| SW-PUERTO | Fa0/1 | SW-BELLAVISTA | Fa0/0 |
| SW-PUERTO | Fa1/0 | SW-ACC-PUERTO | Fa1/15 |
| SW-ACC-PUERTO | Fa1/0 | PC-PUERTO | Ethernet0 |
| SW-BELLAVISTA | Fa0/1 | SW-FRANCIA | Fa0/0 |
| SW-BELLAVISTA | Fa1/0 | SW-ACC-BELLAVISTA | Fa1/15 |
| SW-ACC-BELLAVISTA | Fa1/0 | PC-BELLAVISTA | Ethernet0 |
| SW-FRANCIA | Fa0/1 | SW-BARON | Fa0/0 |
| SW-FRANCIA | Fa1/0 | SW-ACC-FRANCIA | Fa1/15 |
| SW-ACC-FRANCIA | Fa1/0 | PC-FRANCIA | Ethernet0 |
| SW-BARON | Fa0/1 | SW-PORTALES | Fa0/0 |
| SW-BARON | Fa1/0 | SW-ACC-BARON | Fa1/15 |
| SW-ACC-BARON | Fa1/0 | PC-BARON | Ethernet0 |
| SW-PORTALES | Fa0/1 | SW-RECREO | Fa0/0 |
| SW-PORTALES | Fa1/0 | SW-ACC-PORTALES | Fa1/15 |
| SW-ACC-PORTALES | Fa1/0 | PC-PORTALES | Ethernet0 |
| SW-RECREO | Fa0/1 | SW-MIRAMAR | Fa0/0 |
| SW-RECREO | Fa1/0 | SW-ACC-RECREO | Fa1/15 |
| SW-ACC-RECREO | Fa1/0 | PC-RECREO | Ethernet0 |
| SW-MIRAMAR | Fa0/1 | SW-VINA | Fa0/0 |
| SW-MIRAMAR | Fa1/0 | SW-ACC-MIRAMAR | Fa1/15 |
| SW-ACC-MIRAMAR | Fa1/0 | PC-MIRAMAR | Ethernet0 |
| SW-VINA | Fa0/1 | SW-HOSPITAL | Fa0/0 |
| SW-VINA | Fa1/0 | SW-ACC-VINA | Fa1/15 |
| SW-ACC-VINA | Fa1/0 | PC-VINA | Ethernet0 |
| SW-HOSPITAL | Fa0/1 | SW-CHORRILLOS | Fa0/0 |
| SW-HOSPITAL | Fa1/0 | SW-ACC-HOSPITAL | Fa1/15 |
| SW-ACC-HOSPITAL | Fa1/0 | PC-HOSPITAL | Ethernet0 |
| SW-CHORRILLOS | Fa0/1 | SW-EL-SALTO | Fa0/0 |
| SW-CHORRILLOS | Fa1/0 | SW-ACC-CHORRILLOS | Fa1/15 |
| SW-ACC-CHORRILLOS | Fa1/0 | PC-CHORRILLOS | Ethernet0 |
| SW-EL-SALTO | Fa0/1 | SW-VALENCIA | Fa0/0 |
| SW-EL-SALTO | Fa1/0 | SW-ACC-EL-SALTO | Fa1/15 |
| SW-ACC-EL-SALTO | Fa1/0 | PC-EL-SALTO | Ethernet0 |
| SW-VALENCIA | Fa0/1 | SW-QUILPUE | Fa0/0 |
| SW-VALENCIA | Fa1/0 | SW-ACC-VALENCIA | Fa1/15 |
| SW-ACC-VALENCIA | Fa1/0 | PC-VALENCIA | Ethernet0 |
| SW-QUILPUE | Fa0/1 | SW-EL-SOL | Fa0/0 |
| SW-QUILPUE | Fa1/0 | SW-ACC-QUILPUE | Fa1/15 |
| SW-ACC-QUILPUE | Fa1/0 | PC-QUILPUE | Ethernet0 |
| SW-EL-SOL | Fa0/1 | SW-BELLOTO | Fa0/0 |
| SW-EL-SOL | Fa1/0 | SW-ACC-EL-SOL | Fa1/15 |
| SW-ACC-EL-SOL | Fa1/0 | PC-EL-SOL | Ethernet0 |
| SW-BELLOTO | Fa0/1 | SW-LAS-AMERICAS | Fa0/0 |
| SW-BELLOTO | Fa1/0 | SW-ACC-BELLOTO | Fa1/15 |
| SW-ACC-BELLOTO | Fa1/0 | PC-BELLOTO | Ethernet0 |
| SW-LAS-AMERICAS | Fa0/1 | SW-LA-CONCEPCION | Fa0/0 |
| SW-LAS-AMERICAS | Fa1/0 | SW-ACC-LAS-AMERICAS | Fa1/15 |
| SW-ACC-LAS-AMERICAS | Fa1/0 | PC-LAS-AMERICAS | Ethernet0 |
| SW-LA-CONCEPCION | Fa0/1 | SW-VILLA-ALEMANA | Fa0/0 |
| SW-LA-CONCEPCION | Fa1/0 | SW-ACC-LA-CONCEPCION | Fa1/15 |
| SW-ACC-LA-CONCEPCION | Fa1/0 | PC-LA-CONCEPCION | Ethernet0 |
| SW-VILLA-ALEMANA | Fa0/1 | SW-SARGENTO-ALDEA | Fa0/0 |
| SW-VILLA-ALEMANA | Fa1/0 | SW-ACC-VILLA-ALEMANA | Fa1/15 |
| SW-ACC-VILLA-ALEMANA | Fa1/0 | PC-VILLA-ALEMANA | Ethernet0 |
| SW-SARGENTO-ALDEA | Fa0/1 | SW-PENABLANCA | Fa0/0 |
| SW-SARGENTO-ALDEA | Fa1/0 | SW-ACC-SARGENTO-ALDEA | Fa1/15 |
| SW-ACC-SARGENTO-ALDEA | Fa1/0 | PC-SARGENTO-ALDEA | Ethernet0 |
| SW-PENABLANCA | Fa0/1 | SW-LIMACHE | Fa0/0 |
| SW-PENABLANCA | Fa1/0 | SW-ACC-PENABLANCA | Fa1/15 |
| SW-ACC-PENABLANCA | Fa1/0 | PC-PENABLANCA | Ethernet0 |
| SW-LIMACHE | Fa0/1 | SW-PUERTO | Fa0/0 |
| SW-LIMACHE | Fa1/0 | SW-ACC-LIMACHE | Fa1/15 |
| SW-ACC-LIMACHE | Fa1/0 | PC-LIMACHE | Ethernet0 |
| SW-PUERTO | Fa2/0 | SW-SERVIDORES | Fa0/0 |
| SW-LIMACHE | Fa2/0 | SW-SERVIDORES | Fa0/1 |
| SW-SERVIDORES | Fa2/0 | R-SALIDA | Fa0/0 |
| SW-SERVIDORES | Fa1/0 | PC-SERVIDORES | Ethernet0 |

## Estado de validación

La topología tiene 66 nodos y 67 enlaces: 21 equipos de Distribución, 21 de Acceso, SW-SERVIDORES, R-SALIDA y 22 VPCS. Los clientes mantienen sus IP estáticas; DHCP está deshabilitado.

Pruebas completadas:

- Puerto pasó pruebas de las cuatro VLAN desde el PC conectado a Acceso, con alcance a sus gateways en Distribución. Se restauró el PC a Operaciones y se guardó la configuración.
- En las 21 estaciones se verificaron las cinco VLAN y el trunk operativo en ambos extremos. La VLAN 99 se usa exclusivamente como red de gestión, con una subred por estación.
- Los 21 PC alcanzan su gateway, Servidores 10.100.50.10 y la IP de gestión de su switch de Acceso.
- Los 23 equipos de capa 3 tienen la cantidad esperada de vecinos OSPF FULL. Acceso no participa en OSPF y usa ip default-gateway.
- Después de exportar y reiniciar, los 44 equipos Cisco pasaron un inicio de sesión SSHv2 con botlab, privilegio 15 y acceso al modo de configuración. Se verificaron nuevamente los vecinos OSPF de capa 3.
- Después del reinicio se repitieron pings de gateway y comunicación remota en los clientes de Puerto, Limache y Servidores, con resultado correcto.
- Se apagó Fa0/1 en Puerto y el PC detrás de Acceso en Bellavista alcanzó Servidores mediante Francia. Al restaurar el enlace volvió el camino directo mediante Puerto.

El export portable contiene los 66 nodos, los 67 enlaces, los 22 archivos startup.vpc y los 44 archivos de claves privadas. Las configuraciones de arranque se extrajeron del portable y se verificaron completas, evitando pérdidas de caracteres de la consola serial.

Durante la validación se observaron pérdidas iniciales de ping en El Salto. Se revisaron las adyacencias de ambos caminos de igual costo y las repeticiones posteriores pasaron. La configuración de Acceso en Peñablanca se repitió después de una interrupción en la lectura del eco de consola.

Las pruebas certifican conectividad básica y persistencia, no rendimiento sostenido. Los endpoints de Seguridad electrónica, Medio de pago y WiFi se probaron temporalmente en Puerto; las demás estaciones mantienen un cliente permanente de Operaciones. WiFi representa una red IP, sin radio ni controlador WLAN.

No hay ACL de filtrado entre VLAN ni restricción del origen de SSH. La biblioteca SSH concreta del bot y su conexión al laboratorio se validarán al integrarlo en el equipo de Cristopher. Los relojes aún no están sincronizados mediante NTP.

## Próxima etapa

Definir las políticas de comunicación entre Operaciones, Seguridad electrónica, Medio de pago y WiFi. Integrar los servicios del bot y del dashboard en el equipo definitivo de Cristopher y validar allí el consumo, las rutas y los accesos que habilite el equipo de infraestructura.

El sandbox conserva el C7200 sin Cloud, sin NAT real y sin una ruta por defecto hacia una salida inexistente. El paquete de entrega incluye el proyecto portable, configuraciones, inventario de direcciones y resultados de pruebas. Las imágenes Cisco se proporcionan por separado.
