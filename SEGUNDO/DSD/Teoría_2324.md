# ==TEMA 1: Metodologías de diseño y herramientas==
## 1.1. Concepto de sistema digital
Un sistema electrónico procesa una información y la convierte en otra, siendo el soporte una variable eléctrica.
### ♦ Señales digitales y analógicas
![[image.png|450]]
Para convertir de físico (analógico) a digital se usa el sensor mientras que para lo contrario se usa el actuador.

## 1.2. Opciones de implementación
### ♦ Software VS Hardware
El software es fácil y barato pero el hardware tiene altas prestaciones.
Dentro del software se distinguen procesadores de propósito general y específico. Mientras que dentro del hardware se distinguen dispositivos programables y circuitos integrados.

El software, ya sea para propósito general (Java, C) o específico (Assembly), es más flexible en rapidez y diseño. Sin embargo, el hardware, ya sean programables (FPGA) o integrados, es mejor respecto al coste.

## 1.3. Realización del hardware
### ♦ Diferentes opciones para realizar un sistema
- **Off-the-shelf:** FPGAs, SPLD/CPLD, IC. Ya fabricados para comprar. El diseño termina con un bitstream o secuencia binaria.  No se fabrican, se compran. Además, sirven para prototipar ASICs.
- **Manufactured:** Full-custom, ASIC. Fabricados a medida. Las ASICs tienen varias capas y termina con un layout (patrones geométricos para cada capa). Al fabricarse hay que testear tanto si sirve para el propósito para el que ha sido diseñado como si tiene defectos.
### ♦ Interior de un FPGA
![[fpga.png]]
- **CLB:** Bloques lógicos configurables para implementar la lógica.
- **I/O:** Interfaz de conexión para entrada/salida.
- **Red de interconexión:** Para conectar la entrada/salida de los CLB a la interfaz I/O.

Los CLBs se basan en LUTs (Look Up Tables), básicamente, memorias. Cada CLB contiene dos slices y cada slice contiene:
- 4 LUTs de 6 entradas.
- 8 flip flops configurables.
- Multiplexores.
- Lógica dedicada (para aritmética).

## 1.4. Diseño de hardware
> **HLSM:** High Level State Machine
### ♦ Flujo de diseño
![[flujo_diseño.png|400]]
<div style="page-break-before:always"></div>

# ==TEMA 2: Verilog==
Testbench en Verilog:
```verilog
`timescale 1ns/1ns
module comp_test
integer i,j; reg[1:0] a,b;
initial #1000 $finish;
initial begin
for(i=0; i<=3; i=i+1)
begin
for(j=0; j<=3; j=j+1)
begin
a=i; b=j; #10;
end
end
end
comp v_1(out1, out2, out3, a, b);
endmodule
```

## 2.1. Modelado estructural
- Componentes y como están conectados.
- Bloques simples o complejos.
- Instanciamos modules.
- Los modules tienen que estar descritos.
- Los "tipos" primitivos no se instancian (`and`, `or`, etc.).
- **TODO** es concurrente, no importa el orden.
- No se instancia dentro de `always` o `initial`.
### ♦ Tipos de datos:
- **Nets:** se usan en modelado estructural. El más común es `wire`.
- **Variable:** se usan en modelado comportamiento.

## 2.2. Modelado comportamiento
Son _procedures_ o flujos de actividad. Se ejecutan secuencialmente pero en la misma unidad de tiempo de simulación si no se especifica una. No se anidan. Los principales son `initial` (flujo que se ejecuta una vez) y `always` (cíclicamente).
### ♦ Tipos de datos:
- **Nets:** se usan en modelado estructural.
- **Variables:** se usan en _procedures_ (`initial`, `always`...). Conservan su valor hasta nueva asignación. El valor inicial es desconocido (x). El más común es `reg` (almacena un valor lógico). El tipo `reg` no sólo sirve para registros, además tiene el cualificador `signed`.
### ♦ Asignamientos bloqueantes y no bloqueantes:
- **Bloqueante (=):** se usan en _procedures_, no soporta _nets_ y se ejecutan antes de los que le siguen (una a una).
	```verilog
	initial
	begin
	a=1; b=0;
	a=b; // usa b = 0
	b=a; // usa a = 0
	```
- **No bloqueante (<=):** se usan en _procedures_, no soportan _nets_ y **NO BLOQUEA** a los que vienen detrás (todo a la vez).
	```verilog
	initial 
	begin
	a=1; b=0;
	a<=b; // usa b = 0
	b<=a; // usa a = 1
	```
### ♦ Temporización:
- **Control de retrasos (#):** `#Δt sentencia;`
- **Control por eventos (@):** `@(evento/s) sentencia;`
- **Sentencia _wait_:** `wait (expresión) sentencia;` Espera hasta que `expresión == true` y se ejecuta. 
### ♦ Control de flujo:
| Condicionales | Bucles | Otros |
|---|---|---|
| Ternario: `?...:...` | `repeat` | `disable` |
| Condicional: `if...else` | `for` | Paralelismo: `fork...join` |
| Casos: `case:` | `while` |  |
|  | `forever` |  |

## 2.3. Síntesis de circuitos combinacionales
- _Netlist_ de primitivas o módulos combinacionales.
- Asignamientos continuos.
- _Procedures_ `always @ ()`.
- `Functions` y `tasks`.
### ♦ Ejercicios
<hr style="margin-top:10px;">
#### EJERCICIO 2. Realizar una puerta OR de tres entradas.
**a)** Usando primitivas del lenguaje.
```Verilog
module or3entradas(input a, b, c, output sal);
	or c1(sal, a, b, c);
endmodule
```

**b)** Usando asignamientos continuos.
```Verilog
module or3entradas(input a, b, c, output sal);
	assign sal = a || b || c;
endmodule
```
**c)** Usando un flujo de actividad cíclico y un bucle `for`.
```Verilog
module or3entradas(input a, b, c, output reg sal);
	integer i;
	wire[2:0] entrada;
	assign entrada = {a,b,c};
	always @ *
	begin
		sal = 0;
		for(i = 0; i < 3; i = i + 1)
			if(entrada[i] == 1)
				sal = 1;
	end
endmodule
```
**d)** Usando un `procedure` cíclico y un operador reducción.
```Verilog
module or3entradas(input a, b, c, output reg sal);
	always @ *
		sal = |{a,b,c};
endmodule
```

#### EJERCICIO 5. Realizar un codificador de 8 entradas (One-Hot) y 3 salidas.
```Verilog
module cod8(input [7:0] entrada, output reg [3:0] salida);
	always @ *
		case(entrada):
			1: salida = 0; // en decimal
			8'b00000010: salida = 3'b001; // en binario
			4: salida = 2;
			8: salida = 3;
			16: salida = 4;
			32: salida = 5;
			64: salida = 6;
			128: salida = 7;
			default: salida = 3'bx; // desconocido
		endcase
endmodule
```

## 2.4. Síntesis de circuitos secuenciales
Un latch se describe con un condicional incompleto: 
```Verilog
always @ *
	if(clock)
	Q_latch = D;
```
Cuando se incluyen `posedge` o `negedge` en la lista de sensibilidad se sintetizan flip-flops:
```Verilog
always @ (posedge clk) Q_ff <= D;
```
Las señales de reset son importantes para los módulos secuenciales:
```Verilog
always @ (posedge clk or negedge reset) // asíncrono, activo en bajo
	if(reset == 0)
		Q <= 0;
	else
		Q <= D;

always @ (posedge clk) // síncrono, activo en alto
	if(reset == 1)
		Q <= 0;
	else
		Q <= D;
```
La lógica combinacional puede tener registros en sus salidas:
```Verilog
always @ (a or b)
	y = a & b; // se sintetiza una puerta and
always @ (posedge clk)
	y <= a & b; // se sintetiza una and conectada a un flip-flop D
```
### ♦ Ejercicios
<hr style="margin-top:10px;">

#### EJERCICIO 4. Realizar una descripción estructural del siguiente circuito.
![[Pasted image 20230922182358.png|600]]

```Verilog
// ---------------------
// BIESTABLE/FLIP-FLOP D
// ---------------------
module ff_D(input reset, clk, D, output reg Q);
	always @ (posedge clk)
		begin
			if(reset == 1)
				Q <= 0;
			else if(set)
				Q <= 1;
			else
				Q <= D;
		end
endmodule

// ---------------------
// REGISTRO DE 8 BITS
// ---------------------
module reg8(input rst, set, clk, input [7:0] DATAIN, output [7:0] Q);
	ff_D c1(rst, set, clk, DATAIN[0], Q[0]); // por posicion
	ff_D c2(.reset(rst), .set(set), .clk(clk), .D(DATAIN[1]), .Q(Q[1])); // por nombre
	ff_D c3(rst, set, clk, DATAIN[2], Q[2]);
	ff_D c4(rst, set, clk, DATAIN[3], Q[3]);
	ff_D c5(rst, set, clk, DATAIN[4], Q[4]);
	ff_D c6(rst, set, clk, DATAIN[5], Q[5]);
	ff_D c7(rst, set, clk, DATAIN[6], Q[6]);
	ff_D c8(rst, set, clk, DATAIN[7], Q[7]);
endmodule

// ----------------------------
// CIRCUITO FIGURA EJERCICIO 4
// ----------------------------
module circP4(input rst, set, clk, input [7:0] DATAIN, output [7:0] q);
	wire [7:0] R1;
	wire [7:0] R2;
	wire [7:0] R3;
	reg8 c1(rst, set, clk, DATAIN, R1);
	reg8 c2(rst, set, clk, R1, R2);
	reg8 c3(rst, set, clk, R2, R3);
	reg8 c4(rst, set, clk, R3, q);
endmodule
```
#### EJERCICIO 18. Realizar la descripción completa del EJERCICIO 4 en un único módulo.
```Verilog
module(input rst, set, clk, input [7:0] data_in, output [7:0] q);
	reg [7:0] R1, R2, R3, R4;
	always @ (posedge clk)
		begin
			if(rst == 1)
				begin
					R1 <= 0;
					R2 <= 0;
					R3 <= 0;
					R4 <= 0; 
				end
			else if(set == 1)
				begin
					R1 <= 8'b11111111; // binario
					R2 <= 255; // decimal
					R3 <= 8'b11111111;
					R4 <= 8'b11111111;
				end
			else
				begin
					R1 <= DATAIN;
					R2 <= R1;
					R3 <= R2;
					R4 <= R3;
				end
		end
	assign q = R4;
endmodule
```

## 2.4. Síntesis de máquinas de estado finito (FSM)
### ♦ Ejercicios
<hr style="margin-top:10px;">

#### EJERCICIO 14. Realizar la descripción Verilog para la siguiente FSM.
![[Pasted image 20230922190331.png]]

```Verilog
// m = z porque z es HI
module FSMp14(input init, s, m, clk, output, t, p);
	parameter ResetTimer = 2'b00, Wait = 2'b01, Pace = 2'b10;
	reg [1:0] state;
	always @ (posedge clk)
		begin
			if(init == 1)
				state <= ResetTimer;
			else
				case(state)
					ResetTimer: state <= Wait;
					Wait:
						if(s)
							state <= ResetTimer;
						else if(m)
							state <= Pace;
						else
							state <= Wait;
					Pace: state <= ResetTimer;
					default: state <= ResetTimer;
				endcase
		end
	assign t = (state == ResetTimer);
	assign P = (state == Pace);
endmodule
```
#### EJERCICIO 10. Escriba una descripción usando Verilog de un registro de desplazamiento de 4 bits con operación de puesta a cero asíncrona y salida serie.
```Verilog
// SIN OPERADOR >>
module reg4bits(input reset, clk, DSerie, output OutSerie);
	reg [3:0] Registro;
	always @ (posedge clk, posedge reset)
		begin
			if(reset == 1)
				Registro <= 0;
			else
				Registro <= {DSerie, Registro[3:1]};
		end
	assign OutSerie = Registro[0];
endmodule

// CON OPERADOR >>
module reg4bits(input reset, clk, DSerie, output OutSerie);
	reg [3:0] Registro;
	always @ (posedge clk, posedge reset)
		begin
			if(reset == 1)
				Registro <= 0;
			else
				begin
					Registro[2:0] <= Registro >> 1;
					Registro[3] <= DSerie;
				end
		end
	assign OutSerie = Registro[0];
endmodule
```
#### EJERCICIO 11. Escriba una descripción usando Verilog de un contador de 4 bits con las siguientes operaciones: reset asíncrono, carga en paralelo, inhibición, cuenta ascendente y descendente síncronas.

| Reset | Load | Cuenta | Sentido | Operación |
| --- | --- | --- | --- | --- |
| 1 | - | - | - | Puesta a cero |
| 0 | 1 | - | - | Carga en paralelo |
| 0 | 0 | 1 | 0 | Descontar |
| 0 | 0 | 1 | 1 | Contar |
| 0 | 0 | 0 | - | Inhibición |

```Verilog
module cont4bits(input clk, Reset, Load, Cuenta, Sentido, input [3:0] DATAIN, output reg [3:0] Contador);
	
endmodule
```

# ==TEMA 3: Comportamiento temporal de circuitos digitales==
## 3.1. Introducción
Una puerta lógica podemos entenderlo como un elemento compuesto por interruptores que conectan la salida al valor 1 o 0 en función de la entrada mediante transistores. Un modelo sencillo para explicar esto es el modelo RC:
![[Pasted image 20230929175157.png|400]]
- $R_p/R_n$ : Resistencia asociada al transistor $p/n$.
- $C_{INT}$ : Capacidad intrínseca asociada al dispositivo. 
En el modelo RC: $retraso = {0.69RC}$ aproximadamente.

## 3.2. Puertas lógicas e interconexiones
### ♦ Parámetros temporales. Retrasos.
$$
Tiempos~de~trancisión=
\left
\lbrace
\begin{array}{c}
t_f:~Tiempo~de~bajada~(90\%-10\%) \\ t_r:~Tiempo~de~subida~(10\%-90\%)
\end{array}
\right.
$$

$$
Retrasos=
\left
\lbrace
\begin{array}{c}
t_{pLH}:~Tiempo~de~propagación~Bajo~Alto~(50\%-50\%) \\ t_{pHL}:~Tiempo~de~propagación~Alto~Bajo~(50\%-50\%)
\end{array}
\right.
$$
- Se pueden medir otros retrasos para una puerta: tiempo que transcurre hasta que la salida es válida o tiempo que transcurre hasta que la salida empieza a cambiar. Este último se denomina tiempo de contaminación. 
- Los retrasos dependen a su vez del proceso de fabricación y de las condiciones de operación (temperatura, tensión).
- Los retrasos dependen de las condiciones de carga (qué se conecta a la salida de la puerta) 
- Cada camino de entrada/salida tiene su propio retraso. 
- Caracterizar el comportamiento temporal de las puertas es importante para poder analizar el comportamiento de los circuito.
- Los modelos de retrasos que se utilizan para dicho propósito tienen en cuenta dichos factores, y con frecuencia proporcionan tres valores para cada parámetro: mínimo, típico y máximo.
Hay dos tipos de retraso:
- **Retraso inercial:** medida del tiempo mínimo que un pulso de señal debe estar presente en la entrada de un dispositivo para que sus efectos aparezcan en la salida.
- **Retraso de trasnporte:** sólo implica un retraso de la propagación de la señal.

## 3.3. Circuitos combinacionales
- El **retraso** de un circuito combinacional depende de la transición de entradas aplicada. Dependiendo del cambio que se aplique en las entradas, éste se propaga por distintos caminos de señal. 
- **Retraso de camino (path delay):** Tiempo que tarda la entrada en afectar a la salida cuando los cambios se propagan por dicho camino.
- **Camino crítico (critical path):** Camino de señal con mayor retraso. 
- **Retraso máximo (circuit delay):** Retraso del camino crítico. Este es el que se usa como retraso de un circuito combinacional
Como consecuencias de los retrasos, en el circuito se producen pulsos cortos no deseados llamados glitches. Se dice que el sistema presenta azares cuando tiene riesgo de producir glitches. Hay distintas formas de impedir que ocurran:
- Aplicar métodos de diseño.
- Igualar los tiempos de retraso.
- Utilizar sincronizadores.
	![[Pasted image 20230929182251.png|600]]

## 3.4. Circuitos secuenciales con reloj ideal
### ♦ Elementos de memoria
Los elementos de memoria son dispositivos básicos en los sistemas digitales. Estos tienen distintos parámetros:

| **Parámetro**              | **Acrónimo**     | **Descripción** |
| -------------------------- | ---------------- | ---------------- |
| Tiempo de establecimiento  | $t_{setup}$      | Tiempo mínimo que deben mantenerse constantes las entradas antes del flanco                 |
| Tiempo de mantenimiento    | $t_{hold}$       | Tiempo mínimo que deben mantenerse constantes las entradas después del flanco                 |
| Tiempos de propagación     | $t_{pLH (CLK-Q)}$   $t_{pHL (CLK-Q)}$ | Tiempo desde el flanco hasta la respuesta                 |
| Frecuencia máxima de reloj | $f_{max}$                 | Máxima frecuencia de reloj que admite el FF para ser fiable                 |
### ♦ Restricciones
![[Pasted image 20230929184616.png]]
- **Restricción de setup:** Entre dos flancos consecutivos de reloj, se debe permitir que Q1 se establezca a su valor correcto, que se propague a través de la lógica B y que llegue al FF2 con una antelación válida.
	$T_{CLK} \geq t_{p,max}(FF1) + t_{p,max}(B) + t_{setup}(FF2)$ 
- **Restricción de hold:** Una vez que se produce el flanco de reloj, el retraso de propagación de FF1 y de la lógica B debe servir para que durante el tiempo de hold se mantenga en la entrada FF2 el dato anterior al flanco activo de la señal de reloj.
	$t_{p,min}(FF1)+t_{p,min}(B) \geq t_{hold}(FF2)$

> **Reloj ideal:** Un reloj ideal es un reloj cuyo flanco activo llega simultáneamente a todos los elementos del circuito.

## 3.5. Circuitos secuenciales con reloj no ideal
Tienen dos incertidumbres características: el **jitter** y el **clock skew**. El clock skew es aleatorio y el jitter, sistemático.
- **Clock skew:** 
	- Los flancos activos del reloj no alcanzan a todos los elementos de memoria en el mismo instante. 
	- El skew está provocado por diferencias estáticas entre los distintos trayectos de propagación del reloj y por las diferencias en cuanto a carga de las distintas señales del reloj. 
	- Es constante entre un ciclo y otro. 
	- El skew no varía el período del reloj, sino sólo la fase.
	Las restricciones son:
		$T_{CLK} \geq t_{p,max}(FF1) + t_{p,max}(B) + t_{setup}(FF2) + t_{skew}$
		$t_{p,min}(FF1)+t_{p,min}(B) \geq t_{hold}(FF2)+t_{skew}$
- **Jitter:** 
	- Variación aleatoria del instante de tiempo en el que llega el flanco activo del reloj a un punto determinado del circuito. (condiciones de entorno, de carga, etc. que hay en ese momento). 
	- Se trata de una medida de incertidumbre estrictamente temporal y que a menudo se especifica para un punto determinado del circuito: hace referencia al hecho de que el período de reloj puede reducirse o ampliarse de un ciclo a otro.
	Las restricciones son:
		$T_{CLK} \geq t_{p,max}(FF1) + t_{p,max}(B) + t_{setup}(FF2) + 2·t_{jitter}$
		$t_{p,min}(FF1)+t_{p,min}(B) \geq t_{hold}(FF2)+2·t_{jitter}$

![[Pasted image 20230929190302.png|800]]

# ==TEMA 4: Técnicas de optimización a nivel RT==
## 4.1. Introducción
### ♦ Latencia y rendimiento
La latencia es el tiempo que tarda el sistema en producir un resultado. El rendimiento es el número de resultados producidos por unidad de tiempo.
## 4.3. Diseño de alto nivel
### ♦ Planificación de operaciones (Operator Scheduling): 
Asociar operaciones a ciclos de reloj. Para la _Planificación 1_, forzosamente se necesitan dos multiplicadores ya que en el mismo ciclo no se pueden realizar dos operaciones de multiplicación concurrentemente. Sin embargo, para la _Planificación 2_, se puede utilizar un sólo multiplicador ya que las dos operaciones se realizan en ciclos de reloj distintos.
![[Pasted image 20231019203634.png]]
### ♦ Asignación de recursos (Operation Binding):
Se mapea un conjunto de operadores a un conjunto de componentes. Diferentes elecciones implican diferentes costes y retrasos. En la _Asociación 1_ se ha elegido esa posibilidad porque los datos no tienen nada en común. En la _Asociación 2_, se ha elegido esa posibilidad al aparecer $t_3$ tanto en A como en C.
![[Pasted image 20231019204020.png]]
### ♦ Ejemplo 1: Filtro FIR (Finite Impulse Response)
Este filtro convierte una secuencia de entradas digitales en otra secuencia de salidas digitales habiendo modificado la secuencia de entrada. Por ejemplo, la operación: 
$y(t)=c_0·x(t)+c_1·x(t-1)+c_2·x(t-2)~\forall~c_0,~c_1,~c_2\equiv Constantes$ 
- **Planificación 1 (totalmente concurrente):** 2 estados. Uno de cómputo (FC), el datapath produce una salida cada ciclo. 3 Multiplicadores y 2 Sumadores.
![[Pasted image 20231019205129.png|600]]
- **Planificación 2 (serializada):** Una multiplicación por ciclo. 1 Multiplicador y 1 Acumulador. El datapath produce 1 resultado/5 ciclos.
![[Pasted image 20231019205143.png|550]]
## 4.4. Ejercicios: 

<hr>
#### 2. El circuito de la Figura realiza un determinado procesado sobre 5 datos de entrada, generando un resultado en cada ciclo de reloj. Los bloques C1, C2 y C3 son circuitos combinacionales con retrasos de 1, 2 y 3 unidades de tiempo respectivamente. Suponga los registros ideales. La señal de reloj es común a todos los registros. 
![[Pasted image 20231019194123.png]]
**a) Evalúe la latencia, el rendimiento y el periodo mínimo de la señal de reloj.

| $Latencia$               | $Rendimiento$                      | $T_{min}$ |
| ------------------------ | ---------------------------------- | --------- |
| 1 ciclo = 5ut @$F_{MAX}$ | 1 resultado/ciclo = 1 resultado/5ut @$F_{MAX}$ | 5ut                             |                                    |           |

**b) Diseñe una versión con pipeline que produzca un resultado cada 4 unidades de tiempo. No se pueden modificar las componentes combinacionales. Minimice el número de registros de pipeline añadidos. Evalúe rendimiento, latencia y periodo mínimo para el diseño propuesto.**

| $Latencia$                 | $Rendimiento$                                 | $T_{min}$ |
| -------------------------- | --------------------------------------------- | --------- |
| 2 ciclos = 24ut @$F_{MAX}$ | 1 resultado/ciclo = 1 resultado/4ut @$F_{MAX}$ | 4ut       |

#### 3. El circuito de la figura realiza un determinado cálculo sobre las entradas A, B, C y D. Suponiendo los siguientes datos para el comportamiento temporal de sus componentes: 
- **Flip-flops: retraso de propagación, $T_{p_{clk}}$->Q = 0.5ns, $T_{hold}$=0.1ns, $T_{setup}$=0.3ns** 
- **Retraso de propagación máximo de 20 ns para el multiplicador y de 5 ns para el sumador**
- **Retrasos asociados a las salidas y a las entradas nulos**
![[Pasted image 20231019200308.png]]
**a) Calcule el periodo mínimo de la señal de reloj. El reloj, aunque no se muestra, es común para todos los registros e ideal.** 
$T_{min}=25ns+0,5ns+0,3ns=25,8ns$

**b) Determine el rendimiento y la latencia en términos de ciclos de reloj.**
$Latencia = 2~ciclos = 51,6ns~@F_{MAX}$
$Rendimiento = 1~resultado/ciclo = 1~resultado/25,8ns~@F_{MAX}$

**c) Modifique el circuito para mejorar el rendimiento. Evalúe el rendimiento del circuito modificado suponiendo que se opera a la frecuencia máxima posible.**
*$Latencia = 3~ciclos = 62,4ns~@F_{MAX}$
$Rendimiento = 1~resultado/ciclo = 1~resultado/20,8ns~@F_{MAX}$
$T_{min}=20ns+0,5ns+0,3ns=20,8ns$

#### 4. Sea el circuito de la figura con los retrasos de propagación máximos y mínimos de cada bloque combinacional que se muestran en la siguiente tabla.
![[Pasted image 20231019201431.png]]
**a) ¿Qué registro o registros de pipeline (R1, R2, R3) eliminaría para minimizar la latencia (en tiempo) suponiendo operación a la frecuencia máxima? Explique su respuesta.**

| $Latencia Original$   | $Latencia$   | $Latencia~sin~R1$ | $Latencia~sin~R2$ | $Latencia~sin~R3$ |
| --------------------- | ------------ | ----------------- | ----------------- | ----------------- |
| 4 ciclos · 7ns = 28ns | 3 etapas · 7ns = 21ns | 3 etapas · 13ns = 39ns     | 3 etapas · 12ns = 36ns     | 3 etapas · 8ns = 24ns                  |

**b) ¿Qué registro o registros eliminaría para maximizar el rendimiento suponiendo operación a la frecuencia máxima? Explique su respuesta.**
Ninguno ya que el rendimiento ya es máximo con un $T_{min}$ de 7 nanosegundos.
#### 5. Diseña un sistema que calcule $X^2·Y^2+Z^2+W^2$ utilizando el algoritmo mostrado en la Figura P5 y con las siguientes consideraciones:
- **No entrar en el detalle de número de bits**
- **Los datos están disponibles mientras se realiza el cálculo y no se almacenan en registros**
- **Los recursos de cálculo (unidades funcionales) disponibles son: multiplicadores (M), sumadores (S) y elevadores al cuadrado (C)**
- **F1 = X^2
- **F2 = Y^2**
- **F3 = F1 · F2**
- **F4 = Z^2**
- **F5 = F3 + F4**
- **F6 = W^2**
- **F7 = F5 + F6**

**a) Sol 1: Todo el cálculo en un ciclo de reloj**

<table style="border: 1px solid;">
	<tr>
		<td>1</td>
		<td>F1</td>
		<td>F2</td>
		<td>F3</td>
		<td>F4</td>
		<td>F5</td>
		<td>F6</td>
		<td>F7</td>
	</tr>
</table> 

- 4 Cuadrados
- 1 Multiplicación
- 2 Sumas

**b) Sol 2: Todo el cálculo en dos ciclos de reloj**

<table style="border: 1px solid;">
	<tr>
		<td>1</td>
		<td>F1</td>
		<td>F2</td>
		<td>F3</td>
	</tr>
	<tr>
		<td>2</td>
		<td>F4</td>
		<td>F5</td>
		<td>F6</td>
		<td>F7</td>
	</tr>
</table>

- 2 Cuadrados
- 1 Multiplicación
- 2 Sumas

**c) Sol 3: sólo se usa una unidad de cálculo de cada tipo (1C, 1M, 1S)**

<table style="border: 1px solid;">
	<tr>
		<td>1</td>
		<td>F1</td>
	</tr>
	<tr>
		<td>2</td>
		<td>F2</td>
		<td>F3</td>
	</tr>
	<tr>
		<td>3</td>
		<td>F4</td>
		<td>F5</td>
	</tr>
	<tr>
		<td>4</td>
		<td>F6</td>
		<td>F7</td>
	</tr>
</table> 

**d) Sol 3 Alternativa: sólo se usa una unidad de cálculo de cada tipo (1C, 1M, 1S) y SIN ENCADENAMIENTO DE OPERACIONES**

<table style="border: 1px solid;">
	<tr>
		<td>1</td>
		<td>F1</td>
	</tr>
	<tr>
		<td>2</td>
		<td>F2</td>
	</tr>
	<tr>
		<td>3</td>
		<td>F3</td>
		<td>F4</td>
	</tr>
	<tr>
		<td>4</td>
		<td>F5</td>
		<td>F6</td>
	</tr>
	<tr>
		<td>5</td>
		<td>F7</td>
	</tr>
</table> 

# ==TEMA 5: Aritmética en punto fijo==
## 5.1. Sistemas de numeración
El objetivo principal es implementar en el hardware funciones aritméticas, atendiendo a: velocidad, coste, simplicidad... Estos circuitos forman la base del procesado de datos. 
- Un número binario de longitud $n$ es una secuencia ordenada $(x_{n-1}x_{n-2}...x_1x_0)$ de dígitos binarios donde cada dígito $x_i$ toma valores 0 o 1. 
- Esta longitud $n$ es importante ya que los números binarios se almacenan en registros de longitud fija. 
- La base es llamada _radix_.
- El rango de números representables será $[X_{min},~X_{max}]$. Si no cabe en el rango, hay _overflow_.
- El sistema binario es un ejemplo específico de un sistema de numeración. Un sistema de numeración se define por el conjunto de valores y la regla que define el mapeo entre los dígitos y sus valores. Hay dos tipos, convencionales (binario, decimal,...) y no convencionales (bases negativas, dígitos con signo,...).
### ♦ Propiedades de los sistemas de numeración convencionales
- **No redundante:** Cada número tiene representación única, es decir, no hay dos secuencias con el mismo valor.
- **Pesado:** Una secuencia de pesos $(w_{n-1}w_{n-2}...w_1w_0)$ determina el valor de la secuencia $(x_{n-1}x_{n-2}...x_1x_0)$ como: $\sum_{i=0}^{n-1}{x_i·w_i}$ .
- **Posicional:** El peso $w_i$ depende sólo de la posición $i$ del dígito $x_i$ . Se cumple $w_i = r^i$ .
- $r$ es la base del sistema de numeración.
- Si no hay redundancia, $0\leq x_i\leq r-1$ .
### ♦ Conversión entre bases
Pretendemos convertir un número en base $r$ a base $R\neq r$ . Se usan los métodos de divisiones sucesivas para la parte entera y multiplicaciones sucesivas para la parte fraccionaria.
![[Pasted image 20231020180724.png|500]]
### ♦ Números negativos
Hay dos formas para representarlos:
- **Signo-Magnitud:**
	- El signo y la magnitud se representan de forma separada 
	  $x_Sx_{n-2}x_{n-3}~...~x_2x_1x_0$
	  donde $x_S$ es el dígito de signo y  $x_{n-2}x_{n-3}~...~x_2x_1x_0$ la magnitud (n-1 bits).
	- En binario, el rango es $[-(2^{n-1}-1),+(2^{n-1}-1)]$ .
- **Complemento a dos:**
	- Los números positivos se representan igual que en S-M.
	- Un número negativo $(-Y)$ se representa por $(R-Y)$, donde $R$ es una constante.
	- Esta representación satisface $-(-Y)=Y$ ya que $R-(R-Y)=Y$ .
	- No se repite el 0.
	- En binario, el rango es $[-2^{n-1},+(2^{n-1}-1)]$. 
	- Las ventajas de esta representación son:
		- $X-Y=X+(-Y)=X+(R-Y)$ 
		- No hay necesidad de decidir antes de sumar o restar ni de cambiar el orden de los operandos.
	-  Ejemplo Ca2
		$r=2,~k=n=4\rightarrow m = 0, ulp=2^0=1$
		$~~~5=0101$
		$-5=1010+1=1011$ 

## 5.2. Circuitos básicos para aritmética binaria
### ♦ Half Adder
![[Pasted image 20231020190719.png|500]]
### ♦ Full Adder
![[Pasted image 20231020185900.png|500]]
![[Pasted image 20231020185919.png|500]]
### ♦ Restador
![[Pasted image 20231020185944.png|500]]
## 5.3. Arquitecturas de sumadores
### ♦ Ripple Adder
![[Pasted image 20231020190026.png|400]]
Se necesita esperar que atraviese los $n$ FAs antes de que la suma sea válida. Tiene un retraso con crecimiento lineal. El esquema más común para mejorar la velocidad es el Carry Look-Ahead.
### ♦ Carry Look-Ahead (Acarreo Adelantado)
Se generan todos los acarreos en paralelo y ello es posible ya que dependen de las entradas y todas están disponibles simultáneamente. Utiliza las funciones de generación $g_i$ y propagación de acarreo $p_i=a_i+b_i$.
![[Pasted image 20231020190852.png|550]]
Se mejora el retraso pero se empeora la complejidad con un acarreo serie cada 4 sumadores.
![[Pasted image 20231020191227.png|600]]
La idea del CLA es generalizar de nivel de bit a nivel de grupos de bits:
![[Pasted image 20231020191649.png|600]]
### ♦ Sumador de Brent-Kung
Sea una función $F$:
![[Pasted image 20231020192219.png|500]]
Para solucionar el incremento lineal del retraso, se puede usar un árbol binario:
![[Pasted image 20231020192311.png|300]]
Quedando el árbol completo como:
![[Pasted image 20231027174952.png|500]]
- El área es casi el doble de un Ripple.
- El layout es muy compacto.
- Los bits de suma requieren un tiempo adicional constante.
### ♦ Sumadores por suma condicional
- Tienen retraso logarítmico.
- Hay dos grupos de salidas. En un grupo se realiza la suma con carry 1 y en el otro con carry 0.
- Una vez que se conoce el carry, solo se necesita un MUX para elegir la suma correcta, librándonos de la propagación.
![[Pasted image 20231027175641.png|300]]

# ==TEMA 6: Multiplicación, división y gen. de func.==
## 6.1. Multiplicación
### ♦ Combinacional
La multiplicación combinacional es costosa, compleja y permite menos modularidad pero es más rápida. 
### ♦ Secuencial
Sean dos números positivos $X=01011_{(2}$ y $A=01101_{(2}$ para realizar $A\times X$ :
![[Pasted image 20231027182505.png|400]]
Puede pasar que el multiplicando esté en Ca2 y por tanto tenga 1 bit en el más significativo si es negativo: $x_{n-1}=1\rightarrow X=-x_{n-1}2^{n-1}+\tilde{X}$ con $\tilde{X}=\sum_{j=0}^{n-2}{x_j2^j}$ . Ignorando el bit de signo el resultado sería $\tilde{X}·A$. Como buscamos $X·A$, se deberá restar $A$ en Ca2.
### ♦ Multiplicadores de altas prestaciones
- **Carry-Save Adder:** El CSA acepta X operandos de n-bits y genera Y resultados de n-bits. Siendo Y el numero de bits necesarios para representar X.
- **Reducir los productos parciales:** Al examinar simultáneamente dos o más bits del multiplicador, entonces se reducen los productos parciales. Un algoritmo que usa esto es el Algoritmo de Booth. Por ejemplo: $7A=(2^3-2^0)A=2^3A-2^0A=2^3A+Ca2(A)$ 
- **Usando multiplicadores más pequeños:** Si un multiplicador de $n\times n$ bits se construye como un CI, podemos usarlo para hacer multiplicadores más complejos. Un multiplicador de $2n\times 2n$ se puede construir con 4 multiplicadores $n\times n$ ya que:
  $A·X=(A_H·2^n+A_L)·(X_H·2^n+X_L)=A_HX_H2^{2n}+(A_HX_L+A_LX_H)2^n+A_LX_L$ 
  donde $H$ o $L$ son las mitades más y menos significativas.
- **Multiplicación paralela:**
  ![[Pasted image 20231103173621.png|550]]
   
   ## 6.2. División
   ### ♦ Operación y técnicas básicas
   La división es la operación mas compleja y que más consume de las cuatro operaciones.
   En general, dado un dividendo $X$ y un divisor $D$, se genera un cociente $Q$ y un resto $R$ tal que
   $X=Q·D+R$ (con $R < D$), suponiendo $X, D, Q$ y $R$ positivos. $X$ puede ocupar un registro doble ($2k$) mientras que los otros ocupan un registro simple ($k$). $Q$ debe ser menor que el número máximo que admita un registro simple: $Q<2^k$ y $R<D$ . Se debería chequear $D\neq 0$ para la división entre 0.
   **Ejemplo de división 32/6:**
   ![[Pasted image 20231103175734.png]]
   La división entera puede reformularse como división fraccionaria y viceversa. Sólo que el resto debe desplazarse a la derecha $k$ bits.
### ♦ División con restauración
![[Pasted image 20231103180346.png]]
   Para operandos con signo, se usa la expresión $X=Q·D+R$ junto con $sign(R)=sign(X)$ y 
   $|R|<|D|$ .
### ♦ División sin restauración
En esta división almacenamos $q_{k-j}=1$ en el $PR$ , conduciendo a un resto parcial temporalmente incorrecto. Esto es aceptable porque...
- Al inicio $[PR]=u$ .
- Si hemos restaurado el resto parcial $(u-2^kD)$ a su valor correcto $u$, deberíamos continuar con el desplazamiento de $u$ a $2u$ y la nueva resta $2u-2^kD$ .

## 6.3. Generadores de función
Una LUT de $n$ bits puede codificar cualquier función booleana modelando dicha función en su tabla de verdad. LUTs con 4-6 bits de entrada son componentes clave en las FPGAs.
![[Pasted image 20231103180901.png]]
Se usa el algoritmo CORDIC (COordinate Rotation DIgital Computer), conocido como método de dígito por dígito. Se basa en la observación de que si se rota un ángulo $\alpha$ de un vector de longitud unidad cuyo extremo se encuentra inicialmente en $(x,y)=(1,0)$, el nuevo extremo se encuentra en $(X,y)=(\cos\alpha,\sin\alpha)$, por lo que $\cos\alpha$ y $\sin\alpha$ podrían calcularse obteniendo las coordenadas del nuevo extremo después de rotar $\alpha$.
# ==TEMA 8: Optimización del consumo de potencia==
## 8.1. Medida de la potencia disipada
### ♦ Baterías
Al reducir la potencia, se pueden usar fuentes de energía alternativas, o incluso hacer móviles aplicaciones que no lo eran tradicionalmente.
### ♦ Temperatura
El aumento de temperatura puede provocar daños al sistema como acortar la vida útil o la necesidad de utilizar disipadores, por lo que es bueno reducir el consumo de potencia.
### ♦ Potencia instantánea y potencia promedio
- **Potencia instantánea:** 
  $P(t)=\sum_i{V_i(t)·I_i(t)}$
- **Potencia promedio:** 
  ![[Pasted image 20231117175704.png|150]] 
### ♦ Potencia en un inversor CMOS
Se puede calcular la potencia disipada de un sistema digital sumando la potencia de sus elementos (los más básicos: puertas). A partir de la puerta más sencilla, el inversor, se estudian las demás puertas más complejas.
![[Pasted image 20231117181013.png|500]]
En la segunda situación es cuando se consume la mayor cantidad de potencia.
$P=\frac{1}{2}·f·C_L·\alpha·V_{dd}^2$
- $f$ = frecuencia
- $C_L$ = capacidad de carga
- $\alpha$ = actividad de conmutación: número de veces que las puertas conmutan
Extendiendo a sistemas completos:
$\overline{P_{Sistema}}=N·f_{clk}·\overline{C}·\overline\eta·V_{DD}^2$  
## 8.2. Nivel de tecnología
- Elegir la tecnología adecuada (menos capacidad en los nudos y menor tensión de alimentación).
- Aumentar tensiones umbrales (reduce la potencia estática y de corto y no afecta a la de conmutación $\rightarrow$ reduce la velocidad).
- Usar una tensión de alimentación más baja que la permitida (reduce la potencia estática y de corto y no afecta a la de conmutación $\rightarrow$ reduce la velocidad).
## 8.3. Nivel de circuito
- Reducción de la potencia de conmutación.
- Reducción de la potencia por corriente de corto.
- Reducción de la potencia estática.
## 8.4. Nivel lógico
### ♦ Codificación
Sea un diagrama de estados:

|  ![[Pasted image 20231117182510.png]]   |   ![[Pasted image 20231117182752.png]]  |  ![[Pasted image 20231117182759.png]]v   |
| --- | --- | --- |
| Original    |  Contando trancisiones   | Codificada    |

Hay dos estados con un alto número de transiciones que tienen códigos con distancia un bit. En algunos casos la parte combinacional es más compleja.
### ♦ Detener el reloj
![[Pasted image 20231117182859.png|300]]
La función de activación Fa detecta cuando la FSM está internamente inactiva (no hay transiciones) y detiene el reloj. Para detener el reloj se suele usar un circuito como este:
![[Pasted image 20231117183043.png|250]]
### ♦ Particionado de FSM
Se divide la FSM en dos FSMs más sencillas.
![[Pasted image 20231117183211.png|300]]
### ♦ Pre-cálculo
Las salidas se calculan en el ciclo anterior para reducir la conmutación. Si una salida se puede 
pre-calcular, se puede "inhabilitar" el circuito lógico. Hay algunos casos en los que es peor por ejemplo:
![[Pasted image 20231117183427.png|300]]
![[Pasted image 20231117183436.png|400]]
- Se incrementa el área
- El retraso entre R1 y R2 aumenta
- El retraso anterior a R1 se incrementa
### ♦ Balancear caminos
![[Pasted image 20231117183901.png|450]]
### ♦ Retiming
Se localizan los nodos con una alta actividad de azar y alta capacidad de carga. En esos nodos se colocan flip-flops. Hay que asegurarse que el tiempo de ciclo del circuito no incremente y de especificar un límite superior en el número de registros.
## 8.5. Nivel RT
### ♦ Aislar operandos
![[Pasted image 20231117184539.png|500]]
### ♦ Re-especificación del control
La actividad de las unidades desocupadas se puede eliminar escogiendo los valores de control de la lógica de conexión (multiplexores y drivers tri-estado). Los valores de control de la lógica de conexión suelen ser _don’t care_ en la FSM cuando la unidad no se utiliza.
### ♦ Segmentación de la memoria
Una memoria está desocupada cuando almacena un dato irrelevante. Los recursos de memoria se dividen en segmentos (clusters). Cuando un segmento está desocupado se pone en _sleep mode_, es decir se deshabilita el reloj o la señal de refresco.
## 8.6. Nivel arquitectural
Cuando la arquitectura del sistema se basa en un CPU, la carga capacitiva de E/S es mayor que la de los nudos internos del CPU. El consumo de las memorias aumentan con el tamaño.
### ♦ **Técnicas de codificación de bus** 
La potencia disipada depende de la distancia Hamming entre datos sucesivos. 
- La codificación One-hot es la peor al tener buses mayores
- La codificación binaria da el peor pico de actividad
- El código Gray es el mejor y reduce la actividad un 50%
![[Pasted image 20231117185847.png|400]]

- **Bus-Invert:** Se aplica a los buses y requiere una señal adicional invert.
  ![[Pasted image 20231117190242.png|400]]
  La actividad de conmutación de pico se reduce un 50%. Para buses de datos sin datos correlacionados, se reduce la conmutación promedio hasta un 20%, pero aumenta con el tamaño del bus. Requiere lógica adicional en los extremos del bus.

- **T0:** para codificar buses de direcciones. Requiere una señal INC extra.
![[Pasted image 20231117190612.png|200]]
Si $INC=1$ y las direcciones son consecutivas, no se modifica el bus y el receptor calcula la dirección incrementando la anterior. En caso contrario $INC=0$ y se transmite el nuevo dato en el bus.

- **Beach Solution:** se aplica en buses de direcciones. Las líneas del bus se agrupan en _clusters_ cuyas líneas están altamente correlacionadas y para cada _cluster_ se genera una función de codificación adecuada.
### ♦ Optimización de memoria
Es importante minimizar el número de accesos a la memoria externa y su tamaño:
- Incrementando la densidad de código
- Reduciendo la frecuencia de operaciones E/S
- Mejorando las características de la caché
## 8.7. Nivel de software
### ♦ Apagar el sistema
- **No predictivo:** se apaga después de un intervalo fijo.
- **Predictivo:** se apaga basándose en un historial de actividad.

# ==TEMA 9: Entrefases digitales==

**⚠️ Definición:** Pad/s = Pin/es
## 9.1. El encapsulado
Hay varios requerimientos para un encapsulado:
- **Eléctricos:** Capacitancia, resistencia, inductancia...
- **Interfaz:** Número de pines I/O.
- **Mecánicos:** Protección Die/Bond, compatibilidad con el PCB...
- **Térmicos:** Disipar el calor.
- **Coste:** El mínimo posible.
Hay dos tipos de encapsulado:
- **Wire bonding:** Los pads están alrededor de los bordes del chip. Es un proceso lento pero más barato. Al ser los cables más largos puede que haya capacitancias o inductancias parasitarias.
- **Flip chip:** Los pads están encima del núcleo del CI. Hay muchos pines y es un proceso rápido pero caro. No presenta mucha capacitancia o inductancia parasitaria.
El wire bonding tiene algunos requisitos:
- No cruzar cables.
- Aprovechar el mínimo espacio
- Los cables deben estar en el máximo ángulo y longitud.
El encapsulado puede estar a través de agujeros o en la superficie soldado:
![[Pasted image 20231124180740.png|400]]
Los encapsulados tienen varios efectos:
- **Parástios eléctricos:** Inducciones mutuas, acoplos capacitivos (o capacidades parasitarias), límites de corriente...
- **Alimentaciones:** Dominios, señales I/O.
- **Efectos térmicos:** Resistencias térmicas de las cápsulas, disipación externa...
## 9.2. Niveles de tensión
$V_{IH}$= Tensión de entrada en alto
$V_{IL}$= Tensión de entrada en bajo
$V_{OH}$= Tensión de salida en alto
$V_{OL}$= Tensión de salida en bajo
Si queremos que dos tecnologías sean compatibles sus niveles de tensión y será necesaria una circuitería de _glue logic_ o de desplazamiento de nivel.
## 9.3. Tipos de pines I/O
Para que dos sistemas digitales se comuniquen deben ser compatibles (niveles de tensión, tipo de pines, etc). Hay varios tipos de pines:
- **Digital:** 
	- Entradas (I): Datos, test, configuración...
	- Salidas (O): Capacidad de driving, niveles...
	- Salidas tri-estado: Pueden ponerse en alto (H), bajo (L) o alta impedancia (HI).
	- Bidireccionales (I/O): Entrada (I) y salida tri-estado.
	- Relojes (CLK): Entradas específicas para reloj.
- **Analógico:**
- **Alimentación:** Alimentación ($V_{CC}$) y tierra ($GND$).
### ♦ Circuitos de protección
![[Pasted image 20231124183518.png|500]]
### ♦ Estructura de un pad de entrada
El buffer de entrada aísla el interior de las tensiones de la placa (5V y 3,3V). Después la señal se convierte en valores de tensión usados por la circuitería interna (1,2V).
![[Pasted image 20231124183648.png|500]]
### ♦ Estructura de un pad de salida
![[Pasted image 20231124183832.png|400]]
## 9.3. Problemas/Precauciones
### ♦ Efectos de la carga capacitiva
El tiempo de propagación de cualquier buffer digital depende de la carga capacitiva:
$t_p=t_{pi}+\alpha C_L$ 
Las cargas capacitivas de de los nudos externos son particularmente más altas que los internos.
### ♦ Entradas flotantes
Entradas que tienen un uso esporádico o innecesario en ciertas aplicaciones. Cuando un pin de entrada es atacado por uno o varios buffers tri-estado, es posible que el nudo quede en HI durante mucho tiempo.
### ♦ Rebotes de la tierra/alimentación
Los CMOS se caracterizan por producir picos en alimentación y tierra. Son intensos en los buffers potentes (salidas del CI) o si varios conmutan síncronamente (buses). Es una causa habitual de mal funcionamiento.
### ♦ Conmutación simultánea de salidas
Deben tomarse ciertas precauciones. Los picos producirán _ground/power bounce_. La corriente disponible en los buffers de salida se ve limitada lo que puede aumentar los tiempos de propagación en función del número de salidas que conmutan a la vez.
### ♦ Entradas de pendiente baja
Durante un cierto tiempo los niveles lógicos de las señales estarán indeterminados, produciendo un alto nivel de corriente (corriente de corto circuito) entre alimentación y tierra. Se pueden producir rebotes y pérdida de integridad de las señales de entrada.
### ♦ Conflictos en buses
Pueden haber conflictos al escribir dos datos a la vez en un bus, lo que provoca un "cortocircuito" en el bus. 
### ♦ Transitorios de encendido
Idealmente se debe realizar el encendido (_power-up_) debe hacerse lentamente. Un encendido mal realizado puede llevar a un mal funcionamiento temporal o daño permanente.
### ♦ Propagación de señales
Una pista de interconexión es "corta" o "larga" en función de si el tiempo invertido por la señal en recorrerla es comparable o no al tiempo de retardo.
$v=\frac{d}{t}$
$c=f·\lambda~~~Si~ ~L=\lambda_{min}=\frac{c}{f_{máx}}\rightarrow Línea~de~transmisi\acute{o}n$ 
## 9.4. Entrefases digitales
- Las FPGAs modernas tienen innumerables capacidades de configuración y programación de sus pines de entrada/salida. 
- En general pueden configurarse para soportar un número muy elevado de estándares diferentes, incluyendo los totalmente diferenciales. 
- Cada pin puede configurarse como entrada, salida/tri-state, o bidireccional. 
- Puede configurarse y programarse el tipo de terminación resistiva. 
- Pueden configurarse las salidas como tipo open-drain. 
- Las entradas pueden configurarse para tener circuitos de bus-hold. 
- Las entradas pueden configurarse para tener resistencias de pull-up/down .
- Las entradas pueden tolerar señales de estándares de tensión más alta.
- Las señales de I/O pueden ser asíncronas o síncronas.

<h1 style="background-color:salmon;">TEMA 10: RISC-V (No entra al final)</h1>
## 10.1. Introducción
El origen de RISC se dio al compilar un sistema UNIX en un procesador Motorola 68000, al ver que sólo se usaban un 30% de las instrucciones.
### ♦ Procesadores anteriores a RISC-V
- **RISC-I (1981):** Llamado Gold. Tenía 44500 transistores que implementaban 31 instrucciones. Disponía de 78 registros internos de 32 bits.
- **RISC-II (1983):** Llamado Blue. Tenía 39000 transistores. Disponía de 138 registros de 32 bits.
- **SOAR (1984):** RISC convertido para ejecutar Smalltalk. Tenía 35700 transistores NMOS de 4 micras. Consumía 3W y funcionaba a 2,5MHz.
- **SPUR (1988):** Permite multiprocesador. Tenía 115214 transistores. Fabricado en CMOS de 1.6 micras y opera a 10MHz.
- **DLX (1990):** Es MIPS peor simplificado y con propósito educativo. Basado en DLX de OpenRISC. RISC-V se basa en este.
## 10.2. Características
El juego de instrucciones es **reducido**, más que otros ISAs. Hay una clara separación entre el ISA de usuario y el privilegiado. Evita microarquitectura y características dependientes de varias tecnologías. Su ISA es modular y estable.
Hay 4 versiones básicas de las ISAs: rv32E, rv32I, rv64I, rv128I.
A cada versión se le puede añadir varias extensiones:
- **Integer (I):** Básica. Dispone de 32 registros de propósito general.
- **Embedded (E):** Corresponde a la versión I con 16 registros de propósito general.
- **Multiplication (M):** Extensión con operaciones de multiplicación y división.
- **Atómica (A):** Extensión con instrucciones atómicas.
- **Floating point (F):** Punto flotante.
- **Double (D):** Punto flotante con precisión doble.
- **General Purpose (G):** Propósito general.
![[Pasted image 20231201181656.png]]
La longitud de de instrucción es de 32 bits. Se puede tener un conjunto más amplio de la longitud para soportar diferentes extensiones.
La arquitectura interna sigue el siguiente esquema:
- 32 registros de propósito general de N bits (N = 32, 64, 128)
- La versión rvNE dispone de 16 registros de N bits
- El registro x0 no puede escribirse y almacena el valor 0
- La ALU realiza operaciones de números con signo
- Los registros de estado CSR ocupan el espacio de direcciones de 4KB 
  (0x0000-0x0FFF).
Los nombres de los registros siguen el siguiente convenio:
![[Pasted image 20231201182255.png]]
El bus de direcciones es de 32/64 bits (el de 128 está en desarrollo). Los periféricos están mapeados en memoria. Datos de 8 bits (byte), 16 bits (half-word) y 32 bits (word). El almacenamiento está en Little Endian y alineado en memoria.
## 10.3. ISA (Instruction Set Architecture)
Hay 6 tipos de instrucciones:
![[Pasted image 20231201183414.png]]
- Los campos `opcode`, `funct3` y `funct7` contienen el código de instrucción.
- El campo `rd` es el registro destino, `rs1` y `rs2` son registros fuentes.
- Los campos `imm` son o un dato o una dirección de memoria.
Al añadir instrucciones se añaden:
- 14 para instrucciones privilegiadas.
- 8 para versión M
- 11 para A
- 34 para FDQ
- 46 para C
- 12 para las variaciones 64I/128I
Hay varios niveles:
- **Máquina (M):** Máxima prioridad. Obligatorio.
- **Modo usuario (U) y supervisor (S):** Aplicaciones convencionales y con S.O.
- **Modo hipervisor (H):** Monitorización de máquinas virtuales.
Hay 224 registros que ocupan 4KB:
- U: 77
- S: 11
- H: 11
- M: 125
La mayoría son contadores:
- U: 66
- M: 97
