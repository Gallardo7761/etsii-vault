# <mark style="background: #FFF3A3A6;">TEMA 1: Visión general del desarrollo de hardware</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción al hardware</mark>
El hardware es la parte tangible de un sistema informático. Es importante destacar que hardware y software son **indisolubles**. El desarrollo de hardware y software conjuntos se atribuye a la **Ingeniería de Computadores**.
### <mark style="background: #FFB86CA6;">Paradigma del hardware: smartphones</mark>
El componente principal de un smartphone es la **PCB (Printed Circuit Board)**. A su vez, el componente principal de la PCB es el **SoC (System on Chip)**. El SoC habitualmente integra núcleos de CPU y GPU, RAM, ROM, drivers USB, tecnología inalámbrica, etc.
#### <mark style="background: #D2B3FFA6;">Modelo de fabricación de un smartphone</mark>
- **Empresa diseñadora/comercializadora:** diseño final, componentes principales, ensamblaje, venta.
- **Empresa proveedora de SoCs y demás componentes:** diseñan los circuitos integrados y los comercializan. Pueden ser **foundry** o **fabless**.
- **Empresa diseñadora de IPs:** diseñan los componentes que conforman un IC.
- **Empresa fabricante de ICs:** fabrican los ICs diseñados para ellos y otros o exclusivamente para otros (para las fabless generalmente).
## <mark style="background: #ADCCFFA6;">2. Plataformas SoC principales</mark>
Un SoC incluye gran cantidad de componentes. Lo importante no es sólo diseñar el **hardware**, sino también el **software**. El software de un SoC consiste en el kernel de un SO que pueda ejecutarse en el/las CPU/s; y los drivers de cada uno de los periféricos. Se suelen diseñar a partir de **IP Cores**.
### <mark style="background: #FFB86CA6;">IP Cores: Intellectual Property Cores</mark>
Componentes o celdas lógicas reutilizables en múltiples diseños. Se pueden usar dentro de ASIC o de FPGA. Se pueden dividir en varios tipos según:
#### <mark style="background: #D2B3FFA6;">Implementación</mark>
- **Soft Cores:** componentes implementados a nivel RTL, normalmente en lenguajes de descripción de hardware. También se pueden ofrecer en diseño a nivel de puertas lógicas. 
- **Hard Cores:** implementación del core a nivel de transistores.
#### <mark style="background: #D2B3FFA6;">Licencia de uso</mark>
- **Propietarias:** requieren de una licencia comercial para su uso (legal). Por ejemplo: ARM.
- **Abiertas:** equivale al OSS. Por ejemplo: RISC-V.
## <mark style="background: #ADCCFFA6;">3. Plataformas PCB de desarrollo de hardware</mark>
El diseño final consiste en interconectar todos los componentes necesarios, SoC, memorias, etc, entre sí en una PCB. Surge la necesidad de tener PCB para desarrollo a nivel de prototipado. Estas PCB pueden ser microcontrolador, SoC o FPGA y pueden tener los periféricos más habituales ya integrados. El conjunto de la PCB de desarrollo y sus herramientas software forman el **kit de desarrollo**. Se pueden clasificar en tres tipos:
### <mark style="background: #FFB86CA6;">SBM: Single Board Microcontroller</mark>
Hay un gran número por el bajo coste que conlleva fabricar PCB y microcontroladores.
#### <mark style="background: #D2B3FFA6;">STM32 MCU Discovery Kits</mark>
Desarrolladas por STM. Son microcontroladores de 32 bits de arquitectura ARM Cortex.
#### <mark style="background: #D2B3FFA6;">Texas Instruments</mark>
Desarrolladas por TI. Incorporan microcontroladores de la familia msp430 o ARM Cortex. Disponen de una especie de placas de expansión llamadas **booster packs**.
#### <mark style="background: #D2B3FFA6;">ESPRESSIF</mark>
Plataformas desarrolladas por ESPRESSIF que incorporan algún microcontrolador diseñado como empresa fabless por ellos mismos.
#### <mark style="background: #D2B3FFA6;">Wiring</mark>
De esta plataforma deriva arduino.
#### <mark style="background: #D2B3FFA6;">Arduino</mark>
Plataforma hardware de desarrollo abierta, basada principalmente en AVR 8 y ARM Cortex. Tienen placas de expansión llamadas **shields**.
### <mark style="background: #FFB86CA6;">SBC: Single Board Computer</mark>
En el diseño de sistemas empotrados en general se han venido empleando plataformas basadas en microcontroladores. Sin embargo, la evolución tecnológica ha permitido tener microprocesadores de alta capacidad (32/64 bits) y memorias grandes. Se les denomina: Single-Board Computers.
### <mark style="background: #FFB86CA6;">FPGA</mark>
Field Programmable Gate Array. Son básicamente arrays de puertas lógicas programables.
# <mark style="background: #FFF3A3A6;">TEMA 2: Proyecto MySensors</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción a IoT</mark>
El IoT (Internet of Things) es la aplicación de la tecnología para interconectar mediante internet varias cosas que hace años no tenían conexión a internet. Existen varios entornos para aplicaciones IoT, como pueden ser el cuerpo humano, el hogar, las oficinas de trabajo, las fábricas, etc.
### <mark style="background: #FFB86CA6;">Arquitectura de sistemas IoT</mark>
Un sistema IoT se compone de las cosas (things) que proporcionan datos a través de sensores, las redes por las que se transfieren dichos datos y un procesamiento de ellos en un controlador para su posterior uso.
### <mark style="background: #FFB86CA6;">Redes para conexiones IoT</mark>
- WPAN: Wireless Personal Area Network
- WLAN: Wireless Local Area Network
- WMAN: Wireless Metropoly Area Network
- WWAN: Worldwide Wireless Area Network
Se clasifican según dos características principales: **rango de cobertura** y **consumo de potencia**. Hay varias redes muy usadas para corto rango en aplicaciones IoT:
- Bluetooth (Standard y LE)
- Zigbee
- RFID
- RF4CE
- NRF24
- Thread
- WiFi
También hay de largo alcance:
- LP-WAN: Low Power WAN
	- **Espectro sin licencia:**
		- LORAWAN
		- SIGFOX
	- **Espectro con licencia:**
		- LTE-M
		- NB-IoT
## <mark style="background: #ADCCFFA6;">2. Proyecto MySensors</mark>
Proyecto OSH/S (Open Source Hardware/Software) enfocado al IoT DIY (Do-It-Yourself). MySensors es una red de sensores y actuadores que se intercomunican entre sí y también a través de un **gateway** con algún **controlador**. Su arquitectura es la siguiente:
- **Nodos sensores/actuadores:** leen información del sensor y tratan de transmitirla al gateway o reciben órdenes del controlador hacia el actuador.
- **Nodos repetidores:** si un nodo sensor está fuera del alcance del gateway, se comunica con este a través de un repetidor.
- **Gateway:** recibe los datos de los sensores y los pasa al controlador.
### <mark style="background: #FFB86CA6;">Protocolo de comunicación</mark>
`<node-id>;<child-sensor-id>;<command>;<ack>;<type>;<payload>`
# <mark style="background: #FFF3A3A6;">TEMA 3: Diseño y fabricación de PCBs</mark>
## <mark style="background: #ADCCFFA6;">1. ¿Qué es una PCB?</mark>
<div class="nota"><h3>PCB: Printed Circuit Board</h3></div>
Son placas de sustrato no conductor que se emplean para el montaje e interconexión de componentes electrónicos a través de pistas de un material conductor.
### <mark style="background: #FFB86CA6;">Alternativa a la PCB</mark>
Para prototipar se pueden usar protoboards (o regletas), perfboards, stripboards.
## <mark style="background: #ADCCFFA6;">2. Clasificación de las PCB</mark>
### <mark style="background: #FFB86CA6;">Single-sided PCB</mark>
Se interconectan los elementos en una sola cara del substrato, la cara de soldadura (solder side). Los componentes se colocan en la otra. Se emplea para circuitos simples y para minimizar costes. Se suelen fabricar por impresión o por troquelado.
### <mark style="background: #FFB86CA6;">Double-sided PCB</mark>
Se interconectan los elementos en las dos caras del substrato. Generalmente una es la de soldadura y la otra la de componentes. Se emplean en circuitos de mayor densidad de componentes y pistas. Los agujeros de una cara a otra pueden ser con **PTH (plated through-hole)** o sin.
![[Pasted image 20251009170238.png]]
#### <mark style="background: #D2B3FFA6;">Double-sided sin PTH</mark>
La circuitería de ambas caras se realiza o bien soldando en ambas caras o bien con cables a través de agujeros o con algún tipo de ojales (eyelets). Se aconseja minimizar el número de componentes soldados a ambas caras para facilitar el cambio y por la dificultad que supone soldar en la cara de componentes.
#### <mark style="background: #D2B3FFA6;">Double-sided con PTH</mark>
La circuitería de ambas caras se interconecta con la metalización de las paredes de los agujeros metalizados que atraviesan el substrato. Es la más utilizada cuando el circuito es complejo.
![[Pasted image 20251009170726.png]]
### <mark style="background: #FFB86CA6;">Multi-layer PCB</mark>
Los circuitos **VLSI (Very Large Scale Integration)** han aumentado drásticamente la densidad de empaquetado y pistas debido al gran número de pines I/O de algunos chips. Surgen, por tanto, nuevos problemas como el ruido, cross-talk, capacidades parásitas, caídas de tensión, etc. Se emplean láminas de substrato más finas junto con capas de material aislante conocido como **pre-preg (láminas de fibras pre-impregnadas)**. Se usa PTH para interconexión entre capas.
![[Pasted image 20251009171228.png]]
### <mark style="background: #FFB86CA6;">Otros tipos de PCB</mark>
#### <mark style="background: #D2B3FFA6;">Flexibles</mark>
![[Pasted image 20251009171435.png]]
![[Pasted image 20251009171446.png]]
#### <mark style="background: #D2B3FFA6;">HDI (High-Density Interconnect)</mark>
![[Pasted image 20251009171540.png]]
#### <mark style="background: #D2B3FFA6;">IC substrate</mark>
![[Pasted image 20251009171614.png]]
## <mark style="background: #ADCCFFA6;">3. Proceso de implementación en PCB</mark>
### <mark style="background: #FFB86CA6;">1. Diseño</mark>
En esta fase se crea el layout de la PCB mediante la herramienta software adecuada. Se empieza con el esquemático del circuito.
![[Pasted image 20251009173344.png]]
Seguidamente, la herramienta genera el layout con las footprints de los componentes y las pistas entre estos.
![[Pasted image 20251009173436.png]]
Hay varios procesos en la fase de diseño:
- **Creación de librerías de componentes:** antes de realizar un esquemático y posterior layout, es necesario asegurarse de tener todos los símbolos (esquemático) y huellas (footprints) de los componentes a usar.
- **Diseño del esquemático:** se realiza el diseño del esquemático. La mayoría de herramientas permite realizar un **ERC (Electrical Rules Check)** para detectar posibles errores.
- **Diseño del layout:** a partir del esquemático, las herramientas generan un primer layout que habrá que modificar para colocar los componentes en la posición deseada así como enrutar las pistas entre estos. Las herramientas suelen incorporar un **DRC (Design Rule Check)**.
- **Generación de GERBERS:** los archivos GERBER indican la información geométrica de la placa:
	- Gerber de la solder side
	- Gerber de la component side
	- Gerber de los taladros usados
	- Gerber de los agujeros (drills)
	- Gerber de la dimensión de la PCB
	- ...
### <mark style="background: #FFB86CA6;">2. Fabricación</mark>
Se fabrica la PCB en el substrato. El substrato debe de tener diferentes características:
- Mecánicas: rígidos para mantener los componentes, fáciles de taladrar, suficientemente gruesas (entre 0.8mm y 3.2mm pero típico de 1.6mm).
- Químicas: metalizado de los taladros, retardante de las llamas (FR), no absorber demasiada humedad
- Térmicas: disipar el calor, capaz de soportar el calor al soldar
- Eléctricas: constante dieléctrica baja para tener pocas pérdidas a altas frecuencias, punto de ruptura dieléctrica alto
#### <mark style="background: #D2B3FFA6;">Traslado del patrón de circuito al substrato</mark>
**Impresión serigráfica**
Se utilizan tintas especiales resistentes al grabado para marcar el patrón en la capa de cobre. La pintura se puede aplicar con plantillas o un plotter específico para PCBs. Posteriormente se elimina el cobre sobrante no cubierto por la tinta con químicos. También se puede imprimir con tinta conductora usando máquinas como las Voltera.

**Fotograbado**
Usa una transparencia del patrón en negativo para transferirlo a la placa con UV. Requiere placas fotosensibles (el cobre cubierto con resina fotosensible) para que cuando se transfiera, en las zonas que dejen pasar la luz, la resina reaccione.

**Por revelado**
La resina desaparecerá de la placa menos en las zonas del patrón aplicado. Posteriormente se elimina el cobre sin resina con químicos.

**Insoladora**
Caja que dispone de varios tubos fluorescentes de luz UV separados de la superficie por un cristal esmerilado.

**Fresado**
Una máquina tipo plotter hace un "dibujo" sobre la placa empleando fresas que eliminan el cobre de la misma.

**Impresión en material termosensible**
Se imprime el diseño del circuito sobre un papel fotográfico y luego aplicando calor se transfiere a la placa. El cobre no cubierto se elimina con químicos.
#### <mark style="background: #D2B3FFA6;">Metalización o pasta de soldadura</mark>
**STENCIL**: es la mascara que se superpone a la placa dejando al descubierto los pads/vías que deben metalizarse. Suelen estar hechos de acero inoxidable o níquel.
#### <mark style="background: #D2B3FFA6;">Máscara de soldadura</mark>
Para proteger el cobre de la oxidación del aire, se recubre la PCB con un material aislante y no oxidante, dejando al descubierto zonas de soldadura solamente. Le da el color característico a las PCB.
#### <mark style="background: #D2B3FFA6;">Silkscreen</mark>
Capa sobre la máscara de soldadura con tinta no conductora para imprimir texto o información de los componentes sobre la placa.
### <mark style="background: #FFB86CA6;">3. Ensamblaje</mark>
Se montan los componentes en la PCB (ya sean THT o SMD) y se sueldan (con soldadura 
blanda <450ºC o dura >450ºC para plata, oro o acero). 
#### <mark style="background: #D2B3FFA6;">Soldadura</mark>
Durante la soldadura, se dice que el metal de soldadura **"moja al otro metal" (wetted-metal)**, es decir, se adhiere bien al calentarse. En la unión Cu-Sn se forman capas cristalinas cuya resistencia depende de la temperatura y tiempo de calentado (espesor ideal 0.5µm).
Las variables claves de la soldadura son la temperatura, el tiempo, la limpieza, el tipo de flux y la aleación.
#### <mark style="background: #D2B3FFA6;">Uso de flux</mark>
Es como un detergente metálico; limpia óxidos y baja la tensión superficial, ayudando al Sn a extenderse bien.
<div class="nota">El grosor del hilo de Sn debe ser ~1/2 del diámetro del pad</div>
#### <mark style="background: #D2B3FFA6;">Herramientas de soldadura</mark>
- **Soldador:** debe generar el calor necesario para calentar superficies y fundir el material de soldadura. Consta del mango, del elemento de transferencia de calor y de una punta (tip).
- **Soporte del soldador:** se deja encendido el soldador apoyado en el soporte hasta que se caliente para empezar a soldar.
- **Esponja:** esponja de malla de latón o esponja mojada con agua destilada (para no oxidar) para limpiar la punta.
### <mark style="background: #FFB86CA6;">4. Test</mark>
Se somete a la PCB a tests para probar su correcto funcionamiento.
- Sin componentes: a nivel de pistas y con el polímetro
- Con componentes: probar los componentes sobre la placa