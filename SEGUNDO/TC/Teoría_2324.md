# ==TEMA 1: Diodos semiconductores==

## 1.1. Estructura física del diodo de unión
El diodo es un dispositivo electrónico pasivo formado uniendo dos semiconductores.
### ♦ Conductores, semiconductores y aislantes

| ![[aislante.png]] | ![[semiconductor.png]] | ![[conductor.png]]|
|---|---|---|
| <center>AISLANTE</center> | <center>SEMICONDUCTOR</center> | <center>CONDUCTOR</center> |
Los semiconductores más usado son el Silicio (Si) y el Germanio (Ge) con $1.21 eV$ y $0.785 eV$ respectivamente.
### ♦ Semiconductores intrínsecos
![[covalente.png|500]]
Los órdenes de magnitud son del $V$, $mV$ en el caso de la tensión y $mA$, $\mu A$ en el caso de la corriente.
$Nº n^- = Nºp^+$
### ♦ Semiconductores extrínsecos
Se dopan los semiconductores, es decir, se le añaden impurezas.
- **Semiconductores tipo N:** las impurezas son átomos con 5 electrones en valencia (Grupo V).
	$Nº n^- > Nºp^+$
- **Semiconductores tipo P:** Las impurezas son átomos con 3 electrones en valencia (Grupo III).
	$Nº n^- < Nºp^+$
### ♦ La unión PN
Se comporta como un Diodo. En el caso del Silicio, el Potencial de Unión vale $0.71V$.
![[union_pn.png|500]]

## 1.2. Un diodo como dispositivo de dos terminales
### ♦ Símbolo
![[diodo.png|200]]
### ♦ Características tensión/intensidad y región de operación

![[curva_caracteristica-Ge-Si.png|400]]
1. Región de polarización directa o conducción ($V_D>0$)
2. Región de polarización inversa o corte ($-V_Z<V_D<0$)
3. Región de ruptura ($V_D<0$)

Ecuaciones características del diodo:
$I_D=I_S(e^\frac{V_D}{\eta V_T}-1)$
$V_T=\frac{kT}{q}$ donde $T$ es temperatura ($K$), $q=1.6\times10^{-19}C$ y $k=1.38\times10^{-23}\frac{J}{K}$
$\eta=1 (Ge); 2(Si)$
$I_S\equiv$ intensidad inversa de saturación o corriente de fugas.

## 1.3. Recta de carga y punto de trabajo

![[resistencia_diodo_circuito.png|300]] $¿I_D,V_D?$

$-V_{CC}+I_D\times R+V_D=0\Rightarrow I_D=-\frac{1}{R}V_D+V_{CC}$

![[punto_trabajo_recta_carga.png|300]]

## 1.4. Otros diodos semiconductores
### ♦ Diodo Zener
#### ♦ **Curva**
![[diode-diode11.png|300]]
#### ♦ **Símbolo**
![[Pasted image 20231003143208.png|200]]
### ♦ Diodo Schottky
#### ♦ **Curva**
![[Pasted image 20231003144156.png|400]]
#### ♦ **Descripción gráfica**
![[schottky.png|200]]
#### ♦ **Detalles**
- Fáciles de fabricar
- Muchos $e^{-}\rightarrow$ más velocidad.
### ♦ Diodo PIN
#### ♦ **Descripción gráfica**
![[PIN.png|200]]
#### ♦ **Símbolo**
![[Pasted image 20231003144935.png|200]]
#### ♦ **Detalles**
- Menor $I_S$
- Tensión de ruptura elevada.
### ♦ Diodos emisores de luz (LED) y fotodiodos
#### ♦ Símbolos
| <img src="G:\ETSII\SEGUNDO\1er-cuatri\TC\Apuntes_TC\led.png" width="200"></img> | <img src="G:\ETSII\SEGUNDO\1er-cuatri\TC\Apuntes_TC\fotodiodo.png" width="200"></img> |
| --- | --- |
| LED | Fotodiodo |
#### ♦ Detalles
- Muy bajo coste
- Mucha eficiencia

## 1.5. Modelado de dispositivos
### ♦ Condiciones
1. Abstraer el funcionamiento
2. Rango de validez
3. Complejidad VS Precisión
4. Linealidad
### ♦ Tipos de análisis
| a) Gran señal | b) Pequeña señal |
|---|---|
| ![[Pasted image 20231003203152.png]] | ![[Pasted image 20231003203205.png]] |

## 1.6. Análisis de circuitos en gran señal
### ♦ Diodo de unión
- <u>MODELO IDEAL</u>
![[Pasted image 20231003203627.png]] 
Si $I_D>0\Rightarrow V_D=0\rightarrow$ Diodo ON
Si $V_D\leq0\Rightarrow I_D=0$ Diodo OFF

- <u>MODELO DE PRIMERA APROXIMACIÓN</u>
![[Pasted image 20231003203930.png]]
Si $I_D>0\Rightarrow V_D=V_{ON}$
Si $V_D\leq V_{ON}\Rightarrow I_D=0$

- <u>MODELO DE PRIMERA APROXIMACIÓN</u>
![[Pasted image 20231003204324.png]]
Si $I_D>0\Rightarrow V_D=V_\gamma +I_D·R_{ON}$
Si $V_D\leq V_\gamma\Rightarrow I_D=0$
### ♦ Método de análisis
1) Seleccionar el modelo más adecuado.
2) Suponer un estado de los diodos. Resolver.
3) Comprobar si la suposición es correcta. Si no, de nuevo a 2).
### ♦ Fuente de señal constante
Uno de los diodos presentes en el circuito se encuentran en un único estado que depende del valor constante de la señal de entrada. La manera más sencilla de abordar el análisis es, una vez seleccionado el modelo, suponer un posible estado para cada diodo (ON vs OFF), substituirlo por el modelo equivalente y resolver el circuito.
### ♦ Fuente de tensión variable
Los diodos presentes en el circuito pueden encontrarse en distintas zonas de operación, dependiendo del valor de la señal de entrada. La manera más directa de abordar el análisis es resolver el circuito para cada una de las posibles combinaciones de estado de los diodos, determinando el rango de validez de cada una de ellas como función de $V_I$ o $V_O$ . Este modo de análisis permite además obtener la función de transferencia.

> Siguiendo los siguientes pasos, la resolución de circuitos se vuelve más sencilla:
> 1) Definición de variables: $V$,$I$...
> 2) Leyes de mallas y nudos: $\sum_{i=0}^{n}V_i=0$ y la $I$ entrante en un nudo equivale a la saliente. 
> 3) Leyes constitutivas: $V=I·R$ 

## 1.8. Aplicaciones
### ♦ Rectificación: media onda
Formado por un Diodo y una Resistencia en serie.
![[Pasted image 20231003211522.png]]
La función de transferencia y la entrada/salida del rectificador quedarían así:
![[Pasted image 20231003211541.png]]
### ♦ Rectificación: onda completa
![[Pasted image 20231003211642.png]]
Analizando el circuito queda:
- Semiciclo positivo ($V_S>0$): $D_1$ y $D_2$ ON, $D_3$ y $D_4$ OFF $\Rightarrow V_O = V_S$
- Semiciclo negativo ($V_S<0$): $D_1$ y $D_2$ OFF, $D_3$ y $D_4$ ON $\Rightarrow V_O = -V_S$ 
- $V_S=0: D_1$ OFF, $D_2$ OFF, $D_3$ OFF, $D_4$ OFF $\Rightarrow V_O=0$
Con lo cual la función de transferencia y la entrada/salida quedan así:
![[Pasted image 20231003212030.png]]
### ♦ Filtrado
El tipo más común de filtro usa un único condensador (se conoce como detector de pico).
![[Pasted image 20231003212136.png]]
La onda filtrada queda de la siguiente forma:
![[Pasted image 20231003212154.png]]
### ♦ Regulación
El regulador de tensión es un circuito cuyo propósito es proporcionar una tensión de continua de valor constante a su salida. Tiene las siguientes características básicas:
1.- Rango de variación de la tensión de entrada ($V_I,mín$ y $V_I,m\acute{a}x$). 
2.- Valor de regulación de la tensión de salida ($V_Z$). 
3.- Rango de intensidad capaz de ser suministrada por el regulador ($I_L,mín$ y $I_L,m\acute{a}x$).
El circuito regulador más simple es el siguiente:
![[Pasted image 20231003212745.png|300]]
Y su función de transferencia es:
![[Pasted image 20231003212814.png]]
### ♦ Limitadores (clamping)
Los circuitos limitadores o recortadores se emplean para eliminar en la salida una porción de la señal de entrada que se encuentre por encima, por debajo o situada fuera de dos niveles de referencia, sirviendo también como circuitos de protección contra sobretensiones.
![[Pasted image 20231005174946.png|300]]
Su función de transferencia y la onda limitada quedan de la siguiente forma:
![[Pasted image 20231005175035.png|600]]