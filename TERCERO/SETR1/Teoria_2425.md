# <mark style="background: #FFF3A3A6;">TEMA 1: Introducción y conceptos básicos</mark>
## <mark style="background: #ADCCFFA6;">1. Clasificación</mark>
Los sistemas de propósito general se pueden diferenciar de los sistemas empotrados, pero no es una clasificación estricta sino más bien un rango o gradiente.
![[Pasted image 20250206105144.png]]
- **Sistema tiempo real VS Sistema convencional:** Freno ABS frente a procesador de texto.
- **Sistema de uso industrial VS Producto de consumo:** Máquina de empaquetado de patatas fritas frente a consola de videojuegos.
- **Sistema dirigido por eventos VS Sistema dirigido por datos:** Freno ABS frente a MP3.
	- Procesadores digitales de señal (DSP): para procesar flujos de datos.
- **Sistema artesanal/prototipo VS Sistema industrializado:** Puerta de garaje autoconstrucción frente a lavadora comercial.
### <mark style="background: #FFB86CA6;">Dispositivos comunes</mark>
- PC sobremesa.
- Autómatas Programables Industriales (API) o Controladores Lógicos Programables (PLC).
- Microcontroladores.
- Procesadores Digitales de Señal (DSP).
- Lógica programable (FPGAs, PLDs).
- Sistemas _Full Custom_.
### <mark style="background: #FFB86CA6;">Principales diferencias GPC VS Embedded System</mark>
<strong><span style="color:red;">[EXAMEN]</span></strong>
![[Pasted image 20250206111909.png]]
## <mark style="background: #ADCCFFA6;">2. Características especiales de los sistemas empotrados</mark>
### <mark style="background: #FFB86CA6;">Memoria</mark>
- Siempre ejecutan el mismo código: necesitan normalmente más ROM que RAM, el código se encuentra en la ROM y el _stack_ en la RAM, aunque algunos no tienen.
- <strong><span style="color:red;">[EXAMEN]</span></strong> Las variables en la ROM son constantes, pero ¿las globales en RAM se inicializan explícitamente?
	- Si la variable la declaramos e inicializamos a la vez, mientras tengamos el microcontrolador conectado al IDE, es válido. Sin embargo, al funcionar el microcontrolador independientemente, si queremos inicializar una variable global, deberíamos declararla y luego inicializarla dentro del código, ya que no hay intérprete ni nada que la inicialice en tiempo de ejecución. **ACTUALMENTE** se soluciona con un código _startup_ antes del `main` para inicializar este tipo de variables globales. En el código _startup_ también se inicializan punteros de pila.
	- Se puede usar la palabra reservada `const` para establecer que ese código está en ROM, y en este caso se podría inicializar en la declaración.
- No requiere soporte de almacenamiento tipo HD.
- Pueden poseer o no protección para acceder a datos/código internos.
- Por compatibilidad y limitaciones, uso de bancos de memoria.
### <mark style="background: #FFB86CA6;">Datos e instrucciones</mark>
- Preparados para manipulación de bits
- El ISA suele ser recortado: instrucciones de salto condicional, instrucciones aritméticas, signo, modos de direccionamiento, registro de estado "pobre"...
### <mark style="background: #FFB86CA6;">Fallos y robustez</mark>
- No suelen ser sistemas atendidos, el usuario no es consciente de que es un computador.
- Implementan sistemas de tolerancia y recuperación de fallos
- Se han ampliado los rangos de funcionamiento en cuanto a temperatura, radiación, niveles de alimentación, etc
### <mark style="background: #FFB86CA6;">Consumo</mark>
- Optimizados para bajo consumo
- Utilización en aplicaciones móviles modos de bajo consumo (ej: mando a distancia)
### <mark style="background: #FFB86CA6;">Programación y desarrollo</mark>
- Difícil de depurar
- Siempre ejecutan el mismo software
- Los programas no suelen acabar `while(1) {...}`
### <mark style="background: #FFB86CA6;">Tamaño y precio</mark>
- En grandes cantidades son muy baratos
- Multitud de formatos para satisfacer la industria
## <mark style="background: #ADCCFFA6;">3. Sistemas tiempo real</mark>

![[Pasted image 20250206122521.png]]
### <mark style="background: #FFB86CA6;">Sistema Tiempo Real</mark>
Sistema informático en el que la corrección del resultado depende tanto de su validez lógica como del instante en que se produce.
### <mark style="background: #FFB86CA6;">Computador Empotrado</mark>
Computador integrado en algún mecanismo físico para implementar un tipo de control de procesos o toma de datos.
### <mark style="background: #FFB86CA6;">Sistema Empotrado Tiempo Real</mark>
Sistema informático que ejecuta alguna aplicación de tiempo real y tiene al menos un elemento que sea un computador empotrado.
<div class="nota">
<h1>Variables en C</h1>
<ul>
<li><strong>Estáticas: </strong><code>heap</code>, memoria principal</li>
<li><strong>Automáticas: </strong>pila</li>
<li><strong>Dinámicas: </strong>asignación y liberación de memoria con <code>malloc</code> y <code>free</code><br>Como ejemplo de esto:<br><code>int* a = malloc(10*sizeof(int));</code></li><h3>¿Se puede reservar dinámicamente en SSEE?</h3><p>Como la cantidad de memoria en los SSEE es limitada, no es que no se pueda usar, pero hay que tener mucho cuidado al usar la asignación dinámica ya que puede llegar un momento en que se pida reservar más memoria de la disponible. Para asegurar que el sistema sea seguro, se recomienda usar <strong>variables estáticas</strong></p>
</ul>
</div>
### <mark style="background: #FFB86CA6;">Tareas</mark>
Esencialmente hay dos tipos de tareas:
- **Esencial:** presenta restricciones temporales
	- **Críticas:** SIEMPRE se debe completar dentro del plazo
	- **Acríticas:** puede completarse fuera de plazo "sin problemas"
- **No esencial:** no presenta restricciones temporales
Dadas estas definiciones, se pueden clasificar los sistemas de tiempo real en **duros** y **blandos**:
- **STR Duro:** al menos posee una tarea crítica
- **STR Blando:** todas sus tareas son acríticas
### <mark style="background: #FFB86CA6;">Técnicas de programación</mark>
#### 1. Procesamiento secuencial (`while(1)`)
```C
int main()
{
	while(1)
	{
		TareaA();
		TareaB();
		TareaC();
		TareaI(); // añadida temporización
	}
}
```
#### 2. Foreground/Background
![[Pasted image 20250213110517.png|500]]
#### 3. RTOS

# <mark style="background: #FFF3A3A6;">TEMA 2: Estructura de los μC</mark>
## <mark style="background: #ADCCFFA6;">1. Conceptos generales</mark>
### <mark style="background: #FFB86CA6;">Familias de μC</mark>
- Al igual que los GPP, se agrupan en familias.
- Suelen poseer un núcleo común, pero hay más diversidad dentro de las familias.
- Los fabricantes sacan una gran variedad para cubrir cada necesidad.
- Familias históricas 8 bits: Intel 8051, Motorola 68XX, Atmel AVR, etc.
- Familias 32 bits: ARM, Freescale 683XX, etc.
### <mark style="background: #FFB86CA6;">Estructura de memoria</mark>
- Desde el punto de vista del programador, hay al menos dos tipos de memoria (ROM y RAM).
- Si se usa la arquitectura Harvard, las instrucciones para acceder a código o datos pueden ser distintas, y a los registros internos se le pueden asociar punteros de diversos tamaños.
	![[Pasted image 20250213120645.png|500]]
	La más adecuada para μC es la de Harvard ya que los μC suelen tener ROM/Flash para código y memoria de datos.
- En un μC puede estar justificado el que haya múltiples elementos con buses de direcciones distintos:
	- Memoria Programa Interna
	- Memoria Datos Interna
	- Memoria Programa Externa
	- Memoria Datos Externa
	- Registros E/S
	- Direccionamiento bit a bit en memoria y periféricos
<div class="nota">
<h1>Nota</h1>
<p><strong>Single Chip (Microcontrolador, MCU): </strong>RAM y ROM internas</p>
<p><strong>Modo expandido (Microprocesador, MPU): </strong>se sacan desde el chip buses de datos, direcciones y líneas de control hacia fuera para RAM y ROM externas</p>
</div>

## <mark style="background: #ADCCFFA6;">2. Intel 8051</mark>
![[Pasted image 20250220110642.png|500]]
![[Pasted image 20250220110558.png|400]]
![[Pasted image 20250220110616.png]]
### <mark style="background: #FFB86CA6;">Características</mark>
- Punteros de datos y de pila de distinto tamaño que PC (Program Counter) o IP (Instruction Pointer) porque es Harvard.
- Complejidad de mapa de memoria.
- Juego de instrucciones limitado.
- La pila está en memoria de datos interna y limitada a 256B por ser el SP de 8b.
- Instrucciones de acceso memoria-memoria sin pasar por un acc.
- Acumuladores accesibles por dir. directo.
- Necesario acceder a memoria de código para datos constantes -> `movc`.
### <mark style="background: #FFB86CA6;">Mapa de memoria</mark>
![[Pasted image 20250220114954.png]]
Asignación memoria variables:
- DATA: 128B bajos en RAM interna
- BDATA: memoria direccionable  bit
- IDATA: 128B direccionamiento indirecto
- PDATA: memoria por bancos
- XDATA: memoria datos externa
- CODE: memoria de código
**Ejemplos:**
```C
code unsigned char var1;

bdata char var2;
sbit var2bit2 = var2^2;
var2bit2 = 1;
```
## <mark style="background: #ADCCFFA6;">3. Cortex M</mark>
### <mark style="background: #FFB86CA6;">Introducción</mark>
- Arquitectura ARM (Advanced RISC Machines)
- Empezó en 1983 de parte de Acorn Computers Ltd.
- Originalmente para PCs (arquitectura Von Neumann)
- RISC -> instrucciones de 4B. Los más recientes traen un conjunto de instrucciones de 16 bits adicional, llamado **Thumb**.
### <mark style="background: #FFB86CA6;">Buses</mark>
#### <mark style="background: #D2B3FFA6;">Tipos de buses</mark>
- **AHB (Advanced High performance Bus):** memoria, periféricos rápidos...
- **APB (Advanced Peripheral Bus):** periféricos generales
- **PPB (Private Peripheral Bus):** acceso al debugger e interrupciones
![[Pasted image 20250227115420.png|600]]
#### <mark style="background: #D2B3FFA6;">Buses internos</mark>
- **Buses CORE:**
	- **Icode bus AHB:** acceso a la memoria de código (ROM, Flash...) alineados de 32 en 32 bits (al usar instrucciones de 16  bits se toman instrucciones de 2 en 2).
	- **Dcode bus AHB:** acceso a datos almacenados en memoria de código sin alineamiento.
	- **System bus AHB:** acceso R/W a la zona de memoria SRAM (interna, externa, extendida), periféricos y controladores rápidos (DMA)
	- **Internal PPB:** debugger, interrupciones, etc
### <mark style="background: #FFB86CA6;">Mapa de memoria</mark>
![[Pasted image 20250227120032.png|500]]

### <mark style="background: #FFB86CA6;">Registros</mark>
![[Pasted image 20250306113133.png]]
- **R0-R12:** propósito general
	- R0-R7: cualquier instrucción
	- R8-R12: no siempre, por ejemplo para instrucciones Thumb (16b)
- **R13:** Stack Pointer. Hay dos SP, el MSP (Main Stack Pointer) para el modo privilegiado y el PSP (Process Stack Pointer) para el modo no privilegiado.
- **R14:** Link Register. Almacena la dirección de retorno de subrutina.
- **R15:** Program Counter.
- **PSR:** Program Status Register. Internamente 3 registros.
  ![[Pasted image 20250306113416.png]]
	- APSR: flags de la última ejecución
	- IPSR: código de la interrupción en ejecución
	- EPSR: información de la instrucción en ejecución
### <mark style="background: #FFB86CA6;">Set de instrucciones</mark>
![[Pasted image 20250306113057.png]]
#### <mark style="background: #D2B3FFA6;">Instrucciones Thumb y Thumb-2</mark>
- Las instrucciones en los Cortex ocupan 32b.
- Se necesitan 4B por instrucción.
- Un mismo programa ocupa 4 veces más para un Cortex que para un procesador de 8b (ATMEGA, etc)
- Actualmente soportan el juego de instrucciones Thumb-2 (combinan instrucciones de 32b y 16b), pero implica un descenso en el rendimiento al añadir latencia de descodificación de instrucciones.
## <mark style="background: #ADCCFFA6;">4. Manipulación de bits</mark>
- **El método clásico:** Para cambiar un bit de un registro de (por ejemplo) 8 bits, se debe leer, aplicar máscara y escribir el valor modificado. En conclusión: RMW (Read-Modify-Write).
- **El problema:** Si una vez leído el registro, se interrumpe la tarea por otra que modifica otro bit del registro, al escribir el dato de antes, el nuevo bit se machaca.
### <mark style="background: #FFB86CA6;">Métodos de manipulación de bits específicos en μC</mark>
En C tradicional se suelen usar máscaras y campos de bit en structs. En principio, si la unidad mínima direccionable es el byte, los campos de bit implican la necesidad de realizar máscaras para el acceso a los datos sin modificar el resto.
```C
struct mybitfields
{
	unsigned short a : 4;
	unsigned short b : 5;
	unsigned short c : 7;
} test;

int main(void)
{
	test.a = 2;
	test.b = 31;
	test.c = 0;
}
```
Esto depende mucho del **compilador y del μC** ya que puede hacerse tradicionalmente (RMW) o en un sólo paso si se admite el direccionamiento a bit.
#### <mark style="background: #D2B3FFA6;">1. Mapa de memoria direccionable bit</mark>
Hay algunas instrucciones que direccionan a nivel de bit.
![[Pasted image 20250306121610.png]]
#### <mark style="background: #D2B3FFA6;">2. Instrucción compleja que realiza RMW de una vez</mark>
Por ejemplo las instrucciones:
```asm
BRSET dir, mascara, etiqueta
BSET  dir, mascara, etiqueta
BCLR  dir, mascara
BIC   r0, r1, #1 // Pone a 0 en r1 los bits que diga en #X
```
#### <mark style="background: #D2B3FFA6;">3. Direccionamiento de bit dentro de un byte</mark>
```asm
BCF dirf, numbit (PIC)
SBI 0x18, 4 (ATMEGA)
```
#### <mark style="background: #D2B3FFA6;">4. Crear una zona de memoria en la que cada posición de 32 bits está asociada a un bit de otra zona de memoria: bit banding</mark>
![[Captura desde 2025-03-06 12-24-34.png]]
El tamaño máximo de SRAM son 512MB - 32MB ya que estos se usan para apuntar a los bits de la zona de memoria de bit banding.

| bitX     | DATO     | 0x4242028C     |
| -------- | -------- | -------------- |
| **bit3** | **DATO** | **0x42420288** |
| **bit2** | **DATO** | **0x42420284** |
| **bit1** | **DATO** | **0x42420280** |
## <mark style="background: #ADCCFFA6;">5. Interrupciones</mark>
#### <mark style="background: #D2B3FFA6;">Aclaraciones</mark>
Según el origen:
- Hay interrupciones internas al procesador que se producen por algún dispositivo propio dentro del μC.
- Hay interrupciones externas que se activan mediante un pin externo (por nivel o por flanco).
Según el uso:
- Hay interrupciones debidas a fallos (excepciones).
- Hay interrupciones software (como saltos a subrutinas privilegiados).
- Hay interrupciones de gestión de periféricos (las normales).
Posibilidad de enmascarar:
- Interrupciones enmascarables
- Interrupciones no enmascarables (NMI).
### <mark style="background: #FFB86CA6;">Tipos de excepciones</mark>
- **Reset:** tras encendido o aplicar un '0' a la señal reset del CPU. Cuando se produce el CPU para la ejecución independientemente de la instrucción que estuviese ejecutando. Cuando reset se desactiva, se ejecuta la ISR de reset, reiniciando el procesador.
- **HardFault:** se dispara cuando hay un error en el tratamiento de una interrupción/excepción. Ej: ISR no definida
- **MemManage:** ocurre cuando el MPU falla o hay accesos a zonas de memoria reservada.
- **BusFault:** ocurre cuando una transacción en un bus falla o hay un error de E/S. Ej: no existe el periférico en puerto X.
- **UsageFault:** se produce cuando hay un fallo al ejecutar una instrucción
	- Instrucción no definida
	- Acceso desalineado ilegal
	- Estado inválido durante ejecución (por ej. dividir por 0)
	- Error en la finalización de una ISR
### <mark style="background: #FFB86CA6;">Tipos de interrupciones software</mark>
- **SVCall:** supervisor call. Se dispara por la instrucción SVC. La usa el SO para asegurarse de que sus instrucciones no se interrumpan y ejecutar bloques de código de forma atómica.
- **PendSV:** permite acceso privilegiado a memoria y registros.
### <mark style="background: #FFB86CA6;">Manejadores de Interrupciones/Excepciones</mark>
Las rutinas que las manejan se clasifican en:
- **Interrupt Service Routines (ISR):** manejan las interrupciones de los periféricos
- **Fault Handlers:** manejan las excepciones en caso de error
- **System Hanlders:** manejan las interrupciones del SO: PendSV, SVCall, SysTick
### <mark style="background: #FFB86CA6;">Gestión de periféricos</mark>
- A cada fuente de int se le asocia un trozo de código: ISR
- Las ISR en μC pequeños suelen no anidarse
- El concepto de anidamiento implica:
	- Niveles de prioridad
	- Un dispostivo hardware controlador de interrupciones
- Para saber en que sitio esta la ISR: tabla de vectores de interrupcion
![[Pasted image 20250313114416.png]]
### <mark style="background: #FFB86CA6;">NVIC: Nested Vector Interrupt Controller</mark>
- Integrado en el núcleo del Cortex
- Permite asignar niveles de prioridad
- Las interrupciones más prioritarias interrumpen a las de menos prioridad: interrupciones anidadas
- Permite configurar **150 interrupciones** con **16 niveles de prioridad**
- Diseñado para minimizar la latencia
- Permite cambiar la tabla de vectores de 
- PPB (Private Programming Bus): para configurar por software
- Tiene múltiples conexiones con el Core:
	1. Interrumpir al core y enviarle codigo de interrupcion
	2. Conexion E/S para escribir su configuracion
	3. Lineas de excepciones
### <mark style="background: #FFB86CA6;">Registros Cortex M4</mark>
```C
register int nombrereg _asm("r13");
__set_BASEPRI(10);
```
![[Pasted image 20250313115415.png]]
### <mark style="background: #FFB86CA6;">Máscaras del Cortex M3/M4</mark>
- PRIMASK: registro de 1 bit que permite excepciones NMI y hard fault y ninguna más.
- FAULTMASK: registro de 1 bit que permite excepciones NMI y ninguna más.
- BASEPRI: registro de 9 bits que define el nivel de prioridad de las máscaras. Cuando se activa, deshabilita las interrupciones >= que su valor (cuanto mas grande el numero, menor prioridad). Por defecto es 0.
### <mark style="background: #FFB86CA6;">Estado de las interrupciones dentro del controlador</mark>
1. Inactive: no se ha producido y no está pendiente
2. Pending: a la espera de ser atendida
3. Active: se está atendiendo por el CPU y no ha sido completada todavía
4. Active and pending: está siendo atendida por el procesador y se ha vuelto a pedir, por lo que se tiene que ejecutar de nuevo cuando se complete.

<hr>

### <mark style="background: #FFB86CA6;">Registros del NVIC</mark>
<h3 style="color:red;">MASCARAS</h3>
- NVIC_ISER (Interrupt Set Enable Register): si se pone a uno, habilita interrupciones.
- NVIC_ICER (Interrupt Clear Enable Register): si se pone a uno, deshabilita interrupciones.
<h3 style="color:red;">FLAGS</h3>
- NVIC_ISPR: marca interrupcion como pendiente
- NVIC_ICPR: resetea la flag de pendiente
- NVIC_IPRx: prioridad de la interrupcion (8 bits para cada interrupcion, 4 interrupciones en un registro de 32 bits)
- NVIC_IABR: activo si una interrupcion está siendo atendida
![[Pasted image 20250313122633.png]]
<div class="nota">
<h3>Si el BASEPRI del Core está a 0, se usa el nivel de prioridad del NVIC. Sin embargo si está > 0 el del Core, las interrupciones que lleguen al Core desde el NVIC con menos prioridad no se atenderán</h3>
<h3>La prioridad principal sirve para ver que interrupción puede o no interrumpir a otra. Sin embargo, la subprioridad sirve para que en caso de que lleguen "a la vez" se pueda decidir cual va primero. </h3>
</div>
### <mark style="background: #FFB86CA6;">Anidamiento y cambio en caliente</mark>
#### <mark style="background: #D2B3FFA6;">Anidamiento</mark>
  ![[Pasted image 20250320110820.png]]
#### <mark style="background: #D2B3FFA6;">Cambio en caliente (Tail Chaining)</mark>
  ![[Pasted image 20250320110905.png]]
  El **tail chaining** tarda 6 ciclos en producirse.
### <mark style="background: #FFB86CA6;">Late-Arrival Interrupt</mark>
Se produce al lanzarse una interrupción de alta prioridad mientras se está cambiando de contexto debido a una interrupción de baja prioridad. El contexto se guarda con normalidad, y se ejecuta la de alta prioridad, y cuando termine la de baja prioridad y se restaura el contexto.
![[Pasted image 20250320111330.png]]
### <mark style="background: #FFB86CA6;">Interrupciones Externas (mediante GPIO)</mark>
Mediante un controlador de interrupciones externas, pueden haber máximo 16 pines con interrupción externa, ya que multiplexa los pines a las líneas de interrupción.
![[Pasted image 20250320112207.png]]
La estructura del controlador es:
![[Pasted image 20250320112323.png]]
El generador de pulso, es parecido a las interrupciones, pero solamente manda un pulso a un periférico interno.
## <mark style="background: #ADCCFFA6;">6. Modos de arranque del STM32F407</mark>
![[Pasted image 20250320113146.png]]
# <mark style="background: #FFF3A3A6;">TEMA 4: Dispositivos integrados en los μC</mark>
## <mark style="background: #ADCCFFA6;">1. Puertos E/S</mark>
### <mark style="background: #FFB86CA6;">Fundamento de un puerto GPIO</mark>
![[Pasted image 20250320114112.png]]
Un **puerto de salida** es un registro en el que se almacena el valor de un pin externo y permanece hasta que se vuelva a cambiar.
Un **puerto de entrada** es un buffer conectado a un pin cuyo estado se consulta en un determinado momento, y puede cambiar entre consulta y consulta.
#### Push-Pull
Se en el registro de dirección de dato se pone un '1' es modo salida y en el pin hay un 1 o un 0 depende del biestable de datos. Sin embargo, si hay un '0', los transistores están en HI y se lee desde el pin (pin en modo entrada). **No se pueden conectar dos salidas push-pull**.
![[Pasted image 20250320120951.png]]

## <mark style="background: #ADCCFFA6;">2. Timers</mark>
## <mark style="background: #ADCCFFA6;">3. Conversores A/D y DAC</mark>
## <mark style="background: #ADCCFFA6;">4. CLK y alimentación</mark>
