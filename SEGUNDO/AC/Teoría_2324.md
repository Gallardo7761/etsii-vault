# <mark style="background: #FFF3A3A6;">TEMA 1: El Procesador</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
### <mark style="background: #D2B3FFA6;">- Tiempo de CPU</mark>
$$
\begin{equation}
T_{CPU}=\frac{NºInstr\times CPI}{F_{CLK}}=NºInstr\times CPI\times T_{CLK}
\end{equation}
$$
- **Nº Instrucciones:** número de instrucciones ejecutadas. Está condicionado por el programador/compilador y por el ISA.
- **CPI:** ciclos (de reloj) por instrucción. Está condicionado por la propia arquitectura.
- **Periodo:** duración de un ciclo. Está condicionado por la arquitectura/tecnología.
### <mark style="background: #D2B3FFA6;">- Aceleración</mark>
$$
\begin{equation}
Ac_{A-B}=\frac{T_{CPU,~B}}{T_{CPU,~A}}
\end{equation}
$$
Si el resultado es menor que 1, el procesador A no es el más rápido, se intercambian de sitio A y B y a repetir. Si $Ac_{A-B}=1,3$, el procesador A es un 30% más rápido que el B.
### <mark style="background: #D2B3FFA6;">- Procesador monociclo</mark>
Cada instrucción se ejecuta en un ciclo ($CPI=1$). El periodo de reloj lo determina la instrucción de más duración. Puede resultar ineficiente ya que las instrucciones de menor duración desperdician tiempo de procesador.
### <mark style="background: #D2B3FFA6;">- Procesador secuencial multiciclo</mark>
La ejecución de una instrucción se divide en fases. Cada fase se ejecuta en un ciclo. La duración del periodo está determinada por la fase más larga. El tiempo de ejecución de cada instrucción varía ya que dependerá de su número de fases.
### <mark style="background: #D2B3FFA6;">- Procesador segmentado</mark>
Es un símil con una cadena de montaje, es decir, cada instrucción comienza en cada ciclo de reloj antes de terminar la anterior. Se solapan varias instrucciones pero en fases distintas. Las fases no pueden compartir recursos o dará lugar a dependencias.
## <mark style="background: #ADCCFFA6;">2. RISC-V</mark>
### <mark style="background: #D2B3FFA6;">- Características</mark>
Versión RISC-V de 32 bits: 
- Tamaño de palabra de 32 bits: registros, memoria, operaciones.
- Direcciones de 32 bits $\rightarrow$ direccionamiento 4GB
	- Accesos a memoria a nivel de byte, half-word y word.
	- Formato Little Endian.
	- Direccionamiento simple (acceso a datos y a código).
- 32 registros de enteros de 32 bits (también de floating point).
### <mark style="background: #D2B3FFA6;">- Formato de las instrucciones</mark>
- **Formato R**
	![[Pasted image 20240306123152.png]]
	Todas las instrucciones con tres registros. **Function** extiende el campo **opcode**. **Aux** se utiliza en algunas instrucciones. Ejemplos de instrucciones tipo R son: `add, sub, and, or, xor, sll, srl, slt...`
- **Formato I**
	![[Pasted image 20240306123319.png]]
	Todas las instrucciones con inmediatos. Incluye saltos condicionales (`beq, bne, blt, bge...`) y accesos a memoria (loads y stores). Ejemplos de instrucciones tipo I son: `addi, andi, slli, ori, xori...`
- **Formato J**
	![[Pasted image 20240306123449.png]]
	Instrucción de salto incondicional `jal`. Requiere un formato específico pues utiliza el modo de direccionamiento pseudo-absoluto para indicar la dirección de destino del salto.
## <mark style="background: #ADCCFFA6;">3. Camino de datos</mark> 
### <mark style="background: #D2B3FFA6;">- RISC-V monociclo</mark>
![[Pasted image 20240306124213.png]]
### <mark style="background: #D2B3FFA6;">- RISC-V segmentado</mark>
![[Pasted image 20240306124829.png]]

| Etapa | Descripción                                                                                   |
| ----- | --------------------------------------------------------------------------------------------- |
| IF    | Busca la instrucción y actualiza el PC                                                        |
| ID    | Decodifica la instrucción, lee los registros y resuelve saltos si hubiera                     |
| EX    | ALU: realiza la operación correspondiente<br>Carga/Almacenamiento: calcula la dir. de memoria |
| ME    | Carga: lee el dato de memoria<br>Almacenamiento: escribe el dato en memoria                   |
| WB    | Escribe el resultado en el fichero de registros                                               |
## <mark style="background: #ADCCFFA6;">4. Riesgos y dependencias</mark>
- **Dependencia:** si la ejecución de una instrucción está condicionada por otra anterior de alguna forma.
- **Riesgo:** situación que impide iniciar la siguiente instrucción en el próximo ciclo debido a su dependencia. Hay varios tipos de riesgos:
	- **Estructurales:** si se intenta usar un recurso al mismo tiempo.
	- **De datos:** si se altera el orden natural en el que deben producirse las R/W.
	- **De control:** si se ejecutan instrucciones posteriores a un salto no resuelto.
### <mark style="background: #D2B3FFA6;">- Riesgos estructurales</mark>
Cuando varias instrucciones requieren utilizar un mismo recurso
simultáneamente (recurso compartido por varias etapas) sin estar
diseñado para ello. Se pueden solucionar de varias formas:
- Añadir un recurso con capacidad para acceso simultáneo.
- Dotar al recurso con capacidad para acceso simultáneo.
- Bloquear la instrucción (afecta al rendimiento $\rightarrow CPI>1$)
#### 1. Añadir un recurso propio a cada etapa que lo precise
**Ejemplo:** las etapas IF y ME acceden a memoria pero cada una tiene su propia memoria. IF tiene una memoria de instrucciones IM y ME tiene una memoria de datos DM.
![[Pasted image 20240306131213.png]]
#### 2. Recursos con capacidad para acceso simultáneo
**Ejemplo:** el fichero de registros requiere 3 accesos en un mismo ciclo. La etapa ID realiza hasta 2 lecturas y la etapa WB realiza 1 escritura. Se soluciona con un fichero de registros de 3 puertos (2 de R y 1 de W).
![[Pasted image 20240306131335.png]]
#### 3. Bloquear la instrucción
Si el recurso está ocupado la instrucción no podrá ejecutar la etapa por lo que tendrá que esperar al siguiente ciclo.
![[Pasted image 20240306131615.png]]
![[Pasted image 20240306131628.png]]
### <mark style="background: #D2B3FFA6;">- Riesgos de datos</mark>
Situación que impide ejecutar una instrucción por la relación que guardan sus operandos con otros de instrucciones anteriores. Hay de varios tipos:
<mark style="background: #BBFABBA6;">‎ ‎ ‎ ‎ ‎ ‎ </mark> $\equiv$ en RISC-V si
<mark style="background: #FF5582A6;">‎ ‎ ‎ ‎ ‎ ‎ </mark> $\equiv$ en RISC-V no
#### <mark style="background: #BBFABBA6;">1. RAW (Read After Write)</mark>
Intenta leer el operando antes de que una instrucción anterior lo haya escrito. R en fase anterior a la W.
![[Pasted image 20240306132700.png]]
Se debería leer el valor actualizado de R1. Hay varias soluciones para los riesgos tipo RAW:
- **Bloquear:** la instrucción dependiente debe esperar en la etapa ID hasta que la instrucción de la que depende actualice el resultado. Degrada el rendimiento al retrasar la cadena.
- **Adelantamiento:** añadir buses para hacer bypass entre etapas y poder usar resultados calculados entre etapas. Mejora el rendimiento. Requiere MUX y buses.
- **Reordenar:** consiste en separar las instrucciones que producen riesgos de datos entre ellas, reordenando el código pero sin cambiar el funcionamiento de este.
#### <mark style="background: #BBFABBA6;">2. WAW (Write After Write)</mark>
Intenta escribir antes de hacerlo una instrucción anterior. Puede ocurrir si la escritura se realiza en varias etapas o si hay instrucciones de larga duración (tiene fases que duran un ciclo cada una, como `mul` o `div`).
![[Pasted image 20240306132653.png]]
R1 de la primera instrucción se debería haber actualizado antes, por tanto queda desactualizado.
#### <mark style="background: #FF5582A6;">3. WAR (Write After Read)</mark>
Intenta escribir antes de que una instrucción anterior lo haya leído. W en fase anterior a la R.
![[Pasted image 20240306132551.png]]
**NO** se debería leer el valor actualizado de R1.
En RISC-V no se producen ya que se escribe en WB y se lee en ID.
#### <mark style="background: #FF5582A6;">4. RAR (Read After Read)</mark>
No existen pues alterar el orden de R no es un riesgo.
### <mark style="background: #D2B3FFA6;">- Riesgos de control</mark>
Condicionan el flujo de control, es decir, la siguiente instrucción a ejecutar. Dos tipos de instrucciones, saltos condicionales e incondicionales. Las posibles soluciones son:
- **Bloquear:** cancelar la ejecución de las instrucciones que comenzaron indebidamente. Cada instrucción cancelada, 1 ciclo de bloqueo, por lo que empeora el rendimiento.
- **Salto retardado:** no cancelar la ejecución de instrucciones posteriores al salto, pues fueron sustituidas por instrucciones válidas. Lo debe soportar el compilador.
- **Predecir el salto:** predicción estática (en compilación) o dinámica (en ejecución).
### Ejercicio 39.  Sea el procesador RISC-LLV inspirado en el RISC-V con desvíos, con las etapas:

<table border="1">
<tr>
<td>
IF
</td>
<td>
ID
</td>
<td>
LL
</td>
<td>
EX
</td>
<td>
ME
</td>
<td>
WB
</td>
</tr>
</table>
#### Esta arquitectura incluye la etapa LL que debe ser capaz de realizar 2 lecturas simultáneas en la memoria de datos (sólo realizará lecturas). Para ello, la memoria de datos incluye un segundo puerto, pero sólo habilitado para la lectura. Este procesador enriquece el repertorio de instrucciones del RISC-V con nuevas instrucciones de salto condicionales capaces de evaluar la condición del salto comparando valores en memoria mediante el modo de direccionamiento de registro base más desplazamiento. Un ejemplo de una instrucción de este tipo sería:
`beqm 4(x1), 8(x2), etiq` $\rightarrow$ Salta a etiq si (Mem[x1+4] == Mem[x2+8])

a) El formato de la instrucción sería formato I, ya que la instrucción necesita dos registros fuente. El formato se modificaría dividiendo el campo de inmediato en tres partes `dir. salto`, `inm2`, `inm1`, de 6, 5 y 5 bits respectivamente.

b)
**IF:**
PC4 $\leftarrow$ PC+4
**ID:** NUEVOS SUMADORES
A $\leftarrow$ Reg[x1] + 4
B $\leftarrow$ Reg[x2] + 8
+2 Sumadores
**LL:** REUTILIZA MD
A' $\leftarrow$ MD[A]
B' $\leftarrow$ MD[B]
**EX:** REUTILIZA ALU
Si A' == B':
	PC $\leftarrow$ PC + etiq
Si no:
	PC $\leftarrow$ PC4
# <mark style="background: #FFF3A3A6;">TEMA 2: Jerarquía de memoria</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
### <mark style="background: #D2B3FFA6;">- Justificación</mark>
Existen mayores cantidades de memoria y la diferencia de velocidad entre procesador y memoria degrada el rendimiento del procesador. Si la memoria es veloz y de gran tamaño, su coste aumenta mucho. $Coste=K·tamaño$ donde $K$ es una constante que según la tecnología, determina la velocidad.
Hay una jerarquía de memoria, donde se combinan varios tipos de memoria:
- Pequeño tamaño y gran velocidad
- Gran tamaño y poca velocidad
MC (Memoria Caché) -> SRAM: poca capacidad, muy veloz, muy costosa. (CPU)
MP (Memoria Principal) -> DRAM: capacidad, velocidad y coste medios. (CPU)
MS (Memoria Secundaria): gran capacidad, poco veloz, poco costosa. (SO $\rightarrow$ Archivo de paginación/Partición SWAP)

El procesador emite la dirección de memoria:
- **Se comprueba si el dato a buscar está en MC.** 
	- Está $\Rightarrow$ Acierto. Se pasa a CPU.
	- No está $\Rightarrow$ Fallo. Se propaga petición a nivel inferior.
		- **Se comprueba si el dato está en MP.**
			- Está $\Rightarrow$ Acierto. Se transfiere un bloque (conjunto de palabras) de datos a MC
			- No está $\Rightarrow$ Fallo. Se propaga petición a nivel inferior.
				- **Se comprueba si el dato está en MS.**
					- Está $\Rightarrow$ Acierto.
					- No está $\Rightarrow$ Fallo.
### <mark style="background: #D2B3FFA6;">- Funcionamiento</mark>
Hay varios principios:
- **Localidad:**
	- Temporal: Si se accede a un dato, la probabilidad de que se acceda a ese mismo dato en un corto intervalo de tiempo es muy alta.
	- Espacial: Si se accede a un dato, la probabilidad de que se acceda a un dato cercano es muy alta.
- **Inclusión:** Si un dato está en un nivel, también lo estará en todos los inferiores.
- **Coherencia:** Si se modifica un dato en un nivel, eventualmente, se modificará en los niveles inferiores.
### <mark style="background: #D2B3FFA6;">- Rendimiento</mark>
- **Acierto (hit):** Al buscar un dato, este existe en el nivel de memoria en el que se busca.
	- Tasa de aciertos (HR $\rightarrow$ _Hit Rate): $HR=nº~aciertos/nº~accesos=1-MR$
- **Fallo (miss):** Al buscar un dato, este no existe en dicho nivel.
	- Tasa de fallos (MR $\rightarrow$ _Miss Rate_): $MR=nº~fallos/nº~accesos$  
Tiempo de acceso medio: de media cuánto se tarda en acceder a un dato en memoria.
$$
\begin{equation}
T_{acceso}=t_{hit}+MR·P_{miss}
\end{equation}
$$
$$
\begin{equation}
P_{miss}=T_{acceso,~N-1}+t_{transferencia}
\end{equation}
$$
$$
\begin{equation}
t_{transferencia}=tam.~bloque~·~v_{transferencia}
\end{equation}
$$
Donde:
- $t_{hit}\equiv$ tiempo de hit o tiempo que se tarda en acceder al dato en el nivel superior.
- $MR\equiv$ tasa de fallos.
- $P_{miss}\equiv$ penalización por fallo o tiempo perdido si se produce un fallo.
- $t_{transferencia}\equiv$ tiempo de transferencia.
- $v_{transferencia}\equiv$ velocidad de transferencia.
## <mark style="background: #ADCCFFA6;">2. Memoria caché</mark>
Almacén temporal de bloques de MP. 
### <mark style="background: #D2B3FFA6;">- Estructura básica (MC - MP)</mark>

| línea       | bits de control | bloque |
| ----------- | --------------- | ------ |
| L0          |                 |        |
| L1          |                 |        |
| L2          |                 |        |
| L3          |                 |        |
| .<br>.<br>. |                 |        |
**$\uparrow$ MC $\uparrow$**

| bloque      | <-- 8B --> |
| ----------- | ---------- |
| B0          |            |
| B1          |            |
| B2          |            |
| B3          |            |
| .<br>.<br>. |            |
| B$2^{29}-1$ |            |
**$\uparrow$ MP $\uparrow$**

Dada una dirección de memoria de 32 bits $\rightarrow [31:0]$, esta se interpreta en dos campos:
- Nº bloque $\rightarrow [31:3]$
- Desplazamiento de bloque $\rightarrow [2:0]$
**Ejemplo: bloques de $8B=2^3B$**
Capacidad máxima de direccionamiento $\rightarrow 2^{32}B$ 
Hay $2^{29}$ bloques.
### <mark style="background: #D2B3FFA6;">- Políticas según estructura y funcionamiento</mark>

#### <mark style="background: #FF5582A6;">EJEMPLO:</mark>
Bus de direcciones 32b $\rightarrow 2^{32}B\rightarrow$ tamaño MP
MC de 1KB map. directo
Bloques 16B $\rightarrow2^4B$

$2^{32}/2^4=2^{28}$ bloques en MP
dirección de MP $[31:0]$ bits:
- Nº bloque: $[31:4]$ bits
- Desplazamiento bloque: $[3:0]$ bits
dirección ejemplo: $0xAAB38$:
- Nº bloque: AAB3
- Desplazamiento: 8

$2^{10}/2^4=2^6$ bloques (por tanto líneas) en MC
dirección de MC $[31:0]$ bits:
- Etiqueta: $[31:10]$ bits
- Índice de línea: $[9:4]$ bits
- Desplazamiento de bloque: $[3:0]$ bits
dirección ejemplo: $0xAAB38=1010~1010~1011~0011~1000$:
- Etiqueta: $1010~1010~10$
- Índice de línea: $11~0011$
- Desplazamiento: $1000$
#### • Políticas de localización
Hay tres tipos distintos de memoria caché.
- **MC de mapeado directo:** un bloque de MP se ubicará en una línea concreta de MC. Utiliza el módulo (%) como función de correspondencia. 
  $nº~bloque~\%~nº~líneas=ubicación~del~bloque~en~MC$
  Para interpretar la dirección de memoria:
	  - **Bit de válido (V):** 
	  - **Etiqueta:** 
	  - **Índice de línea:** puntero que apunta al número de línea.
	  - **Desplazamiento de bloque:** 
	  dir $\rightarrow$ índice de línea $\rightarrow$ línea MC $\rightarrow$ se comprueba V:
		  - si $V=0$ no hay nada útil, si $V=1$:
			  - si $etiqueta~!=$ no es el bloque que quiero acceder $\rightarrow$ FALLO
				  - si $etiqueta==~\rightarrow$ ACIERTO
  Los **pros** son que es muy rápida al localizar/ubicar y necesita muy poco hardware.
  El único **contra** que tiene es una alta tasa de fallos (MR).

- **MC totalmente asociativa:** un bloque de MP se puede ubicar en cualquier línea de MC. No se usa función de correspondencia para ubicar los bloques, se usa únicamente el bit de 
  válido (V) $\rightarrow$ en el primer $V=0$ ahí se ubica.
  La dirección de memoria se interpreta:
	  - **Etiqueta**
	  - **Desplazamiento de bloque**
  El único **pro** que tiene es que tiene una baja tasa de fallos (MR).
  Los **contras** son que necesita mucho hardware, es muy lenta y que necesita espacio adicional.
  
- **MC asociativa por conjuntos de $N$ vías:** un bloque de MP se puede ubicar en un subconjunto de líneas de tamaño $N$. Se trata básicamente de $N$ "cachés" en paralelo en las cuales se especifica el conjunto $C_n$ y dentro de ese conjunto se elige el subconjunto ("caché") correspondiente. Se vuelve a usar como función de correspondencia el módulo ($\%$) para elegir el subconjunto.
  Para interpretar las direcciones de memoria:
	  - **Etiqueta**
	  - **Índice de conjunto**
	  - **Desplazamiento de bloque**
  Para saber el número de conjuntos se hace igual que mapeo directo excepto que:
  $2^{10}/2^4=2^6$ líneas $\rightarrow 2^6/2^2$ (líneas) $=2^4$ conjuntos
#### • Políticas de escritura:
Hay dos:
- **Frente a acierto:** 
	- Write-Through (WT): modifica el bloque en MC y en MP. Es **más lento** pero **mantiene la coherencia**.
	- Copy-Back (CB): sólo modifica la MC. Es **más rápido** pero **mantiene una incoherencia temporal**. Surge un nuevo bit de control **D (Dirty)**, cuya función es indicar si está modificado el dato **' 1 '** o si no lo está **' 0 '**. Antes de colocar el bloque en MC desde MP, se comprueba el bit **D**, y si este se encuentra a **' 1 '**, se actualiza el bloque que se quiere sustituir desde MC a MP y luego se coloca el bloque nuevo en MC. 
- **Frente a fallo:**
	- Write-Allocation (WA): copia el bloque de MP a MC.
	- No Write-Allocation (NWA): modifica directamente en MP.
#### • Políticas de reemplazo:
Sólo se aplican a cachés con asociatividad cuando se produce el fallo y el conjunto está lleno. Las más conocidas son:
- **Aleatoria:** Sustituye un bloque al azar. Muy utilizada.
- **LRU (Less Recently Used):** Se sustituye el bloque que más tiempo lleva sin usarse. Alto coste.
- **FIFO (First-In First-Out):** Se sustituye el bloque que más tiempo lleva en caché.
- **LFU (Less Frequently Used):** Se sustituye el bloque que se usa menos veces.
### <mark style="background: #D2B3FFA6;">- Tipos de MC</mark>
Según la información:
- **Unificada:** es única para instrucciones y datos. Las etapas IF y MEM acceden a la misma caché. La **ventaja** es que balancea el espacio dedicado a instrucciones y datos (menos fallos) pero la **desventaja** es que como no se puede acceder a instrucciones y datos simultáneamente, habrán ciclos de bloqueo.
- **Separada:** cachés separadas para instrucciones y datos. Hay acceso simultáneo a IF y MEM. El **inconveniente**es que al fijar el tamaño dedicado a instrucciones y datos no se equilibra la carga (ligeramente más fallos).

Para mejorar el rendimiento se debe reducir el $T_{acceso}$ . Se puede conseguir reduciendo la frecuencia de fallos (menor frecuencia de fallos), las penalizaciones por fallo (menor tiempo en transferir un bloque) o el tiempo de acierto (menor tiempo en acceder a caché).
## <mark style="background: #ADCCFFA6;">3. Tipos de fallos</mark>

### <mark style="background: #D2B3FFA6;">- Fallo forzoso</mark>
Se producen cuando el bloque nunca ha sido cargado en memoria. También conocido como de arranque en frío. Habrá más fallos forzosos con bloques pequeños debido al principio de localidad espacial. Se reducen aumentando el tamaño de bloque.
### <mark style="background: #D2B3FFA6;">- Fallo por capacidad</mark>
Se producen cuando la caché se llena. Se reduce aumentando la caché.
### <mark style="background: #D2B3FFA6;">- Fallo por conflicto</mark>
Se produce cuando el número de bloques en memoria accedidos y que están en un mismo conjunto de caché, es mayor a la asociatividad de la caché. Sólo ocurre en mapeado directo o por conjuntos. Se reducen aumentando la asociatividad.
## <mark style="background: #ADCCFFA6;">4. Distintos niveles de MC</mark>
Añadir nuevos niveles a caché para reducir aún más los accesos a MP. Para 3 niveles sería así:
- CPU accede a L1, en caso de fallo a L2, en caso de fallo a L3...
- Los fallos de L1 serán accesos a L2, los fallos de L2 serán accesos a L3...
- El fallo producido en cada nivel de cachés es:
	- $ff_{L1}=\frac{L_{1-2}}{Acc}$
	- $ff_{L2}=\frac{L_{2-3}}{L_{1-2}}$
	- $ff_{L1}=\frac{F}{L_{2-3}}$
  Dando lugar a una tasa de fallos global como la siguiente:
  $ff_{Global}=ff_{L1}·ff_{L2}·ff_{L3}$
# <mark style="background: #FFF3A3A6;">TEMA 3: Entrada y Salida</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>

![[Imagen de WhatsApp 2024-04-29 a las 21.27.14_7678cee1.jpg|400]]
### <mark style="background: #D2B3FFA6;">- Justificación</mark>
La misión principal del módulo E/S es adaptar los dispositivos externos para su conexión al bus del sistema. No se pueden conectar directamente por varias razones:
- A menudo la velocidad de los dispositivos externos es menor que la de la memoria y CPU.
- Hay muchísimos periféricos como para adaptar toda la lógica que hace falta para todos.
- Los formatos y tamaños de datos suelen ser distintos en el periférico y en el computador.
## <mark style="background: #ADCCFFA6;">2. Módulo de E/S</mark>
Dentro del módulo hay **controladores** que son módulos E/S sencillos con el hardware mínimo para que el dispositivo funcione y **canales o procesadores E/S**, que son procesadores con su set de instrucciones orientado a E/S.

**Periférico**: Dispositivo externo + módulo E/S

Un módulo E/S contiene varios registros (generalmente de 8b). Hay tres tipos
- **Estado:** Nos da información del estado del dispositivo.
- **Control:** Solicita operaciones al dispositivo
- **Datos:** L/E
## <mark style="background: #ADCCFFA6;">3. Tipo de E/S</mark>
### <mark style="background: #D2B3FFA6;">- Según conexión con el sistema</mark>
#### <mark style="background: #FFB86CA6;">E/S Separada</mark>
![[Imagen de WhatsApp 2024-04-29 a las 21.42.39_5d0c4006.jpg|350]]
El acceso a E/S se realiza a través de un espacio de direcciones diferente al de memoria. <u>Ejemplo:</u> la arquitectura IA32 dispone de un espacio de direccionamiento E/S de 16b al que se accede con instrucciones `IN` `OUT`.
#### <mark style="background: #FFB86CA6;">E/S Mapeada</mark>
![[Imagen de WhatsApp 2024-04-29 a las 21.47.52_c51a3dca.jpg|350]]
El acceso a E/S se realiza a través de un espacio de direcciones mapeado en memoria. A los periféricos se les asignan posiciones de memoria como si fueran variables. <u>Ejemplo:</u> RISC, Motorola 68000.

Las **ventajas** de la E/S mapeada son que la UC es más sencilla, que es más rápida y que el ISA es más sencillo. Las **desventajas** son que el espacio de memoria se comparte, que hay que optimizar el código y que hay que tener cuidado con la MC (una forma de arreglar los errores con MC es configurar el espacio de direcciones ocupado por dispositivos como **no cacheable** evitando el uso de MC para estas direcciones).
#### <mark style="background: #FFB86CA6;">E/S Mapeada VS E/S Separada / Ejercicios</mark>

| E/S Mapeada                                                             | E/S Separada                                      |
| ----------------------------------------------------------------------- | ------------------------------------------------- |
| Ensamblador (RISC-V):<br>`load/store`<br>$\uparrow$ Práctica $\uparrow$ | C:<br>`inb/outb`<br>$\uparrow$  Teoría $\uparrow$ |
##### Ejercicio 1
<hr>
Control (dirbase + 0) -> Escribir "S"
Estado (dirbase + 1) -> Si el MSB == 1 -> Lectura disponible
Datos (dirbase + 2)
1. Escribir S en control
2. Comprobar si MSB == 1
	   - Leer registro de datos
### <mark style="background: #D2B3FFA6;">- Según comunicación con el procesador</mark>
De abajo a arriba:
- $\uparrow$ dependencia CPU
- $\downarrow$ coste
- $\downarrow$ HW necesario
#### <mark style="background: #FFB86CA6;">E/S programada (polling o sondeo)</mark>
El procesador tiene el control absoluto en la comunicación. 
![[IMG-20240601-WA0014.jpg|300]]
1. El procesador configura el módulo E/S. (Registro de control)
2. Consulta constante del estado del dispositivo. (Registro de estado)
3. El procesador lee el dato. (Registro de datos)
Las **ventajas** son que no necesita hardware adicional y que es muy rápido ya que está continuamente esperando a que el estado del del dispositivo cambie. Como **inconvenientes**, el procesador sólo está pendiente del estado del dispositivo.
##### Ejercicio
<hr>
Sean los registros RE (estado), RC (control) y RD (datos). Donde:
![[IMG-20240601-WA0021.jpg|200]]
- B = Busy
- R = Solicitar lectura
- D = Data ready
```C
unsigned char res;
while((inb(0x100) & 0x01) != 0); // Esperar que el dispositivo no esté ocupado
outb(inb(0x101) | 0x01, 0x101); // Solicitar lectura
while((inb(0x100) & 0x80) == 0); // Esperar que el dato esté disponible
res = inb(0x102);
```
#### <mark style="background: #FFB86CA6;">E/S por Interrupciones</mark>
![[IMG-20240601-WA0017.jpg|300]]
Hay dos tipos de interrupciones:
- **Hardware:** Se producen por la CPU o por E/S.
	- Internas: Generadas por la CPU, comúnmente conocidas como Excepciones.
	- Externas: Generadas por E/S.
		- Vectorizables: se usa un vector (o identificador). <mark style="background: #ABF7F7A6;">Daisy chain (consulta hardware)</mark>, <mark style="background: #ABF7F7A6;">Arbitraje del bus</mark>.
		- No vectorizables (autovectorizadas): la CPU identifica por sí misma quién ha generado la interrupción. <mark style="background: #ABF7F7A6;">Múltiples líneas de INT</mark>, <mark style="background: #ABF7F7A6;">Consulta software</mark>.
- **Software:** Relacionadas con el S.O.

Se le otorga al módulo E/S la capacidad de avisar al procesador cuando finalice una tarea, evitando las esperas activas.
1. El procesador configura el módulo E/S. (Registro de control)
2. El procesador cambia de contexto y el módulo E/S transfiere información con el dispositivo E/S.
3. Módulo E/S lanza interrupción.
4. El procesador atiende la interrupción.
5. El procesador ejecuta la RSI (Rutina Servicio de Interrupción).
La **ventaja** es que no hay espera activa. Los **inconvenientes** son que hace falta más hardware y es más lento en atender al módulo E/S.
##### <mark style="background: #FF5582A6;">Implementaciones</mark>
Surgen **4 implementaciones** al tener varios dispositivos E/S. ¿Cómo identificar el dispositivo que lanza la interrupción? ¿Prioridad al tener varios dispositivos interrumpiendo a la vez?
- <mark style="background: #ABF7F7A6;">Múltiples líneas de interrupción:</mark> 
  ![[IMG-20240601-WA0019.jpg|400]]
  Se identifica el dispositivo E/S mediante la línea de interrupción que se haya activado. La prioridad de los dispositivos se maneja según la CPU lea las señales. Como **inconveniente**: número limitado de dispositivos E/S. Como **ventaja**: identificar el dispositivo es muy sencillo.
- <mark style="background: #ABF7F7A6;">Consulta software:</mark> 
   ![[IMG-20240601-WA0020.jpg|400]]
   ![[IMG-20240601-WA0015.jpg|400]]
  Si se activa INT, se ejecuta una rutina de interrupción general. Se identifica el dispositivo E/S mediante lectura de registros de estado del módulo E/S. La prioridad se maneja por orden de lectura del estado. Como **ventaja**: no hay límite de dispositivos E/S. Como **inconveniente**: más lento, ya que se tienen que leer todos los registros de estado.
- <mark style="background: #ABF7F7A6;">Consulta hardware (daisy chain):</mark> 
  ![[IMG-20240601-WA0018.jpg|400]]
	- El módulo E/S activa INT
	- El procesador atiende la interrupción y activa INTA.
	- El módulo que haya generado la interrupción escribe su vector de interrupción (VI) en el bus de datos.
	- El procesador ejecuta el código correspondiente al VI.
	- El módulo E/S desactiva INT.
	- El procesador desactiva INTA.
  Se identifica al dispositivo E/S mediante el VI. Y la prioridad es en orden de lectura.
  Como **ventajas**: tantos dispositivos E/S como quiera, mucho más rápido que consulta software. Como **inconveniente**: mucho más hardware necesario.
- <mark style="background: #ABF7F7A6;">Arbitraje del bus:</mark> 
  ![[IMG-20240601-WA0016.jpg|400]]
	- El módulo de E/S pide acceso al bus de datos.
	- El módulo de E/S activa INT si está libre.
	- El procesador activa INTA.
	- El módulo E/S envía al procesador su VI.
  La prioridad la define el arbitrador (que puede ser todo lo complejo que quiera).
#### <mark style="background: #FFB86CA6;">E/S por DMA</mark>
La CPU "se desentiende" de la E/S. Aparece el **Controlador DMA (DMAC)**.
![[Imagen de WhatsApp 2024-06-07 a las 12.39.53_81ad7889.jpg|400]]
1. CPU configura DMAC.
   - Indica al DMAC la dirección de memoria del módulo E/S.
   - Indica la operación a realizar (R/W).
   - Indica el tamaño de los datos (nº palabras a transferir).
   - Especifica la dirección de memoria inicial.
2. CPU configura módulo E/S.
   ![[Imagen de WhatsApp 2024-06-07 a las 12.42.14_b5d9a85f.jpg|400]]
3. CPU cambia de contexto.
4. DMAC y E/S transfieren información (hasta que Cuenta sea 0):
	4.1. Módulo E/S activa DMA_REQ
	4.2. DMAC lee el dato y lo guarda en el registro Dato
	4.3. DMAC activa DMA_ACK
	4.4  DMAC pone la dirección en el Bus de Direcciones y el dato en el Bus de Datos y activa la escritura en memoria.
	4.5. DMAC actualiza la dirección y la cuenta (restando 1).
	4.6. DMAC desactiva DMA_ACK y el módulo E/S desactiva el DMA_REQ.
5. DMAC manda interrupción a CPU (activa TC)
Puede llegar a haber conflictos al usar la CPU y el DMAC los mismos AB y DB.
##### <mark style="background: #FF5582A6;">Implementaciones</mark>
1. <mark style="background: #ABF7F7A6;">Memoria multipuerto</mark>
   ![[Imagen de WhatsApp 2024-06-07 a las 12.44.18_4894e2e8.jpg|400]]
   No se usa porque es costosa.
2. <mark style="background: #ABF7F7A6;">Arbitraje del bus</mark>
   ![[Imagen de WhatsApp 2024-06-07 a las 12.50.01_654ee200.jpg|400]]
   - Módulo E/S avisa a CPU dato disponible.
   - Pregunta a CPU si bus disponible.
   - Si CPU activa HOLD_ACK, DMAC, avisa a E/S.
	Hay varias implementaciones
	1. **Ráfaga:** DMAC tiene acceso al bus hasta que termine todas las transferencias.
	2. **Robo de ciclo:** DMAC sólo tiene acceso durante un ciclo. (fly_by $\rightarrow$ E/S escribe directamente en Memoria).
	3. **Transparente:** DMAC sólo tiene acceso cuando CPU no utiliza el bus.
### EJERCICIO 5
<hr>

| 7   | 6   | 5   | 4   | 3   | 2   | 1   | 0   |
| --- | --- | --- | --- | --- | --- | --- | --- |
|     |     |     |     |     |     |     |     |
c) 
```C
#define R 0x0f
while((inb(R) & 0x08)!=0);
outb(((inb(R) & 0x10) >> 2) | (inb(R) & 0xfb),R);
```