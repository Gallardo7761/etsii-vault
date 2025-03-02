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