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