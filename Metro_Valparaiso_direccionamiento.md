# Direccionamiento y VLAN del sandbox

## VLAN y puertos de acceso

| VLAN | Servicio | Puertos del switch de Acceso |
|---|---|---|
| 10 | Operaciones | Fa1/0 a Fa1/3 |
| 20 | Seguridad electrónica | Fa1/4 a Fa1/7 |
| 30 | Medio de pago | Fa1/8 a Fa1/11 |
| 40 | WiFi | Fa1/12 a Fa1/14 |
| 99 | Gestión | Trunk Fa1/15; SVI de gestión |

Las VLAN son locales a cada estación. Reutilizar el ID de VLAN no extiende el dominio de broadcast entre estaciones: el anillo usa interfaces de capa 3 y OSPF área 0. Cada red tiene máscara /24 y gateway .1. Todos los endpoints usan IP estática; no se usan servidores DHCP ni relay DHCP. Los VPCS de Operaciones usan 10.<estación>.10.10/24 con gateway 10.<estación>.10.1. Para pruebas en las otras VLAN, el cliente usa 10.<estación>.<VLAN>.10/24 y gateway 10.<estación>.<VLAN>.1. Cada IP debe registrarse en el inventario para evitar duplicados.

Los VPCS existentes se conectan a Fa1/0 del switch de Acceso, en Operaciones. El trunk une Fa1/15 de Acceso con Fa1/0 de Distribución y permite VLAN 10, 20, 30, 40 y 99. Distribución conserva los gateways de datos y anuncia la subred de gestión por OSPF; Acceso no enruta y usa el gateway de VLAN 99. Los puertos de las otras VLAN quedan preparados para endpoints futuros o para pruebas temporales con el VPCS. WiFi emula la red IP de clientes; no emula cobertura radio ni un controlador WLAN.

No se han definido ACL de filtrado entre VLAN. El enrutamiento permitirá comunicación entre redes activas mientras no se añadan políticas. La asignación de VLAN por sí sola no constituye aislamiento de seguridad de capa 3.

## Subredes por estación

| ID | Estación | Operaciones | Seguridad electrónica | Medio de pago | WiFi | Loopback |
|---|---|---|---|---|---|---|
| 1 | PUERTO | 10.1.10.0/24 | 10.1.20.0/24 | 10.1.30.0/24 | 10.1.40.0/24 | 10.255.0.1/32 |
| 2 | BELLAVISTA | 10.2.10.0/24 | 10.2.20.0/24 | 10.2.30.0/24 | 10.2.40.0/24 | 10.255.0.2/32 |
| 3 | FRANCIA | 10.3.10.0/24 | 10.3.20.0/24 | 10.3.30.0/24 | 10.3.40.0/24 | 10.255.0.3/32 |
| 4 | BARON | 10.4.10.0/24 | 10.4.20.0/24 | 10.4.30.0/24 | 10.4.40.0/24 | 10.255.0.4/32 |
| 5 | PORTALES | 10.5.10.0/24 | 10.5.20.0/24 | 10.5.30.0/24 | 10.5.40.0/24 | 10.255.0.5/32 |
| 6 | RECREO | 10.6.10.0/24 | 10.6.20.0/24 | 10.6.30.0/24 | 10.6.40.0/24 | 10.255.0.6/32 |
| 7 | MIRAMAR | 10.7.10.0/24 | 10.7.20.0/24 | 10.7.30.0/24 | 10.7.40.0/24 | 10.255.0.7/32 |
| 8 | VINA | 10.8.10.0/24 | 10.8.20.0/24 | 10.8.30.0/24 | 10.8.40.0/24 | 10.255.0.8/32 |
| 9 | HOSPITAL | 10.9.10.0/24 | 10.9.20.0/24 | 10.9.30.0/24 | 10.9.40.0/24 | 10.255.0.9/32 |
| 10 | CHORRILLOS | 10.10.10.0/24 | 10.10.20.0/24 | 10.10.30.0/24 | 10.10.40.0/24 | 10.255.0.10/32 |
| 11 | EL-SALTO | 10.11.10.0/24 | 10.11.20.0/24 | 10.11.30.0/24 | 10.11.40.0/24 | 10.255.0.11/32 |
| 12 | VALENCIA | 10.12.10.0/24 | 10.12.20.0/24 | 10.12.30.0/24 | 10.12.40.0/24 | 10.255.0.12/32 |
| 13 | QUILPUE | 10.13.10.0/24 | 10.13.20.0/24 | 10.13.30.0/24 | 10.13.40.0/24 | 10.255.0.13/32 |
| 14 | EL-SOL | 10.14.10.0/24 | 10.14.20.0/24 | 10.14.30.0/24 | 10.14.40.0/24 | 10.255.0.14/32 |
| 15 | BELLOTO | 10.15.10.0/24 | 10.15.20.0/24 | 10.15.30.0/24 | 10.15.40.0/24 | 10.255.0.15/32 |
| 16 | LAS-AMERICAS | 10.16.10.0/24 | 10.16.20.0/24 | 10.16.30.0/24 | 10.16.40.0/24 | 10.255.0.16/32 |
| 17 | LA-CONCEPCION | 10.17.10.0/24 | 10.17.20.0/24 | 10.17.30.0/24 | 10.17.40.0/24 | 10.255.0.17/32 |
| 18 | VILLA-ALEMANA | 10.18.10.0/24 | 10.18.20.0/24 | 10.18.30.0/24 | 10.18.40.0/24 | 10.255.0.18/32 |
| 19 | SARGENTO-ALDEA | 10.19.10.0/24 | 10.19.20.0/24 | 10.19.30.0/24 | 10.19.40.0/24 | 10.255.0.19/32 |
| 20 | PENABLANCA | 10.20.10.0/24 | 10.20.20.0/24 | 10.20.30.0/24 | 10.20.40.0/24 | 10.255.0.20/32 |
| 21 | LIMACHE | 10.21.10.0/24 | 10.21.20.0/24 | 10.21.30.0/24 | 10.21.40.0/24 | 10.255.0.21/32 |

## Sala de servidores y salida

SW-SERVIDORES: VLAN 50 SERVIDORES, 10.100.50.0/24, gateway 10.100.50.1; Fa1/0 a Fa1/15. Direcciones estáticas para integración futura; sin DHCP de servidores. Loopback 10.255.0.100/32. R-SALIDA: loopback 10.255.0.254/32, participa en OSPF con el switch de servidores. No hay Cloud, NAT real ni anuncio de ruta por defecto.

Las loopbacks de Distribución, Servidores y R-SALIDA son direcciones estables para SSH. Los equipos de Acceso usan su SVI de VLAN 99, según la tabla de gestión.

## Enlaces de tránsito

| Equipo A | Puerto A | IP A | Equipo B | Puerto B | IP B | Red |
|---|---|---|---|---|---|---|
| SW-PUERTO | Fa0/1 | 10.254.0.1 | SW-BELLAVISTA | Fa0/0 | 10.254.0.2 | 10.254.0.0/30 |
| SW-BELLAVISTA | Fa0/1 | 10.254.0.5 | SW-FRANCIA | Fa0/0 | 10.254.0.6 | 10.254.0.4/30 |
| SW-FRANCIA | Fa0/1 | 10.254.0.9 | SW-BARON | Fa0/0 | 10.254.0.10 | 10.254.0.8/30 |
| SW-BARON | Fa0/1 | 10.254.0.13 | SW-PORTALES | Fa0/0 | 10.254.0.14 | 10.254.0.12/30 |
| SW-PORTALES | Fa0/1 | 10.254.0.17 | SW-RECREO | Fa0/0 | 10.254.0.18 | 10.254.0.16/30 |
| SW-RECREO | Fa0/1 | 10.254.0.21 | SW-MIRAMAR | Fa0/0 | 10.254.0.22 | 10.254.0.20/30 |
| SW-MIRAMAR | Fa0/1 | 10.254.0.25 | SW-VINA | Fa0/0 | 10.254.0.26 | 10.254.0.24/30 |
| SW-VINA | Fa0/1 | 10.254.0.29 | SW-HOSPITAL | Fa0/0 | 10.254.0.30 | 10.254.0.28/30 |
| SW-HOSPITAL | Fa0/1 | 10.254.0.33 | SW-CHORRILLOS | Fa0/0 | 10.254.0.34 | 10.254.0.32/30 |
| SW-CHORRILLOS | Fa0/1 | 10.254.0.37 | SW-EL-SALTO | Fa0/0 | 10.254.0.38 | 10.254.0.36/30 |
| SW-EL-SALTO | Fa0/1 | 10.254.0.41 | SW-VALENCIA | Fa0/0 | 10.254.0.42 | 10.254.0.40/30 |
| SW-VALENCIA | Fa0/1 | 10.254.0.45 | SW-QUILPUE | Fa0/0 | 10.254.0.46 | 10.254.0.44/30 |
| SW-QUILPUE | Fa0/1 | 10.254.0.49 | SW-EL-SOL | Fa0/0 | 10.254.0.50 | 10.254.0.48/30 |
| SW-EL-SOL | Fa0/1 | 10.254.0.53 | SW-BELLOTO | Fa0/0 | 10.254.0.54 | 10.254.0.52/30 |
| SW-BELLOTO | Fa0/1 | 10.254.0.57 | SW-LAS-AMERICAS | Fa0/0 | 10.254.0.58 | 10.254.0.56/30 |
| SW-LAS-AMERICAS | Fa0/1 | 10.254.0.61 | SW-LA-CONCEPCION | Fa0/0 | 10.254.0.62 | 10.254.0.60/30 |
| SW-LA-CONCEPCION | Fa0/1 | 10.254.0.65 | SW-VILLA-ALEMANA | Fa0/0 | 10.254.0.66 | 10.254.0.64/30 |
| SW-VILLA-ALEMANA | Fa0/1 | 10.254.0.69 | SW-SARGENTO-ALDEA | Fa0/0 | 10.254.0.70 | 10.254.0.68/30 |
| SW-SARGENTO-ALDEA | Fa0/1 | 10.254.0.73 | SW-PENABLANCA | Fa0/0 | 10.254.0.74 | 10.254.0.72/30 |
| SW-PENABLANCA | Fa0/1 | 10.254.0.77 | SW-LIMACHE | Fa0/0 | 10.254.0.78 | 10.254.0.76/30 |
| SW-LIMACHE | Fa0/1 | 10.254.0.81 | SW-PUERTO | Fa0/0 | 10.254.0.82 | 10.254.0.80/30 |
| SW-PUERTO | Fa2/0 | 10.254.0.85 | SW-SERVIDORES | Fa0/0 | 10.254.0.86 | 10.254.0.84/30 |
| SW-LIMACHE | Fa2/0 | 10.254.0.89 | SW-SERVIDORES | Fa0/1 | 10.254.0.90 | 10.254.0.88/30 |
| SW-SERVIDORES | Fa2/0 | 10.254.0.93 | R-SALIDA | Fa0/0 | 10.254.0.94 | 10.254.0.92/30 |

OSPF usa área 0, enlaces punto a punto y passive-interface default; solo las interfaces de tránsito forman vecinos. Las SVI y loopbacks se anuncian cuando están operativas. Una SVI sin un puerto activo en su VLAN puede quedar down y su red no se anunciará hasta conectar un endpoint.

## Configuraciones

Se conservan 44 archivos de comandos Cisco y 44 configuraciones de arranque completas (-startup.cfg), además de 22 archivos VPCS. Las configuraciones y el proyecto portable se entregan por separado; este repositorio contiene documentación.

## Estado final

Configuraciones de Distribución y Acceso aplicadas y guardadas en las 21 estaciones. SSHv2 verificado en los 44 equipos Cisco después de exportar y reiniciar. Clientes estáticos, trunks, VLAN, OSPF y recuperación ante una caída de enlace validados.

## Gestión de los switches de Acceso

| Estación | Red VLAN 99 | Distribución / gateway | Acceso / SSH |
|---|---|---|---|
| PUERTO | 10.1.99.0/24 | 10.1.99.1 | 10.1.99.2 |
| BELLAVISTA | 10.2.99.0/24 | 10.2.99.1 | 10.2.99.2 |
| FRANCIA | 10.3.99.0/24 | 10.3.99.1 | 10.3.99.2 |
| BARON | 10.4.99.0/24 | 10.4.99.1 | 10.4.99.2 |
| PORTALES | 10.5.99.0/24 | 10.5.99.1 | 10.5.99.2 |
| RECREO | 10.6.99.0/24 | 10.6.99.1 | 10.6.99.2 |
| MIRAMAR | 10.7.99.0/24 | 10.7.99.1 | 10.7.99.2 |
| VINA | 10.8.99.0/24 | 10.8.99.1 | 10.8.99.2 |
| HOSPITAL | 10.9.99.0/24 | 10.9.99.1 | 10.9.99.2 |
| CHORRILLOS | 10.10.99.0/24 | 10.10.99.1 | 10.10.99.2 |
| EL-SALTO | 10.11.99.0/24 | 10.11.99.1 | 10.11.99.2 |
| VALENCIA | 10.12.99.0/24 | 10.12.99.1 | 10.12.99.2 |
| QUILPUE | 10.13.99.0/24 | 10.13.99.1 | 10.13.99.2 |
| EL-SOL | 10.14.99.0/24 | 10.14.99.1 | 10.14.99.2 |
| BELLOTO | 10.15.99.0/24 | 10.15.99.1 | 10.15.99.2 |
| LAS-AMERICAS | 10.16.99.0/24 | 10.16.99.1 | 10.16.99.2 |
| LA-CONCEPCION | 10.17.99.0/24 | 10.17.99.1 | 10.17.99.2 |
| VILLA-ALEMANA | 10.18.99.0/24 | 10.18.99.1 | 10.18.99.2 |
| SARGENTO-ALDEA | 10.19.99.0/24 | 10.19.99.1 | 10.19.99.2 |
| PENABLANCA | 10.20.99.0/24 | 10.20.99.1 | 10.20.99.2 |
| LIMACHE | 10.21.99.0/24 | 10.21.99.1 | 10.21.99.2 |
