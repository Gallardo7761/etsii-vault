# <mark style="background: #FFF3A3A6;">TEMA 1: Introducción</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
![[Pasted image 20260127125053.png]]
### <mark style="background: #FFB86CA6;">Características de los SSEE</mark>
Depende de su funcionalidad compleja (algoritmos, interfaz de usuario), la operación en tiempo real (tiempo real duro, tiempo real blando) y de los requerimientos (costes, consumos, etc).
### <mark style="background: #FFB86CA6;">Conclusiones</mark>
Tienen múltiples aplicaciones y se adaptan a varios requerimientos de funcionalidad, tamaño, coste, prestaciones, etc. Existen herramientas que automatizan el diseño de estos.
## <mark style="background: #ADCCFFA6;">2. Evolución de la tecnología de microelectrónica</mark>
#### <mark style="background: #D2B3FFA6;">Ley de Moore</mark>
El número de transistores por mm² se duplica cada dos años. 
![[Pasted image 20260127130918.png]]
Las compañías que fabrican los circuitos integrados, se llaman _foundries_. El ranking lo lidera TSMC.
![[Pasted image 20260127131523.png]]
Samsung a diferencia de las demás compañías _fabless_ que dependen de _foundries_, es _foundry_ y diseñador de ICs. 
### <mark style="background: #FFB86CA6;">Transistores</mark>
![[Pasted image 20260127132218.png]]
Los transistores tradicionales se quedaron obsoletos en 2017 debido a la aparición de efectos cuánticos por la reducción de la anchura del canal entre la zona $n+$ y $n-$ del transistor. Por tanto se cambió a la estructura **FinFET**.

De nuevo, volvieron a aparecer efectos cuánticos en FinFET al bajar a $3nm$, por tanto se cambió a la estructura **Gate-All-Around**.
![[Pasted image 20260127133026.png]]
El proceso de **litografía** se basa en superponer máscaras para dejar pasar la luz y grabar en el silicio las diferentes capas. Al bajar el tamaño del canal, la litografía a evolucionado desde la luz visible, pasando por la luz ultravioleta hasta la actual **UVE (UltraVioleta Extrema)** con una $\lambda=13,5nm$, rozando casi la frontera con los rayos X.

Hoy en día se intentan fabricar ICs en 3D. Pero no se consiguen todavía porque el calor generado fundiría el circuito. Lo que si existe son **Memorias en 3D**.
![[Pasted image 20260127133921.png]]
## <mark style="background: #ADCCFFA6;">3. Conceptos sobre FPGA</mark>
Una FPGA (Field Programmable Gate Array) es una estructura formada por matrices de puertas lógicas interconectadas. 
![[Pasted image 20260127134152.png]]
Las FPGA pueden implementar CLBs como:
- Puertas lógicas
- MUX
- LUTs
### <mark style="background: #FFB86CA6;">Posibles arquitecturas</mark>
![[Pasted image 20260127134655.png]]
### <mark style="background: #FFB86CA6;">Técnicas de programación</mark>
![[Pasted image 20260127134809.png]]
La RAM estática se implementa con biestables y registros pero lo malo es que es volátil.
![[Pasted image 20260127135016.png]]
Los antifusibles lo bueno que tienen que no son volátiles pero no es **reprogramable**.
![[Pasted image 20260127135050.png]]
Lo bueno de las EPROM/EEPROM es que no son volátiles y **SI** son reprogramables.
![[Pasted image 20260127135127.png]]
En definitiva, las FPGA básicamente se constituyen de: bloques lógicos interconectados y accesibles mediante puertos I/O.
## <mark style="background: #ADCCFFA6;">4. FPGA de AMD/Xilinx</mark>
Los CLB de Xilinx se organizan en **slices**. Cada slice tiene 4 LUT de 6 entradas, 8 flip-flops y lógica de carry. Los bloques que interconectan los cables entre CLB se llaman en Xilinx **SB (Switch Box)** que son, al fin y al cabo, MUX. 
### <mark style="background: #FFB86CA6;">Red de interconexión</mark>
![[Pasted image 20260127140620.png]]
Hay **Single Lines** y **Long Lines**. Las single lines van entre los CLB. Las long lines se saltan 1 de cada 4 SB para reducir latencia.

También existen **líneas dedicadas al CLK** para evitar el _clock skew_. Se suele hacer distribuyendo las líneas en H. De esta forma se consigue que el reloj dentro del circuito, aunque desfasado respecto al CLK fuente, sea el mismo en todas las partes del circuito.
### <mark style="background: #FFB86CA6;">Input/Output Blocks</mark>
![[Pasted image 20260127141254.png]]
### <mark style="background: #FFB86CA6;">Reloj</mark>
![[Pasted image 20260127141610.png]]
El reloj se puede programar mediante el **Digital Clock Manager (DCM)**. 
### <mark style="background: #FFB86CA6;">Encapsulado</mark>
Estos FPGA usan el encapsulado **BGA (Ball Grid Array)**. 
## <mark style="background: #ADCCFFA6;">5. Plataformas de desarrollo PSoC</mark>
### <mark style="background: #FFB86CA6;">Familia Zynq</mark>
Integran **Processing Systems (PS)** y **Programmable Logic (PL)**. El PS integra un APU (Application Processing Unit) ARM Cortex-A9, caches, memoria, interfaces, un driver DMA, periféricos y puertos I/O. El PL integra lo clásico de una FPGA (CLB, DSP, I/O). 
![[Pasted image 20260129124806.png]]
#### <mark style="background: #D2B3FFA6;">Processing System</mark>
El procesador de la APU es arquitectura Harvard. También dentro de la APU hay una MMU. El **Snoop Control Unit (SCU)** permite a los núcleos del Cortex comunicarse con la caché L2 y la RAM compartidas y con la PL.

Esta familia también dispone de una unidad **NEON MPE (Media Processing Engine)**. Proporciona operaciones SIMD de alto rendimiento (suma, resta, multiplicación, máx, mín, aproximación de la raíz inversa, etc).

Por último incluye una **MIO (Multiplexed I/O)**.
#### <mark style="background: #D2B3FFA6;">Programmable Logic</mark>
Contiene CLB, matrices de conmutación, IOBs, DSP y RAM de bloques. Cada CLB contiene dos _slices_ (según Xilinx/AMD).
![[Pasted image 20260129125503.png]]
#### <mark style="background: #D2B3FFA6;">Comunicación entre PL y PS</mark>
Se pueden comunicar mediante **buses AXI**, **interfaces MIO** u otras señales como **reset, IRQ, etc**.
![[Pasted image 20260129125719.png]]
### <mark style="background: #FFB86CA6;">Familia Versal</mark>
Es una evolución de la familia Zynq. La PL no es ya derivada de la familia Artix. En el PS hay dos procesadores ARM: APU para propósito general y RPU para tiempo real (cada uno dual core). Esta familia incluye también un motor de IA.
#### <mark style="background: #D2B3FFA6;">AI Engine</mark>
Cada AI Engine contiene:
- **Unidad escalar:** procesador RISC 32b con RF, puntero y multiplicador 32x32, funciones no lineales (sin, cos, etc).
- **Unidad vectorial:** unidad de vector de 512b punto fijo. Punto flotante _single precision_.
- **Unidad de carga** de datos
- **Interfaz con memoria**
Esta familia también dispone de GPU, ISP (Image Signal Processing), VCU (Video Coder Unit), VDU (Video Decoding Unit).
### <mark style="background: #FFB86CA6;">Placa de desarrollo ZedBoard</mark>
![[Pasted image 20260129130447.png]]
# <mark style="background: #FFF3A3A6;">TEMA 2: Vivado</mark>
## <mark style="background: #ADCCFFA6;">1. Flujo de diseño</mark>
Primero se describe el sistema en un HDL, luego se hace una _behavioural simulation_, luego la síntesis (puertas lógicas y biestables), la implementación (LUTs y biestables) y por último la programación. Diseñaremos sistemas con un **lenguaje de alto nivel (C/C++)** y con el **IP Integrator**. El IP Integrator usa una librería de componentes para generar una descripción RTL del sistema.
![[Pasted image 20260129132034.png|350]]
### <mark style="background: #FFB86CA6;">Módulos IP</mark>
Los módulos IP tienen varios formatos estándares en IEEE. Uno de los formatos es el **IP-XACT**. Al fin y al cabo es un archivo XML para documentar módulos IP usando metadatos. Las herramientas compatibles con este estándar pueden interpretar, configurar, implementar y cambiar módulos IP. Vivado soporta varios estándares como TCL, AXI, IP-XACT, Verilog, C, etc.
# <mark style="background: #FFF3A3A6;">TEMA 3: Desarrollo de SSEE</mark>
## <mark style="background: #ADCCFFA6;">1. Procesadores empotrados</mark>
#### <mark style="background: #D2B3FFA6;">SoC</mark>
Se conoce como SoC (System on Chip) un sistema completo integrado en un sólo chip. Si además el sistema es una FPGA, tenemos un PSoC (Programmable SoC). 
#### <mark style="background: #D2B3FFA6;">Flujo de diseño</mark>
Requieren un flujo de diseño muy ligado entre el HW y SW. 
#### <mark style="background: #D2B3FFA6;">Soft y Hard Cores</mark>
Distinguimos entre **Soft Cores** y **Hard Cores**. Los soft cores son componentes sintetizados y configurados en la FPGA (Ej: MicroBlaze) mientras que los hard cores ya están fabricados en silicio en la placa (Ej: ARM Cortex).
### <mark style="background: #FFB86CA6;">Procesadores empotrados para ASICs y FPGA</mark>
![[Pasted image 20260205135914.png]]
![[Pasted image 20260205135941.png]]
## <mark style="background: #ADCCFFA6;">2. Arquitectura de MicroBlaze</mark>
- RISC
- 32b
- Harvard
- Instrucciones de 32b
- 32 regs de 32b
- Multiplicador HW
- MMU
### <mark style="background: #FFB86CA6;">Mecanismos de comunicación</mark>
#### <mark style="background: #D2B3FFA6;">Bus AMBA</mark>
Es un bus de ARM que se ha convertido en un estándar _de facto_ en los SSEE. En realidad es una familia de buses que contiene a:
- AHB: es un bus de alta velocidad y rendimiento.
- APB: AHB simplificado
- AXI4: contiene las características de AHB y APB. Conexión PP.
- AXI4-Lite: versión simplificada de AXI4
- AXI4-Stream: conexión PP (Ej: MicroBlaze-Periférico) usando una FIFO. Se usan para periféricos de alta velocidad, sobre todo para intercambiar flujos de datos.
- ACE:
- ACE-Lite:
- CHI:
- ATB:
Los buses AXI se usan tanto para instrucciones como para datos. 
#### <mark style="background: #D2B3FFA6;">Bus LMB</mark>
Es un bus propio originalmente de IBM que permite acceder a memoria en **un sólo ciclo de reloj**, a diferencia del AXI que necesita varios. Sirve para acceder a los bloques de memoria en un ciclo y aprovechar así la arquitectura _pipeline_.
### <mark style="background: #FFB86CA6;">Arquitectura de memoria</mark>
Es arquitectura Harvard. Cada espacio se direcciona con 32b (4GB). Se pueden direccionar palabras enteras, medias palabras y bytes. Los datos están alineados. La E/S es mapeada.
![[Pasted image 20260210125017.png|300]]
#### <mark style="background: #D2B3FFA6;">Tipos de datos en memoria</mark>
Antes de la V8.00, se guardaban los datos en formato **Big-Endian** y **Bit-Reversed** (el MSB es el 0 y el LSB el 31). Hoy puede usar tanto **Big-Endian** como **Little-Endian** con el parámetro `C_ENDIANNESS`. 
### <mark style="background: #FFB86CA6;">Registros de propósito general</mark>
Se pueden usar como uno quiera, sin embargo, algunos tienen funciones predeterminadas:
**R0:** siempre almacena el valor cero
**R1-R13:** registros de propósito general
**R14:** direcciones de retorno de “interrupciones”
**R15:** registro de propósito general
**R16:** direcciones de retorno de “breaks”
**R17:** direcciones de retorno de “excepciones”
**R18-R31:** registros de propósito general
### <mark style="background: #FFB86CA6;">Registros de propósito especial</mark>
**PC:** contador de programa
**MSR:** registro de estado
**EAR,ESR:** debug HW
### <mark style="background: #FFB86CA6;">Interrupciones</mark>
1. Reset
2. Hardware Exception
3. NMI
4. Break
5. Interrupt
6. User Vector (exception)
Con un controlador de interrupciones se pueden tener 32+ interrupciones enmascarables. IE en MSR habilita/deshabilita las interrupciones.
### <mark style="background: #FFB86CA6;">FPU</mark>
ADD, SUB, MUL, DIV, Comp, Conv, SQRT, soporte NaN, etc.
### <mark style="background: #FFB86CA6;">Pipeline</mark>
Tiene tres fases: _fetch_, _decode_, _execute_.
#### <mark style="background: #D2B3FFA6;">ISA</mark>
87 instrucciones RISC en dos formatos:
- **Tipo A:**
  ![[Pasted image 20260210130956.png]]
- **Tipo B:**
  ![[Pasted image 20260210131013.png]]
## <mark style="background: #ADCCFFA6;">3. Librería de módulos IP</mark>
### <mark style="background: #FFB86CA6;">GPIO</mark>
Este periférico tiene dos canales (buses) [31:0] y una entrada al bus **dAXI** [31:0]. Con esto se puede configurar cada canal como I o como O y cada canal tiene dos registros: **DATA** y **TRI**. Al estar mapeados en memoria (0x0, 0x4, 0x8, 0xC) se puede leer y escribir de ellos como si se leyera o escribiera en memoria.
### <mark style="background: #FFB86CA6;">UART</mark>
Es un periférico conectado al dAXI con dos entradas Rx y Tx. Es _full-duplex_. Usa FIFOs Rx Tx de 16 caracteres.
### <mark style="background: #FFB86CA6;">Timer</mark>
Tiene dos módulos timer idénticos. Cada timer tiene varios modos de operación: generación, captura, PWM y cascada. Tienen 3 registros: 
- contador 32b asc/desc
- registro de carga
- registro de control/estado
### <mark style="background: #FFB86CA6;">Controlador de interrupciones</mark>
Hasta 32 señales de interrupción. Prioridad por el LSB (0 prioridad más alta). Tiene registros de control (máscara, habilitación, control de nivel/flanco, etc).
### <mark style="background: #FFB86CA6;">MDM</mark>
Soporte para JTAG. Hasta 8 procesadores MicroBlaze. Interfaces AXI4-Lite o PLBv46. Breakpoints, watchpoints. Run/Stop/Step.
## <mark style="background: #ADCCFFA6;">4. Flujo de diseño</mark>
![[Pasted image 20260210134109.png|500]]
Al desaarrollar software en un computador, el IDE y la aplicación se ejecutan en la misma máquina (código nativo). Sin embargo en un SE, se ejecutan cada uno en un sitio (compilación cruzada). 