# SSH y gestión del laboratorio con Distribución y Acceso

Los 44 equipos Cisco usan SSHv2, RSA de 2048 bits, cuenta local botlab con privilegio 15 y líneas VTY que aceptan exclusivamente SSH. Las credenciales vigentes están en Credenciales_SSH_LAB.txt, entregado por separado. La contraseña no se cambió al añadir Acceso.

El proyecto portable contiene claves privadas y las configuraciones incluyen hashes de contraseña; conservar el paquete como material privado del laboratorio. SSH usa TCP 22. Los puertos de consola de GNS3 (5000 y siguientes) son un mecanismo distinto.

## Inventario SSH

| Equipo | Dirección | Puerto |
|---|---|---|
| R-SALIDA | 10.255.0.254 | 22 |
| SW-ACC-BARON | 10.4.99.2 | 22 |
| SW-ACC-BELLAVISTA | 10.2.99.2 | 22 |
| SW-ACC-BELLOTO | 10.15.99.2 | 22 |
| SW-ACC-CHORRILLOS | 10.10.99.2 | 22 |
| SW-ACC-EL-SALTO | 10.11.99.2 | 22 |
| SW-ACC-EL-SOL | 10.14.99.2 | 22 |
| SW-ACC-FRANCIA | 10.3.99.2 | 22 |
| SW-ACC-HOSPITAL | 10.9.99.2 | 22 |
| SW-ACC-LA-CONCEPCION | 10.17.99.2 | 22 |
| SW-ACC-LAS-AMERICAS | 10.16.99.2 | 22 |
| SW-ACC-LIMACHE | 10.21.99.2 | 22 |
| SW-ACC-MIRAMAR | 10.7.99.2 | 22 |
| SW-ACC-PENABLANCA | 10.20.99.2 | 22 |
| SW-ACC-PORTALES | 10.5.99.2 | 22 |
| SW-ACC-PUERTO | 10.1.99.2 | 22 |
| SW-ACC-QUILPUE | 10.13.99.2 | 22 |
| SW-ACC-RECREO | 10.6.99.2 | 22 |
| SW-ACC-SARGENTO-ALDEA | 10.19.99.2 | 22 |
| SW-ACC-VALENCIA | 10.12.99.2 | 22 |
| SW-ACC-VILLA-ALEMANA | 10.18.99.2 | 22 |
| SW-ACC-VINA | 10.8.99.2 | 22 |
| SW-BARON | 10.255.0.4 | 22 |
| SW-BELLAVISTA | 10.255.0.2 | 22 |
| SW-BELLOTO | 10.255.0.15 | 22 |
| SW-CHORRILLOS | 10.255.0.10 | 22 |
| SW-EL-SALTO | 10.255.0.11 | 22 |
| SW-EL-SOL | 10.255.0.14 | 22 |
| SW-FRANCIA | 10.255.0.3 | 22 |
| SW-HOSPITAL | 10.255.0.9 | 22 |
| SW-LA-CONCEPCION | 10.255.0.17 | 22 |
| SW-LAS-AMERICAS | 10.255.0.16 | 22 |
| SW-LIMACHE | 10.255.0.21 | 22 |
| SW-MIRAMAR | 10.255.0.7 | 22 |
| SW-PENABLANCA | 10.255.0.20 | 22 |
| SW-PORTALES | 10.255.0.5 | 22 |
| SW-PUERTO | 10.255.0.1 | 22 |
| SW-QUILPUE | 10.255.0.13 | 22 |
| SW-RECREO | 10.255.0.6 | 22 |
| SW-SARGENTO-ALDEA | 10.255.0.19 | 22 |
| SW-SERVIDORES | 10.255.0.100 | 22 |
| SW-VALENCIA | 10.255.0.12 | 22 |
| SW-VILLA-ALEMANA | 10.255.0.18 | 22 |
| SW-VINA | 10.255.0.8 | 22 |

## Integración del bot

Propuesta: conectar una VM Linux al puerto Fa1/1 de SW-SERVIDORES (VLAN 50), con IP estática 10.100.50.20/24 y gateway 10.100.50.1. Esa dirección sigue siendo una reserva propuesta; todavía no se ha añadido la VM. .1 es el gateway y .10 el VPCS de prueba.

Si el bot se ejecuta en el sistema operativo del PC de Cristopher, habrá que conectar la red virtual con el host mediante un enlace de gestión y configurar las rutas. No se añadió Cloud ni salida real a Internet.

Los IOS son antiguos (12.4 y 15.2). Se probó SSH con clientes Cisco emulados; debe verificarse la negociación con la biblioteca concreta del bot. Mantener cualquier ajuste de compatibilidad limitado al laboratorio.

Acceso tiene no ip routing y un único SVI de gestión, 10.ID.99.2/24. Su gateway es Distribución 10.ID.99.1. Distribución anuncia esa subred por OSPF; las loopbacks y gateways de datos anteriores se conservan. La VLAN 99 es local por estación y no se extiende por el anillo.

## Mejoras siguientes

Definir ACL entre las redes de usuarios y restringir el origen de SSH cuando se confirme la dirección del bot. Centralizar Syslog y sincronizar NTP. Añadir clientes permanentes en Seguridad electrónica, Medio de pago y WiFi. Si se requieren funciones de switches modernos, elegir una imagen compatible: los C3725 son routers con módulos NM-16ESW, usados como representación del laboratorio.

No hay ACL aplicadas ni NTP centralizado. Cada estación tiene un solo switch de Distribución, uno de Acceso y un trunk entre ellos; la redundancia del anillo no protege una falla de ese trunk o de esos equipos locales.
