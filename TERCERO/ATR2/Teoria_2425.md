# <mark style="background: #FFF3A3A6;">TEMA 1: Nivel de Red (DHCP-NA(P)T-ICMP)</mark>
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
# <mark style="background: #FFF3A3A6;">TEMA 2: Nivel de Red (Routing)</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
Para que el datagrama se entregue con éxito se debe cumplir:
- El prefijo de dirección destino debe corresponder a una sola red
- Los routers y hosts que tienen un prefijo de red común deben ser capaz de intercambiar datos sin ayuda de un router
- Cada red (a nivel 2) debe estar conectada al menos con otra red mediante un router
Un nodo (router o host) tiene que tener una tabla de enrutamiento. Suele tener los campos **Red**, **Próximo Salto**, **Interfaz**.
![[Pasted image 20250225091113.png]]
En Windows, la **Métrica** es el "coste" que tiene esa ruta si se escoge. Se puede valorar en términos de velocidad, TTL o _delay_ temporal.
## <mark style="background: #ADCCFFA6;">2. Reenvío / Enrutamiento</mark>
Hay varios pasos en el reenvío:
1. Validar cabecera
2. Procesar opciones
3. Analizar destino
4. Buscar destino en la tabla de enrutamiento
5. Decrementar TTL
6. Fragmentar
7. Calcular checksum
8. Reenviar al PS
9. Enviar ICMP
## <mark style="background: #ADCCFFA6;">3. Información global</mark>
Todos los routers tienen la topología completa y la información de coste de los enlaces (**Algoritmo "link state"**).
## <mark style="background: #ADCCFFA6;">4. Información descentralizada</mark>
Los routers conocen los vecinos físicamente conectados y el coste del enlace a sus vecinos. En un proceso iterativo, intercambia información con sus vecinos (**Algoritmo "distance vector"**).
## <mark style="background: #ADCCFFA6;">5. Enrutamiento estático</mark>
Las rutas cambian lentamente. Es decir, todas las entradas de la tabla se habrán introducido manualmente. La información de ida no proporciona información sobre como volver ni lo asegura. **Convergencia:** todos los routers tienen información precisa y coherente.
## <mark style="background: #ADCCFFA6;">6. Enrutamiento dinámico</mark>
Las rutas cambian más rápido (actualizaciones periódicas y respuesta a los cambios en los enlaces).
### <mark style="background: #FFB86CA6;">Sistemas autónomos</mark>
Grupo de prefijos de red de uno o varios ISP en el que existe una política de enrutamiento única y claramente definida.
- Dentro de cada AS se utilizan protocolos de routing interiores (IGP)
- Entre cada AS se usarán protocolos de routing exteriores (EGP)
### <mark style="background: #FFB86CA6;">Componentes de un algoritmo de enrutamiento</mark>
1. Un proceso para enviar y recibir información sobre redes alcanzables a/de otros routers
2. Un proceso para calcular rutas óptimas (costes en cada enlace)
3. Un proceso para reaccionar y avisar de cambios en la topología
### <mark style="background: #FFB86CA6;">Cálculo del camino más corto</mark>
#### <mark style="background: #D2B3FFA6;">Vector distancia (Bellman-Ford)</mark>
- Cada router conoce la distancia (o coste) de sus vecinos directamente conectados
- Un router envía una lista de actualizaciones de enrutamiento a sus vecinos que contiene las redes alcanzables por él y su  distancia
- Si todos los routers actualizan las redes alcanzables/distancias con la información recibida de sus vecinos, la red converge.
- Cada router conoce:
	- Su ID
	- Sus interfaces
	- La distancia hasta el siguiente router de cada interfaz
- Actualizaciones periódicas: cada (~90s) se envía una actualización
- Actualizaciones por cambios: si hay cambios en la métrica de un enlace, se envía inmediatamente
- Actualizaciones de toda la TE: la mayoría de los protocolos que usan vector distancia, envían toda la TE a sus vecinos en las actualizaciones
- Tiempo para invalidar rutas: se invalidan las rutas en la TE si no son refrescadas, tras un valor típico 3-16 veces periodo de actualización
![[Pasted image 20250307112154.png]]
#### <mark style="background: #D2B3FFA6;">Estado del enlace (Dijkstra)</mark>
- Cada router conoce la distancia de sus vecinos directamente conectados
- La información de distancia (LS: Link State) es enviada por broadcast a todos los routers de la red
- Todos los routers construyen la topología de la red
- Cada router calcula el camino más corto a todas las redes alcanzables de manera independiente
## <mark style="background: #ADCCFFA6;">7. RIP</mark>
### <mark style="background: #FFB86CA6;">RIPv1</mark>
- Sufre los problemas típicos del vector distancia
- Sólo útil en redes pequeñas (5-10 routers)
- Métrica basada en número de saltos únicamente
- Máximo 15 saltos. Infinito 16.
- La información se intercambia cada 30s.
Los mensajes RIP se encapsulan en UDP en el puerto 520.
- Dirección destino broadcast dirigido
- Solo procesa actualización si origen pertenece a la red de la interfaz
En un mensaje RIP pueden enviarse hasta 25 entradas del vector distancias
- Vectores grandes -> varios mensajes
- Invalidación tras 180s
No usa máscara de red en los mensajes -> no soporta CIDR ni VLSM.
### <mark style="background: #FFB86CA6;">RIPv2</mark>
- Permite CIDR (tiene el campo de la máscara de subred).
- Campo próximo salto
![[Pasted image 20250307115920.png]]

# <mark style="background: #FFF3A3A6;">TEMA 3: Redes inalámbricas y móviles</mark>
Dos retos importantes aunque diferentes:
- **Sin cables:** comunicación a través de un enlace inalámbrico
- **Movilidad:** gestionando las conexiones cuando un usuario cambia de punto de acceso a la red sin perder su conexión
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
![[Pasted image 20250314114257.png]]
![[Pasted image 20250314114314.png]]
Hay dos formas de conectar dispositivos de forma inalámbrica
### <mark style="background: #FFB86CA6;">Modo infraestructura</mark>
![[Pasted image 20250314115427.png]]
- Las estaciones base conectan dispositivos de forma cableada.
- Transferencia: dispositivo cambia de estación base que provee la conexión a la red cableada.
# UN PUNTO DE ACCESO NO TIENE IP, ES DE NIVEL 2

![[Pasted image 20250314115632.png]]
**Inundación:** Tabla vacía. El equipo X transmite a la estación A.
**Envío de tráfico:** Si A contesta a X, el punto de acceso recibe una trama en el puerto I con dirección MAC_A de origen y MAC_X destino.
### <mark style="background: #FFB86CA6;">Modo ad hoc</mark>
![[Pasted image 20250314115857.png]]
- No hay estaciones base
- Los nodos sólo pueden transmitir a otros nodos en su alcance
- Los nodos se organizan a sí mismos en red: enrutamiento a través de ellos mismos
### <mark style="background: #FFB86CA6;">Otros modos</mark>
- **Modo Máster:** una estación inalámbrica actúa como punto de acceso
- **Modo Monitor:** permite capturar paquetes sin asociarse a un punto de acceso o red ad-hoc, es decir, permite monitorizar la red sin transmitir tráfico a la misma (modo pasivo).
- **Modo Promiscuo:** permite capturar paquetes de la red, pero hay que estar en ella.
## <mark style="background: #ADCCFFA6;">2. Topologías</mark>
- **BSS (Basic Service Set):** topología de red formada por un punto de acceso y estaciones inalámbricas.
- **ESS (Extended Service Set):** topología de red formada por varias BSS interconectadas entre ellas.
- **IBSS (Independent Basic Service Set):** topología de red formada únicamente por estaciones inalámbricas, operando en modo ad-hoc.
## <mark style="background: #ADCCFFA6;">3. Sistemas de distribución</mark>
### <mark style="background: #FFB86CA6;">Cableado</mark>
A través de red LAN IEEE 802.3
### <mark style="background: #FFB86CA6;">Inalámbricos</mark>
- **WLAN con rutas preconfiguradas estáticas:**  WLAN basado en IEEE 802.11
- **WLAN con rutas no preconfiguradas o Redes Mesh:** Las redes Mesh se definen como el conjunto AP interconectadas mediante enlaces inalámbricos ocn configuración dinámica (algoritmos). El objetivo de las redes Mesh es llegar a todos los rincones del sitio con varios AP en una "malla" (mesh).
## <mark style="background: #ADCCFFA6;">4. Diferencias entre tipos de enlace</mark>
En redes inalámbricas hay problemas de obstáculos y distancia, ya que si hubiese tres equipos A,B,C, A y C no "se ven" y como la señal se debilita con la distancia entre ellos o el obstáculo entre ellos no se pueden comunicar.

| Obstáculo                            | Distancia                            |
| ------------------------------------ | ------------------------------------ |
| ![[Pasted image 20250314121904.png]] | ![[Pasted image 20250314121916.png]] |
## <mark style="background: #ADCCFFA6;">5. Code Division Multiple Access (CDMA)</mark>
Código único asignado a cada usuario. Todos los usuarios comparten la misma frecuencia pero cada uno tiene una frecuencia de chip para codificar los datos. Permite a usuarios coexistir y transmitir simultáneamente.
- **Señal codificada:** datos originales x secuencia de chip
- **Decodificación:** producto escalar entre la señal codificada y la secuencia de chip
![[Pasted image 20250314122451.png]]