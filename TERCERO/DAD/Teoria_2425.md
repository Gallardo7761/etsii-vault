# <mark style="background: #FFF3A3A6;">TEMA 1: Introducción a los sistemas distribuidos</mark>
El paradigma clásico cliente-servidor es lo que se llama centralizado. Un ejemplo de arquitectura distribuida sería una red P2P (como torrent). Y por último un ejemplo de red descentralizada sería una red como la red Tor.
## <mark style="background: #ADCCFFA6;">1. Introducción a los SSDD</mark>
### <mark style="background: #FFB86CA6;">Qué es un sistema distribuido</mark>
Un sistema distribuido es un conjunto de computadores débilmente acoplados (independientes) interconectados a través de una red que colaboran para realizar una tarea. Los orígenes se remontan a los años 80:
- **Principios de los 80:** Los computadores son máquinas costosas y muy grandes. Sistemas operativos multiusuario de tiempo compartido.
- **A partir de los años 80:** Auge de los computadores personales. Bajo coste. Avances tecnológicos continuos. SO monousuario.
- **A partir de los años 90:** Aparecen las redes de computadores de alta velocidad (LAN y WAN).
### <mark style="background: #FFB86CA6;">Propiedades de los sistemas distribuidos</mark>
- **Concurrencia de los componentes:** no se habla ya de concurrencia a nivel de componente del computador, si no de los distintos computadores que componen el sistema.
- **Carencia de un reloj global:** no existe un reloj que sincronice todos los computadores, sino un coreógrafo que se dedica a hacerlo.
- **Fallos independientes de los componentes:** si falla una máquina se balancea la carga a otras máquinas.
- **Uso de un sistema de comunicación:** WAN, LAN, etc.
- **Ausencia de memoria común:** entendiendo como memoria, la física (RAM, HD). Sin embargo, puede y hay BD compartida.
- **Sincronización del trabajo**
- **Ausencia de un estado global**
- **Comunicación a través de mensajes**
### <mark style="background: #FFB86CA6;">Ejemplos de sistemas distribuidos</mark>
- **Internet**
- **Intranet**
- **Computación móvil y ubicua:** computación que se puede llevar a cabo en cualquier lugar
- **Redes P2P**
- **Grid computing:** paralelizar el problema a resolver con varios equipos para reducir el tiempo de resolución. De ahí viene el nombre de "grid" es una cuadrícula de equipos en paralelo. 
- **Redes de sensores**
- **Red bancaria**
- **Servidor FTP**
- **BD distribuidas**
## <mark style="background: #ADCCFFA6;">2. Características de los SSDD</mark>
El principal objetivo de un SD es que el sistema se presente a los usuarios como un único computador monoprocesador virtual. Para ello se persiguen los siguientes conceptos:

| Transparencia  | Descripcion                                                                      |
| -------------- | -------------------------------------------------------------------------------- |
| Acceso         | Diferencias en la representación de los datos y como acceder a los recursos      |
| Localización   | Dónde se encuentra un recurso                                                    |
| Migración      | Que un recurso pueda moverse de un lugar a otro                                  |
| Relocalización | Que un recurso pueda moverse de un lugar a otro mientras está en uso             |
| Replicación    | Que puedan existir réplicas de un recurso                                        |
| Concurrencia   | Que un recurso pueda ser compartido por varios usuarios                          |
| Resiliencia    | Permitir la recuperación de un recurso en caso de fallos sin que se vea afectado |
| Persistencia   | Que un recurso pueda estar en memoria o en disco                                 |
### <mark style="background: #FFB86CA6;">Seguridad en SSDD</mark>
En un sistema distribuido la información se transmite entre los diferentes nodos del mismo mediante paso de mensajes. Hay varios requisitos para garantizar la seguridad:
- El emisor debería saber que su mensaje se ha recibido.
- El receptor de un mensaje debería saber que el que le ha enviado el mensaje es el emisor adecuado.
- Tanto el emisor como el receptor deberían tener garantías de que los mensajes no son observados ni alterados durante la transmisión (no hay un ataque MITM).
### <mark style="background: #FFB86CA6;">Rendimiento en SSDD</mark>
La existencia de más recursos debería permitir que las apps se ejecuten más rápido en un SD que en uno centralizado. Sin embargo, hay algunos problemas:
- Las apps tienen que ser paralelizadas
- Minimizar el tráfico de red
- Equilibrio de carga
- Conseguir que un SD sea transparente, flexible y fiable
<div class="nota"><h1>Heterogeneidad</h1><p>Un SD se puede componer de conjuntos de computadores que pueden difrerir tanto en HW como en SW, esto implica que pueda haber incompatibilidades:</p><ul><li>Los CPUs y lenguajes no usan la misma representación de los datos</li><li>Gran diversidad de protocolos</li></ul></div>
## <mark style="background: #ADCCFFA6;">3. Protocolos</mark>
- **Servicios de red:** capa de servicios estándar en internet: HTTP, DNS, FTP, SMTP
- **Servicios web:** conjunto de servicios montados sobre HTTP: motores de búsqueda, servicios de directorio, cambio de moneda, SMS. Utilizan SOAP y XML.
- **Apps de red:** aplicaciones específicas desarrolladas por usuarios finales. Utilizan middlewares para la comunicación.
## <mark style="background: #ADCCFFA6;">4. Tipos de sistemas</mark>
### <mark style="background: #FFB86CA6;">Computación en clúster</mark>
Conjunto de computadores similares conectados a través de una red LAN de alta velocidad. Se usa para computación en paralelo usualmente. Cada clúster es un conjunto de nodos "slave" monitoreados por los nodos "master".
### <mark style="background: #FFB86CA6;">Computación en red</mark>
Nodos con marcadas diferencias de hardware y tecnología de red. 
### <mark style="background: #FFB86CA6;">Computación en la nube</mark>
Recursos virtualizados alojados en el centro de datos del proveedor. Para el usuario, está "alquilando su propia máquina", sin embargo, seguramente sea compartida con otros usuarios. Son bastante escalables ya que permiten configurarse dinámicamente.
#### <mark style="background: #D2B3FFA6;">Modelos de computación en la nube</mark>
![[Pasted image 20250203114652.png]]
### <mark style="background: #FFB86CA6;">Plataformas de computación en la nube</mark>
#### <mark style="background: #D2B3FFA6;">Amazon Web Services (AWS)</mark>
![[Pasted image 20250203115134.png]]
#### <mark style="background: #D2B3FFA6;">Google Cloud Platform (GCP)</mark>
![[Pasted image 20250203115222.png]]
#### <mark style="background: #D2B3FFA6;">Microsoft Azure</mark>
![[Pasted image 20250203115240.png]]
## <mark style="background: #ADCCFFA6;">5. Problemas</mark>
- **Compatibilidad de tipos de datos:** distintos SO que tienen distintos tipos de datos que no siempre son compatibles entre sí.
- **Fallos del servido:** debido a que los componentes pueden ser remotos, un fallo de cualquiera puede hacer que toda la app falle.
- **Fallos del cliente:** el servidor debe saber como responder a fallos del cliente.
- **Reintento de llamadas:** si se hace una llamada a un método en un servidor para generar una orden de compra muy grande, y el servidor responde, pero se pierde la respuesta por un fallo de red, no es muy eficiente volver a enviar la orden de compra.
- **Seguridad:** autenticar a los usuarios, cifrar la información que viaja por la red, evitar DDoS, etc.
- **Sincronización de la hora:** para que no haya incoherencia con la hora real.
- **Formato de los datos**
- **Disponibilidad de los servidores**
- **Acceso a los sistemas de forma remota**
- **Posibilidad de ver los datos por muchas personas**
# <mark style="background: #FFF3A3A6;">TEMA 2: Modelos y arquitecturas</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
- Cómo interaccionan los módulos
- Qué roles ejercen los procesos involucrados
- Cuál es su correspondencia con nodos físicos
- "Topología" de la aplicación distribuida
### <mark style="background: #FFB86CA6;">Patrones más conocidos</mark>
Las arquitecturas más frecuentes en SSDD son:
- Cliente/Servidor
- P2P
- Editor/Subscriptor
- Arquitectura de varios niveles
## <mark style="background: #ADCCFFA6;">2. Arquitectura Cliente/Servidor</mark>
Un equipo llamado servidor, realiza ciertas tareas denominadas **servicios**, como pueden ser ofrecer archivos, ejecutar comandos, enrutar datos a una impresora, etc. El cliente es el equipo que **solicita los servicios**. 
### <mark style="background: #FFB86CA6;">Desventajas</mark>
El servidor, a medida que se necesita escalabilidad, puede transformarse en un cuello de botella y por tanto convirtiéndose el servidor en un punto crítico de fallo. 
### <mark style="background: #FFB86CA6;">Características varias</mark>
El servidor ofrece una colección de servicios  que el cliente debe conocer. Normalmente se realizan peticiones específicas al servidor: recurso, operación, etc.
### <mark style="background: #FFB86CA6;">Reparto de funcionalidad</mark>
Respecto al "grosor" (cantidad de trabajo que realiza) del cliente:
- **Ligeros (Thin/Lean/Slim Client):** 
	- Menor coste de operación y mantenimiento
	- Mejor seguridad
- **Pesados (Thick/Fat/Rich Client):**
	- Mayor autonomía a cambio de gastar más recursos locales
	- Mejor escalabilidad: el cliente gasta menos recursos de red y servidor
	- Más ágil en respuesta al usuario
### <mark style="background: #FFB86CA6;">Conexiones</mark>
En el caso de usar esquema con conexión:
- 1 conexión por cada petición: 1 operación C-S
- Más sencillo pero mayor sobrecarga (9 mensajes TCP por petición)
Como solución: varias peticiones usan una misma conexión
- Más complejo pero menor sobrecarga
- 1 cliente 1 conexión
### <mark style="background: #FFB86CA6;">Estados</mark>
- **Ventajas de servicios con estado**
	- Mensajes de petición más cortos
	- Mejor rendimiento (pero información en memoria)
	- Favorece optimización del servicio con predicciones
- **Ventajas de servicio sin estado**
	- Más tolerante a fallos
	- Peticiones autocontenidas
	- Reduce el nº de mensajes
	- Más económicos para el servidor (menos uso de memoria)
- **Servicio sin estado (stateless) de la propuesta REST**
	- El estado que se almacena lo hace el cliente y lo envía al servidor (ej: HTTP+cookies)
## <mark style="background: #ADCCFFA6;">2. Arquitectura Publisher/Subscriber</mark>
Es un sistema de eventos distribuidos:
- Un subscriptor S (subscriber) muestra interés por eventos y se subscribe a ciertos eventos. 
- Un editor P (publisher) genera un evento y se lo envía a subscriptores interesados en el mismo.
- Operaciones:
	- Subscribir \[alta/baja\] (S$\rightarrow$)
	- Publicar (P$\rightarrow$)
	- Notificar ($\rightarrow$S)
### <mark style="background: #FFB86CA6;">Características</mark>
- Paradigma asíncrono y desacoplado espacialmente
- Editores y subscriptores no se conocen entre sí (a diferencia de C-S)
- En algunos casos también desacoplado temporalmente
- Normalmente es _push_ (subscriptor recibe notificaciones), y como alternativa está el _pull_ subscriptor pregunta si hay mensajes, pero no es nada escalable.
## <mark style="background: #ADCCFFA6;">3. Arquitectura P2P</mark>
- **Desestructurados:**
	- Ubicación de recursos impredecible
	- Cada nodo posee recursos
	- Corresponden a sistemas con nodos más autónomos
- **Estructurados:** 
	- Ubicación de recursos predecibles y dependiente de la topología
	- Generalmente definida por función hash distribuida para buscar la máquina que posee el recurso
	- Corresponden a sistemas con nodos más cooperativos
# <mark style="background: #FFF3A3A6;">TEMA 3: Procesos locales</mark>
## <mark style="background: #ADCCFFA6;">1. Sockets</mark>
Un **socket** es un método para comunicar programas entre sí mediante el paradigma C-S. Un socket también es una dirección en internet tipo IP:puerto.
## <mark style="background: #ADCCFFA6;">2. Servlets</mark>
Programa Java que se ejecuta en un servidor Web y construye o sirve páginas web. De esta forma se pueden construir páginas dinámicas, basadas en diferentes fuentes variables.
# <mark style="background: #FFF3A3A6;">TEMA 4: Diseño de servidores y escalabilidad</mark>
## <mark style="background: #ADCCFFA6;">1. Problema de los servidores de apps tradicionales</mark>
- Un thread controla una tarea
- Los threads esperan a que finalice la tarea que está ejecutando mientras que los demás están en queue
- No escalable
- Muchos threads probablemente
- Sistema de concurrencia muy complejo
## <mark style="background: #ADCCFFA6;">2. Vert.X</mark>
- Framework políglota de propósito general sobre la JVM.
- APIs asíncronas, event-driven, non-blocking.
- MUY escalable
![[Pasted image 20250227131516.png|450]]
<small>Ejemplo verticle</small>

Con Vert.X se puede hacer:
- TCP/SSL server
- HTTP/S server
- Websockets
- Acceso distribuido al EB (Event Bus)
- Intercambio de Maps/Sets
- Logging
## <mark style="background: #ADCCFFA6;">3. Arquitectura</mark>
### <mark style="background: #FFB86CA6;">Estructura</mark>
- Vert.X Core
- Verticle
- Event bus
- Messages
- Handlers
- Modules
### <mark style="background: #FFB86CA6;">Vert.X Core</mark>
- Cada verticle una instancia de Vert.X
- Cada instancia de Vert.X en una instancia propia de la JVM
- Un event loop que sirve conexiones de manera asíncrona
- La mayor parte del trabajo real se lleva a cabo en un pool de threads en segundo plano
- Se emplean todos los cores disponibles como threads en segundo plano
### <mark style="background: #FFB86CA6;">Verticle</mark>
- Cada Verticle está aislado con diferentes class loaders
- Un Verticle nunca es ejecutado por más de un thread concurrentemente
- No existen deadlocks, dependencias, etc. El desarrollo se hace pensando en single thread.
#### <mark style="background: #D2B3FFA6;">Standard Verticles</mark>
Se ejecutan en el mismo thread que el event loop. 
#### <mark style="background: #D2B3FFA6;">Worker Verticles</mark>
Están diseñados para tareas bloqueantes. Se ejecutan en un thread distinto que **SÍ** se puede bloquear.
#### <mark style="background: #D2B3FFA6;">Multithread Worker Verticles</mark>
Se ejecutan de manera concurrente en varios threads (no se suele usar).
### <mark style="background: #FFB86CA6;">Event Bus</mark>
- Pilar central de la estructura Vert.X
- Permite que se comuniquen los Verticles
- No depende del lenguaje
- Independiente del equipo en el que se ejecute
- Válido para C y S
El event bus soporta varios mecanismos de comunicación:
- Publish/Subscribe
- P2P
- Request/Response
### <mark style="background: #FFB86CA6;">Messages</mark>
- **Addressing:** se envian a una direccion. String con el nombre del verticle + destinatario.
- **Handlers:** reciben los mensajes. Se deben registrar con una dirección y pueden registrarse con varias.
- **Publish/Subscribe:** los mensajes se publican a una dirección. Todos los handlers subscritos recibirán el mensaje
- **P2P y Req/Res:** envío de mensajes a un único handler. El receptor puede responder.
### <mark style="background: #FFB86CA6;">Modules</mark>
Mongo DB, Redis, MySQL, SMTP, JDBC, Jersey, Promises, Guice, Spring, Vertigo, Metrics, Facebook, Yoke, Stomp, NoDyn, GCM, SocketIO, Sessions, Via, RxJava
