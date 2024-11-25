# <mark style="background: #FFF3A3A6;">TEMA 1: Conceptos básicos</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
Estudiar la red a nivel de tecnología, y arquitectura. 
- Una **red** es un conjunto de hosts o sistemas finales interconectados que se pueden comunicar a través de esta. 
- La **tecnología** es un conjunto de procedimientos, métodos y elementos físicos que permiten transmitir información. 
- La **arquitectura** es el conjunto de capas y protocolos de una red de comunicaciones (nombre de capa y protocolo). La **pila de protocolos**, en cambio, es el conjunto de protocolos asociados a **cada capa** de una red de comunicaciones (solamente protocolos).
## <mark style="background: #ADCCFFA6;">2. Modelo OSI vs TCP/IP</mark>
~~~tabs
---tab OSI

![[Pasted image 20240916084750.png|150]]

Modelo jerárquico dividido en capas o niveles basado en "divide y vencerás" para cubrir todos los servicios de una red. Debido a la jerarquía, las capas tienen un nivel de complejidad mayor cuanto más abajo están, al igual que son más cercanas al usuario cuanto mas arriba están.

---tab TCP/IP

![[Pasted image 20240916092616.png|150]]

Modelo que simplifica el OSI agrupando algunas capas:
- Aplicación, Presentación y Sesión $\rightarrow$ Aplicación
- Transporte (Host a Host)
- Red
- Enlace y Físico $\rightarrow$ Host a Red
~~~
## <mark style="background: #ADCCFFA6;">3. Relación entre los niveles/capas</mark>
Los niveles inferiores suministran servicios a los superiores. El nivel superior "usa" el servicio del nivel inferior. Por ejemplo, TCP es cliente de IP.
### <mark style="background: #FFB86CA6;">Encapsulación</mark>
Por ejemplo al pedir una página web:
**(ENVÍO)**
- HTTP monta el mensaje HTTP y pide servicio a TCP.
- TCP añade la cabecera TCP al mensaje recibido desde HTTP y pide servicio a IP.
- IP añade la cabecera IP al datagrama TCP y pide servicio a IEEE 802.3.
- IEEE 802.3 añade cabecera y cola al paquete IP y se envía la trama.
**(RECEPCIÓN)**
- IEEE 802.3 recibe la trama (binario), desencapsula (quita cabecera y cola) y "sube" el paquete IP.
- IP recibe el paquete IP, desencapsula (quita cabecera) y "sube" el datagrama TCP.
- TCP recibe el datagrama TCP, desencapsula (quita cabecera) y "sube" el mensaje HTTP.
- HTTP recibe el mensaje HTTP y "monta" la respuesta.
## <mark style="background: #ADCCFFA6;">4. Nivel de transporte</mark>
Proporciona comunicación lógica entre procesos de aplicación que se ejecutan en diferentes hosts: los mensajes de aplicación se dividen en **segmentos** que se pasan al nivel de red. Los protocolos de transporte tienen lugar en los extremos. Existen dos protocolos del nivel de transporte:
![[Pasted image 20240918110700.png|600]]
### <mark style="background: #FFB86CA6;">Protocolo TCP</mark>

![[Pasted image 20240918112218.png|500]]

- **Source Port (16b):** puerto del nivel de aplicación en el host transmisor.
- **Destination Port (16b):** puerto del nivel de aplicación en el host receptor.
- **Sequence Number (32b):** número del primer octeto.
- **ACK (32b):** número de secuencia esperado del otro extremo (confirmando los anteriores).
- **Data Offset:** indica que "trozo" es en la segmentación.
- **Flags:** banderas booleanas (1/0):
	- SYN: crear
	- FIN: cerrar
	- RST: rechazar
	- ACK: reconocer
- **Window (16b):** tamaño máximo de cada "trozo".
### <mark style="background: #FFB86CA6;">Protocolo UDP</mark>

![[Pasted image 20240918114306.png|500]]

- **Source Port (16b):** puerto del nivel de aplicación en el host transmisor.
- **Destination Port (16b):** puerto del nivel de aplicación en el host receptor.
- **Length:** tamaño del campo de datos.
- **Checksum:** control de errores.
## <mark style="background: #ADCCFFA6;">5. Nivel de red</mark>
Define un esquema de direccionamiento. Establece las herramientas necesarias para definir un enrutamiento entre hosts. Emplea el servicio de entrega de R_PDUs suministrado por el nivel de enlace, permitiendo intercambiar datos entre dispositivos que implementen mínimo hasta nivel de enlace.
### <mark style="background: #FFB86CA6;">Protocolo IPv4</mark>
Características:
- No fiable
- No orientado a la conexión
- Direccionamiento jerárquico de 32b (direcciones IP)
- Existen direcciones especiales (127.0.0.1, 192..., 10..., 172..., 0.0.0.0, 255.255.255.255,...)
- Soporta fragmentación

![[Pasted image 20240918115601.png|600]]
- **Version (4b):** si es v4 o v6.
- **Total Length (16b):** tamaño de la cabecera y datos.
- **Fragment Offset:** para fragmentación.
- **TTL (8b):** _time to live_ o número de saltos por los que puede pasar el mensaje.
- **Source & Destination Address (32b):** direcciones IP de los hosts en transmisor y receptor.

### <mark style="background: #FFB86CA6;">Direccionamiento IP</mark>

![[Pasted image 20240918120609.png|350]]
Un host está configurado si:
- Tiene dirección IP
- Tiene router frontera
- Tiene máscara de red
### <mark style="background: #FFB86CA6;">Sockets</mark>
Un socket es el par dirección_ip:puerto. A su vez, un par de sockets, identifica de manera única una conexión en Internet.
-  Define un extremo de la comunicación.
- Identifica un proceso en internet. 
## <mark style="background: #ADCCFFA6;">6. Identificación de clientes</mark>
- TCP y UDP con **puerto**
- IP con **Protocol**
- Ethernet con **Ethertype**
- IEEE 802.3 (MAC) emplea LLC (IEEE 802.2)
# <mark style="background: #FFF3A3A6;">TEMA 2: Servicios de directorio</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
- **Configuración y administración:** DHCP.
- **Acceso remoto:** SSH.
- **Gestión de archivos:** FTP.
- **Servicios de impresión:** Compartición de recursos.
- **Información:** Compartir y buscar información (www, IPv).
- **Comunicación:** Intercambio de texto, audio y/o vídeo entre usuarios (e-mail, chat, VoIP).
Existen dos paradigmas:
### <mark style="background: #FFB86CA6;">Cliente-Servidor</mark>

![[Pasted image 20240925105710.png|300]]
**Servidor:**
- Siempre activo
- Dirección IP estática
**Cliente:**
- Se comunica con el Servidor
- No se comunica con otros clientes
- Dirección IP dinámica
- Conexión intermitente
#### <span style="color:red"><strong>Inconveniente:</strong></span> Puede haber un cuello de botella en el Servidor

Para resolver el cuello de botella se pueden usar servidores de respaldo que pueden servir tanto para contener _backups_ del servidor principal y funcionar en caso de fallo o para balancear la carga entre el principal y ellos y descongestionar el principal.

![[Pasted image 20240925110142.png|400]]
### <mark style="background: #FFB86CA6;">Peer-to-Peer (P2P)</mark>

![[Pasted image 20240925110416.png|350]]
No existe un "servidor principal". Los elementos de la red no tienen por qué estar siempre activos, se pueden comunicar entre sí y pueden tener IPs dinámicas.
### <mark style="background: #FFB86CA6;">Híbrido</mark>

![[Pasted image 20240925111040.png|350]]
El servidor no ofrece el servicio en sí, sino que ofrece información necesaria para acceder al mismo. La comunicación entre estaciones es P2P.
**Ejemplo:** Chat/VoIP
- El servidor centralizado almacena la información necesaria para que los pares se encuentren.
- El usuario registra dicha información al conectarse a la red.
- El usuario solicita información al servidor para comunicarse con otro par.
## <mark style="background: #ADCCFFA6;">2. Introducción al servicio de directorio</mark>
Un Servicio de Directorio se define como una o más aplicaciones que permite administrar y organizar los recursos y usuarios de una red. Para ofrecerlo, se debe:
- Almacenar información diferenciada por tipo y establecer su formato.
- Definir operaciones que se pueden realizar (leer, modificar, borrar, ...).

Para ofrecer el servicio se deben definir:
- **Reglas de acceso** tipo de usuario, autenticación.
- **Procedimientos** paradigma, formato de mensajes, mensajes de ops extendidas
- **Estructura del servicio:** dónde almacenar la información, relaciones entre "entidades".
### <mark style="background: #FFB86CA6;">Características</mark>
- Los servicios de directorio están **optimizados para consultas** (uso de caché). 
- Implementa un **modelo distribuido** de almacenamiento (zonas DNS, dominios, prefijos). 
- Permite extender los tipos de información.
- Ofrece capacidades avanzadas de búsqueda.
- Capacidad de replicación.
## <mark style="background: #ADCCFFA6;">3. LDAP (Lightweight Directory Access Protocol)</mark>
- Basado en X.500.
	- LDAP usa TCP/IP respecto al modelo OSI que propone X.500
	- LDAP reduce la carga respecto a X.500
- Es un protocolo de aplicación.
- Ofrece un modelo mucho más simple tanto a nivel de programación como de administración.
### <mark style="background: #FFB86CA6;">Visión global</mark>
El paradigma que sigue es Cliente-Servidor.
- **Cliente:** Directory User Agent (DUA)
- **Servidor:** Directory System Agent (DSA)
![[Pasted image 20240925115054.png|500]]
**Directorio:** Colección de sistemas abiertos que cooperan para
ofrecer servicios de directorios.

LDAP no especifica cómo o donde almacenar información: cualquier DUA podrá operar con cualquier implementación de LDAP.
### <mark style="background: #FFB86CA6;">Introducción</mark>
LDAP define dos modelos:
- Modelo de Información del Directorio: formato y tipos de información
- Modelo de protocolo: operaciones que se pueden realizar 
## <mark style="background: #ADCCFFA6;">4. Modelo de información del directorio</mark>
### <mark style="background: #FFB86CA6;">Modelo de información del usuario del directorio (DUA)</mark>
Toda la información contenida en el directorio se denomina **Directory Information Base (DIB)**. Un DIB contiene dos tipos de información:
- **Información del usuario:** suministrada y gestionada por el usuario.
- **Información administrativa y operacional**
#### Entrada
![[Pasted image 20240925120207.png|350]]
Unidad básica de información (colección de información). Hay diversos tipos: entrada objeto (representa un objeto), entrada alias (representa un nombre alternativo).
#### Atributo
![[Pasted image 20240925120452.png|350]]
Cada entrada es un conjunto de atributos que contienen información sobre el objeto. Pueden representar atributos de usuario o de información administrativa u operacional. La **descripción** de un atributo se compone de un tipo de atributo y un conjunto de 0 o más opciones. 
**Ejemplo de atributo con su descripción:** givenName - CN
#### Tipo de atributo
![[Pasted image 20240925120750.png|350]]
El tipo de atributo define si el atributo puede tener múltiples valores, la sintaxis, las reglas de relación... El tipo indica si es de usuario u operacional. LDAP permite establecer jerarquía.
#### Clase de objeto
![[Pasted image 20240925120909.png|350]]
Familia de objetos que comparten características. Cada objeto pertenece al menos a una clase. Puede haber herencia de clases. Cada clase se identifica mediante un **Object IDentification (OID)**. Cada clase identifica el conjunto de atributos _permitidos_ en las entradas y _requeridos_ (también dichos conjuntos se heredan de la superclase).
#### Esquema
![[Pasted image 20240925121156.png|350]]
Es un conjunto de definiciones y restricciones en relación a:
- La estructura del Directorio.
- Las formas de nombrar entradas.
- Información que puede contener una entrada.
- Atributos usados.
- Jerarquías y relaciones.
El esquema permite al Directorio:
- Prevenir la creación de entradas subordinadas a la clase incorrecta.
- Prevenir la adición de tipos de atributos inapropiados.
- Asegurarse de que la sintaxis sea correcta.
#### EJEMPLO DE ENTRADA
![[Pasted image 20240925121537.png]]

#### <mark style="background: #D2B3FFA6;">Organización de las entradas</mark>
El conjunto de entradas que representan el DIB se organizan jerárquicamente en un árbol conocido como **Directory Information Tree (DIT)**. Cada nodo es una entrada del **DIB**. Las aristas representan relaciones entre entradas. 
![[Pasted image 20240930133135.png]]
#### <mark style="background: #D2B3FFA6;">Identificación de las entradas</mark>
Cada entrada se identifica mediante un **Relative Distinguished Name (RDN)**. RDN es uno o más pares de descripción de un atributo y su valor asignado. El RDN debe ser único entre todos los nodos hijos de un nodo padre. 
<u>Ejemplo</u>: UID=12345, CN=Peter García+L=Sevilla

Cada entrada posee también otra forma de identificarla llamada **Distinguished Name (DN)**, que si el RDN es el relativo este es el "absoluto", es decir, todos los RDN encadenados desde la raíz, identificando así a la entrada de forma unívoca en el DIT. 
<u>Ejemplo</u>: UID:nobody@example.com,DC=example,DC=comCN: Peter García+L=Sevilla,OU=Ventas,O=ACMELtd,L=Sevilla,C=ESv

![[Pasted image 20240930133748.png]]
#### <span style="color:red"><strong>Importante:</strong></span> el nodo raíz debe tener SIEMPRE `dc`

#### ¿Qué inconveniente tiene tener un servidor secundario en LDAP?
Dicho servidor debe tener una copia del primario para que el servicio LDAP siga activo en caso de caída del primario. 
### <mark style="background: #FFB86CA6;">Modelo de información del sistema (DSA)</mark>
Varios DSAs pueden ofrecer acceso al DIT. El servidor maestro mantiene la información original. Los servidores "shadowing" o "caching" poseen copias de dicha información, con todo lo que conlleva: si se cachea en t=0, se puede volver a actualizar la información en t=n y **NO** estará en la caché (habría que actualizar periódicamente la caché basado en un _timestamp_ y en un tiempo máximo de cacheado).
- **Contexto de nombre:** un subárbol de entradas que se gestiona en un sólo DSA maestro. 
- **Prefijo del contexto:** secuencia de RDNs desde el raíz del DIT al nodo inicial del contexto de nombre (DN del nodo inicial)
#### <mark style="background: #D2B3FFA6;">Subentradas</mark>
Son una clase especial de entrada. Se emplean para gestionar información asociada con un subárbol.
#### <mark style="background: #D2B3FFA6;">Atributo <code>objectClass</code></mark>
Cada entrada del DIT posee este atributo. Especifica las clases de objeto de una entrada. Los valores pueden ser modificados por los clientes pero el atributo no lo pueden eliminar.
### <mark style="background: #FFB86CA6;">Modelo de información administrativa y operacional</mark>
#### <mark style="background: #D2B3FFA6;">Atributos operacionales</mark>
Se usan por los servidores para propósito administrativo y operacional. Normalmente no son visibles si no se solicitan explícitamente.
<u>Ejemplos</u>: `creatorsName`, `createTimestamp`, `modifiersName`, `modifyTimestamp`.
#### <mark style="background: #D2B3FFA6;">Atributos operacionales (X.500)</mark>
- **Atributos operacionales del Directorio:** Representan información operacional y/o administrativa. `createTimestamp`, `ditContentRules`
- **Atributos operacionales compartidos por DSA:** Representan información del Modelo de Información del DSA que se comparte entre DSAs.
- **Atributos operacionales específicos del DSA:** Representan información del Modelo de Información del DSA que es específica de dicho DSA.
## <mark style="background: #ADCCFFA6;">5. Modelo de protocolo</mark>
Un cliente (DUA) comienza una sesión con LDAP conectándose al puerto TCP (por defecto 389) del servidor LDAP y le solicita que realice alguna acción.
![[Pasted image 20241002112436.png|500]]
Las solicitudes son generalmente independientes entre ellas y son atómicas (se realizan transacciones como en SQL). El comportamiento solicitud/respuesta es asíncrono.
### <mark style="background: #FFB86CA6;">Operaciones en LDAP</mark>
`Bind:` Autenticación
`Unbind:` Cierra la conexión TCP
`Unsolicited notification:` El otro tipo de respuesta que puede enviar el servidor, se manda sin haber solicitado nada. Se usa para notificar a todos los clientes que no se va a poder realizar operaciones por X motivo.
`Search:` buscar un nodo
`Modify:` modificar cualquier atributo siempre que no sea parte del DN
`Add:` añadir un nodo
`Delete:` eliminar un nodo
`Modify DN:` modificar el DN
`Compare:` comparar información entre dos DSA
`Abandon:` parar solicitud que se ha enviado pero no ha terminado
`Start TLS:` inicia el mecanismo de seguridad para encriptar 
`Extended:` añadir operaciones a LDAP
# <mark style="background: #FFF3A3A6;">TEMA 3: Servicio de Nombres de Dominio (DNS)</mark>

- **Cómo se forman los nombres de los servidores**
- **Que información guarda cada servidor**
- **Cómo se organiza la información**
- **Protocolo que establezca los mensajes a transmitir**
## <mark style="background: #ADCCFFA6;">1. Sistema de nombres</mark>
Hay dos tipos: **plano** y **jerárquico**.
![[Pasted image 20241009111053.png|600]]
DNS es **jerárquico**. 
## <mark style="background: #ADCCFFA6;">2. Introducción</mark>
### <mark style="background: #FFB86CA6;">Espacio de nombres</mark>
Todo nombre unívoco de un host en Internet termina en **"."** de forma que un posible nombre podría ser: www.dte.us.es. Empezando desde la derecha se puede formar un árbol de nombres con raíz en el **"."**
![[Pasted image 20241009112826.png|500]]
Características:
- **Dominios:** nodos intermedios del árbol y todo lo que hay por debajo de este.
- **Hostname:** nodos hoja del árbol
- Cada nodo puede tener una etiqueta de como máximo 63 caracteres
- Un path (hostname + dominio) puede ser de 127 niveles como máximo
- Los hijos del nodo **"."** (Raíz) se llaman TLD (Top Level Domain). 
- Los hijos de los nodos TLD son **autoritativos** (de primer, segundo... nivel). Al igual que los servidores son **autoritativos** si son capaces de resolver una consulta (suelen estar en los nodos autoritativos).
- Para nombrar a un host se puede hacer:
	- **FQDN (Fully Qualified Domain Name):** nombre completo del nodo (www.dte.us.es.)
	- **PQDN (Partially-Qualified Domain Name):** nombre **relativo** de un nodo respecto a otro (www.dte.us respecto a es)
- En cada nodo (no hoja) hay un servidor DNS, por lo que hay un gran número de servidores organizados jerárquicamente. El servidor DNS de un nodo tiene información relacionada con el FQDN de dicho nodo.
- En todo el mundo existen 13 servidores raíz gestionados por **IANA (Internet Assigned Numbers Authority)**.
	![[Pasted image 20241009120833.png|600]]
- Hay varios tipos de TLD:
	- Genéricos (gTLD): .net, .com, .org (hay restringidos: .edu, .gov, .mil)
	- Geográficos (ccTLD): .es, .fr, .jp (los gestionan los países, los proporciona IANA)
	- Zona inversa: .arpa
	- Reservados: .test, .example, .invalid, .localhost
### <mark style="background: #FFB86CA6;">Contratar un dominio</mark>
- **Registry:** mantienen los TLDs. Hay uno por dominio. Definen las "reglas" del dominio.
- **Registrant:** nombre del usuario final (que recibe el dominio).
- **Registrador:** empresas que actúan como intermediarios entre el registrant y el registry.
- **Registrer:** conocido como 'whois'. Directorio que recopila información sobre el registrant. Alojado por el registry, actualizado por el registrador a petición del registrant.
### <mark style="background: #FFB86CA6;">Delegar un dominio entre servidores</mark>
Los dominios se delegan a entidades que los gestionan. El padre únicamente contendrá información para indicar a quien preguntar para obtener los datos del dominio delegado.
**Mapa DNS:** información que contiene el servidor DNS
**Zona DNS:** de quién tiene información el servidor DNS (hijos del nodo)
### <mark style="background: #FFB86CA6;">Resolución de nombres</mark>
Consulta: www.a2.rediris.com.
1. El cliente envía la solicitud a su servidor DNS por defecto. Este no pertenece a la jerarquía.
2. El servidor DNS por defecto no tiene la información y le envía la consulta al raíz.
3. El raíz no tiene la información y envía la IP del servidor TLD com.
4. El servidor DNS por defecto envía la consulta al servidor TLD com.
5. El servidor com no tiene la información y envía la IP del servidor autoritativo rediris.
6. El servidor DNS por defecto envía la consulta al servidor autoritativo rediris.
7. El servidor rediris (al estar delegando) no tiene la información y envía la IP del servidor autoritativo de segundo nivel a2.
8. El servidor DNS por defecto envía la consulta al servidor autoritativo de segundo nivel a2.
9. El servidor a2 se encarga de la zona DNS consultada, por lo que devuelve la IP del servidor www.
10. El servidor DNS por defecto envía la respuesta al cliente.
![[Pasted image 20241014092122.png|400]]
**Consulta recursiva:** el cliente solicita la respuesta completa al servidor y si este no tiene la respuesta está obligado a buscarla. Como puede suponer un problema a la hora de reservar recursos, **todos** los servidores raíces no las admiten.
**Consulta iterativa:** el cliente solicita la respuesta parcial o completa a la consulta, con la información que contenga el servidor.
### <mark style="background: #FFB86CA6;">Caching y TTL</mark>
El objetivo de esto es no sobrecargar el servidor con consultas ya resueltas hace poco. Con cada consulta/respuesta el servidor aprende:
- La dirección IP del servidor sobre el que se ha consultado.
- La dirección IP de los servidores DNS que han intervenido. (La IP de com permite hacer consultas sin pasar por raíz).
Cada entrada de la caché se almacena durante TTL segundos. Si el TTL es pequeño sobrecarga el servidor, y si es grande puede dar lugar a incoherencias.
<div class="nota"><h4>NOTA</h4><p>En la caché de un servidor sólo se guarda el par hostname/IP si esta NO pertenece al dominio que el servidor gestiona</p></div>
### <mark style="background: #FFB86CA6;">Formato de mensajes DNS</mark>

![[Pasted image 20241014100406.png|350]]
<div class="nota"><h4>NOTA</h4><p>RR = Registro de Recursos</p></div>
- **identification:** identificar el servidor
- **flags:** especificar datos de la consulta, si es iterativa o recursiva, si es solicitud o respuesta, etc.
- **number of questions:** número de solicitudes en el mensaje
- **number of answers RRs:** número de respuestas en el mensaje
- **number of authority RRs:** información de otros servidores DNS que intervienen en la resolución.
- **campos de datos:** los datos especificados por la cabecera
### <mark style="background: #FFB86CA6;">Registros DNS</mark>
#### <mark style="background: #D2B3FFA6;">Registro SOA</mark>
![[Pasted image 20241014101113.png|350]]
- Estructura del registro:
	- **a2.rediris.com. :** el dominio que se está creando
	- **IN SOA**: tipo de registro
	- **ns.a2.rediris.com. :** servidor DNS principal del dominio
	- **postmaster.a2.rediris.com. :** dirección de correo del admin del domino (postmaster@...).
	- **Serial:** fecha de la última versión del SOA formato YYYYMMDDnn.
	- **Refresh:** tiempo máximo que puede pasar un servidor secundario sin contactar con el primario.
	- **Retry:** segundos para que el secundario recontacte con el primario.
	- **Expire:** tiempo que el secundario considera el mapa DNS adecuado antes de borrarlo (cuando se hace Refresh reinicia).
	- **Minimum:** TTL usado para cachear solicitud que no está en el momento de la consulta.
- Mantiene información de la zona DNS.
#### <mark style="background: #D2B3FFA6;">Registro NS</mark>
<div class="nota"><p>Es el más importante y el que más problemas causa en examen</p></div>
![[Pasted image 20241020204107.png]]
- Estructura del registro:
	- **rediris.com.** : el dominio que se está configurando
	- **dns_rediris.rediris.com. :** el FQDN de un servidor DNS
- Especifica los servidores DNS del dominio
- No existe orden de prelación
- Se deben distribuir geográficamente por distintas redes
- Pueden ser máquinas de otros dominios
#### <mark style="background: #D2B3FFA6;">Registro MX</mark>
![[Pasted image 20241020204340.png]]
- Estructura del registro:
	- **Número después de MX:** prioridad
- Especifican los servidores de correo electrónico del dominio
- El correo se debe entrega al servidor de correos con menor prioridad
- Los servidores de correo no tienen por qué estar en el mismo dominio
#### <mark style="background: #D2B3FFA6;">Registro A</mark>
![[Pasted image 20241020204701.png]]
- Determinan relación existente entre el nombre de la máquina y su IP
- El nombre que aparece a la izquierda puede ser RQDN o FQDN pero <strong><span style="color:red;">se recomienda FQDN</span></strong>.
#### <mark style="background: #D2B3FFA6;">Registro CNAME</mark>
![[Pasted image 20241020204947.png]]
- Permite definir "alias"
- Se distingue entre el alias y el nombre canónico
- Si se emplean RQDN el dominio corresponderá con el dominio por defecto
- En el ejemplo, www.rediris.com es un alias de la dirección titan.rediris.com
# <mark style="background: #FFF3A3A6;">TEMA 4: Servicios de correo electrónico</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
Es uno de los servicios más extendidos en Internet. Hay varios tipos de aplicaciones:
- Interfaz gráfica (Outlook, Thunderbird, Apple Mail)
- Modo texto (Pine, elm, mail)
- Web (Squirrelmail, Gmail)
### <mark style="background: #FFB86CA6;">Conceptos</mark>
**Cuenta de usuario:** Identificador de usuario que permite acceder al servicio. Tiene asociado el nombre de usuario (usuario@dominio.com) y contraseña.
**Buzón de correo:** Donde se almacenan los correos
**Alias de correo**
**Lista de correo:** Correo a múltiples usuarios.
### <mark style="background: #FFB86CA6;">Componentes</mark>
**MUA (Mail User Agent) :** Cliente.
**MTA (Mail Transfer Agent) :** Servidor de correo.
**MDA (Mail Delivery Agent) :** Proceso software en el MTA que sirve para colocar los correos recibidos en el buzón de correo correspondiente al usuario.
### <mark style="background: #FFB86CA6;">Procedimiento</mark>
![[Captura de pantalla de 2024-11-20 10-54-39.png]]
## <mark style="background: #ADCCFFA6;">2. Formato de mensajes (IMF)</mark>
Internet Mail Format. Todo mensaje está compuesto de cabecera y cuerpo separadas por "\n". 
- **Cabeceras:** 
	- To:
	- From:
	- Subject:
	- Date:
- **Cuerpo:**
	- Mensaje ASCII no extendido (7 bits). No se puede adjuntar archivos.
## <mark style="background: #ADCCFFA6;">3. SMTP (Simple Mail Transfer Protocol)</mark>
**Características:**
- Paradigma C-S
- Usado entre MUA $\rightarrow$ MTA y MTA $\rightarrow$ MTA
- TCP
- Tiene tres fases:
	- Handshaking (saludo inicial)
	- Transferencia
	- Cierre
- Basado en comando/respuesta
### <mark style="background: #FFB86CA6;">Respuestas</mark>
Generalmente formadas por un código numérico formado por un texto libre y tres cifras.
La primera cifra indica el éxito o fracaso del comando:
- **2xx** indica que se ha aceptado el comando anterior
- **3xx** indica que se ha aceptado parcialmente el comando anterior
- **4xx** indica que se ha producido un error temporal que impide aceptar el comando, pero que si se reintenta más tarde puede que funcione
- **5xx** indica que se ha producido un error permanente que se volverá a producir si se reintenta más tarde
### <mark style="background: #FFB86CA6;">Comandos</mark>
- **HELO/EHLO:** es el primer comando que debe enviar el cliente. Se informa de su nombre de dominio.
- **MAIL FROM:** indica el remitente del mensaje (se le devuelve el mensaje si falla). A veces se requiere que el dominio detrás de "@" exista, en cambio casi nunca se comprueba si el usuario existe. La información asociada a este comando **no tiene que coincidir** con las cabeceras.
- **RCPT TO:** indica el destinatario del mensaje. No tiene por qué coincidir con los To o CC del correo y tampoco tiene por qué aceptarse completamente (p.e. fallo de autenticación).
- **DATA:** permite escribir el mensaje de correo. La respuesta 3xx indica que se espera a que se complete el mensaje y determinar si se acepta. Si se acepta, deberá indicarse tras la señal de fin del mensaje.
  _Una línea finaliza con ".". Si se quiere incluir un "." hay que poner ".."._ 
  - **QUIT:** cierra la conexión
### <mark style="background: #FFB86CA6;">Cabeceras Received</mark>
Se añaden en cada "salto", se deben leer desde el final al principio (LIFO). 
![[Pasted image 20241125143529.png]]
## <mark style="background: #ADCCFFA6;">4. MIME</mark>
Son un tipo de cabeceras que, a diferencia de IMF Estándar, permite el adjuntado de archivos.
### <mark style="background: #FFB86CA6;">Cabeceras</mark>
- **Mime-Version:** Actualmente 1.0
- **Content-type:** le indica al cliente de correo el tipo y subtipo del mensaje o parte de este.
	- Por defecto: Content-type: text/plain; charset=us-ascii
- **Content-Description:** Campo de información
- **Content-Transfer-Encoding:** Indica el tipo de codificación que se ha empleado en esa parte del mensaje.
	-  7 bits: US-ASCII, no sirve para binarios ni caracteres especiales
	- 8 bits y binary: Ligeramente diferentes, permiten ASCII 7 bits y caracteres especiales
	- **quoted-printable** y **base64**: Permiten ASCII 7 bits, caracteres especiales y ficheros adjuntos.
### <mark style="background: #FFB86CA6;">Codificación</mark>
- **base64:** Para codificar un fichero cualquiera:
	- El fichero binario se divide en bloques de 24 bits (3B).
	- Cada bloque se codifica con una letra (tomando los bits de 6 en 6 según tabla).
	- Se transmiten esas letras formando líneas de 72 caracteres.
	- Si los bits no son múltiplos de 24, se añaden bits a 0 hasta formar un número entero de grupos de 6 bits y se añaden uno o dos "=" como relleno al final.
	  ![[Pasted image 20241125144536.png]]
- **quoted-printable:** Se emplea si la mayor parte del mensaje se puede codificar con ASCII.
	- Los caracteres 29-127 se codifican en US-ASCII
	- El resto se representan de forma "=XX" donde XX es el hex de ese carácter. (Una ñ: Una =F1)
#### <mark style="background: #D2B3FFA6;">Caracteres extendidos en cabeceras</mark>
Las cabeceras se transmiten en 7 bits. Podría ser necesario que en el asunto, From o To aparezcan caracteres especiales. Para solucionar eso, se usa el siguiente esquema:
- Encoded-word= “=?”charset ”?” encoding “?” encoded-text “?=“
- charset es el juego de caracteres; encoding: q=quoted-printable, b=Base64
- Los espacios se sustituyen por “_”, los “_” por su codificación.
## <mark style="background: #ADCCFFA6;">5. POP</mark>
- Protocolo simple (buzón inbox únicamente en el servidor)
- El servidor asigna un ID único a cada mensaje (opcional)
- Se diseñó para operar en modo "descarga y borrar" (el cliente de correo descarga el mensaje y lo borra del servidor).
- Se puede descargar un mensaje sin borrarlo pero da problemas de rendimiento al almacenar todo el correo en el mismo buzón.
- El servidor no almacena información del estado de los mensajes.
- Usa el puerto 110 (sin SSL) o el 995 (con SSL).
### <mark style="background: #FFB86CA6;">Fases</mark>
- **Autorización:** 
  ![[Pasted image 20241125145413.png]]
  USER/PASS
  -ERR en caso de error
- **Transacción:** 
  ![[Pasted image 20241125145439.png]]
  STAT: indica el número de emails y tamaño
  LIST: numera los mensajes (sesión) y tamaño
  RETR: descarga un mensaje concreto
  NOOP: no hace nada
  UIDL: muestra ID persistente
  TOP: devuelve sólo cabeceras
  - **Actualización:** 
    ![[Pasted image 20241125145544.png]]
	Se inicia al ejecutar el comando QUIT. Se  borran los mensajes marcados, y aunque puede dar error, se cierra la conexión sí o sí.
## <mark style="background: #ADCCFFA6;">6. IMAP</mark>
