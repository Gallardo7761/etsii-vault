# <mark style="background: #FFF3A3A6;">TEMA 1:  Introducción a ARM</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
ARM (Advanced RISC Machines) nació en 1990 en Inglaterra con una idea clara: procesadores **RISC**, simples y eficientes, frente a los **CISC** de Intel y compañía.  
No fabrican chips, **venden licencias** de sus diseños, así que empresas como ST, NXP, Texas Instruments o Microchip integran sus núcleos en sus propios micros.

Las arquitecturas ARM se dividen en:
- **Cortex-A**: para sistemas potentes (Android, Linux, etc.).  
- **Cortex-R**: para sistemas de tiempo real críticos (automoción, industria).  
- **Cortex-M**: para microcontroladores de bajo consumo y control embebido (como los STM32).  

El **Cortex-M4** es un modelo intermedio: más potente que el M0/M3, pero más eficiente que el M7. Tiene soporte para **operaciones en coma flotante** y un rendimiento excelente en control digital y procesamiento de señales (DSP).
## <mark style="background: #ADCCFFA6;">2. Estructura general del Cortex-M4 y sus registros</mark>
El núcleo **Cortex-M4** es de **32 bits**, arquitectura **RISC**, y usa una versión reducida del conjunto de instrucciones ARM llamada **Thumb-2**, que combina instrucciones de 16 y 32 bits para optimizar memoria sin perder rendimiento.
### <mark style="background: #FFB86CA6;">Registros generales</mark>
- **R0–R12** → registros de propósito general (datos, direcciones, etc.).
- **R13 (SP)** → *Stack Pointer*, apunta a la cima de la pila.
- **R14 (LR)** → *Link Register*, guarda la dirección de retorno tras una llamada a subrutina.
- **R15 (PC)** → *Program Counter*, indica la instrucción actual.
### <mark style="background: #FFB86CA6;">Registros de estado</mark>
- **PSR (Program Status Register)** → conjunto de tres registros (APSR, IPSR, EPSR) que indican el estado del procesador, la instrucción en ejecución y las interrupciones activas.
- **CONTROL** → define el nivel de privilegio, la pila activa y si la **FPU** está disponible.
### <mark style="background: #FFB86CA6;">Registros de coma flotante</mark>
- 32 registros de 32 bits (**S0–S31**) → precisión simple.
- 16 registros de 64 bits (**D0–D15**) → precisión doble (empaquetados en los S).
- **FPSCR** → contiene flags como negativo, cero, overflow, NaN o modo de redondeo.
Este diseño permite al Cortex-M4 realizar cálculos complejos en tiempo real sin depender de software lento, ideal para audio, sensores o controladores PID.
## <mark style="background: #ADCCFFA6;">3. Ejecución de instrucciones: Pipeline, modos Thumb y eficiencia</mark>
El **pipeline** (tubería de ejecución) del Cortex-M4 tiene 3 etapas:
1. **IF (Instruction Fetch)** – Se lee la instrucción de memoria.
2. **ID (Instruction Decode)** – Se decodifica.
3. **EX (Execute)** – Se ejecuta.
Esto permite solapar instrucciones, mejorando el rendimiento (una instrucción por ciclo en el mejor caso).
### <mark style="background: #FFB86CA6;">Instrucciones Thumb y Thumb-2</mark>
ARM clásico usa instrucciones de 32 bits (rápidas, pero grandes).  
**Thumb (16 bits)** mejora la densidad de código (programas más pequeños), aunque algo más lentos (~20% más).  
**Thumb-2** mezcla ambas: usa 16 bits para instrucciones simples y 32 bits para las complejas. Así:
- Se ahorra **≈30% de memoria**.
- Se mantiene **rendimiento casi igual** al modo ARM.
En sistemas embebidos donde la memoria Flash es limitada, esta diferencia es clave.
## <mark style="background: #ADCCFFA6;">4. Memoria y organización interna</mark>
Los **ARM Cortex-M** trabajan con una organización de memoria **alineada a 32 bits**, aunque el M4 admite accesos desalineados, lo que mejora el aprovechamiento de la RAM.  
### <mark style="background: #FFB86CA6;">Bit-band Mapping</mark>
Permite acceder a **bits individuales** de una palabra mediante direcciones alias.  
Por ejemplo: escribir directamente el bit 0 de un registro sin usar máscaras.  
Esto facilita muchísimo el control de GPIO o flags sin operaciones lógicas extra.
### <mark style="background: #FFB86CA6;">Jerarquía de buses en STM32F4</mark>
Dentro del micro, los distintos módulos (CPU, DMA, periféricos, memorias) se conectan por una red de buses:
- **AHB1/AHB2 (168 MHz):** periféricos rápidos (GPIO, USB FS, DMA).  
- **APB2 (84 MHz):** periféricos intermedios (ADC, timers).  
- **APB1 (42 MHz):** periféricos lentos (USART, I2C).  
Esto evita cuellos de botella y permite que la CPU y el DMA trabajen en paralelo.
## <mark style="background: #ADCCFFA6;">5. Interrupciones, excepciones y control del sistema</mark>
El Cortex-M4 incluye varios módulos clave para gestionar eventos externos o internos:
### <mark style="background: #FFB86CA6;">NVIC (Nested Vector Interrupt Controller)</mark>
- Permite **interrupciones anidadas** con diferentes niveles de prioridad (hasta 150 líneas).
- Las más prioritarias pueden **interrumpir** a otras de menor prioridad.
- Soporta cambio rápido de contexto (“tail-chaining”) y evita restauraciones innecesarias.
- También gestiona **Late-Arrival**, cuando llega una interrupción urgente en medio del cambio de contexto.
### <mark style="background: #FFB86CA6;">SCB (System Control Block)</mark>
Contiene info del sistema, versión del core, vector de interrupciones, etc.
### <mark style="background: #FFB86CA6;">MPU (Memory Protection Unit)</mark>
Define zonas de memoria con permisos específicos, previniendo accesos indebidos (muy útil con RTOS).
### <mark style="background: #FFB86CA6;">SysTick</mark>
Temporizador de 24 bits que genera interrupciones periódicas.  
En RTOS se usa como *tick del sistema* para planificar tareas.
### <mark style="background: #FFB86CA6;">Tipos de excepciones más comunes</mark>
- **Reset:** reinicia el procesador.
- **HardFault:** error grave (por ejemplo, ISR no definida).
- **MemManage / BusFault / UsageFault:** fallos de acceso o ejecución.
También hay **interrupciones software**, disparadas desde código:
- **SVCall:** llamada a supervisor (modo privilegiado).
- **PendSV:** cambio de contexto entre tareas (RTOS).
- **SysTick:** temporizador base del sistema operativo.
## <mark style="background: #ADCCFFA6;">6. STM32F407: el ejemplo clásico del Cortex-M4</mark>
El **STM32F407** de STMicroelectronics es el ejemplo típico para este núcleo:
- Frecuencia máxima: **168 MHz** (≈210 DMIPS).  
- **1 MB Flash / 192 KB SRAM**.  
- **3 ADC de 12 bits**, **2 DAC**, **17 timers**, **2 DMA**.  
- **Interfaces**: USB OTG, CAN, SPI, I²C, USART, SDIO, Ethernet, cámara digital.  
- **FSMC:** permite usar memorias externas (Flash, SRAM).  
La memoria Flash es relativamente lenta (4 ciclos de lectura por palabra), pero ST lo soluciona con el **ART Accelerator**:
### <mark style="background: #FFB86CA6;"> ART Accelerator</mark>
- Usa una **prefetch queue** para precargar bloques de instrucciones.  
- Tiene **predictor de saltos** y **branch cache**, reduciendo esperas.  
- Si la predicción acierta, el procesador no pierde ciclos y mantiene **CPI ≈ 1**.
## <mark style="background: #ADCCFFA6;">7. Arranque, reloj y desarrollo</mark>
### <mark style="background: #FFB86CA6;">Modos de arranque (BOOT0 y BOOT1)</mark>
- **Modo 0:** arranca desde la Flash (modo normal).  
- **Modo 1:** arranca desde el Bootloader interno (permite reprogramar por USB, UART, etc.).  
- **Modo 3:** arranque desde memoria externa.  
El Bootloader es esencial para recuperar un micro "brickeado" o actualizar firmware sin programador.
### <mark style="background: #FFB86CA6;">Sistema de relojes</mark>
- **HSI (16 MHz interno)** y **HSE (4–26 MHz externo)**.  
- **PLL** multiplica frecuencia hasta 168 MHz (para core y buses).  
- Algunos periféricos (USB, SDIO, I2S) necesitan la precisión del HSE.  
### <mark style="background: #FFB86CA6;">Entorno de desarrollo</mark>
- **Keil MDK-ARM:** compilador comercial, muy optimizado (pago).  
- **STM32CubeIDE:** gratuito, basado en Eclipse + GCC, integra configuración, generación de código y depuración.  
- **MEMS Studio / X-CUBE-AI / NanoEdge AI:** herramientas de ST para sensores, IA y análisis de datos.
## <mark style="background: #ADCCFFA6;">8. Ecosistema STM32 y placas de desarrollo</mark>
ST ofrece un entorno completo para prototipar y desarrollar:
- **Placas Discovery** (como STM32F4-Discovery): traen periféricos, LEDs y sensores.  
- **Nucleo Boards**: diferentes tamaños (32, 64, 144 pines), con compatibilidad Arduino.  
- **DISCO-L475VG-IOT01A**: ideal para proyectos IoT, con sensores, WiFi y BLE.  
- Nuevos modelos como **STM32MP157** (dual-core con Linux) y **STM32N6** para IA y edge computing.
# <mark style="background: #FFF3A3A6;">Tema 2 – Sistemas en Tiempo Real</mark>

## <mark style="background: #ADCCFFA6;">1. Qué es un Sistema en Tiempo Real (STR)</mark>
Un sistema en tiempo real (**RTS**, Real-Time System) es aquel en el que **la validez del resultado depende no solo de que sea correcto, sino de que se obtenga dentro de un plazo concreto (deadline)**. No basta con que funcione, debe hacerlo *a tiempo*.

En sistemas de propósito general lo importante es la **exactitud del resultado**, mientras que en tiempo real lo crucial es **cumplir los plazos de respuesta**, buscando comportamiento determinista más que potencia bruta.  
(Ejemplo: un STM32 a 168 MHz puede ser más fiable que un i9 a 4.5 GHz si debe garantizar tiempos exactos).
### <mark style="background: #FFB86CA6;">Tipos de sistemas en tiempo real</mark>
- **Tiempo real duro:** el incumplimiento del plazo provoca fallo catastrófico (ej: marcapasos).  
- **Tiempo real firme:** el retraso degrada el sistema, pero no lo destruye (ej: router).  
- **Tiempo real blando:** el retraso resta utilidad, pero el resultado sigue siendo válido (ej: termómetro).
## <mark style="background: #ADCCFFA6;">2. Modelos de programación</mark>

### <mark style="background: #FFB86CA6;">Procesamiento secuencial (bucle de scan)</mark>
Estructura básica:
```C
while(1)
{
  Task1();
  Task2();
  ...
  TaskN();
}
```
- Todas las tareas se ejecutan una tras otra.  
- No se permiten esperas bloqueantes ni interrupciones largas.  
- El periodo total del sistema equivale a la duración del bucle.  
- Comunicación entre tareas mediante **variables globales**.  
- Tiene **variabilidad temporal**, ya que no todas las tareas tardan lo mismo.
### <mark style="background: #FFB86CA6;">Primer plano / segundo plano (Foreground / Background)</mark>
- Es un superloop con **interrupciones**.  
- Problemas comunes:
  - Accesos simultáneos a variables globales (condiciones de carrera).  
  - Evitar esperas bloqueantes dentro del bucle.  
- Aparece el fenómeno de **Loop Jitter**: el tiempo del ciclo varía según interrupciones, caché o condiciones de código.
### <mark style="background: #FFB86CA6;">Multitarea</mark>
Uso de **múltiples tareas concurrentes** coordinadas por un **RTOS (Real-Time Operating System)**.  
Cada tarea puede bloquearse esperando eventos, y el planificador decide cuál se ejecuta en cada instante.
## <mark style="background: #ADCCFFA6;">3. Cuándo usar un RTOS</mark>
Un RTOS se emplea cuando la aplicación requiere:
- **Determinismo:** misma respuesta ante el mismo estímulo.  
- **Fiabilidad:** detección y recuperación de errores.  
- **Complejidad:** demasiadas funciones para un simple superloop.  
- **Operación fail-soft:** capacidad de degradarse sin colapsar.  
- **Múltiples tareas concurrentes o temporizadas.**
### <mark style="background: #FFB86CA6;">Ventajas del RTOS</mark>
- **Modularidad:** código dividido en tareas independientes.  
- **Mantenibilidad y extensibilidad:** fácil de modificar o ampliar.  
- **Reusabilidad y portabilidad:** las tareas pueden migrar entre plataformas.  
- **Trabajo en equipo:** distintos devs trabajan en tareas separadas.  
- **Facilidad de testeo:** permite probar tareas por separado.  
- **Eficiencia energética:** evita esperas activas y permite modos de bajo consumo.
## <mark style="background: #ADCCFFA6;">4. Núcleo y planificación de tareas</mark>

### <mark style="background: #FFB86CA6;">El planificador</mark>
El **planificador** es el núcleo del RTOS y decide qué tarea se ejecuta en cada momento.  
Se ejecuta:
- Periódicamente (por interrupción SysTick).  
- Cuando una tarea se bloquea o libera un recurso.  
Debe ser **rápido, determinista y eficiente** (normalmente implementado en ensamblador).
### <mark style="background: #FFB86CA6;">Políticas de planificación</mark>
- **Dinámicas:** cambian prioridades en tiempo de ejecución (ej: EDF – *Earliest Deadline First*).  
  - Alta precisión pero poca estabilidad en sobrecarga.  
- **Estáticas:** prioridades fijas asignadas al compilar.  
  - **Colaborativas:** la tarea decide cuándo ceder la CPU (yield).  
    - Simples pero poco fiables; una tarea puede bloquear todo.  
  - **Apropiativas:** la CPU pasa automáticamente a la tarea de mayor prioridad lista.  
    - Determinismo alto, ideal para sistemas embebidos.
## <mark style="background: #ADCCFFA6;">5. Tareas en un RTOS</mark>

### <mark style="background: #FFB86CA6;">Concepto general</mark>
Una **tarea** es un bucle infinito que ejecuta su código y luego se bloquea esperando un evento.  
El planificador ejecuta siempre la tarea lista con mayor prioridad.  
Si varias tienen la misma prioridad → **round-robin** (rotación en cada tick).
**Bloqueos posibles:**
- Semáforos, colas, notificaciones.  
- Esperas temporales (por ticks o tiempo real).
### <mark style="background: #FFB86CA6;">Tipos de tareas</mark>
- **Cíclicas:** se repiten indefinidamente.  
- **Periódicas:** ejecutadas con una frecuencia fija.  
- **Eventuales:** activadas por eventos externos.  
- **One-Shot:** se crean, ejecutan una vez y se destruyen.
## <mark style="background: #ADCCFFA6;">6. Control de tareas y memoria</mark>
### <mark style="background: #FFB86CA6;">Estados de una tarea</mark>
Estados principales:
- **Ready:** lista para ejecutarse.  
- **Running:** ejecutándose.  
- **Blocked:** esperando evento o tiempo.  
- **Suspended/Deleted:** detenida o eliminada.  
El planificador mueve las tareas entre estos estados según su situación.
### <mark style="background: #FFB86CA6;">Bloques de Control de Tareas (TCB)</mark>
Cada tarea tiene un **TCB (Task Control Block)** con:
- Puntero a pila y código.  
- Prioridad y estado.  
- Datos de uso de CPU/pila.  
- Enlaces a otras tareas (listas enlazadas).
### <mark style="background: #FFB86CA6;">Memoria y pila</mark>
- Cada tarea tiene su **propia pila**, donde se guarda su **contexto** (variables locales, direcciones de retorno, registros).  
- Las variables globales se almacenan en una memoria común.
**Detección de desbordes:**
1. **Software:** el kernel revisa el uso de pila y lanza excepción si se supera.  
2. **Hardware:** MPU/MMU detecta accesos fuera de rango y genera una excepción.
## <mark style="background: #ADCCFFA6;">7. Hooks del RTOS y gestión de errores</mark>

### <mark style="background: #FFB86CA6;">Hooks</mark>
Tareas internas del RTOS que reaccionan a eventos especiales:
- **Idle Task:** se ejecuta cuando no hay ninguna tarea lista; no puede bloquearse.  
- **Stack Overflow Hook:** se lanza al detectar desbordamiento de pila.  
  Suspende la tarea afectada y permite ejecutar acciones como reset, aviso o modo seguro.
## <mark style="background: #ADCCFFA6;">8. Timers Software</mark>

### <mark style="background: #FFB86CA6;">Qué son</mark>
Un **timer software** es una función que se ejecuta con una **periodicidad fija**.  
No pueden usar llamadas bloqueantes del RTOS.
**Tipos:**
- **One-Shot:** se ejecuta una sola vez tras un tiempo (ej: WatchDog).  
- **Periódico:** se repite cada cierto periodo.
### <mark style="background: #FFB86CA6;">Gestión interna</mark>
- Controlados por la tarea interna **TimerServiceTask**.  
- No usan timers hardware, sino el **tick del sistema**.  
- Cada timer se gestiona con una **cola de mensajes interna**.  
- Los callbacks usan la pila de TimerServiceTask (configurable).