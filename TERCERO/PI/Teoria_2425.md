# <mark style="background: #FFF3A3A6;">TEMA 1: Introducción</mark>
## <mark style="background: #ADCCFFA6;">1. Estructura básica de un computador</mark>
![[Pasted image 20250218105957.png|400]]
![[Pasted image 20250218110013.png|400]]
![[Pasted image 20250218110027.png|450]]
## <mark style="background: #ADCCFFA6;">2. Entrada/Salida</mark>
![[Pasted image 20250218110130.png|500]]
El sistema de E/S tiene varias funciones:
- **Direccionamiento:** para seleccionar el dispositivo
- **Sincronización:** para iniciar la transferencia
- **Transferencia:** método de transferencia
#### <mark style="background: #D2B3FFA6;">Interfaz E/S (simplificada)</mark>

![[Pasted image 20250218110255.png|500]]
### <mark style="background: #FFB86CA6;">Entrada/Salida programada</mark>
- La comunicación siempre la inicia la CPU
- Método de _queries_ para conocer el estado del módulo E/S
- **Inconvenientes:**
	- La CPU tiene que gastar tiempo de ejecución en atender a los procesos E/S
- **Ventajas:**
	- Alta velocidad
Hay dos tipos de direccionamiento:
- **Mapeado en memoria:** la CPU ve los dispositivos E/S como posiciones de memoria. Es más sencillo pero se pierde espacio de memoria.
- **Aislado:** las direcciones E/S se diferencian mediante una señal de control.
### <mark style="background: #FFB86CA6;">Entrada/Salida por interrupciones</mark>
- Un dispositivo puede llamar la atención de la CPU:
	- El módulo E/S provoca la interrupción
	- La CPU le comunica un orden de E/S y vuelve a lo suyo (cambio de contexto)
	- Cuando la subrutina de E/S se ejecuta el módulo de E/S lo comunica para que el CPU decida cuál será su próxima acción y estado.
- **Ventajas:**
	- Atención inmediata
	- El CPU puede hacer otras cosas mientras el dispositivo E/S está ocupado

# <mark style="background: #FFF3A3A6;">TEMA 2: Buses locales normalizados</mark>
## <mark style="background: #ADCCFFA6;">1. Concepto de bus normalizado</mark>
**Bus:** conjunto de líneas eléctricas sobre la PCB. Es un medio compartido. 
### <mark style="background: #FFB86CA6;">Ejemplo de bus y sus líneas</mark>
Un ejemplo de bus sería el **Bus de Control** con las líneas típicas:
- Memory write
- Memory read
- I/O Write
- I/O Read
- Transfer ACK
- Bus Request
- Bus Grant
- Int Request
- Int ACK
- Clock
- Reset
### <mark style="background: #FFB86CA6;">Procesos de transferencia</mark>
Por ejemplo para la escritura E/S:
1. El módulo de E/S que quiere iniciar la transferencia solicita el uso del bus (**Bus Request**)
2. El arbitrador concede el bus (**Bus Grant**)
3. Sitúa en el bus de direcciones (AB) la dirección o puerto E/S donde quiere transferir el dato
4. Sitúa el dato a transferir en el bus de datos (DB)
5. Activa la línea de **I/O Write** del bus de control
6. El destinatario ha recibido el dato (**Transfer ACK**)
7. Deja libre el bus para su uso
<div class="nota"><h4>NOTACIÓN</h4><ul><li><strong>Bus Master:</strong>Módulo que inicia la transmisión</li><li><strong>Bus Slave:</strong>Módulo direccionado por el Bus Master</li><li><strong>Arbitrador:</strong>circuito especial que recoge las peticiones para tomar el control del bus y decide quién lo toma</li></ul></div>
### <mark style="background: #FFB86CA6;">Arquitectura de un PC actual</mark>
![[Pasted image 20250225110824.png]]
## <mark style="background: #ADCCFFA6;">2. Métodos de conexión de un dispositivo al bus local</mark>
### <mark style="background: #FFB86CA6;">Conexión directa</mark>
**Restricciones:**
- Depende del CPU
- Sólo lo puede usar un dispositivo local para evitar problemas de impedancia por extra carga
- Costosa, debido a la alta frecuencia
- No permite transferencias de datos entre CPU y otros dispositivos mientras se usa este bus con otros periféricos
**Ejemplo:** VLB tipo A
![[Pasted image 20250225111612.png|300]]
### <mark style="background: #FFB86CA6;">Conexión mediante buffer</mark>
**Mejoras:**
- Sólo presenta una impedancia. Usualmente hasta 3 dispositivos
**Restricciones:** 
- En esencia, con buffer y el local son un único bus: cualquier transferencia iniciada por CPU alcanzará el bus local con buffer. **NO** es posible la utilización simultánea.
**Ejemplo:** VLB tipo B
![[Pasted image 20250225111801.png|300]]
### <mark style="background: #FFB86CA6;">Conexión con filosofía workstation</mark>
**Mejoras:**
- Introducción de caché L2 unida a un puente para adaptar las velocidades de transferencia entre bus local del CPU y el de E/S.
- Independencia del procesador que implementa la CPU.
**Ejemplo:** PCI
![[Pasted image 20250225111938.png|300]]
## <mark style="background: #ADCCFFA6;">3. PnP</mark>
### <mark style="background: #FFB86CA6;">El problema</mark>
- Hay un número limitado de IRQs, canales DMA, puertos de E/S y regiones de memoria E/S
- Algunas IRQs y direcciones están muy estandarizadas
- No hay automatización de las tareas de configuración de periféricos
### <mark style="background: #FFB86CA6;">Solución PnP</mark>
1. El programa de configuración PnP encuentra todos los dispositivos que soportan PnP y pregunta a cada uno qué recursos del bus necesitan.
2. Decide qué recursos puede adjudicar.
3. Establece un criterio mediante el cual adjudicar recursos del bus.
## <mark style="background: #ADCCFFA6;">4. Bus SPI</mark>
- El bus SPI (Serial Peripheral Interface) fue desarrollado por Motorola en 1980. Sus ventajas respecto a otros sistemas han hecho que se convierta en un estándar.
- El bus SPI tiene arquitectura Master-Slave. El dispositivo maestro puede iniciar la comunicación con uno o varios esclavos y  envía (Tx) o recibe (Rx) datos de ellos. Los esclavos ni pueden iniciar comunicación ni comunicarse entre ellos.
- Hay una línea para transmisión (Tx) y otra para recepción (Rx) por lo que la comunicación es Full Dúplex.
- Es síncrono, el dispositivo maestro proporciona el CLK.
![[Pasted image 20250225112945.png|400]]
- **MOSI (Master-Out, Slave-In):** para la comunicación maestro a esclavo.
- **MISO (Master-In, Slave-Out):** para la comunicación esclavo a maestro.
- **SCK (Clock):** señal de CLK del maestro.

![[Pasted image 20250225113139.png|600]]
## <mark style="background: #ADCCFFA6;">5. Bus I2C</mark>
- El estándar I2C (Inter-Integrated Circuit) requiere únicamente dos cables para su funcionamiento, uno para CLK y otro para el envío de datos (SDA). El funcionamiento es más complejo así como su circuitería.
   ![[Pasted image 20250225113929.png]]
- En el bus cada dispositivo tiene una dirección, que se emplea para acceder a ellos de forma individual. Esta dirección puede ser fijada por hardware o software.
- En general, cada dispositivo conectado al bus debe tener dirección única. Se podría cambiar la dirección o implementar un segundo bus.
- Este bus tiene una arquitectura maestro-esclavo. Es posible que haya más de un maestro pero **SÓLO** un maestro a la vez.
- Es síncrono, el maestro proporciona CLK.
- Tiene resistencias de pull-up a $V_{CC}$ .
# <mark style="background: #FFF3A3A6;">TEMA 3: Teclados</mark>
## <mark style="background: #ADCCFFA6;">1. Fundamentos físicos</mark>

![[Pasted image 20250311105624.png]]
![[Pasted image 20250311110117.png]]
Las teclas están "mapeadas" a unos códigos llamados Scan:

![[Pasted image 20250311110853.png]]

![[Pasted image 20250311111634.png]]
## <mark style="background: #ADCCFFA6;">2. Estructura y funcionamiento</mark>

![[Pasted image 20250311111721.png]]
![[Pasted image 20250311112420.png]]

# <mark style="background: #FFF3A3A6;">TEMA 5: La interfaz ATA/IDE</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
La interfaz usada para comunicar el disco duro y unidades ópticas con el PC se suele llamar IDE (Integrated Drive Electronics) aunque su nombre oficial es ATA (Advanced Technology Attachment). 
- **ATA**: ATA originalmente es una interfaz paralela de 16 bits. 
- **SATA:** Al final de los 2000 se presentó una nueva interfaz: SATA (Serial ATA) que sería adoptada por los PC de sobremesa y portátiles a los pocos años. SATA envía los bits uno a uno, lo que permite que los cables sean más pequeños y finos y con más rendimiento. SATA es compatible con ATA a nivel software.
### <mark style="background: #FFB86CA6;">Conectores</mark>
![[Pasted image 20250507203728.png]]

## <mark style="background: #ADCCFFA6;">2. Tipos de ATA</mark>

#### UDMA == Ultra-ATA

- **ATA-1:** Originalmente basada en el bus ISA.
	- M-S
	- Programmable I/O y Direct Memory Access
	- Traducción de CHS y LBA admiten hasta 136,9GB pero limitado por BIOS a 8,4GB
- **ATA-2:** 
	- PIO y DMA más rápidos
	- Gestión de energía
	- Dispositivos extraíbles. Soporte PCMCIA (PC Card, una especie de tarjeta de expansión de memoria).
- **ATA-3:** 
	- S.M.A.R.T para auto análisis e informes del estado de la unidad.
	- LBA obligatorio
	- Seguridad con contraseña
- **ATA/ATAPI-4:** 
	- Interfaz comun para CD-ROM, CD-RW, disquete, zip, cinta, etc
	- Agrega transferencia UDMA/33 (33MB/s)
- **ATA/ATAPI-5:** 
	- Duplica la velocidad de UDMA/33 (66MB/s).
	- Cables de 80 hilos
- **ATA/ATAPI-6:** 
	- Reduce tiempos de configuración y aumenta la velocidad de reloj,
	- Aumenta la velocidad de transferencia de UDMA a 100MB/s
	- CHS se vuelve obsoleto
	- LBA se extendió de 228 a 248 admitiendo discos de hasta 144.12PB.
- **ATA/ATAPI-7:** 
	- Transferencias UDMA de 133MB/s. 
	- SATA 1.0 como parte del estándar ATA-7
- **ATA/ATAPI-8:** 
	- Agrega las versiones SATA 2.x y 3.x.
	- Reemplaza funciones largas de R/W
	- Comando TRIM para SSD, para informar al SO que bloques no están en uso para borrarse y prepararse para W en el futuro.
### <mark style="background: #FFB86CA6;">Limitaciones de BIOS</mark>
**Bios UEFI para arrancar desde unidades >2,2TB**. El límite de 8,4GB viene del sistema CHS:
- Cylinders: 1024 máx (registro 10b)
- Heads: 255 máx (registro 8b)
- Sectors per track: 63 máx (registro 6b)
En total: $1024\times 255\times 63\times 512 \text{bytes}/\text{sector}=8,4GB$
### <mark style="background: #FFB86CA6;">Conversión CHS/LBA y LBA/CHS</mark>
De CHS a LBA:
$LBA=(((C\times HPC)+H)\times SPT)+S-1$

De LBA a CHS:
$C=int(LBA/SPT/HPC)$
$H=int((LBA/SPT)\mod HPC)$
$S=(LBA\mod SPT)+1$

Donde: 
- LBA: Logical Block Address
- C: Cylinder
- H: Head
- S: Sector
- HPC: Heads per cylinder
- SPT: Sectors per track
- int X: parte entera de X
- X mod Y: X%Y (resto de X/Y)
## <mark style="background: #ADCCFFA6;">3. Barreras</mark>
<span style="color:red;">Sectores de 512B típicos</span>
### 528MB
Fue el límite de las BIOS con CHS
1024 cilindros, 16 cabezas, 63 sectores = 528 MB
### 2,1GB
BIOS que traducían hasta:
4096 cilindros, 16 cabezas y 63 sectores = 2,1 GB
### 4,2GB
BIOS que traducían hasta :
1024 cilindros, 256 cabezas virtuales, y 63 sectores = 4,2 GB
### 8,4GB
Límite del INT13h extendido de 24 bits
$2^{24}=8,4~GB$
### 137GB
Límite del LBA28: $2^{28}$ sectores = 137GB
Se crea LBA48 para solucionarlo (144.12 PB).
### 2,2TB
La limitación viene de que MBR (Master Boot Record) usa un campo de 32 bits para la cantidad de sectores. 
Se usa GPT, que usa 64 bits, para solucionarlo (9.4 ZB)
# <mark style="background: #FFF3A3A6;">TEMA 6: Disco Duro</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
Consiste en discos giratorios con cabezales que se mueven sobre los discos (aunque sin llegar a tocarlos) y almacenan datos en los sectores (trocitos) de las pistas (anillos concéntricos). Los sectores son normalmente de 512B o 4KB. Hace años giraban sobre 3600rpm aunque ahora las velocidades más usadas son de 5400rpm, 7200rpm, 10000rpm, 15000rpm.
## <mark style="background: #ADCCFFA6;">2. Pistas y sectores</mark>
Los sectores se numeran empezando por 1 y las cabezas y cilindros empezando por 0. Por ejemplo, un disquete de 1.44MB contiene 80 cilindros (0-79) y dos cabezas (0,1) mientras que cada pista en cada cilindro tiene 18 sectores (1-18).
## <mark style="background: #ADCCFFA6;">3. Formato y particiones</mark>
Hay dos tipos de formato, de bajo nivel (LLF) o de alto nivel (HLF). El comando FORMAT realiza el de alto nivel, el de bajo nivel se realiza en fábrica. Para realizar HLF, se requiere crear particiones (secciones del disco con una letra asignada, como C:). En resumen, los pasos son:
- Formateo LLF
- Particionado
- Formateo HLF
### <mark style="background: #FFB86CA6;">Distintos formatos de particiones</mark>
- **FAT (File Assignation Table):** Compatible con DOS y Windows 9x/Me. Admiten nombres de archivos de 11 caracteres como máximo (8 caracteres + 3 de extensión de archivo) en DOS y 255 caracteres en W9x o posterior. Se usan números de 12 o 16 bits para identificar grupos, lo que resulta en volúmenes máximos de 2GB.
- **FAT32:** Volumen único máximo de 2TiB y tamaño de archivo máximo de 4GB, ya que se usan 32 bits.
- **exFAT:** Volúmenes y archivos de 512TB.
- **NTFS:** Nombres de archivos de 256 caracteres y volúmenes y archivos de hasta 16EB teóricos.
## <mark style="background: #ADCCFFA6;">4. Rendimiento y velocidad de transferencia</mark>

$$
\begin{equation}
T_{ACCESO}=T_{BUSQUEDA}+latencia~~~~\text{ms}
\end{equation}
$$
**Tiempo de acceso:** Es la cantidad de tiempo total promedio requerida para que la unidad acceda a un sector aleatorio.
$$
\begin{equation}
T_{BUSQUEDA}=T_{DESPLAZAMIENTO}\times\frac{\text{nº pistas}}{2}+T_{ESTABILIZACION}
\end{equation}
$$
$$
\begin{equation}
\text{latencia}=\frac{T_{ROTACION}}{2}=\frac{\frac{1}{v_{ROTACION}}}{2}=\frac{0,5}{v_{ROTACION}}=\frac{0,5}{\frac{rpm}{60}}=\frac{30}{rpm}~~~\text{ms}
\end{equation}
$$
**Transferencia externa:** velocidad a la que se pueden mover los datos entre la placa base y el buffer del disco.
**Transferencia interna:** velocidad R/W del disco:
- Velocidad de rotación: rpm
- Densidad (sectores/pista): 
  $$
  \begin{equation}
  \frac{\text{nº bits sectores}}{2\pi\text{r}}=\frac{\frac{\text{nº sectores}}{\text{pista}}\times\text{512 B}\times\text{8 bits}}{2\pi\text{r}}
  \end{equation}
   $$
   
Finalmente, el **Tiempo de R/W medio** es:
$$
\begin{equation}
T_{R/W}=T_{ACCESO}+T_{Tx}+T_{DRIVER}=\text{latencia}+T_{BUSQUEDA}+\frac{1}{v_{Tx} (B/s)}+T_{DRIVER}
\end{equation}
$$

# <mark style="background: #FFF3A3A6;">TEMA 8: Hardware de vídeo</mark>
## <mark style="background: #ADCCFFA6;">1. Fundamentos físicos</mark>
### <mark style="background: #FFB86CA6;">Monitor CRT</mark>
![[Pasted image 20250520105622.png]]
La frecuencia de refresco horizontal (48KHz) sirve para "rellenar" el monitor horizontalmente, mientras que la vertical (100Hz) sirve para retornar el refresco a la primera línea.
### <mark style="background: #FFB86CA6;">Monitor LCD</mark>
![[Pasted image 20250520110041.png]]
Varias capas de cristal líquido en varias orientaciones. Los rayos que atraviesan: píxel encendido. Los rayos que no atraviesan: píxel apagado.
- **Iluminación transmisiva:** fuente propia (LCD)
- **Iluminación reflexiva:** fuente externa (pantallas de relojes, calculadoras, etc)
La imagen se forma mediante una matriz de células LCD.
- **Matriz pasiva:** LCD clásico.
	- Disposición en forma de enrejado
	- La luz se genera globalmente y la matriz la modifica
- **Matriz activa:** TFT (Thin Film Transistor). Matriz de transistores fotoemisores (FET). Cada célula tiene luz propia. Mejor resolución y contraste.
![[Pasted image 20250520110439.png]]
# <mark style="background: #FFF3A3A6;">TEMA 9: Hardware de audio</mark>
Normalmente tarjeta de audio de 16 bits. Para la síntesis se necesita un generador. Normalmente un sintetizador FM (Frequency Modulation) y una Wave Table. La reproducción la realiza el DSP. Desde el punto de vista de los estándares:- 
- **Estándares HW**:
	- **Sound Blaster:** de Creative Labs. Primer estándar de audio digital. **Se realiza la reproducción y síntesis por HW**. 
- **Estándares SW**:
	- **DirectSound:** estándar software de sonido de Microsoft.
		- **DirectX:** sobre todo para juegos. Introduce DS3D (DirectSound 3D) para audio 3D.
	- **A3D:** Aureal & NASA. Permite añadir a DirectSound acceleración HW. 
	- **EAX:** Creative Labs. Similar a A3D pero menos prestaciones.