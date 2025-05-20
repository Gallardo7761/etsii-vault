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
## UN PUNTO DE ACCESO NO TIENE IP, ES DE NIVEL 2

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
## <mark style="background: #ADCCFFA6;">6. LAN Inalámbrica IEEE 802.11</mark>
![[Pasted image 20250321110514.png]]
La arquitectura de LAN 802.11 es básicamente:
![[Pasted image 20250321110756.png]]
- Host inalámbrico comunica con la estación base (Punto de Acceso, AP). Conjunto de servicios básico BSS (modo infraestructura: hosts, AP; modo ad hoc: hosts).
### <mark style="background: #FFB86CA6;">Escaneo activo/pasivo</mark>
- **Escaneo pasivo:** 
  1. Los AP envían tramas baliza
  2. Host envía petición de asociación al AP seleccionado
  3. Se recibe en el Host una respuesta de asociación desde el AP
  4. IP, Netmask, RF, DNS.
     
    ![[Pasted image 20250321111135.png]]
- **Escaneo activo:**
  1. Broadcast con una trama de sondeo desde el Host.
  2. Respuesta a la trama de sondeo enviadas desde los AP.
  3. Host envía petición de asociación al AP seleccionado.
  4. Se recibe en el Host una respuesta de asociación desde el AP.
     ![[Pasted image 20250321111817.png]]
### <mark style="background: #FFB86CA6;">Emisión/Recepción</mark>
![[Pasted image 20250321112719.png]]
- **Emisor:** 
  1. Si tras un tiempo **DIFS** está el canal libre, se transmite la trama entera. 
  2. Si está ocupado el canal, inicia un tiempo aleatorio de espera. El contador va bajando mientras el canal se queda libre. Intentará transmitir cuando el contador expire. 
  3. Si no ACK, incrementa el contador y vuelve a 2.
- **Receptor:**
  Si trama recibida OK: devuelve ACK después de **SIFS**
#### <mark style="background: #D2B3FFA6;">IDEA: para evitar las colisiones</mark>
Permitir al emisor "reservar" el canal en lugar de acceder aleatoriamente evitando colisiones con tramas largas. 
- El emisor transmite primero pequeños paquetes de solicitud de transmisión (RTS) usando CSMA (los RTS pueden colisionar pero son cortos).
- El AP responde preparado para enviar (CTS). 
- Cuando el resto de Hosts reciben el CTS, aplazan sus transmisiones y el Host emisor actual transmite su trama.
![[Pasted image 20250321113321.png]]
### <mark style="background: #FFB86CA6;">Trama 802.11</mark>
![[Pasted image 20250321113414.png]]
- **Dirección 1:** MAC Rx
- **Dirección 2:** MAC Tx
- **Dirección 3:** MAC de la interfaz del router a la que el AP está conectado
- **Dirección 4:** sólo en modo ad-hoc
- **CRC:** control de errores
![[Pasted image 20250321113739.png]]
![[Pasted image 20250321113941.png]]
## <mark style="background: #ADCCFFA6;">7. Movilidad</mark>
### <mark style="background: #FFB86CA6;">Definiciones</mark>
**Dirección permanente:** permanece constante (ej. 128.119.40.186)
**Care-of-address (COA):** dirección en la red visitada (ej. 79.129.13.2)
**Red ajena (visited network):** red en la que reside actualmente el dispositivo (ej. 79.129.13.0/24)
**Corresponsal:** host que quiere comunicarse
**Agente ajeno (foreign agent):** entidad en la red ajena que se encarga de funciones de movilidad.
### <mark style="background: #FFB86CA6;">Enrutamiento indirecto</mark>
![[Pasted image 20250321115758.png]]
### <mark style="background: #FFB86CA6;">Enrutamiento directo</mark>
![[Pasted image 20250321115951.png]]
# <mark style="background: #FFF3A3A6;">TEMA 4: Transmisión de Datos</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
**Datos:** entidades que contienen información
- Digitales: datos binarios (0's y 1's)
- Analógicos: datos en forma de ondas variables con el tiempo (por ejemplo onda sonora de la voz a través del teléfono)
**Señales:** representaciones eléctricas/electromagnéticas de los datos
**Señalización:** proceso de conversión de datos en señales físicas adecuadas al medio
- Digital: codificación
- Analógica: modulación
**Transmisión:** proceso de comunicación de datos mediante propagación y procesado de señales
- Digital: ondas cuadradas discretas, por ejemplo redes de computadores
- Analógica: datos analógicos transmitidos de forma analógica, por ejemplo TV/radio
### <mark style="background: #FFB86CA6;">Datos convertidos A/D</mark>
- **Módem (modulador/demodulador):** se utiliza cuando los datos digitales se envían como transmisión analógica
- **Códec (codificador/decodificador):** se utiliza cuando se envían datos analógicos a través de señales de transmisión digital
### <mark style="background: #FFB86CA6;">Ventajas de la transmisión digital</mark>
- **Menos errores:** más fácil detectarlos y corregirlos porque son binarios. Una onda cuadrada débil se puede propagar fácilmente en forma perfecta.
- **Velocidades de transmisión más altas:** por ejemplo fibra óptica.
- **Más eficiente:** es posible enviar más datos digitales a través de un circuito dado, el circuito puede ser "encapsulado".
- **Más seguro:** más fácil cifrar un _bit stream_.
- **Más simple para VVD (voz, vídeo y datos):** se pueden mezclar más fácilmente (binarios).
## <mark style="background: #ADCCFFA6;">2. Configuración de circuitos</mark>
### <mark style="background: #FFB86CA6;">Punto a punto</mark>
![[Pasted image 20250510201725.png]]
Se usa cuando los hosts generan suficientes datos como para llenar la capacidad del circuito. Cada host tiene su propio circuito para llegar al otro host en la red (caro).
### <mark style="background: #FFB86CA6;">Multipunto</mark>
![[Pasted image 20250510201911.png]]
Se usa cuando cada host no tiene que usar el circuito continuamente. Es mas barato, ya que no hay tantos cables y es más fácil de cablear. Solo un host puede usar el circuito a la vez.
## <mark style="background: #ADCCFFA6;">3. Flujo de datos</mark>
### <mark style="background: #FFB86CA6;">Simplex</mark>
Los datos van nen una sola dirección (radio y TV por cable).
### <mark style="background: #FFB86CA6;">Half-duplex</mark>
Los datos fluyen en ambos sentidos pero solo en uno a la vez (walkie talkies).
### <mark style="background: #FFB86CA6;">Full-duplex</mark>
Los datos fluyen en ambos sentidos a la misma vez.
## <mark style="background: #ADCCFFA6;">4. Modos de transmisión</mark>
### <mark style="background: #FFB86CA6;">Transmisión serie</mark>
![[Pasted image 20250510205021.png|400]]
Se puede usar a distancias más largas ya que los bits permanecen en orden.
$$
\begin{equation}
V_b=\frac{1}{T_b}
\end{equation}
$$
### <mark style="background: #FFB86CA6;">Transmisión en paralelo</mark>
![[Pasted image 20250510205203.png|400]]
Se utiliza para distancias cortas ya que los bits en paralelo tienden a dispersarse.
$$
\begin{equation}
nV_b=\frac{1}{T_b}, \text{n  número de líneas}
\end{equation}
$$
<div class="nota">
<h3>Tipo de sincronización</h3>
<ul>
<li><strong>Heterosincronizada:</strong> El reloj es único y está en el Tx. También es posible situar el reloj en el Rx. Esta estructura no se da con frecuencia.</li>
<li><strong>Autosincronizada:</strong> Única línea por la que se envían datos y CLK. El reloj tiene que estar en el Tx. Para conseguir esto, se recurre a algo llamado <strong>CÓDIGO DE LÍNEA</strong></li>
</ul>
</div>
$$
\begin{equation}
\frac{T}{2}>(T'-T)\times{L}
\end{equation}
$$
T' = CLK - retraso
T = CLK + retraso
L = longitud de la trama
## <mark style="background: #ADCCFFA6;">5. Técnicas de señalización</mark>
Define cómo los niveles de voltaje se corresponden con valores binarios. Hay varios ejemplos: unipolar, bipolar, RTZ, NRZ, Manchester. También describe con qué frecuencia el transmisor puede transmitir datos.
### <mark style="background: #FFB86CA6;">Manchester</mark>
Utilizado en Ethernet. Define un valor de bit por una transición de medio bit, es decir, pasar de alto a bajo es un 0 y pasar de bajo a alto es un 1.
- **Bifase:** siempre hay una transición a mitad del bit que recupera el CLK y los datos. Usado en IEEE 802.3
- **Bifase diferencial:** la transición en la mitad del bit es solo para sincronizar. Usado en IEEE 802.5.
### <mark style="background: #FFB86CA6;">NRZ-L (Non Return to Zero-Level)</mark>
- Niveles de tensión distintos para 0 y 1
- Nivel de tensión constante mientras dure el bit (no retorna a 0)
- Frecuentemente, tensión negativa para un valor y positiva para el otro
### <mark style="background: #FFB86CA6;">NRZ-I (Non Return to Zero-Inverted)</mark>
- Los datos se codifican mediante presencia o ausencia de una transición de la señal al principio del intervalo
- Nivel de tensión constante durante cada bit
- Es **codificación diferencial** ya que se representan los datos por cambios. Es más fácil detectar flancos. Arrastra errores por la polaridad.
## <mark style="background: #ADCCFFA6;">6. Transmisión serie asíncrona</mark>
- CLK en Rx y Tx independientes
- La información se divide en grupos de bits llamados caracteres. Cara caracter está compuesto de 5 u 8 bits + unos bits de cabecera y cola que permiten corregir desviaciones temporales en el Rx respecto al Tx.
- Para una transmisión:
	1. Bit de Start ('0')
	2. Datos de uno en uno de LSB a MSB
	3. Bit de Stop ('1')
# <mark style="background: #FFF3A3A6;">TEMA 5: Medios de transmisión</mark>
Para seleccionar un tipo de medio u otro se suele tener en cuenta:
- **Ancho de banda:** cuanto más, más rápida la transmisión
- **Dificultades en la transmisión:** por ejemplo atenuación de señal
- **Interferencias**
- **Número de receptores:** a más Rx más atenuación
## <mark style="background: #ADCCFFA6;">1. Medios guiados</mark>
### <mark style="background: #FFB86CA6;">Par trenzado UTP VS STP VS S-FTP</mark>
- UTP: par trenzado normal
- FTP: par trenzado apantallado con un "papel de aluminio"
- S-FTP: par trenzado apantallado y envuelto en una malla
  ![[Pasted image 20250510221130.png|500]]
  ![[Pasted image 20250510221146.png|500]]
### <mark style="background: #FFB86CA6;">Coaxial</mark>
![[Pasted image 20250510221309.png]]
Alambre de cobre formado por núcleo y malla. Buen ancho de banda y buena inmunidad al ruido. 50$\Omega$ digital, 75$\Omega$ analógico.
- Mayor ancho de banda que el UTP
- Menor atenuación
- Ideal para señales analógicas como TV
### <mark style="background: #FFB86CA6;">Fibra óptica</mark>
![[Pasted image 20250510221553.png|500]]
Se basa en la reflexión total interna para transmitir la luz. Pueden usarse LED o Láser. Hay una relación entre la longitud de onda, el tipo de fibra y la velocidad de transmisión.
## <mark style="background: #ADCCFFA6;">2. Medios no guiados</mark>
### <mark style="background: #FFB86CA6;">Radio</mark>
- Omnidireccional
- Un Tx varios Rx
- Bandas: LF, MF, HF, VHF
- Fáciles de generar
- Largas distancias
- Atraviesan paredes
- Absorbidas por la lluvia
- Interferencias posibles por equipos eléctricos
#### <mark style="background: #D2B3FFA6;">Factores que afectan a la transmisión en <i>line of sight</i></mark>
- Pérdida en el espacio libre por distancia
- Absorción atmosférica: vapor de agua (22GHz) y oxígeno (60GHz)
- Trayectorias múltiples
- Refracción
### <mark style="background: #FFB86CA6;">Microondas</mark>
![[Pasted image 20250510222011.png|500]]
![[Pasted image 20250510222038.png|500]]
- Frecuencias muy altas de 3GHz a 100GHz
- Longitud de onda muy pequeña
- Antenas parabólicas
- Rx y Tx en _line of sight_
- A 100m de altura se alcanzan 80Km sin repetidores
- Rebotan en los metales (radar)
### <mark style="background: #FFB86CA6;">Satélites</mark>
![[Pasted image 20250510222503.png|700]]

| Banda | Frecuencia | Uso                       |
| ----- | ---------- | ------------------------- |
| L     | 1 GHz      | Antenas omnidireccionales |
| S     | 2 GHz      | NASA                      |
| C     | 6/4 GHz    | Comercial, teléfono       |
| X     | 8/7 GHz    | Militar, Gobierno         |
| Ku    | 14/12 GHz  |                           |
| Ka    | 30/20 GHz  | Intersatélite             |
| V     | 40 GHz     |                           |
| Q     | 60 GHz     |                           |
#### <mark style="background: #D2B3FFA6;">Ventajas</mark>
- Comunicación sin cables
- Gran cobertura
- Disponibilidad de banda ancha
- Instalación rápida de una red
- Bajo coste añadir otro Rx
- Servicio total proporcionado por el satélite en sí
# <mark style="background: #FFF3A3A6;">TEMA 6: Alteraciones en las transmisiones</mark>
## <mark style="background: #ADCCFFA6;">1. Atenuación</mark>
- La potencia de la señal decae con la distancia
- Depende del medio
- Se suele medir en **dB/m**
- La potencia debe ser suficiente fuerte para ser detectada y lo suficientemente más grande que el ruido para recibirse sin error
- **Se usan amplificadores/repetidores**
- **Es mayor a más frecuencia**
### <mark style="background: #FFB86CA6;">Ganancia y atenuación</mark>
#### dB = Indica la relación entre dos valores de P, V o I
La ganancia expresada en dB sería:
- Relación de potencias: $G(dB)=10\times\log_{10}{\frac{P_2}{P_1}}$
- Relación de tensiones: $G(dB)=20\times\log_{10}{\frac{V_2}{V_1}}$ ya que $P=V^2/R$ y el cuadrado multiplica fuera
- Relación de intensidades: $G(dB)=10\times\log_{10}{\frac{I_2}{I_1}}$
#### dBm = unidad de medida absoluta que mide potencia respecto a 1mW
$$
\begin{equation}
P(dBm)=10\times\log_{10}{\frac{P(mW)}{1mW}}
\end{equation}
$$
Esto es muy util si hay atenuaciones y amplificaciones (ganancias) sucesivas:
$$
\begin{equation}
P_2(dBm)=P_1(dBm)+\sum_i{G_i(dB)}-\sum_j{A_j(dB)}\rightarrow\text{G, A: Ganancia, Atenuación}
\end{equation}
$$
#### Pérdida en medios no guiados
$$
\begin{equation}
L=10\times\log(4\pi/\lambda)^2~~~\text{(dB)}
\end{equation}
$$
## <mark style="background: #ADCCFFA6;">2. Desvanecimiento</mark>
Desaparece la señal de forma transitoria. En teoría se puede restablecer en el Rx con control automático de ganancia, a menos que sea muy pequeña. Es causado por condiciones atmosféricas.
## <mark style="background: #ADCCFFA6;">3. Distorsión armónica</mark>
- Ocurre sólo en medios guiados
- **La velocidad de propagación varía con la frecuencia**
- Particularmente crítico en datos digitales a velocidades altas
## <mark style="background: #ADCCFFA6;">4. Ruido</mark>
Son señales no deseadas que se superponen con la señal que se quiere transmitir a lo largo del proceso de transmisión.
#### <mark style="background: #FFB86CA6;">Ruido térmico</mark>
Se debe al calor, aparece en todos los dispositivos electrónicos
$$
\begin{equation}
N_0=k\times{T}~~~\text{(W/Hz)}
\end{equation}
$$
$$
\begin{equation}
k=1,38\times10^{-23}~\text{J/ºK (constante de Boltzmann)}
\end{equation}
$$
$$
\begin{equation}
T\equiv\text{temperatura en Kelvin}
\end{equation}
$$
### <mark style="background: #FFB86CA6;">Ruido magnético natural</mark>
- **Atmosférico:** producido por perturbaciones en la atmósfera. El rayo es la fuente más visible de este tipo de ruido. Se propagan como las ondas de radio. Es importante hasta los 20KHz y en TV (500MHz).
- **Espacial:** 
	- Solar: erupciones solares
	- Cósmico: rayos cósmicos
### <mark style="background: #FFB86CA6;">Ruido magnético artificial</mark>
- Arrancar y parar motores
- Vibraciones mecánicas -> eléctricas
- Fluorescentes
- Diafonía
- Conmutación de circuitos
### <mark style="background: #FFB86CA6;">Ruido de rebote en los cables</mark>
![[Pasted image 20250510224703.png]]
$$
\begin{equation}
V_{TOTAL}=V_++V_-
\end{equation}
$$
$$
\begin{equation}
\rho\equiv\text{coeficiente de reflexión}=\frac{V_-}{V_+}
\end{equation}
$$
### <mark style="background: #FFB86CA6;">Ruido por diferencia en GNDs</mark>
![[Pasted image 20250510224952.png|500]]
Se produce cuando hay diferencias de tension en la GND del Tx y Rx. Es un problema importante. Puede producir problemas graves.
### <mark style="background: #FFB86CA6;">Ruido de intermodulación</mark>
Superposición de señales al multiplexar varias en un canal
### <mark style="background: #FFB86CA6;">Ruido de cuantización</mark>
![[Pasted image 20250510225115.png]]
Ocurre al digitalizar. Son los "trozos sobrantes" de la señal digital respecto a la analógica.
### <mark style="background: #FFB86CA6;">Ruido AWGN</mark>
$$
\begin{equation}
p(z)=\frac{1}{\sqrt{2\pi\sigma}}e^{-(z-\mu)^2/2\sigma^2}
\end{equation}
$$
ADITIVO + BLANCO + GAUSSIANO = AWGN
## <mark style="background: #ADCCFFA6;">5. Fórmulas</mark>
### <mark style="background: #FFB86CA6;">Nyquist (capacidad del canal)</mark>
Canales ideales, libres de ruidos. Si cada forma de onda transmite un bit, tenemos $C=2B\text{ bps}$. Se mejora usando señales multinivel
$$
\begin{equation}
C=2B\times\log_2{M}~~~\text{(bps)}
\end{equation}
$$
### <mark style="background: #FFB86CA6;">Shannon (capacidad del canal)</mark>
Considera el ruido del canal. Relaciona la capacidad y la relación señal/ruido (SNR).
$$
\begin{equation}
SNR=\frac{\text{Potencia señal (S)}}{\text{Potencia ruido (N)}}
\end{equation}
$$
$$
\begin{equation}
SNR_{dB}=10\log_{10}{\frac{\text{Potencia señal}}{\text{Potencia ruido}}}~~~\text{(dB)}
\end{equation}
$$
$$
\begin{equation}
C=B\log_2{(1+SNR)}~~~\text{(bps)}
\end{equation}
$$
### <mark style="background: #FFB86CA6;">BER (Bit Error Rate)</mark>
$$
\begin{equation}
BER=\frac{\text{nº de bits erroneos}}{\text{nº de bits transmitidos}}
\end{equation}
$$
### NOTAS
- Si SNR es grande la comunicación es buena (mas señal que ruido)
- Si es pequeño, mala o imposible

# <mark style="background: #FFF3A3A6;">TEMA 7: Digitalización, modulación, multiplexión</mark>
## <mark style="background: #ADCCFFA6;">1. Muestreo de una señal</mark>
$$
\begin{equation}
f_{MUESTREO}\geq2\times{f_{MÁX.}}
\end{equation}
$$
