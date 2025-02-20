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

## <mark style="background: #ADCCFFA6;">3. Cortex M</mark>

## <mark style="background: #ADCCFFA6;">4. Manipulación de bits</mark>

## <mark style="background: #ADCCFFA6;">5. Interrupciones</mark>

