# <mark style="background: #FFF3A3A6;">TEMA 1: Redes de Computadores</mark>
## <mark style="background: #ADCCFFA6;">1. ¿Qué es Internet?</mark>
Internet son millones de dispositivos de computación (sistemas finales o hosts) ejecutando aplicaciones de red, conectados mediante enlaces de comunicación físicos (fibra, cobre) o inalámbricos (radio, satélite ).
## <mark style="background: #ADCCFFA6;">2. ¿Qué es un protocolo?</mark>
Un protocolo define el formato y orden de los mensajes enviados y recibidos entre entidades de red, y las acciones tomadas en la transmisión y/o recepción de un mensaje.
## <mark style="background: #ADCCFFA6;">3. La frontera de la red</mark>
El núcleo de la red son routers interconectados, una red de redes. En la frontera de la red hay dos modelos:
- **Cliente/Servidor:** sistema terminal cliente solicita y recibe un servicio de un servidor. Ej.: Navegador/Servidor web...
- **Peer-Peer:** mínimo uso o ninguno de servidores dedicados. Ej.: Skype, BitTorrent...
En la estructura de la red existen varios medios físicos guiados y no guiados.
- **Guiados:** las señales se transportan por un medio sólido: par trenzado, coaxial, fibra.
- **No guiados:** las señales se propagan del espacio: microondas, WLAN (WiFi), móviles (4G), satélite.
## <mark style="background: #ADCCFFA6;">4. El núcleo y estructura de la red</mark>
Los datos se transfieren de distinta forma en la red:
- **Conmutación de circuitos:** circuito dedicado por llamada, donde se reservan recursos (red telefónica). El ancho de banda es la capacidad del router. Los recursos dedicados no se comparten. El rendimiento se garantiza al reservar recursos. Se puede dividir el enlace en frecuencia o tiempo.
- **Conmutación de paquetes:** los datos se envían a través de la red en pequeños fragmentos (paquetes). Los paquetes de distintos usuarios comparten recursos de la red. Cada paquete usa el ancho de banda completo. Los recursos no se reservan se usan según se necesitan.
La red se divide en lo siguiente:
![[Pasted image 20240304111735.png|650]]
## <mark style="background: #ADCCFFA6;">5. Rendimiento</mark>
Hay cuatro fuentes de retardos:
- $d_{proc}\equiv$ retardo de procesamiento:
	Determinar enlace de salida, y comprobar errores a nivel de bit. Típicamente menos de $\mu s$.
- $d_{tx}\equiv$ retardo de transmisión:
	$L\equiv$ longitud del paquete (bits).
	$R\equiv$ ancho de banda (bps).
	$d_{tx}=\frac{L}{R}$ 
- $d_{prop}\equiv$ retardo de propagación:
	$d\equiv$ longitud del enlace físico.
	$s\equiv$ velocidad de propagación.
- $d_{cola}\equiv$ retardo de cola:
	Tiempo de espera antes de ser transmitido. Depende del nivel de congestión del router. Típicamente $\mu s$ o $ms$. $d_{cola}=a·d_{tx}$
$$
\begin{equation}
d_{nodal}=d_{proc}+d_{cola}+d_{tx}+d_{prop}
\end{equation}
$$
## <mark style="background: #ADCCFFA6;">6. Capas de protocolos, modelos de servicio</mark>

### - Encapsulación
![[Pasted image 20240304114201.png]]
### - Desencapsulación
![[Pasted image 20240304114218.png]]
### - Fragmentación
![[Pasted image 20240304114226.png]]
### - Reensamblaje
![[Pasted image 20240304114233.png]] 
# <mark style="background: #FFF3A3A6;">TEMA 2: Capa de Aplicación</mark>
## <mark style="background: #ADCCFFA6;">1. Principios de las aplicaciones en red</mark>
Un proceso es un programa que se ejecuta en un equipo. El proceso cliente es el que inicia la comunicación y el servidor el que espera a ser contactado. Un proceso envía/recibe mensajes a/de su socket. Un socket es como una puerta el emisor envía el mensaje a través de la puerta de salida y el emisor la recibe por la puerta de entrada.
## <mark style="background: #ADCCFFA6;">2. DNS</mark>
Base de datos distribuida (descentralizada) implementada con una jerarquía de servidores de nombres. Las funciones del DNS son:
- Traducir dominio a IP.
- Traducir IP a dominio.
- Permite asociar alias a dominios.
- Permite asignar servidores de correo a un dominio.
Usa UDP como protocolo. El cliente envía mensajes al puerto 53 del servidor.
## <mark style="background: #ADCCFFA6;">3. Web y HTTP</mark>
Una página web consiste en un fichero HTML que contiene una serie de objetos referenciados. Los objetos pueden ser ficheros HTML, imágenes JPEG, applets Java... El fichero HTML en sí es un objeto más. El fichero base no contiene los objetos referenciados (anchors), lo que contiene son las referencias a los objetos. Cada objeto es direccionable a través de su URL (Uniform Resource Locator).
`www.us.es/centros/index.html`
Hostname  ----- Pathname -----

HTTP: Hyper Text Transfer Protocol. Protocolo de nivel de aplicación para la web. Modelo cliente/servidor. Usa TCP. El HTTP puede ser persistente o no persistente. El RTT (Round-Trip Time) o tiempo de ida y vuelta es el tiempo que tarda desde que se solicita hasta que se recibe una respuesta. El tiempo de respuesta (TR) es $TR=2·RTT+TTO$. 
Ejemplo de petición HTTP:
![[Pasted image 20240304122600.png]]
Un ejemplo de mensaje de respuesta HTTP:
![[Pasted image 20240304122744.png]]
# <mark style="background: #FFF3A3A6;">TEMA 3: Capa de transporte</mark>
## <mark style="background: #ADCCFFA6;">1. Servicios de nivel de transporte</mark>
Proporciona comunicación lógica entre aplicaciones de hosts diferentes. Los protocolos de transporte se ejecutan en los hosts, no en los routers. La capa de transporte extiende el servicio de la capa de red para proporcionar un servicio de comunicación lógica entre los procesos de aplicación.
## <mark style="background: #ADCCFFA6;">2. Multiplexión y demultiplexión</mark>
- **Multiplexión:** Recolectar datos de múltiples sockets, crear los T_PDU añadiendo la T_PCI que se usará luego al demultiplexar. 
- **Demultiplexión:** entregar en el socket correcto el T_UD de los T_PDU recibidos gracias a la T_PCI.
![[Pasted image 20240304123937.png]]
## <mark style="background: #ADCCFFA6;">3. Transporte sin conexión: UDP (User Datagram Protocol)</mark>

![[Pasted image 20240304124333.png]]
### - El checksum
El emisor trata la T_PDU como una secuencia de enteros de 16 bits. Simplificando un poco, podemos decir que suma todos los enteros de 16 bits que componen la T_PDU y luego calcula el Ca1. Coloca el valor calculado en el campo de checksum de la T_PCI.
El receptor calcula el checksum, otra vez, de la misma forma que lo hizo el emisor, sobre la T_PDU. Comprueba si el checksum calculado es idéntico al valor del campo checksum de la T_PDU recibida.
## <mark style="background: #ADCCFFA6;">4. TCP (Transmission Control Protocol)</mark>

![[Pasted image 20240304124953.png]]
# <mark style="background: #FFF3A3A6;">TEMA 4: La Capa de Red</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
- El nivel de red envía las T_PDU desde un sistema final origen a uno destino.
- En el emisor se encapsula cada T_PDU en un R_PDU.
- En el receptor se entregan las T_PDU al nivel de transporte.
- El nivel de red está presente tanto en sistemas finales como en routers.
- Los routers examinan la R_PCI de todas las R_PDU.
Algunos de los servicios que podría proporcionar una capa de red del modelo OSI son:
- Entrega garantizada.
- Entrega garantizada con retardo limitado.
- Entrega de los paquetes en orden.
- Ancho de banda mínimo garantizado.
- Seguridad.
### - Comunicación con el Nivel de Enlace
Cada interfaz de red implementa un determinado protocolo de nivel de enlace/físico conocido como **tecnología de enlace**, **tecnología de red** o simplemente **tecnología**.
Cada interfaz tiene asociada una **dirección física** o **dirección MAC** de **48 bits**. Por ejemplo, 00:BF:3C:23:45:30.
### - Límites del dominio de broadcast
El nivel de enlace a través de la interfaz, le ofrece al nivel de red un servicio no fiable de entrega de R_PDUs entre routers o sistemas finales conectados mediante dispositivos como mucho hasta nivel de enlace (switch, hub, etc).
Los routers y sistemas finales conectados de esta forma se dice que están en el mismo **dominio de broadcast**. 
### - Direccionamiento
Esta función permite definir una forma de identificar únicamente a todos los dispositivos conectados. Se conoce como **dirección de nivel 3/de red** (dirección IP en caso de TCP/IP). Todo dispositivo que tenga nivel de red tiene dirección de nivel de red. Se usan esquemas de direccionamiento jerárquico **Red.Host**.
- Una parte de la dirección identifica la red lógica (**Red**).
- Otra parte identifica el dispositivo con nivel 3 (**Host**).
### - Dos funciones clave
- **Reenvío (forwarding):** el router mueve R_PDU de una interfaz de entrada a la de salida apropiada. **Analogía:** camino a tomar en una intersección.
- **Enrutamiento (routing):** el router determina la ruta que han de tomar las R_PDU desde un origen a un destino. **Analogía:** planificar un viaje origen-destino.
### - Interacción enrutamiento - envío
Cada router tiene su tabla de enrutamiento (routing tables).
![[Pasted image 20240318182742.png]]
## <mark style="background: #ADCCFFA6;">2. Router en redes de datagrama</mark>
### - Interfaces de un router
La tecnología que se utilice **en cada interfaz** de un router es independiente (puede tener Wi-Fi, Ethernet, etc). Las interfaces se identifican con una letra que indica la tecnología y un número para distinguirlas si hay varias de la misma tecnología:
- Ethernet: E0, E1...
- Fast Ethernet: Fa0, Fa1...
- Gigabit Ethernet: Gi0, Gi1...
### - Redes lógicas
- En cada interfaz de un router, utilizando el medio apropiado, se pueden conectar sistemas finales u otros routers (pudiendo usar dispositivos hasta nivel de enlace). 
- Todos los equipos conectados en el mismo dominio de broadcast pertenecen a la misma red lógica, y procesarán todas las E_PDU que en la E_PCI tengan una dirección física broadcast/multicast.
- Cada interfaz de un router está en un dominio de broadcast diferente.
- Un router no es transparente.
### Ejemplo
<hr>
1. **¿Cuántos dominios de broadcast hay? ¿Cuántas redes lógicas hay?**
   Dos dominios de broadcast (azul y naranja). Dos redes lógicas (1 y 2).
2. **¿Y si a 1.3 y 1.4 se le asignase en la dirección de nivel 3 el identificador de red 3?**
   Seguirían habiendo dos dominios y habrían tres redes (1, 2, 3).
3. **Si 1.1 quiere enviar datos a 2.2. ¿A quién tendrá que entregar la R_PDU que encapsule esos datos el nivel de red de ese sistema final? ¿Qué direcciones de nivel 3 origen y destino aparecerán en esa R_PDU?**
   Se lo tiene que entregar al siguiente dispositivo con nivel de red. Direcciones origen 1.1 y destino 2.2.
4. **¿Qué dirección de nivel 3 origen tendrá la R_PDU que reciba el sistema final 2.2 en la R_PCI?**
   1.1.
### - Tablas de enrutamiento
Las dos funciones del nivel de red hacen uso de la tabla:
- La función de enrutamiento para modificar su contenido.
- La función de reenvío para consultar la interfaz de salida.
Tanto los sistemas finales como los routers tienen tablas de enrutamiento.
Una entrada de la TE contiene como mínimo:
![[Pasted image 20240318185453.png|500]]
Las entradas de la TE se rellenan:
- **Automáticamente:** al asignar direcciones de nivel 3 a los dispositivos con nivel de red se incluye una entrada para la red lógica a la que este pertenece.
- **Manualmente:** mediante comandos.
- **Dinámicamente:** Mediante protocolos de enrutamiento que implementan algoritmos que determinan el mejor camino hacia una determinada red lógica. Específico de los routers.
Cuando el nivel de red tiene una R_PDU que debe enviar debe usar la R_PCI para averiguar el próximo salto.

| Red | Próximo Salto | Interfaz |
| --- | ------------- | -------- |
| 1   | ------        | E        |
| 2   | 1.2           | E        |
| 3   | 1.2           | E        |
| 4   | 1.2           | E        |
**Tabla Host 1.1**

| Red | Próximo Salto | Interfaz |
| --- | ------------- | -------- |
| 1   | ------        | E0       |
| 2   | ------        | E1       |
| 4   | ------        | E2       |
| 3   | E0_R2         | E2       |
**Tabla Router 1**

| Red | Próximo Salto | Interfaz |
| --- | ------------- | -------- |
| 3   | ------        | E1       |
| 4   | ------        | E0       |
| 1   | E2_R1         | E0       |
| 2   | E2_R1         | E0       |
### Ejemplo 2
<hr>
**¿Es posible que lleguen las R_PDU de 1.3 a 3.1? ¿Y viceversa?**
   
| Red       | Próximo Salto | Interfaz |
| --------- | ------------- | -------- |
| 1         | ------        | E        |
| 0.0.0.0/0 | 1.2           | E        |
**Tabla Host 1.1**

| Red | Próximo Salto | Interfaz |
| --- | ------------- | -------- |
| 1   | ------        | E0       |
| 3   | ------        | E2       |
**Tabla Router 1**

| Red       | Próximo Salto | Interfaz |
| --------- | ------------- | -------- |
| 3         | ------        | E        |
| 0.0.0.0/0 | 3.N           | E        |
**Tabla Host 3.1**

Sí se puede. Viceversa también.
## <mark style="background: #ADCCFFA6;">3. IP: Protocolo de Internet</mark> 
![[Pasted image 20240321160110.png|500]]
### <mark style="background: #D2B3FFA6;">- Formato del datagrama IPv4</mark>
![[Pasted image 20240321160653.png|500]]
Cada tecnología de enlace tiene limitada la cantidad máxima de bytes que puede transportar de E_SDU (R_PDU) conocido como MTU (Maximum Transfer Unit). Si el tamaño de la R_PDU es mayor que la MTU de la interfaz se fragmenta en varias R_PDU de tamaño adecuado. El nivel de red par se encarga de reensamblar.
### <mark style="background: #D2B3FFA6;">- Direccionamiento IPv4</mark>
Permite definir de forma única a todos los dispositivos de una red. Cada dispositivo que tenga nivel de red tiene su dirección de nivel de red del esquema <mark style="background: #D2B3FFA6;">Red</mark> **.** <mark style="background: #BBFABBA6;">Host</mark>.
- Se usan 32 bits (4B) con un esquema tal que:
  ![[Pasted image 20240321162041.png|250]]
- La notación es:
  Dirección de 32b: 11001000001010001000000000100000
  Agrupada en Bytes: 11001000 00101000 10000000 00100000
  Formateado a decimal con puntos: 200.40.128.32
- Se pueden configurar manualmente o automáticamente.
#### <mark style="background: #FFB86CA6;">Tipos de direcciones IPv4</mark>
- **Unicast:** Sirven para enviar IP_PDU a un único destino.
- **Broadcast:** Usada como destino, sirve para enviar IP_PDU a todos los dispositivos de una red lógica.
- **Multicast:** Usada como destino, sirve para enviar IP_PDU a un grupo de dispositivos de una red lógica.
#### <mark style="background: #FFB86CA6;">Direcciones especiales</mark>
- **Todos los bits a '0' (0.0.0.0):** O no se ha empezado a configurar el dispositivo o es la ruta por defecto en la TE.
- **ID de red $\neq$ 0 y bits de host a '0' (ej: 192.168.0.0):**  Identificador de la red lógica (se usa en la TE).
- **ID de red $\neq$ 0 y bits de host a '1':** Broadcast dirigido. Identifica todos los dispositivos de una red.
- **Todos los bits a '1' (255.255.255.255):** Broadcast limitado. Identifica todos los dispositivos de la red a la que pertenezca el origen.
- **127.0.0.1 (localhost):** Comunicar procesos internamente en un equipo.
#### <mark style="background: #FFB86CA6;">Esquemas de direccionamiento</mark>
- <mark style="background: #FF5582A6;"><b>Classful:</b></mark>
  ![[Pasted image 20240321163752.png|500]]
  Clase A: 1.0.0.0 - 126.0.0.0 -> $2^7-2$ redes de $2^{24}-2$ direcciones asignables.
  Clase B: 128.0.0.0 - 191.255.0.0 -> $2^{14}-2$ redes de $2^{16}-2$ direcciones asignables.
  Clase C: 192.0.0.0 - 223.255.255.0 -> $2^{21}-2$ redes de $2^8-2$ direcciones asignables.
- <b><mark style="background: #FF5582A6;">Classless:</mark></b>
  Para determinar qué parte identifica a la red y que parte al host se usa la notación barra (**/x**). **x** puede valer de 0 a 32, por ejemplo 223.234.0.0/16 sería clase B.
  Dado un prefijo **/x** se pueden direccionar $2^{32-x}-2$ dispositivos, por ejemplo 223.234.0.0/16 en la que la dirección de broadcast sería 223.234.255.255 y el rango de IPs asignables sería 223.234.0.1 - 223.234.255.254.
	  - **Máscara de red:** la máscara de red o de subred es un número de 32b en los que los primeros X bits están a '1' y los 32-X bits están a '0'. Ejemplo: /22 -> Red 22b a '1', Host 10b a '0'. 255.255.252.0. Ejemplo con IP: 223.234.0.25/17 sería: 223.234.0.25 (IP), 255.255.128.0 (Máscara).
  Este esquema se conoce como **CIDR (Classless InterDomain Routing)**. Permite asignar los bloques de direcciones IPv4 según necesidades reales, sin desperdiciar IPs.
  ### EJEMPLO Dir sin clase
  <hr>
  223.1.4.1/22 (IP)
  255.255.252.0 (netmask)
  **Broadcast de 223.1.4.0/22 ->** 223.1.7.255
  
  223.1.0.0/10 = 223.63.255.255
  223.00000001.0.0 -> 223.00111111.255.255 -> 223.63.255.255
  
  $2^{10}-2$ dispositivos/direcciones
  
  **Host:** 223.1.12.2/22
  
| Red        | Próximo Salto | Interfaz |
| ---------- | ------------- | -------- |
| 223.1.4.0  | ------        | E0       |
| 223.1.12.0 | ------        | E2       |
| 223.1.8.0  | ------        | E1       |
Router

| Red        | Próximo Salto  | Interfaz |
| ---------- | -------------- | -------- |
| 223.1.12.0 | ------         | E        |
| 0.0.0.0/0  | 223.1.12.27/22 | E        |
Host

223.1.4.3/22 =   11011111 00000001 00000100 00000011
255.255.252.0 = 11111111 11111111 11111100 00000000
and = 11011111 00000001 00000100 00000000 = 223.1.4.0
La operación and entre la IP destino y la netmask indica si esa entrada en la TE se elige o no (si coincide con el ID de red se elige si no no).
#### <mark style="background: #FFB86CA6;">Creación de subredes con CIDR</mark>
200.215.10.0/24 
- ID: 200.215.10.0
- Broadcast: 200.215.10.255
Para hacer una/s subred/es (ejemplo 5) se "piden prestados" bits a los de host, en este caso 8, así que se pueden pedir 3b (101 son 3b) a los 8 para las subredes.

## 200.23.16.0/23 = 11001000 00010111 00010000 00000000
### 200.23.16.0/24 = 1101000 00010111 00010000 00000000
### 200.23.17.0/24 = 1101000 00010111 00010001 00000000
#### 200.23.16.1/25 = 1101000 00010111 00010001 00000001
.
.
.
La búsqueda en la TE se realiza empezando por las redes /32 y se acaba con las redes /0. Se elige como próximo salto una red que tenga mayor Nº de bits en común con la IP destino.

<hr>

**4 Subredes**
10.20.30.224/27 = <mark style="background: #FFB8EBA6;">00001010 00010100 00011110 111</mark><mark style="background: #FFB86CA6;">00000</mark>
- ID de red = 10.20.30.224
- Broadcast = 10.20.30.255
- Dir IP's asignables = 10.20.30.225 - 10.20.30.254 = 29 direcciones
10.20.30.224/27 pasa a /29:
- 10.20.30.232/29 [224,231]
- 10.20.30.240/29 [232,239]
- 10.20.30.248/29 [240,247]
- 10.20.30.254/29 [248,255]
#### <mark style="background: #FFB86CA6;">IP's privadas y NAT (Network Access Translation)</mark>
Hay unos rangos de IP's para asignar sólo a redes privadas y **no pueden aparecer en internet**.
![[Pasted image 20240408191837.png]]
El NAT se suele implementar en los routers.
![[Pasted image 20240408191957.png]]
### <mark style="background: #D2B3FFA6;">- Introducción a ICMP (Internet Control Message Protocol)</mark>
Usado por sistemas finales y routers para comunicar información a nivel de red (errores, host inalcanzable, etc.). Las ICMP_PDUs son encapsuladas en IP_PDUs (funciona sobre IP). Ejemplos de mensajes ICMP son:
- _Echo Request_/_Echo Reply_ usado por ping.
- _TTL excedido_ usado por tracert.
### <mark style="background: #D2B3FFA6;">- Funcionamiento</mark>
El nivel de red requiere que los dispositivos estén configurados, es decir:
- IP configurada, máscara y router por defecto (router frontera). Se puede hacer manualmente o automáticamente (DCHP, _Dynamic Host Configuration Protocol_).
- En los routers hay que configurar la IP de cada interfaz activa y su correspondiente máscara.
- En los hosts es recomendable configurar uno o varios servidores DNS para resolver IP's.
- La TE tiene que estar rellena. La de un host con sus dos entradas y la del router para cada red accesible por este.
# <mark style="background: #FFF3A3A6;">TEMA 5: La Capa de Enlace</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción y servicios</mark>
### <mark style="background: #D2B3FFA6;">- Terminología</mark>
- **Nodo:** Todo dispositivo que tenga nivel de enlace.
- **Enlace (link):** Canales de comunicación que conectan nodos adyacentes (medios físicos guiados y no guiados). Pueden ser **punto a punto** (dos nodos conectados por un medio físico) o **multipunto** (varios nodos conectados por un medio físico).
- **Trama (frame):** Es la E_PDU.
### <mark style="background: #D2B3FFA6;">- Contexto</mark>
El nivel de enlace transfiere las E_PDUs de un nodo a otro adyacente por el enlace. Dependiendo del medio usado es posible que el nivel de enlace pueda enviar y recibir simultáneamente (**full-duplex**) o no simultáneamente (**half-duplex**).
## <mark style="background: #ADCCFFA6;">2. Funciones</mark>
- **Construir la trama:** [Cabecera | E_UD | Cola] pero en el contexto de la asignatura será
  [E_PCI | E_UD]
- **Sincronismo de trama:** Distinguir dónde empieza y acaba cada E_PDU dentro del flujo de datos.
- **Identificación de nodos (direccionamiento):** Direccionar cada nodo por su _dirección física/MAC_.
- **Detección de errores:** Añadiendo bits adicionales a la E_PCI.
- **Control de acceso al medio:** En medios compartidos hay dos soluciones:
	- **Centralizado** si un nodo "master" gestiona el acceso de otros nodos "esclavos" al medio.
	- **Distribuido** si todos los nodos se coordinan para saber en cada momento "a quién le toca". Se puede realizar por:
		- **Contienda:** Un nodo usa el enlace si el nivel físico informa de que está libre. En caso de que colisionen se espera un tiempo aleatorio para retransmitir.
		- **Rotación circular:** Cada nodo accede al medio pasado un tiempo fijo _t_.
## <mark style="background: #ADCCFFA6;">3. Redes de Área Local (LAN)</mark>
Tecnología de red más usada. Permiten conectar los sistemas finales y routers dentro del dominio de broadcast. Implementan a través de la interfaz de red, los niveles de enlace y físico.

**NOTA:** El nivel de enlace se divide en dos subniveles _LLC (Link Layer Control)_ que realiza por ejemplo el control de flujo y _MAC (Medium Access Control)_ que realiza por ejemplo el sincronismo, la detección de errores o el direccionamiento. 
### <mark style="background: #D2B3FFA6;">- Direcciones MAC</mark>
Una dirección MAC es un número de 48 bits. Se representa en distintas notaciones pero en todas se agrupan en bytes en hex:
- 1B:03:F2:45:78:25
- 1B.03.F2.45.78.25
- 1B03F2.457825
Existen tres tipos de MAC:
- **Unicast:** Sirven para enviar E_PDUs a un único destino.
- **Broadcast:** Usada como destino, sirve para enviar E_PDUs a todos los nodos del dominio de broadcast (FF:FF:FF:FF:FF:FF).
- **Multicast:** Usada como destino, sirve para enviar E_PDUs a un grupo de nodos del dominio de broadcast (1b en el LSB).
### <mark style="background: #D2B3FFA6;">- Ethernet 802.3</mark>
Tecnología LAN mas usada, ya que es fácil de usar y barata. Muchos estándares distintos:
- En común el subnivel MAC.
- Permite multipunto o punto a punto.
- Diferentes protocolos de nivel físico.

Los nodos se conectan mediante un dispositivo llamado **concentrador** o **hub**. Un hub es un dispositivo de nivel 1 que permite que todo lo que se envíe por una interfaz llegue al resto de interfaces. 

Un nodo se considera a sí mismo destino de una MAC_PDU si el campo de dirección MAC de la MAC_PCI tiene un valor que coincide con la **MAC unicast** que tiene de fábrica, la **MAC broadcast** y las **MACs multicast** que tuviera configuradas.

<mark style="background:red;color:white;padding:3px;"><strong>Un enlace multipunto no puede ser NUNCA full-duplex.</strong></mark>
#### <mark style="background: #FFB86CA6;">- MAC_PDU</mark>
IP_PCI = 20B
TCP_PCI = 20B
UDP_PCI = 8B
MAC_PCI = 26B

| <------------- 1B ------------->                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------- |
| Delimitador de Comienzo (8B)                                                                                                  |
| **Direccion MAC destino (6B)**                                                                                                |
| **Dirección MAC origen (6B)**                                                                                                 |
| **Longitud/Tipo (2B)**<br>Si < 1500 longitud<br>Si > 1536 tipo                                                                |
| **MAC_UD (46B-1500B)**<br>Si < 46 se rellena con 0's.<br>----------------------------------<br>                   **Relleno** |
| **CRC (4B)**                                                                                                                  |
(MTU Ethernet = 1500B)
### <mark style="background: #D2B3FFA6;">- Conmutadores, puentes (switches)</mark>
Un switch está formado por un conjunto de interfaces red, cada una de igual o distinta tecnología LAN. Las interfaces se identifican de forma similar a un router (E0, E1, Fa0...). Cada interfaz de un switch es un enlace distinto que puede ser punto a punto o multipunto conocido como **dominio de colisión/ancho de banda**.
E=Ethernet; R:10 Mbps ($10^7$ bps)
Fa=Fast Ethernet; R:100Mbps ($10^8$ bps)
Gi=Gigabit Ethernet; R:1Gbps ($10^9$ bps)
#### <mark style="background: #FFB86CA6;">- Funcionamiento</mark>
Un switch (conocido como **store&forward**) procesa todas las MAC_PDU que recibe para:
- **Aprender** la ubicación del nodo origen de esta.
- **Reenviarla** si procede por una interfaz distinta a la del origen.
Un switch mantiene una tabla de direcciones MAC conocida como tabla de conmutación.
![[Pasted image 20240502153451.png]]
Cada entrada de la TC tiene:
- Interfaz
- MAC
- Timestamp
Se puede rellenar manualmente (no es usual) o dinámicamente. Un switch requiere de _buffers_ para ir almacenando las MAC_PDU que van llegando antes de procesarlas. Introduce _delays_.
## <mark style="background: #ADCCFFA6;">4. Protocolo ARP</mark>
El protocolo ARP provee la dirección MAC destino asociada a una IP destino mediante la caché ARP. Al realizar la búsqueda en esta caché pueden pasar dos cosas:
1. $\exists$ MAC_D $\Rightarrow$ ARP provee la MAC
2. ∄ MAC_D $\Rightarrow$ ARP no puede proveer la MAC, por lo que **desencadena el mecanismo pregunta/respuesta** (ARP Request/Reply) en su dominio de Broadcast (ff:ff:ff:ff:ff:ff) para que el host con dicha MAC responda con esta.
![[Pasted image 20240502161000.png]]
### <mark style="background: #D2B3FFA6;">- Caché ARP</mark>
Las cachés ARP se pueden rellenar manualmente (poco común) o dinámicamente usando el protocolo ARP. En caso de que la IP no tenga MAC asignada, mediante una ARP_PDU tipo Request se solicita la MAC asociada a esta en el dominio de broadcast. Una ARP_PDU tipo Reply, cuyo origen es el dispositivo con esa MAC, indicará que MAC tiene asignada dicha IP.
## <mark style="background: #ADCCFFA6;">5. Ejemplo </mark>
![[Pasted image 20240502162232.png|300]]
a) 
7 (quitando router y switches). 2 (quitando router).

b) 
10Mbps (Ethernet)

c) 
R1: 3b (2^3b = 8b -> E F D B, broadcast, router, id)
R2: 3b (Router, broadcast, id, A, C)

d) 
193.198.25.0/28 ? IDs ? Broadcast ?
Es válido porque hay 4b para host, por tanto se pueden crear dos subredes con 3b para host cada una. 193.198.25.0/29 y 193.198.25.8/29. 193.198.25.7 y 193.198.25.15.

e) 
TE, TC y cachés ARP **VACÍAS**.

![[Pasted image 20240502163736.png|300]]
a)
255.255.255.248

b)
193.198.25.5/29 (E0)

c)
TC: vacía
TE: B,E,F,D (dos entradas, red de arriba y puerta de enlace); A,C (dos entradas, red de abajo y puerta de enlace)
Caché ARP: vacía
![[Pasted image 20240502164547.png|300]] ![[Pasted image 20240502164624.png|300]]
Prueba de conectividad (ping) desde A (193.198.25.10) a C (193.198.25.11)
A consulta su TE y hace la operación AND con la IP destino y cada IP de la TE para averiguar la entrada a escoger de la TE. SWITCH_1 y el resto de cachés ARP están vacías porque están en otro dominio de Broadcast. **Si hubiera más switches en el mismo dominio se verían afectados si sus TC no tuviesen las MAC destino**.

| Interfaz | MAC               |
| -------- | ----------------- |
| E0       | 00:0C:01:00:01:0A |
| E1       | 00:0C:01:00:01:0C |
SWITCH_2

| IP            | MAC               |
| ------------- | ----------------- |
| 193.198.25.11 | 00:0C:01:00:01:0C |
Caché ARP (A)

| IP            | MAC               |
| ------------- | ----------------- |
| 193.198.25.10 | 00:0C:01:00:01:0A |
Caché ARP (C)

| IP  | MAC |
| --- | --- |
|     |     |
Caché ARP (Router)
#### <mark style="background: #ABF7F7A6;">INACTIVIDAD</mark>
Ping de C a F. 
1. C consulta su TE y al no estar en la misma red que F, va al próximo salto, que en este caso es su puerta de enlace predeterminada. Consulta la caché ARP inicialmente vacía y envía una ARP_Request al dominio de broadcast, respondiendo el router con su MAC en una ARP_Reply. 
2. El router consulta su TE y F se encuentra en su misma red. Al no tener la MAC de F, se realiza una ARP_Request/ARP_Reply rellenando las cachés ARP.
![[Pasted image 20240502170240.png|300]]

| Interfaz | MAC               |
| -------- | ----------------- |
| E3       | 00:0C:01:00:01:E0 |
| E1       | 00:0C:01:00:01:0F |
SWITCH_1

| Interfaz | MAC               |
| -------- | ----------------- |
| E1       | 00:0C:01:00:01:0C |
| E2       | 00:0C:01:00:01:E1 |
SWITCH_2

| IP           | MAC               |
| ------------ | ----------------- |
| 193.198.25.9 | 00:0C:01:00:01:E1 |
Caché ARP (C)

| IP           | MAC                |
| ------------ | ------------------ |
| 193.198.25.5 | 00:0C:01:00:01:E0  |
Caché ARP (F)

| IP            | MAC               |
| ------------- | ----------------- |
| 193.198.25.11 | 00:0C:01:00:01:0C |
| 193.198.25.4  | 00:0C:01:00:01:0F |
Caché ARP (Router)

## Problemas
<hr>
### Problema 1
En la red LAN de la figura, considere que inicialmente los puentes P1 y P2 tienen las tablas de direcciones vacías. Indique para la siguiente secuencia de tramas (ordenada) en qué dominios de colisión (anchos de banda) es visible (incluyendo el origen) y el contenido de la tabla de direcciones de los puentes.
![[Pasted image 20240502171353.png|450]]
**a) Trama con origen la estación A y destino la estación B.**
Inicialmente en los tres dominios C1, C2 y C3 porque están las TC vacías. P1 y P2 aprende donde está A.
**b) Trama con origen la estación D y destino la estación A.** 
Se "ve" en C2 y C1 porque P1 ya "sabe" que A está en C1.
**c) Trama con origen la estación B y destino la estación D.**
Se "ve" en C1 y C2 porque P1 ya "sabe" que D está en C2.