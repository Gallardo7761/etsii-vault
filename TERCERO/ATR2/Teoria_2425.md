# <mark style="background: #FFF3A3A6;">TEMA 1: Nivel de Red</mark>
El protocolo IP no ofrece un servicio orientado a la conexión (es no fiable). Emplea el servicio de entrega de R_PDU empleado por el nivel de enlace (utiliza servicio de su nivel inferior), permitiendo intercambiar información entre hosts o routers y dispositivos que implementen como mucho N.E.D.

Establece las herramientas necesarias para **enrutar**, es decir, definir el camino a seguir por los datos de extremo a extremo.
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
### <mark style="background: #FFB86CA6;">Esquema de direccionamiento</mark>
Todo dispositivo en una red debe poseer una dirección IP para poder transmitir datos usando TCP/IP. Hay dos versiones de IP: IPv4 (32b) e IPv6 (128b). Las direcciones se pueden configurar de dos formas:
- **Estática:** El usuario configura la IP en el host suministrada por el administrador de la red.
- **Dinámica:** Se asigna automáticamente una IP al host mediante DHCP.
La dirección IPv4  **NO ES LO MISMO** que la configuración IPv4. Para decir que un equipo tiene una configuración IPv4 correcta debe tener como mínimo dirección IP y máscara de red. Adicionalmente se suele especificar la dirección IP del gateway/puerta de enlace/router frontera.
## <mark style="background: #ADCCFFA6;">2. Configuración dinámica de direcciones</mark>
### <mark style="background: #FFB86CA6;">DHCP</mark>
Hay varias formas de asignación dinámica de direcciones IP:
- **Asignación automática:** 
#### <mark style="background: #D2B3FFA6;">Ciclo básico</mark>
DHCP tiene un ciclo de mensajes básico tal que:
- **DHCP DISCOVER:** intenta encontrar un servidor DHCP
  Se envía un paquete IP con IP origen 0.0.0.0 y destino 255.255.255.255 (broadcast). 
- **DHCP OFFER:** el/los servidores DHCP ofrecen una dirección IP
  El servidor DHCP, si lo hay, envía un paquete IP con IP origen su propia IP y destino la de broadcast
- **DHCP REQUEST:** el cliente pide ciertos parámetros
  Se envía un paquete al servidor DHCP con IP origen 0.0.0.0 y destino 255.255.255.255, aunque la IP está "pre-asignada" todavía no se le ha concedido al host dicha IP.
- **DHCP ACK:** ACK de confirmación desde el servidor
  El servidor DHCP envía un ACK en un paquete IP con IP origen la suya y destino la de broadcast.
Tras completar el ciclo básico el cliente envía una petición ARP llamada **ARP Gratuito** para ver si cualquier otro host tiene la IP que se le ha asignado. Si no obtiene respuesta, se queda con esa IP. Si la obtiene vuelve a solicitar otra.
#### <mark style="background: #D2B3FFA6;">Formato de mensajes</mark>
Los mensajes DHCP tienen un formato específico:
- Los primeros 4 Bytes se denominan _magic cookie_.
- Cada transacción (petición/respuesta) tiene un id.
## <mark style="background: #ADCCFFA6;">3. Control de errores</mark>
IP no implementa un control de errores, así que en caso de que se produzca un error existen protocolos que se "montan" sobre IP para este propósito.
### <mark style="background: #FFB86CA6;">ICMP (Internet Control Message Protocol)</mark>
Es usado por hosts y routers para comunicar información a nivel de red (informe de errores o avisos). Se encapsulan en datagramas IP, por lo que los dispositivos deben ser de hasta nivel 3.
### Formato de mensajes
- **Type:** Indica el tipo de mensaje ICMP
- **Code.** Indica el subtipo de mensaje
- **Checksum:** Para detectar mensajes corruptos

| Tipo | Código | Descripción             |
| ---- | ------ | ----------------------- |
| 0    | 0      | Respuesta de eco (ping) |
| 3    | 0      | Red inalcanzable        |
| 3    | 1      | Host inalcanzable       |
| 3    | 2      | Protocolo inalcanzable  |
| 3    | 3      | Puerto inalcanzable     |
| 5    | 0      | Redireccionamiento      |
| 8    | 0      | Petición de eco (ping)  |
| 11   | 0      | TTL excedido            |
## <mark style="background: #ADCCFFA6;">4. Traducción de direcciones</mark>
Hay dos alternativas:
- **Dinámica:** El router NAT asignará de forma temporal las IPs públicas cuando haya necesidad de "saltar" a internet.
	- El router tiene un rango de IPs asignables y además una tabla para guardar dichas asignaciones de IPs.
- **Estática:** Se configuran las asignaciones de forma permanente por el administrador de la red.
