# ==TEMA 1: Introducción a los Sistemas Operativos==
## 1.1. Concepto de Sistema Operativo
El Sistema Operativo es una capa de software para interactuar con el computador. En definitiva, es una interfaz que permite al usuario explotar y desarrollar aplicaciones de manera más simple.
### ♦ Estructura del sistema informático:
![[sistema_informatico.png|500]]

## 1.2. Evolución histórica
### ♦ 1ª Generación:
- Lámpara termoiónica (transistores primitivos).
- Tarjetas perforadas.
- Tambor magnético.
Hay dos máquinas relevantes:
- **ENIAC** (1945).
- **EDSAC** (1949).
Las máquinas comerciales de esta época son:
- **Univac I** y **Mark I** (1951).
- **IBM 701** (1952).
- **IBM 702** e **IBM 650** (1953).
#### - Formas de explotación:
- **Acceso sin restricciones:** acceso por turnos, los errores son costosos.
- **Acceso reservado al operador (usuario especializado):**
	Compilar -> Enlazar -> Ejecutar -> Imprimir
- **Procesamiento discontinuo:** El operador organiza el trabajo en tandas:
	Compilar todo -> Enlazar todo -> Ejecutar UNO POR UNO.
### ♦ 2ª Generación:
- **TRADIC** (1955).
- **IBM 7094** (1962).
- **PDP-1** (1961).
#### - Formas de explotación:
- **Ejecución continua de trabajos:** un programa de control automatiza la ejecución. Surge el concepto de **trabajo** (sucesión de operaciones que conforman una unidad de ejecución).
- **Procesamiento por lotes:** se usan máquinas de generación anterior y se ejecuta en las actuales:
	Tarjetas perforadas -> IBM 1401 -> Cintas -> IBM 7094 -> Cintas -> IBM 1401.
- **Acceso interactivo:** aparece el terminal (teclado + pantalla), el usuario escribe órdenes y recibe resultados. Ejecución por lotes.
### ♦ 3ª Generación:
Surge el concepto de multiprogramación. Se atiende de parte del CPU a varios procesos mientras se ejecutan otras tareas. Las dos máquinas relevantes son:
- **IBM 360** (1964).
- **MULTICS** (Multiplexed Information and Computing Service)
	MULTICS -> UNICS -> UNIX
#### - Formas de explotación:
- **Sistemas interactivos en tiempo compartido:** múltiples terminales conectados a un sistema (_mainframe_) que atiende a todos mediante multiprogramación.
- **Procesamiento por lotes con multiprogramación:** evolución del de 2ª Generación. Técnica SPOOL (Simultaneous Peripheral Operation On-Line).
### ♦ 4ª Generación:
Surge el _ethernet_ (1973) y al final, internet (1984). Las máquinas relevantes son:
- **Familia de ordenadores**.
- **Pipelines**.
- **Varios _cores**_.
- **PC (Personal Computer)**.
#### - Formas de explotación:
- **SPOOL, Cintas, etc**.
- **Interfaz (GUI ó UI)**.
- **PCs:** monoprogramados, interactivos, servicio único.
### ♦ 5ª Generación:
Surgen máquinas con cientos de procesadores (Marenostrum V).
#### - Formas de explotación:
- Una mezcla entre 4ª Generación, PCs multiprogramados y GUI.

## 1.3. Tipos de sistemas operativos
- **Servidor:** Windows Server, Linux, Red Hat, CentOS.
- **Multiprocesador:** Amoeba, Solaris-MC, MACH Kernel, Linux.
- **Personal:** Windows 11, macOS, Linux.
- **En tiempo real:** RT-Linux, VxWorks, QNX.
- **Integrados:** Android, iOS, Windows Phone.
- **Web:** ChromeOS, ChromiumOS, WebOS.
- **Nube:** Open Stack, Open Nebula.

<div style="page-break:before"></div>


# ==TEMA 2: Fundamentos==
## 2.1. Conceptos básicos
- **Memoria:** módulos de memoria donde el S.O. escribe y lee datos binarios correspondientes a archivos, multimedia o cualquier tipo de _objeto_ con el que se trabaje.
	![[Memorias.svg|500]]
	- **DDR:** Double Data Rate. Lectura y escritura en flancos de subida y bajada. Se clasifican en los tamaños DIMM, SODIMM, LPDDR.
	- **PROM:** Programmable ROM. Basada en fusibles, por lo que sólo permite escribir una vez. Ej: Arcade.
	- **EPROM:** Erasable Programmable ROM. Basada en luz UV. Ej: BIOS.
	- **EEPROM:** Electrically Erasable Programmable ROM. Memorias flash, se borran en un _flash_ es decir, muy rápido.
- **Bus:** interconexión entre los distintos componentes.
- **DMA (Direct Access Memory):** acceso a memoria sin pasar por el CPU. Ej: GPU, tarjeta de red...

Hay dos arquitecturas principales, la de computadores de escritorio y los portátiles o reducidos, es decir, smartphones/tablets:

| ![[pc-arc.png]] |
|---|
|![[phone-arc.png]]|
### ♦ Interrupciones o excepciones.
Una interrupción es una alteración del flujo de ejecución. Las causas pueden ser: interrupción hardware, excepciones, instrucciones de petición de interrupción (INT, TRAP).
Las interrupciones se tratan de la siguiente forma:
1. Se termina la instrucción actual.
2. Se almacena el estado del CPU en la pila.
3. El CPU entra en modo supervisor.
4. Se determina la dirección del SSI (Subrutina de Servicio de Interrupción).
5. Salta a la dirección del SSI.
6. Se restaura el estado de ejecución.
7. Se continúa la ejecución.
### ♦ Modos de ejecución del procesador.
![[MODOS DE EJECUCIÓN.svg|500]]
Las instrucciones privilegiadas son las que acceden a recursos hardware y software externo. Si en modo usuario se ejecuta una instrucción privilegiada ocurre una excepción o se ignora.
### ♦ Arranque del sistema
El arranque es una secuencia de operaciones que tiene como estado final, el usable.
![[Tipos de cargadores (o loaders).svg|500]]
	- **BIOS:** Basic Input Output System
	- **UEFI:** Unified Extensible Firmware Interface
	- **GRUB:** GNU Grand Unified Boot

![[Sin título-1.png|500]]


## 2.2. Conceptos de Sistemas Operativos
### ♦ Procesos:
Un proceso es un programa en ejecución al que se le asigna un espacio en memoria. El proceso en ejecución se puede dividir en hilos, los cuales comparten espacio en memoria.
### ♦ Llamadas al sistema:
Petición de un proceso al S.O. para obtener un servicio. El conjunto de llamadas al S.O. se agrupan en una API (Application Programming Interface) como POSIX, Win32 o WinFX. La API se implementa con rutinas o interrupciones.
 - **Implementación mediante rutinas:** la dirección de la rutina puede estar en un punto fijo, apuntada por una variable o en un montador de enlaces (CALL SYS).
 - **Implementación mediante interrupciones:** hay un punto de entrada único. Se usa una instrucción `INT n`.
**Modo supervisor $\Rightarrow$ Determinar dir. punto de entrada $\Rightarrow$ S.O. ejecuta en supervisor $\Rightarrow$ Retorno**
### ♦ Usuarios
Personas autorizadas a usar el sistema. Se identifican por UID (User IDentificator) o GID (Group).
### ♦ Archivos:
Conjuntos de información organizados en directorios y subdirectorios. La identificación del dispositivo depende del S.O. (C:/, D:/, /dev/).
### ♦ Intérpretes:
Programa interactivo que lee comandos y los traduce a llamadas del sistema (Power Shell, CMD, Bash...).
### ♦ Interfaz gráfica:
Intérprete gráfico (Explorer, GNOME, Aero...).

## 2.3. Modelos de diseño
### ♦ Modelo monolítico
Todo el sistema operativo se encuentra en un único espacio de memoria, pero dificulta la depuración y mantenimiento. Linux, UNIX, Windows anterior a NT.
- Cualquier rutina llama (CALL) a cualquier otra.
- Cualquier rutina manipula cualquier estructura de datos, lo que es más eficiente pero a su vez más complejo debido a que se tiene que recompilar el kernel cada vez que se añade un nuevo módulo.
La estructura de un sistema monolítico incluye lo siguiente:
- **Dispatcher:** es el punto de entrada al S.O. (llamadas al sistema, instrucciones prohibidas). Se llaman a las rutinas de servicio.
- **SSI (Subrutina de Servicio de Interrupción):** tabla de vectores de interrupción.
### ♦ Modelo en estratos
Se colocan capas de software sobre el hardware para obtener una máquina ampliada. Es más fácil de mantener y depurar debido a que es modular pero hay probabilidad de perder código.

<table border=1>
	<tr>
		<td>Proceso de usuario</td>
	</tr>
	<tr  style="color:tomato;">
		<td>Capa 3: Entrada/Salida</td>
	</tr>
	<tr  style="color:orange;">
		<td>Capa 2: Comunicación proceso-consola</td>
	</tr>
	<tr  style="color:green;">
		<td>Capa 1: Gestión de memoria</td>
	</tr>
	<tr  style="color:blue;">
		<td>Capa 0: Planificación</td>
	</tr>
	<tr>
		<td>Hardware</td>
	</tr>
</table>

Cada capa usa las funciones de su capa inferior. 
- **Capa 3:** 1 Proceso = 1 Entrada/Salida
- **Capa 2:** 1 Proceso = 1 Consola virtual
- **Capa 1:** 1 Proceso = 1 Espacio de memoria
- **Capa 0:** Hardware multiprogramado
### ♦ Modelo micronúcleo
Hacer el núcleo tan simple como sea posible. El núcleo mínimo tiene multiprogramación, comunicación entre procesos y atención a interrupciones. Es fácil de depurar y mantener pero son más lentos y requieren más memoria.

## 2.4. Ejemplos de organización interna
### ♦ Linux
Al igual que otros sistemas UNIX (Bell lab, AT&T, 1970), en el cual está basado, consta de núcleo del sistema (kernel), librerías del sistema y utilidades del sistema. Es difícil mantener ya que muchas personas trabajan en el kernel pero se pueden cargar módulos cuyas dependencias gestiona el kernel.

<table border=1>
	<tr>
		<td style="color:green;">Utilidades del sistema</td>
		<td>Procesos de usuario</td>
	</tr>
	<tr  style="color:blue;">
		<td colspan="2">Bibliotecas del sistema</td>
	</tr>
	<tr  style="color:orange;">
		<td colspan="2">Núcleo</td>
	</tr>
</table>
### ♦ Android
El kernel está basado en Linux, mientras que las librerías están en C/C++ y la máquina virtual en Java. Por último está el Application Framework, que son las aplicaciones "normales", es decir, de usuario.
### ♦ Minix
Es un S.O. micronúcleo puro. En modo supervisor se realizan las tareas de reloj y del sistema mientras que el resto está en modo usuario. El proceso vital de Minix es el proceso `init` el cual se encarga de la inicialización.
### ♦ Windows
Es un S.O. monolítico. En el modo supervisor se controla la interfaz gráfica, el kernel, el gestor de dispositivos, etc. Mientras que en el modo usuario, se controlan los procesos de soporte (Ej: login), los subsistemas de entorno y los procesos de servicio (ServiceControlManager) y aplicaciones de usuario. Los dos últimos llaman a las DLLs (Dynamic Linking Libraries) en vez de al sistema.

# ==TEMA 3: Procesos==
## 3.1. Modelado de procesos
Suponiendo un sistema monoprocesador, el tiempo de procesador se reparte entre todos los procesos. Los procesos alternan periodos de ejecución con periodos de inactividad. Las consecuencias de esto son:
- El tiempo que tarda en ejecutarse un proceso no es el mismo que el consumido de la CPU.
- Las operaciones (de cálculo o bloqueantes) que dependen del tiempo se realizan mediante llamadas al sistema.
## 3.2. Rendimiento de la multiprogramación
Que la CPU esté ocupada el 100% del tiempo depende de la relación $\rho(\omega, n)$ donde $\omega=\frac{T. Bloqueo}{T. Total}$ .
## 3.3. Estado de los procesos
Durante ejecución los procesos alternan periodos de actividad e inactividad: pasan por distintos estados. El diagrama mínimo de estados para cualquier sistema es:
- **Planificación no apropiativa:**
El sistema operativo no interviene en la ejecución del proceso.
```plantuml
Preparado -> Activo : activación
Activo -> Bloqueado : bloqueo
Bloqueado -> Preparado : fin de bloqueo
```

- **Planificación apropiativa:**
El sistema operativo ordena el retorno a preparado.
```plantuml
Preparado -> Activo : activación
Activo -> Bloqueado : bloqueo
Activo -> Preparado : apropiación
Bloqueado -> Preparado : fin de bloqueo
```
### ♦ Diagrama de estados ampliado
```plantuml
En_espera -> Preparado :
Preparado -> Activo : activación
Activo -> Terminado : terminación
Activo -> Bloqueado : bloqueo
Bloqueado -> Preparado : fin de bloqueo
Bloqueado -> Trancisión : fin de bloqueo (recursos no disponibles)
Trancisión -> Preparado : recursos disponibles
```

## 3.4. El PCB (Process Control Block)
El Process Control Block es una estructura de datos relevantes para el S.O. sobre cada proceso. El contenido habitual del PCB en cualquier S.O. es el siguiente:
- **Identificación del proceso (PID).**
- **Estado del procesador.**
- **Información relativa a la planificación:** estado del proceso, tiempo de actividad, prioridad, tipo...
- **Información relativa a la gestión de memoria:** mapa de memoria, páginas asignadas...
- **Información sobre lectura/escritura:** archivos abiertos, dispositivos asignados...
Se puede gestionar el PCB mediante una tabla en la que se fija el tiempo de compilación, el arranque del sistema la instalación del S.O. Dicha tabla se puede indexar por PID. Suelen haber Linked Lists involucradas.

## 3.5 Conmutación de procesos
La conmutación es la secuencia de pasos a llevar a cabo para retirar la CPU a un proceso y asignársela a otro. Las causas que pueden producir la conmutación pueden ser la llamada al sistema bloqueante o la apropiación. La secuencia de conmutación A $\rightarrow$ B tiene varios pasos:
1. La CPU pasa a modo supervisor.
2. Se guarda el estado de la CPU en el PCB del proceso A.
3. Se actualiza el estado de A y se inserta en la lista que toque.
4. Se trata el suceso que provoca la conmutación.
5. Se elige el proceso B aplicando el criterio de planificación.
6. Se actualiza el estado de B a "activo".
7. Se reestablece el estado de la CPU a partir del PCB de B.

## 3.6. Hilos
Algunos lenguajes que incluyen soporte para hilos son ADA o Java. Java lo que hace al manejar hilos es mapear directamente los hilos del kernel.
- **Paralelismo:** ejecución de procesos al mismo tiempo.
- **Concurrencia:** gestión de procesos (no tiene por qué ser al mismo tiempo). Se aprovecha el bloqueo de unos procesos para ejecutar otros.
- **Multiprogramación:** múltiples programas/procesos cargados en memoria.
- **Multiprocesamiento:** varios procesadores físicos (independientemente de los núcleos que tengan). Se ejecutan varios procesos e hilos.
- **Monoprocesamiento:** El único procesador ejecuta el proceso.
- **Hilo:** secuencia de ejecución dentro de un proceso. Comparten todos los recursos con el proceso.
- **Implementación de hilos en el núcleo:**
	- A nivel kernel $\rightarrow$ planificar hilos.
	- A nivel usuario $\rightarrow$ hilo gestionado por una librería que implementa un planificador.
- **Daemon (Disk And Execution MONitor):** programa que se ejecuta en segundo plano.

Los hilos se pueden crear por:
- **Herencia:** 
```Java
class miHilo extends Thread {
	@Override
	public void run() {
		List<Integer> ls = List.of(1,2,3,4,5,6);
		ls.stream().forEach(System.out::println);
	}
}
```
- **Delegación:**
```Java
class miHilo2 implements Runnable {
	
	public void run() {
		List<Integer> ls = List.of(1,2,3,4,5,6);
		ls.stream().forEach(System.out::println);
	}
}
```

# ==TEMA 4: Planificación de procesos==
La planificación se encarga de repartir el tiempo de la CPU. Los objetivos de la planificación son los siguientes:
- **Tiempo de ejecución ($t$):** La suma de todos los instantes de tiempo en los que la CPU ha estado activa. 
- **Tiempo de terminación ($T$):** Tiempo que tarda la CPU en terminar la ejecución de principio a fin.
- **Índice de penalización ($P=\frac{T}{t}$ ):** Relación existente entre el tiempo de terminación ($T$) y el tiempo de ejecución ($t$). Un índice para saber cuánto ha tardado de más respecto a lo planeado.
- **Tiempo de respuesta:** Tiempo que tarda desde que se "dispara" el proceso hasta que se empieza a ejecutar.
- **Tiempo de Sistema de un proceso:** Tiempo que el S.O. emplea trabajando para satisfacer las peticiones.
- **Tiempo de inactividad ($T-t$):** Tiempo que la CPU ha estado ociosa.

## 4.1. Planificación apropiativa y no apropiativa
En la planificación no apropiativa, una vez que se asigna la CPU, sólo se retira en caso de bloqueo. Ofrece la posibilidad de devolución explícita.
En la planificación apropiativa el planificador puede retirar la CPU.
### ♦ Planificación no apropiativa
Una vez asignada CPU sólo se retira en caso de bloqueo. Hay posibilidad de devolución explícita.
- Ventajas:
	- Determinismo.
	- Menor nº de conmutaciones → menor sobrecarga.
- Inconvenientes
	- Proceso largo monopoliza la CPU.
	- Baja fiabilidad. ¿Y si un proceso se cuelga?
### ♦ Planificación apropiativa
El planificador puede retirar la CPU. 
- Ventajas:
	- Indeterminismo.
	- Mayor nº de conmutaciones $\rightarrow$ mayor sobrecarga.
- Inconvenientes:
	- El proceso es más corto y no monopoliza la CPU.
	- Muy fiable.
Es necesaria en:
- Sistemas multiusuario interactivos.
- En determinados tipos de sistemas en tiempo real.

## 4.2. Métodos de planificación
### ♦ Planificación por prioridades
A cada proceso se le asigna una prioridad. El planificador mantiene la lista de procesos preparados ordenada por prioridad. El proceso que está activo será el que tiene mayor prioridad.
- Si el proceso activo se bloquea $\rightarrow$ se activa el primero de la lista. 
- Si el proceso pasa de bloqueado a preparado:
	- Si prioridad > proceso activo $\rightarrow$ apropiación.
	- Si prioridad < proceso activo $\rightarrow$ se inserta en el lugar correspondiente en la lista.
Se debe asignar prioridades siguiendo un criterio:
- Procesos con más tareas $\uparrow$ , procesos más rápidos $\uparrow$ o en los sistemas por lotes los procesos con más E/S $\uparrow$ .
### ♦ Por turno rotatorio
Se establece un tiempo de ejecución máximo (ranura, cuanto, cuantum, quantum, $q$). Se asigna la CPU por turnos de duración máxima = $q$ a los procesos preparados. l tiempo máximo empleado en una rotación es $n · q$  y el tiempo de respuesta $(n-1)<= q$ . Puede ocurrir que los cuantos se acumulen, en tal caso, la parte no consumida se añadirá al siguiente tras terminar bloqueo.
### ♦ Planificación por colas multinivel
El planificador tiene varias colas de procesos preparados. Cada cola se puede planificar por turno rotatorio. Hay posibilidad de diversos criterios.
- **Prioridad:**
![[Pasted image 20231004180208.png|500]]
- **Realimentación:**
![[Pasted image 20231004180237.png|500]]

## 4.3. Planificación en multiprocesadores
### ♦ Tipos de multiprocesadores
- **Acoplamiento débil:** Hay coordinación entre procesadores (operación bloqueante). No da lugar a la planificación.
- **Procesadores especializados:** Su función no es ejecutar código. Colaboran con el CPU principal (ejemplo: GPU computing). No da lugar a la planificación.
- **Acoplamiento fuerte:** Hay coordinación entre procesadores (operación no bloqueante). Posibilita el paralelismo.
- **_Hyperthreading_ y procesadores multicore:** Cuando hablamos de _hyperthreading_, la unidad de control está parcialmente replicada y se permite la ejecución de múltiples hilos. Mientras que, en los procesadores multicore, la unidad de control está replicada y permite el paralelismo de instrucciones. Ambos casos los trata el S.O. como un multiprocesador.
### ♦ Paralelismo
Una aplicación paralela está formada por varios procesos/hilos que pueden colaborar entre sí. Existen varios grados de paralelismo:
- **Procesos independientes:** no hay colaboración entre ellos. Ejemplo: PowerPoint y VirtualBox.
- **Interacción débil:** la colaboración se da de forma esporádica. Ejemplo: Comunicación al principio y fin.
- **Interacción media:** la colaboración se produce de forma frecuente. Ejemplo: Cálculo de matrices, 1 trozo = 1 CPU.
- **Interacción fuerte:** la colaboración se produce muy frecuentemente, cada pocas instrucciones. Ejemplo: Photoshop (1 hilo para detección de contorno, 1 hilo para detección de píxeles).
### ♦ Aspectos de diseño
- **Sistema amo/esclavo:** Pueden haber $n$ procesos activos y la planificación es no concurrente, en resumen, es más simple. Sin embargo, el inconveniente es el posible cuello de botella                   (_bottleneck_) que puede haber si hay muchos procesadores.
- **Multiprocesamiento simétrico:** No hay cuello de botella y es más fiable e incluso tolerante a fallos. Los inconvenientes de este diseño es que todo es concurrente incluso la planificación, por lo que es más complejo.
Hay dos formas de asignar procesos a procesadores:
- **Asignación dinámica:** Facilita repartir la carga de trabajo pero cambiar un proceso de procesador puede implicar penalización si se usa memoria no compartida. También es más complejo.
	![[Pasted image 20231004183603.png|400]]
- **Asignación estática:** Es más simple ya que tiene menos tomas de decisiones y permite implementar planificación por grupos. Hay que tener cuidado con el balance de carga de trabajo ya que si se balancea mal, un procesador puede estar muy cargado y otro sin trabajar.
	![[Pasted image 20231004183750.png|400]]

## 4.4. Planificación de hilos en multiprocesadores
La particularidad de la planificación de hilos es su aplicabilidad a las aplicaciones con interacción fuerte. Se puede hacer por compartición de carga, por planificación de grupos, por asignación de procesadores en exclusividad o por planificación dinámica.
### ♦ **Compartición de carga:**
Se mantiene una lista global de hilos preparados. Al primer procesador que queda libre, se le asigna el primer hilo de la lista.
- **Ventajas:** 
	- Repartición de carga entre procesadores
	- Método de selección inmediato
	- Se puede implementar como sistema simétrico.
- **Inconvenientes:**
	- El siguiente hilo no tiene por que ser del mismo proceso.
	- La cola de hilos es un recurso compartido.
	- No apto para grados de interacción alto, ya que no garantiza el paralelismo real.
### ♦ Planificación por grupos
Indicado para planificaciones con grado de interacción medio y alto. La idea es formar grupos de hilos que se ejecutan en paralelo activándolos y desactivándolos en bloque.
### ♦ Asignación de procesadores en exclusividad
Cuando se crea un hilo se le asigna un procesado, que no se le retirará en ningún momento (ni si quiera en bloqueo). El CPU sólo se libera en la terminación.
### ♦ Planificación dinámica
La asignación/liberación de procesadores las efectúa el propio proceso. Está pensado para lenguajes como Ada y Java. El S.O. dispone de una API para esto.

## 4.5. Ejemplos de planificación
### ♦ Planificación en MINIX
Se realiza mediante colas multinivel:
- TASK_Q: turno rotatorio hasta bloqueo (sin apropiación).
- SERVER_Q: turno rotatorio hasta bloqueo.
- USER_Q: turno rotatorio con ranura de 100ms.
	![[Pasted image 20231004185827.png|500]]
### ♦ Planificación en Windows
Hay 32 colas multinivel o prioridades según Microsoft.
- **Planificación de prioridades variable:**
	- Prioridad inicial = prioridad del proceso [1..15] + prioridad relativa del hilo [-2..2]
	- Priority Boost: si en varias ocasiones el hilo se bloquea antes de terminar el cuanto, se incrementa su prioridad.
	- Límites: el superior = 15, el inferior = [-2..1]
- **Duración del cuanto:**
	- Windows (sobremesa): 2 ticks.
	- Windows (server): 12 ticks.
- **Si hay N procesadores:**
	- Los N-1 hilos de mayor prioridad se asignan a los N-1 procesadores y se activan hasta bloqueo.
	- El CPU restante se comparte entre el resto de hilos.
- **Un hilo puede establecer:**
	- Un procesador _ideal_: el preferido para activarse.
	- Un procesador _last_: el último que debería activarse.
	- Máscara de afinidad: conjunto de procesadores sobre los cuales se puede activar.
### ♦ Planificación en Linux
Hay 140 colas multinivel (también llamadas prioridades). Hay varios tipos de hilo:
- **SCHED_FIFO:** Sólo pierde CPU en bloqueo, si es apropiado por otro SCHED_FIFO de mayor prioridad o voluntariamente con `sched_yield()`. Si hay más de un hilo SCHED_FIFO con la misma prioridad, se activa el que lleva más tiempo esperando.
- **SCHED_RR:** Igual que SCHED_FIFO pero si hay más de un hilo SCHED_RR con la misma prioridad se hace un turno rotatorio.
- **SHED_OTHER:** Turno rotatorio en orden descendente. Prioridad dinámica = prioridad base + bonificación [-5..5]. 
- **Duración del cuanto:** Los cuantos duran entre 5ms y 800ms.
- **Sistemas multiprocesadores:** Cada procesador tiene sus propias colas, se reparte el trabajo mediante dominios de planificación (conjunto de procesadores que se mantienen equilibrados).
La planificación actual de Linux es el algoritmo CFS (Completely Fair Scheduling).
### ♦ Planificación en Android
Una aplicación Android es un conjunto de componentes, los cuáles pueden ser actividades (interactivas) y servicios (no interactivos). Por defecto, todos los componentes se ejecutan en un proceso. Tiene una particularidad y es que es el S.O. el que decide la destrucción del proceso. El ciclo de vida de un proceso en Android es el siguiente:
1. Aborto de proceso vacío (ejecución futura más lenta).
2. Aborto de proceso de fondo (se guarda el estado para la ejecución futura).
3. Aborto de proceso de servicio (MP3 $\rightarrow$ se corta la música).
4. Aborto de proceso visible.

# ==TEMA 5: Control de la concurrencia==
## 5.1. Cooperación entre procesos
Los procesos frecuentemente cooperan en la realización de tareas. Suelen compartir **recursos**$^{1}$. Pueden ocurrir **condiciones de carrera**$^{2}$.
1. Cualquier entidad de software o hardware.
2. Corrupción del estado de un recurso causada por su actualización concurrente por parte de varios procesos (o hilos).
Una **sección crítica** es la porción de código que accede a un recurso compartido. Garantizar que en cada momento se pueda ejecutar máximo una sección crítica asociada a un recurso es la **exclusión mutua**.
## 5.2. Estrategias optimista y pesimista
### ♦ Control pesimista
Garantizar la exclusión mutua en el acceso a los recursos. La ventaja que tiene es que siempre es viable. Sin embargo, como inconvenientes, esta estrategia siempre fuerza tiempos de espera y algunos métodos que la implementan pueden ser muy drásticos.
### ♦ Control optimista
No se fuerza la exclusión mutua, por tanto, pueden darse condiciones de carrera. Si se da una condición de carrera, se detecta y soluciona. La ventaja de esta estrategia es el mayor grado de paralelismo, pero esto será posible o no en función de la naturaleza del recurso. Si hay muchos procesos concurrentes puede aumentar el número de conflictos y el coste de las reparaciones.

## 5.3. Métodos de bajo nivel
### ♦ Enmascarado de interrupciones
Si no hay interrupciones, no hay apropiación. Tiene algunos inconvenientes:
- Mientras las interrupciones estén deshabilitadas, el proceso tiene control absoluto de la máquina, por lo que no se atenderá a dispositivos. También puede que no se refresque la memoria.
- En un sistema monoprocesador es factible pero en uno multiprocesador no ya que las interrupciones se deshabilitan a nivel de procesador.
### ♦ Suspensión de la conmutación
Se pide al planificador que suspenda la conmutación hasta nueva orden, previniendo los problemas con dispositivos y el refresco de memoria. Los inconvenientes son la posibilidad de condiciones de carrera con rutinas de interrupción y que tampoco es adecuado para multiprocesadores.
### ♦ Espera ocupada
Esperar a que algo ocurra realizando continuas comprobaciones. El método habitual es el uso de cerrojos, variables compartidas que indican el estado del recurso (libre u ocupado). Sin embargo, este método desperdicia el tiempo de CPU, por lo que sólo es admisible en procesadores dedicados, sistemas monoprogramados y el kernel.
### ♦ Espera guardada
Cuando la condición se cumple o se deja de cumplir ocurre.

## 5.4. Semáforos
### ♦ Concepto
Un semáforo es un tipo abstracto de datos que encapsula un contador entero y una lista de procesos bloqueados en el semáforo. Tiene dos operaciones:
- **Down:** 
```java
if(s.contador==0) {
	// bloquear proceso en s
} else {
	s.contador--;
}
```
- **Up:**
```java
if(s.listaProcesos.size() > 0) {
	// reanudar uno
} else {
	s.contador++;
}
```
Para garantizar la exclusión mutua, se asocia a un recurso un semáforo `S` con valor inicial 1. Las secciones críticas serían `down(s)` y `up(s)`.
### ♦ Semáforos binarios
Son semáforos con dos estados: `true` o `false`. 
- **Down:** 
```java
if(s.contador==0) {
	// bloquear proceso en s
} else {
	s.estado = cerrado;
}
```
- **Up:**
```java
if(s.listaProcesos.size() > 0) {
	// reanudar uno
} else {
	s.estado = abierto;
}
```
### ♦ Semáforos en la práctica
![[Pasted image 20231011181052.png|650]]
La **granularidad** es el número de recursos controlados por cada semáforo. La granularidad fina es asignar un semáforo por recurso y la gruesa es asignar un semáforo a todos los recursos. La granularidad fina tiene un mayor grado de paralelismo, sin embargo, al haber un mayor número de semáforos hay más posibilidad de interbloqueo. La granularidad gruesa es la inversa de la fina, es decir, tiene menor grado de paralelismo y hay un menor número de semáforos al haber sólo uno.

## 5.5. Mensajes
### ♦ Concepto de mensaje
Es una primitiva de la sincronización y comunicación. Se basa en un tipo de objeto buzón, compartido entre procesos. Los mensajes y envían y leen a y desde los buzones. Estos buzones tienen asociado un identificador.
- **`send(buzon, mensaje):`** Coloca el mensaje en el buzón. El buzón puede tener múltiples mensajes pero tiene una capacidad limitada. Nunca se bloquea al proceso que envía el mensaje al usar `send`, si el buzón se llena se rechazan nuevos mensajes.
- **`receive(buzon, mensaje):`** Retira un mensaje del buzón. Si el buzón está vacío bloquea el proceso hasta que haya un mensaje.

## 5.6. Monitores
### ♦ Concepto de monitor
![[Pasted image 20231011184304.png|550]]
### ♦ Reanudación mediante `signal()`
- `signal()` actúa como `return` 
- Se le da prioridad al proceso reanudado. El proceso que hace `signal()` se detiene hasta que el proceso reactivado deja libre el monitor. Si se hace otro `signal()` $\rightarrow$ lista de procesos pendientes de reactivación.
- Prioridad a proceso que hace `signal()`. El proceso puede hacer varios `signal()` $\rightarrow$ lista de procesos pendientes de reactivación. Puede cambiar la condición `C` de espera.

# ==TEMA 6: Interbloqueo==
## 6.1. Formación del interbloqueo
Espera cíclica entre dos o más procesos. Cada proceso espera para conseguir un recurso asignado a otro. El número de procesos puede ser arbitrario. Al usar un recurso compartido se sigue la secuencia: Solicitar $\rightarrow$ Utilizar $\rightarrow$ Liberar.

## 6.2. Modelado del interbloqueo
Un conjunto $P$ de procesos está interbloqueado si y sólo si cada proceso $P_i\in P$ :
- Está en espera de un evento que sólo puede ser provocado por otro proceso $P_j\in P$ .
- Al menos un proceso $P_k\in P$ espera un evento que sólo puede ser provocado por $P_i$ .
### ♦ Condiciones de Coffman
1. **Exclusión mutua:** para que se produzcan interbloqueos es necesario que los recursos se asignen en exclusividad.
2. **Retención y espera:** para que se produzcan interbloqueos es necesario que un proceso con recursos asignados pueda solicitar nuevos recursos, y esperar manteniendo sus recursos asignados si los nuevos están ocupados.
3. **No apropiación:** un recurso ocupado sólo puede ser liberado por el proceso que lo tiene asignado.
4. **Espera circular**: para que se produzca un interbloqueo es necesario y suficiente que exista un conjunto $\{P\}$ de procesos y un conjunto $\{R\}$ de recursos tal que $P_1$ espera a $R_1$ que está asignado a $P_2$ , el cual espera a $R_2$ que está asignado a $P_3$ , ... , y $P_n$ espera a $R_n$ que está asignado a $P_1$ .
### ♦ Aleatoriedad del interbloqueo
Al aleatorizar el orden de ejecución de los procesos, se evita el interbloqueo ya que se van liberando y solicitando recursos y bloqueando procesos según el orden aleatorio.

## 6.3. Estrategias
1. **Detección y recuperación:** Dejamos que los procesos se ejecuten libremente. Si se interbloquean, se detecta y soluciona (dicha solución es costosa en cuanto a recursos y tiempo de trabajo).
2. **Prevención:** Cuatro condiciones necesarias para la formación de interbloqueo (Coffman) se evitará en todo momento que al menos una se ejecute (lo cual es demasiado severo cotidianamente).
3. **Predicción:** La ocurrencia de un interbloqueo depende del orden de las operaciones de asignación.

## 6.4. Técnicas de detección y recuperación
### ♦ Inspección del grafo
Antes de colocar una arista $\rightarrow$ se recorre el grafo en profundidad y si se vuelve a visitar el nodo de partida, es cíclico. Para detectar esto, se lanza un algoritmo de detección cuando se solicita un recurso ocupado.
### ♦ Matrices binarias de relación
![[Pasted image 20231016161044.png|600]]
- **Propiedades de las matrices binarias:**
	- Sea $R$ la matriz de una relación $R: A\rightarrow B$ 
	- Sea $S$ la matriz de una relación $S:B\rightarrow C$
		$T=R+S$ es la matriz binaria de la relación $R \cup S$ 
	- **Grafo $\rightarrow$ Matriz**
			![[Pasted image 20231016161945.png|700]]
		![[Pasted image 20231016162508.png|500]]
	- **Matriz $\rightarrow$ Grafo**
		- Construir $W$ y $A$ 
		- Construir $T=W\times A$ 
		- Calcular cierre transitivo de $T$
		- Analizar resultado
		![[Pasted image 20231016163100.png]]
### ♦ Detección matricial
Método matricial que trata aquellos casos en los que hay múltiples instancias equivalentes de un recurso. Aísla grupos de procesos que no pueden seguir ejecutándose porque no pueden satisfacer sus peticiones pendientes. Se expresa el estado en notación matricial:
- $A$ (asignaciones): $a_{ij}=$ nº de instancias del recurso $j$ asociado al proceso $i$.
- $S$ (peticiones pendientes): $s_{ij}=$ nº de instancias del recurso $j$ pendientes de asignar al proceso $i$. 
- $D$ (recursos disponibles): $d_i=$ nº de instancias del recurso $i$ disponibles.
- $E$ (recursos existentes): $e_i=$ nº total de instancias del recurso $i$.
### ♦ Eliminación
Abortamos uno de los procesos del ciclo: sus recursos quedan libres y pueden ser reasignados. Lo malo de esto, es que se debe seguir un criterio para saber que proceso abortar, y el trabajo realizado por el proceso en cuestión, se pierde.
### ♦ Puntos de sincronismo
Un punto de sincronismo es el almacenamiento en el disco del estado de un proceso (estado de CPU, espacio de memoria, recursos asignados y su estado...). Como inconvenientes de esto:
- Es requerido espacio en disco para el punto de sincronismo.
- También se necesita tiempo para crear y restaurar el punto.
- No se pueden controlar estados externos al sistema (conexiones a BDD...).
### ♦ Apropiación temporal
Los recursos también forman parte del ciclo. En caso de que se pudiera guardar y recuperar el estado de un recurso...
![[Pasted image 20231018182028.png|300]]

## 6.5. Técnicas de prevención
Coffman propone cuatro condiciones necesarias para la formación de interbloqueo. Las técnicas son:
- **Supresión de la exclusión mutua:** utiliza un patrón cliente-servidor para acceder al recurso. Sólo un proceso (servidor) tiene acceso al recurso y los demás procesos envían peticiones al servidor. Ej. SPOOL.
- **Supresión de retención y espera:** se solicitan todos los recursos simultáneamente, si alguno no está disponible, se espera hasta que estén todos libres.
- **Supresión de la no apropiación:** Si un proceso con recursos asignados solicita un recurso que no está disponible, pierde los recursos asignados (impidiendo también la retención y espera).
- **Supresión de la espera circular:** se establece una relación de orden entre los recursos. Todos los procesos que necesitan más de uno, los solicitan según ese orden. 

## 6.6. Técnicas de predicción
Si se tuviera información sobre como los procesos van a usar los recursos, tal vez se podría forzar unas asignaciones que nunca llevaran a interbloqueo. Se suele usar el algoritmo del Banquero:
- Algoritmo que recibe este nombre por asemejarse a la forma en que los bancos gestionan las cuentas a crédito
- Requiere conocer de antemano las necesidades máximas de recursos por parte de cada proceso.
- Suposición: cuando un proceso ha conseguido todos los recursos que necesita, los libera.
Los inconvenientes de esta técnica son la posible espera innecesaria y que conocer las necesidades de antemano es poco realista.
# ==TEMA 7: Administración de la memoria==
## 7.1. Introducción
Las necesidades más importantes de los procesos son:
- Disponer de un espacio lógico de memoria propio.
- Estar aislados/protegidos respecto a otros procesos.
- Compartir memoria con otros procesos.
- Poder usar tanta memoria como necesiten.
- Poder modificar su mapa de memoria a su conveniencia.
La memoria se gestiona desde tres puntos de vista: procesos, regiones y zonas.
## 7.2. Gestión a nivel de procesos
Hay tres problemas a resolver:
1. **Protección:** Proteger al S.O. ante los procesos y a los procesos entre sí.
2. **Gestión de la memoria:** Identificar direcciones de memoria libres y ocupadas, y de las ocupadas a que proceso pertenecen.
3. **Criterio para asignar memoria a los procesos**
### 2. Gestión de la memoria
La memoria se asigna y libera conforme se inician y acaban los procesos. Se suele gestionar en unidades de asignación (_ticks_) de 16, 32, 64 bytes...
![[Pasted image 20231030154917.png|200]]![[Pasted image 20231030155214.png|200]]![[Pasted image 20231030155232.png|200]]

El principal problema es la fragmentación, que ocurre cuando un proceso en la memoria termina y deja un espacio de memoria libre entre dos ocupados. Como la compactación/desfragmentación es costosa en el tiempo se suelen usar mapas de bits y listas de control.
### ♦ Mapas de bits
![[Pasted image 20231030155448.png|500]]
Se asignan a los ticks ocupados un "1" y a los libres un "0", quedando una cadena de bits que indica los espacios libres en memoria. Hay dos cálculos básicos que se realizan en los mapas de bits:
- Dado un bit cuya posición en el mapa es $p$, pertenece a una dirección $d$ de la siguiente forma:
	$d=p\times T+BASE$ siendo $BASE$ la dirección donde comienza el espacio de memoria.
- Dada una dirección de memoria $d$, la posición $p$ en el mapa del bit que la representa es:
	$p=(d-BASE)/T$ siendo $T$ el tamaño de la unidad de asignación. 
### ♦ Listas de control
![[Pasted image 20231030160001.png|550]]
Por cada bloque libre u ocupado, hay un nodo con la dirección inicial del bloque, el tamaño en _ticks_, el estado (libre u ocupado) y el proceso al que está asignado. La lista está ordenada por dirección de memoria. La compactación en las listas de control es más fácil y asequible.
### ♦ Mapas de bits VS Listas de control
En los mapas de bits los algoritmos son más complicados, la eficiencia es constante y sólo almacena información sobre el estado de cada _tick_. La eficiencia en el tiempo depende de la fragmentación.
Mientras que, en las listas de control, es más fácil buscar un hueco de un tamaño dado y permite gestionar más información. También es más complicado liberar un bloque. A diferencia del mapa, la eficiencia en tiempo y espacio, depende de la fragmentación.
### 3. Criterio de asignación de la memoria a procesos
Se desea un criterio eficiente y que minimice la fragmentación. Los criterios son:
### ♦ Primer ajuste
Se asigna el primer bloque libre con el que se pueda satisfacer la petición. La ventaja es que es eficiente ya que no es necesario recorrer el mapa/lista entera. Pero no tiene en cuenta la fragmentación, por lo que la eficiencia puede disminuir con el tiempo.
### ♦ Siguiente ajuste
Se asigna el primer bloque libre tras el último bloque asignado, **no desde el principio**. Cuando se llega al final se sigue por el principio. Respecto al primer ajuste, se ha solucionado la degradación con el tiempo. El inconveniente es que sigue sin tener en cuenta la fragmentación.
### ♦ Mejor ajuste
Se asigna el bloque más pequeño con el que se pueda satisfacer la petición. Si hubiera alguno del mismo tamaño, se elige. Los inconvenientes son que hay que procesar todo el mapa/lista y que tiende a dejar fragmentos muy pequeños.
### ♦ Peor ajuste
Funciona a la inversa que el mejor ajuste. El objetivo es dejar residuos lo más grandes posibles para aprovechar. Los inconvenientes son que hay que procesar todo el mapa/lista y que algunas peticiones no se pueden satisfacer.
![[Pasted image 20231030161751.png|300]]
Por ejemplo en esta situación, por mejor ajuste si se podría satisfacer, pero por peor ajuste, el proceso más ligero se asignaría al bloque más grande y el proceso más pesado ya no cabe.
## 7.3. Gestión a nivel de regiones
- Cuando se inicia un proceso, se reserva un área.
- STACK / Pila: almacena los datos necesarios para una llamada de función en un programa.
- HEAP / Montón: asignación de memoria dinámica. Se administra por `malloc`,`realloc` y `free`.
- BSS: segmento de datos no inicializados.
- DATA: segmento de datos inicializados.
- Código: instrucciones ejecutables.
 ![[Pasted image 20231030162229.png|200]]
El espacio de memoria se organiza en regiones. Todo proceso tiene al menos Código, STACK y HEAP. Un archivo ejecutable contiene un mapa de memoria y el contenido de las regiones.
## 7.4. Gestión a nivel de zonas
Algunas regiones pueden estar divididas en zonas (bloques de memoria). Las operaciones soportadas son:
- Reservar una zona.
- Liberar una zona previamente reservada.
- Cambiar tamaño de una zona.
Se utilizan los mismos mecanismos de gestión que en la gestión a nivel de proceso (listas y mapas).
## 7.5. Paginación
Todos los procesos tienen direcciones lógicas que se traducen a unas direcciones físicas.
![[Pasted image 20231030163730.png|650]]
El traductor divide las direcciones lógicas en número de página y desplazamiento. Las direcciones lógicas pasan a llamarse páginas y las físicas, marcos.
### ♦ Traducción de direcciones paginadas
La traducción se lleva a cabo mediante una tabla de páginas. Dicha tabla se indexa por número de página y proporciona el número de marco.
![[Pasted image 20231030163924.png|400]]
Para visualizar esto mejor se puede imaginar de la siguiente forma:
- Tanto el conjunto de direcciones lógicas como físicas son folios con números de página (nº página en las lógicas y nº marco en las físicas) y números de línea (desplazamiento).
### ♦ Organización de la tabla de páginas
El tamaño de la tabla es determinado por el tamaño del espacio de direcciones y el tamaño de cada página. Si el espacio de direcciones es de 4GB y el tamaño de la página de 4KB habría 1M de entradas. Hay varias posibilidades:
- **Tabla de páginas en el traductor:** Se aloja en la MMU (Memory Management Unit) de la CPU, el conjunto de registros indexados por nº de página. Es viable si hay pocas páginas (microcontroladores). El inconveniente es que la tabla de páginas se debe sustituir cada vez que hay alguna conmutación o cuando se transfiere el control al S.O.
- **Tabla de páginas en memoria:** La tabla se saca del traductor y se mete en memoria. El traductor sólo contiene la dirección de la primera entrada y el tamaño de la tabla. El inconveniente es la necesidad de realizar dos accesos, uno a la tabla de páginas y otro a la dirección efectiva.
  Se puede solucionar con la memoria asociativa (TLB, Table Look-ahead Buffer), una especie de memoria caché en la que se almacena la página actual.
  - **Tabla de páginas multinivel:** La idea es paginar la tabla de páginas, lo cual facilita la manipulación y almacenamiento de la tabla. Como ventajas: la tabla no tiene por qué ocupar direcciones consecutivas ni estar cargada completamente. El inconveniente es que el tiempo de acceso se multiplica.
	![[Pasted image 20231030165308.png|600]]
- **Tabla de páginas para todo el sistema:** También son multinivel y en este caso el MMU tiene dirección de una tabla que tiene una entrada por proceso y contiene dirección de la tabla de páginas/directorio de dicho proceso. El inconveniente es que el tiempo de acceso se multiplica, pero se puede solucionar indexando la tabla por PID + nº de página.
	![[Pasted image 20231030165449.png]]
- **Traductores sin tabla de páginas:** El hardware no impone la estructura de la tabla y sólo dispone del TLB. SI no se encuentra la página, se lanza una excepción la cual es comunicada al S.O. Entonces el S.O. determina la relación nº página - nº marco y actualiza la memoria asociativa. Como ventajas: es más flexible, el traductor se simplifica y parte del ahorro se puede invertir en aumentar el TLB.
### ♦ Elementos de administración
Listas de marcos asignados a procesos. Hay un nº de marcos fijo. Se crea la tabla con tantas entradas como marcos haya. En cada entrada: siguiente marco en la lista y -1 si es el último. En el PCB de cada proceso, están los índices del primer y último marco asignado al proceso.

# ==TEMA 8: Memoria virtual==
## 8.1. Introducción
**Carga de páginas bajo demanda:** Los procesos comienzan su ejecución sin necesidad de transferir ninguna de sus páginas a memoria. Inicialmente no hay ninguna página "presente". En caso de que una página falle, el núcleo busca la página y se carga en cualquier marco libre.
## 8.2. Elementos de análisis
### ♦ Cadena de referencias
Es una secuencia de páginas referidas por un proceso. Suponemos que un proceso hace los siguientes accesos a memoria (página/desplazamiento):
0/0, 0/2, 0/6, 0/8, 1/0, 1/1, 1/4, 0/8, 2/15, 2/18, 2/15, 2/18, 255/32, 255/36, 2/15...
Se accede a las páginas: 0, 1, 0, 2, 255, 2...
### ♦ Índices de valoración
Sirven para medir las prestaciones de los criterios.
- **Número de fallos de página con $m$ marcos:** $F(m)$
- **Tasa de fallos de página:** $f=\frac{F(m)}{A}$ 
- **Tasa de fallos de página en caliente:** $f_c=\frac{F(m)-m}{A}$
- **Número de fallos de página en caliente:** $F(m)-m$ 
### ♦ Curva paracorde
Representación del número de fallos de páginas para una cadena de referencias, frente al número de marcos usados. Sea $p$ el número de páginas distintas:
![[Pasted image 20231106160628.png|400]]
### ♦ Principio de localidad
- **Localidad espacial:** Si una página es accedida, es muy probable que se acceda eventualmente a una página próxima a ella.
- **Localidad temporal:** Si una página es accedida en un instante de tiempo, es muy probable que en el futuro inmediato vuelva a ser accedida.
## 8.3. Criterios de sustitución de páginas
### ♦ Página óptima (OPT o MIN)
Expulsar la página que va a tardar más tiempo en ser referida. Implica conocimiento del futuro por lo que no es implementable en la práctica. Es útil para comparar con otros criterios. 
![[Pasted image 20231106161638.png|400]] ![[Pasted image 20231106161343.png|400]]
### ♦ Aleatorio
Expulsar páginas al azar. Algoritmo de implementación trivial. Si algún criterio se parece a este es muy malo. 
![[Pasted image 20231106161835.png|400]]
### ♦ Sustitución por orden de carga (FIFO)
Se expulsa la página que lleva más tiempo cargada. La ejecución es secuencial, luego las páginas se cargan, se usan, y se dejan de usar. Es fácil de implementar pero tiene malos resultados:
- Que las páginas "dejen de ser necesarias" no significa que "ya no son necesarias".
- Las más usadas deben permanecer más tiempo en memoria $\rightarrow$ son las atacadas.
- **Anomalía de Belady:** el número de errores de página aumenta respecto al número de marcos.
![[Pasted image 20231106162050.png|400]]
![[Pasted image 20231106162308.png|400]]
### ♦ Página no usada recientemente (NRU)
Expulsar una página que lleve algún tiempo sin usarse. Por principio de localidad, no hay motivos para pensar que vaya a ser necesaria en el futuro inmediato. Se ayuda de los bits R (ó A) y M (o D) de la tabla de páginas, que son página accedida y modificada respectivamente. Se puede implementar de dos formas:
- **Método 1: Segunda oportunidad:** Lista ordenada por orden de carga. Ante una sustitución, la primera candidata es la más antigua, si dicha página tiene R = 1, se pone R = 0 y se pasa al final de la lista. Se puede implementar mediante el **Método del reloj** usando una lista circular.
- **Método 2: Tercera oportunidad (o Multics):** Lista ordenada por orden de carga. La primera candidata es la más antigua. Si dicha página tiene R = 1, se pone R = 0 y se pasa al final de la lista. Si no, si dicha página tiene M = 1 se pone un bit M' = 1 y M = 0 y se pasa al final de la lista. Si no, es página víctima.
### ♦ Página usada menos recientemente (LRU)
Expulsar la página que lleve más tiempo sin usarse. Cuanto más tiempo haga que no se use, por principio de localidad, menos probable es que haga falta en el futuro inmediato.
![[Pasted image 20231106163611.png|400]]
Como la solución mediante listas es inviable en la práctica, se usa la aproximación discreta.
### ♦ Aproximación discreta a LRU
Se realizan periodos de muestreo mediante un temporizador. Para cada página se cuenta el número de periodos consecutivos que no se ha detectado su uso. En cada expiración del temporizador, se comprueban las páginas que tienen activo R. Las páginas que lo tengan activo, se pone a 0 su contador y bit R. Aquellas que no lo tienen activo, se incrementa el contador.
### ♦ Sustitución por envejecimiento
Se asocia cada página a una variable con un número de bits. Se revisan periódicamente los bits R de la tabla de páginas. AL final de cada periodo, se desplazan a la derecha los bits de cada variable, siendo el bit R el que entra como más significativo y se pone a 0 en la tabla. Ante una sustitución se elige la página con menor valor de la variable.
## 8.4. Propiedades de pila
- $M(m,t)$: Conjunto de páginas cargadas en un instante de tiempo $t$ con $m$ marcos.
- **Propiedades de pila:** $M(m,t)\subseteq M(m+1,t)$ 
	- **Consecuencia 1:** Si en un instante $t$,
	  $p\in M(m,t)\rightarrow p\in M(m+1,t)$ 
	- **Consecuencia 2:** si en el instante $t$, $p$ no produce fallo de página con $m$ marcos, tampoco lo  hará con $m+1$.
	- **Consecuencia 3:** el número de fallos de página no aumenta si aumenta el número de marcos: $F(m+k)\leq F(m), \forall k>0$ 
- Si un criterio cumple una propiedad de pila no produce anomalías de Belady.
## 8.5. Memoria virtual y multiprogramación
### ♦ Criterios locales y globales
Al elegir una página víctima, ¿se elige entre las páginas del proceso o las globales del sistema? Las consecuencias de usar un criterio local es que el número de páginas cargadas en memoria permanece constante.
- **Criterios locales:** Como sólo opera con las páginas del proceso es más eficiente, pero no se sabe el número inicial de marcos a asignar.
- **Criterios globales:** La asignación de marcos se adapta dinámicamente a las necesidades de los procesos. Esto favorece a los procesos activos pero los inactivos se rearrancan en caliente.
- **Criterios mixtos:** El algoritmo de sustitución opera sobre un conjunto más pequeño de marcos. El número de marcos se equilibra dinámicamente.
### ♦ Modelo del conjunto de trabajo
**Conjunto de trabajo:** Conjunto de páginas referidas durante dicha fase.
El conjunto de trabajo cambia dinámicamente. Si durante una fase, un proceso tiene más marcos que páginas en el conjunto de trabajo de fase, los sobrantes no se usan. Si tiene menos marcos que páginas, es posible que se de la hiperpaginación.
**Hiperpaginación:** Aumento de frecuencia de fallos de página hasta el punto de que se produce un fallo de página cada pocas instrucciones. Puede ser local o global.
Basándonos en lo anterior se puede usar otro criterio de sustitución:
**WS-Clock:** Algoritmo del reloj, pero con tercera oportunidad en función del conjunto de trabajo observado (conjunto de páginas referidas en el intervalo $[t-\Delta t, t]$).
### ♦ Aplicación de la frecuencia de fallos de página
En lugar de determinar qué páginas forman aparte del conjunto de trabajo, determinamos al menos cuántas son. Se asignan dinámicamente el número suficiente de marcos. Un buen criterio no debería quitar una página del conjunto de trabajo.
Para ajustar el número de marcos, se mide la frecuencia de fallos de página. Se fija un límite superior e inferior. Si se supera el superior se asignan más marcos y si se baja del inferior se retiran marcos.
### ♦ Métodos de reserva
Hay un mejor funcionamiento si hay marcos libres cuando se produce un fallo de página. 
- **Daemon de paginación:** Se ejecuta cuando no hay otra cosa que hacer (o periódicamente) si el número de marcos libres es menor que $K$. Arrebata marcos a otros procesos aplicando un criterio de sustitución y los prepara para el uso por otro proceso.
### ♦ Carga de páginas por anticipado
Cargar las páginas antes de que hagan falta, evitando fallos de página. Por localidad espacial, cuando se transfiere una página se transfieren las adyacentes, probablemente en el mismo acceso.

# ==TEMA 9: Lectura y escritura==
## 9.1. Dispositivos de L/E
Para el S.O. son cajas negras que son capaces de recibir órdenes, transferir información e informar sobre su estado. Se pueden clasificar mediante:
- **Forma de acceso:** Acceso secuencial o directo.
- **Forma de intercambiar información:** Dispositivos de bloque y de caracteres.
## 9.2. Controladores de dispositivos
### ♦ Controladores y adaptadores
El controlador es la interfaz electrónica del dispositivo el cual gobierna el funcionamiento de este y recibe órdenes del procesador. En los sistemas más simples son específicos. La alternativa a lo específico del controlador, es la interfaz estándar. Esta interfaz normaliza las características eléctricas y mecánicas y los protocolos.
### ♦ Estructura lógica de un controlador
Tiene una arquitectura en tres niveles:
- Interfaz al bus (hardware).
- Registros (software).
- Interfaz al dispositivo (hardware).
Para acceder a los registros hay dos tipos:
- Mapeados en el espacio de L/E, que se accede mediante instrucciones `IN`, `OUT`, etc. Algunos procesadores disponen de espacio L/E separado.
- Mapeados en memoria, que suelen ser microcontroladores.
### ♦ Acceso directo a memoria (DMA)
Dispositivo capaz de transferir datos entre el dispositivo y la memoria. Lo programa el procesador con la dirección del registro del dispositivo, la dirección inicial del bloque y el nº de bytes a transferir. Cuando se termina: interrupción.
![[Pasted image 20231113160109.png|500]]
## 9.3. Modos de realizar las operaciones de L/E
### ♦ Mediante sondeo
La CPU programa las operaciones y monitoriza el estado del dispositivo mediante sondeo. Si se gestionan varios dispositivos se puede realizar por:
- **Prioridad uniforme:** Se atienden **sí o sí** todos los dispositivos secuencialmente.
- **Prioridad escalonada:** Con que haya un dispositivo preparado, se atiende y se vuelve a buscar dispositivos preparados.
Comparación en base a tiempo de respuesta y ancho de banda (BW):
- Suponiendo $n$ dispositivos con $T\equiv$ tiempo de transferencia para cualquiera de ellos.
	![[Pasted image 20231113161538.png|450]]
Los inconvenientes son la espera ocupada y la difícil intercalación de gestión de dispositivos con otras actividades.
### ♦ Mediante interrupciones
Los dispositivos suelen dispone de alguna señal de interrupción. Esta señal de interrupción se puede conectar a la entrada de interrupción de CPU. Los dispositivos se pueden configurar para que generen interrupción cuando requieran atención. La ventaja de esto es que la gestión de dispositivos se desacopla del resto de actividades.
Si se gestionan múltiples dispositivos:
- Una interrupción por dispositivo.
- Una interrupción para todos los dispositivos.
- Agrupar dispositivos: una interrupción por grupo.
Se suelen usar Controladores Programables de Interrupciones (PIC). El PIC es un dispositivo normal y corriente que dispone de M entradas de interrupción y una salida. Este informa de que interrupción se ha producido.
### ♦ Realizar operaciones L/E por sondeo o interrupciones
Si el dispositivo es asíncrono (teclado, ratón...) se hará por interrupciones. Si es síncrono y la CPU es mucho más rápida, por interrupciones pero si el dispositivo es del mismo orden de velocidad que la CPU, es admisible el sondeo.
## 9.4. Principios de diseño del software de L/E
a) Diseño por estratos: software dividido en capas que se intercomuniquen.
b) Garantizar la independencia de los programas respecto a dispositivos.
c) Criterio uniforme de denominación de dispositivos y archivos.
d) Tratar los errores lo más próximo posible a su origen: tratar de no propagar el error.
e) Homogeneizar transferencias de bloque y de  caracteres.
f) Igualar transmisiones para el usuario.
g) Considerar la naturaleza compartible del dispositivo.
## 9.5. Diseño modular del sistema de L/E
Se tienen 4 niveles software y 2 hardware:
![[Pasted image 20231113163356.png|500]]

# ==TEMA 10: Gestión de dispositivos==
## 10.1. Discos magnéticos
### ♦ Hardware de los discos

![[Pasted image 20231115175424.png|450]]
![[Pasted image 20231115181022.png]]
Las pistas se dividen en sectores, pero en los discos antiguos, todas las pistas tienen el mismo número de sectores, mientras que en los ZBR, las exteriores tienen más que las interiores, lo que los dota de más capacidad y velocidad r/w.
![[Pasted image 20231115181726.png]]
Los sectores transfieren los bits a velocidad de giro constante. La estructura interna de un sector se compone de:
- Sincronización
- Información de identificación
- Datos
- Código de Corrección de Errores (CRC)
- Espaciadores
- Información para el servomotor (motor de las cabezas lectoras)
Los modos de direccionamiento son LBA (Linear Block Address), que es un array, y CHS (Cylinder Head Sector), que es una matriz de tres dimensiones. 
Características de las cabezas lectoras:
- Muy alta densidad de grabación
- Gira permanentemente a una velocidad típica (3600, 7200, 9600, 12000, 15000 rpm.
### ♦ Prestaciones
Para optimizar los tiempos de transferencia en el disco se pueden realizar varias optimizaciones:
- **Tiempo de búsqueda:** Si el gestor de disco tiene la lista de búsquedas pendientes, se ordena de forma inteligente.
- **Demora de rotación:** Transferir los sectores en el mismo orden en que pasan, incluso por adelantado.
- **Tiempo de transferencia:** Reservar bloques en caché o el buffer.
### ♦ Tratamiento de errores
Hay cinco tipos de errores: 
- **De programación:** El gestor recibe una petición sin sentido del nivel superior. Se soluciona rechazando la petición.
- **De controlador:** El disco no responde porque el controlador está basado en un microcontrolador y se puede colgar. Se soluciona reiniciando ya que la línea de reset está mapeada en un bit de registro de comandos.
- **Transitorios de datos:** Al leer un sector y comprobar el CRC, se comprueba que los datos no son correctos. Las posibles causas pueden ser varias:
	- **Mecánicas:** Daño físico del disco.
	- **Electrónicas:** Interferencias con otros dispositivos.
	- **Error permanente de datos**: Se soluciona intentando de nuevo la operación, si tras un número de reintentos no hay éxito, se informa a nivel superior.
  - **Errores permanentes de datos:** El disco tiene sectores físicamente dañados y no utilizables debido a defectos de fabricación, aterrizaje de cabezas lectoras, desgaste...
    Hay que evitar usar ese sector. Hay varias soluciones dependiendo del administrador de archivos:
       - **MINIX:** Formar un archivo especial que contiene todos los sectores defectuosos.
	- **NTFS:** Crear un archivo del sistema con los números de bloques defectuosos.
	- **FAT:** marcar sectores como defectuosos en la FAT.
	- **Pista/cilindro con sectores de reserva:** se sustituye por software o hardware los sectores defectuosos con los de reserva de la pista/cilindro.
- **De localización:** Tras mover la cabeza al cilindro, se comprueba que no está donde se esperaba o que no se encuentra un sector $i$ pero si el $i-1$ e $i+1$. Para la primera se recalibran las cabezas y para la segunda se reintenta la operación.
## 10.2. SSD
### ♦ Hardware de los SSD

![[Pasted image 20231115185018.png|450]]
Para leer y escribir en el disco:
- Se escribe el sector en el buffer.
- La interfaz de cada celda flash: 8/16 bits.
- Un sector se escribe en paralelo disperso en varias celdas.
- Para escribir la celda debe estar vacía (si no, hay que borrarla primero).
El SSD tiene varias particularidades:
- **Amplificación de la escritura:** El tamaño típico del sector son 4KB. Para escribir hay que borrar previamente unidades de borrado de un tamaño típico 128KB-256KB. El borrado de un sector puede afectar a otros.
  - **Número finito de borrados:** Las celdas soportan un número finito de borrados (10K, 100K...).
### ♦ SSD y sistemas de archivos
Las optimizaciones que se le pueden realizar a los SSD son:
- **Wear Leveling:**
	- Dinámico: Se establece una relación entre los números lógicos y físicos del sector en el momento de la escritura. Los archivos que no se reescriben, no provocan desgaste en sus sectores.
	- Estático: Igual que el dinámico pero moviendo los sectores que no se reescriben.
- **Comando TRIM :** Si los sectores grabados en una unidad de borrado y ya no están en uso, no es necesario leer y regrabar, reduciendo la amplificación de la escritura. Lo malo es que esta información es desconocida por la unidad. Mediante el comando TRIM se informa a la unidad que sectores dejan de estar en uso.
## 10.3. RAID
**RAID:** Redundant Array of Independent Disks
La idea es usar varios discos en paralelo para simular un disco con mejor rendimiento y fiabilidad. Se puede implementar por software y por hardware. Hay varios tipos de RAID:
### ♦ RAID 0
N discos simulan un disco de mayor capacidad. La información se divide en bandas (strips) de N sectores consecutivos. Cada sector de una banda se almacena en un disco y se transfieren en paralelo. Mejora el tamaño y la tasa de transferencia, pero es menos fiable.
### ♦ RAID 1
N discos idénticos en espejo. La información se escribe simultáneamente en todos los discos. Mejora la tasa de transferencia **sólo en lectura**. El tamaño es el mismo en todos. Es más fiable al haber "backups".
### ♦ RAID 4
N+1 discos simulan un disco de mayor capacidad. La información se divide en bandas como en RAID 0 + un disco de paridad. Se escribe en paralelo, pero el disco de paridad se convierte en un cuello de botella. Mejora el tamaño y la fiabilidad. Esta última mejora ya que si se detecta que un sólo disco falla, se puede recuperar mediante la paridad.
### ♦ RAID 5
RAID 4 pero la paridad se reparte por turnos entre los demás discos, quitando el cuello de botella.
### ♦ RAID 6
RAID 5 pero añade otro bloque más de "paridad". Utiliza un código Reed-Solomon para corregir errores, por lo que es más tolerante a fallos. Se pueden combinar niveles de RAID.
## 10.4. Reloj en tiempo real
### ♦ Hardware
Dispositivo temporizador más simple: Temporizador de Intervalo Programable (PIT). Consta de un registro de valor inicial, un contador autodecrementable con un comparador, un oscilador y circuitería de control.
### ♦ Actividades relacionadas con el reloj
 - **Fecha y hora:** Contador en memoria y cada tick lo incrementa. Los posibles formatos de representación son:
	 - Nº de ticks desde fecha y hora predeterminada.
	 - Nº de segundos desde fecha y hora predeterminada + Nº de ticks desde el último segundo.
	 - Nº de días desde fecha y hora predeterminada + hora en ticks.
- **Cuanto de tiempo:** Programar un tick $t << q$ tal que $q=N\times{t}$. Se cuenta el número de ticks en cada interrupción. Cuando se alcancen N ticks el cuanto expira.
- **Temporizadores y alarmas:** 
	- **Temporizador:** Artefacto software que hace algo dentro de $t$ unidades de tiempo.
	- **Alarma:** Artefacto software que hace algo a las $h$ horas del día $d$.
  Cuando un temporizador o alarma expira se puede enviar una señal, enviar mensajes, abrir un semáforo, ejecutar un proceso, etc. Se pueden implementar de dos formas:
	  - **Forma 1:** Tabla de alarmas/temporizadores. Un contador por cada proceso (nº de ticks hasta expiración). Se decrementan con cada tick y al llegar a 0 expira.
	  - **Forma 2:** Lista de horas de expiración. Expresar temporizadores/alarmas como horas absolutas. Crear lista ordenada por hora de expiración. Por cada tick se compara la hora actual con la(s) primera(s).
	  - **Forma 3:** Lista diferencial. Lista ordenada cronológicamente de manera incremental. El primer nodo son los ticks restantes hasta la próxima expiración. El nodo $i$ son los ticks desde la anterior expiración.
## 10.5. Terminales
### ♦ Externos
Sistema autónomo conectado a través de algún tipo de sistema de comunicaciones
### ♦ Internos
Pantalla, teclado y ratón.
![[Pasted image 20231127162130.png|400]]
- **Pantalla:** El adaptador _barre_ un área de memoria que representa la pantalla (memoria de vídeo). La memoria de vídeo puede estar en el adaptador o en la memoria principal. Se puede interpretar la memoria de vídeo en modo texto o gráfico.
  ![[Pasted image 20231127162401.png|500]]
  - **Teclado:** Dispone de un microcontrolador que explora el array de teclas n veces por segundo. Al pulsar y soltar una tecla se generan sus respectivos eventos. Se captura el _scancode_ de la tecla que está en el registro de estado del teclado.
  - **Ratón:** Dispone de un microcontrolador que genera un evento por pulsar y liberar una tecla y por desplazamiento. Por cada evento, se proporciona un vector de desplazamiento y el estado de las teclas. Hay varias tecnologías:
	  - **Mecánico:** Una bola con dos varillas para calcular el vector.
	  - **Óptico:** Una cámara que va capturando la imagen en cada movimiento y al superponerla calculan el vector como la diferencia de las dos imágenes.
	  - **Láser:** Un láser que funciona sobre una alfombrilla con una cuadrícula.
## 10.6. Gestión de la energía
El consumo del procesador se debe a las capacidades parásitas de las puertas lógicas. Para reducir el consumo se puede reducir la tensión, bajar la frecuencia o ponerlo en modo SLEEP.
El consumo de los dispositivos se debe a su circuitería. Además, los componentes que mas suelen gastar son los motores de los discos magnéticos o los paneles LCD.
### ♦ Especificación ACPI
**ACPI:** Advanced Configuration and Power Interface. 
Pretende estandarizar la administración de energía por parte del S.O.
- **Procesador:** Tiene varios estados: C0 (funcionamiento normal), C1 (HALT: no ejecuta instrucciones pero puede reanudarse casi inmediatamente), C2 (STOP-CLOCK: no ejecuta instrucciones, mantiene el estado pero puede tardar en reanudarse), C3 (como C2, pero no es necesario que las cachés se mantengan coherentes).
- **Dispositivos:** Tiene varios estados: D0 (operativo), D1 y D2 (intermedios) y D3 (OFF, Hot si tiene alimentación y Cold si no).
- **Estados globales:** 
  - G0: 
    WORKING -> funcionando 
    AWAY MODE -> pantalla apagada pero funciona el sistema
  - G1: 
    SLEEPING:
	    S1: CPU OFF, RAM ON, DISP OFF
	    S2: CPU OFF
	    S3: STANDBY -> SOLO RAM ON
	    S4: HIBERNATION ->RAM OFF (volcada en disco antes).
- G2: 
	SOFT OFF -> apagado pero se puede arrancar con el botón de encendido
- G3:
	MECHANICAL OFF -> apagado y solo se enciende con el interruptor mecánico
# ==TEMA 11: Administración de Archivos==
## 11.1. Visión general
La función del administrador de archivos es implementar archivos y directorios sobre la estructura plana de array de sectores de las unidades. Este recibe peticiones de los procesos y envía peticiones a los gestores de dispositivos. Entre los servicios que puede realizar se encuentran:
- **Servicios sobre archivos completos:** crear, destruir, copiar, renombrar, etc.
- **Servicios sobre contenido de archivos:** leer, añadir, modificar, modificar, truncar, etc.
- **Servicios sobre sistema de archivos:** crear o borrar directorios, montar dispositivos, crear sistema de archivos, etc.
- **Otros servicios:** protección, encriptado, compartición de archivos, control de concurrencia, etc.
### ♦ Particionado de unidades físicas
Una unidad física se puede dividir en unidades lógicas:
- **MBR Partition Table:** En placas compatibles con BIOS. Permiten hasta cuatro particiones primarias o hasta tres primarias y una extendida (almacenamiento). El límite de tamaño para una partición es de 2TB.
- **GUID Partition Table:** Hasta 128 particiones. El límite de tamaño para una partición es de 2ZiB ($2^{70}$ bytes).
### ♦ Concepto de volumen
Es una unidad lógica (contiene un sistema de archivos). En UNIX se monta sobre un único árbol de directorios. En Windows se le asignan letras. La estructura típica de un volumen es:
- Cargador software.
- Estructura de datos de gestión del espacio libre.
- Estructura de datos de gestión de bloques defectuosos.
- Estructuras de gestión de bloques asignados.
- Directorio raíz.
### ♦ Gestión del espacio libre
El espacio de la unidad se asigna en grupos de sectores consecutivos (clusters), siendo más eficiente al manejar menos bloques.
### ♦ Gestión de bloques defectuosos
Las unidades siempre tienen sectores no utilizables. Dichos sectores no se pueden asignar a archivos. Las soluciones habituales son:
- Asignarlos a un archivo del sistema que nunca se usará.
- Crear un archivo que contiene la lista de bloques no utilizables.
- Integración con la gestión del espacio ocupado.
### ♦ Gestión del espacio ocupado
¿Qué archivos hay en el volumen? Dado un archivo, ¿qué bloques lo forman? Acceso directo: si quiero transferir la posición X de un archivo, ¿qué bloque de disco he de transferir?
### ♦ Directorio
Estructura de datos del sistema de archivos que contiene información sobre archivos contenidos en el mismo. Es normalmente jerárquico. Todo volumen contiene un directorio raíz. La información habitual es el nombre, fecha de creación, permisos, etc.
### ♦ Archivo
Secuencias de bytes que se pueden leer byte a byte o bloque a bloque. Si el dispositivo es direccionable permite acceso directo.
## 11.2. Sistema de archivos FAT
![[Pasted image 20231129181311.png|500]]
- **Sector 0 (Arranque):** Contiene MBR en unidades particionables. En una partición, o unidad no particionable, puede contener el cargador software del S.O.
- **Sector 1 (FS Information):** Datos útiles cacheados para acelerar algunas operaciones. Número de bloques libres y número del último bloque asignado.
- **FAT:** Gestiona el estado libre y ocupado del volumen. Es una tabla con una entrada por bloque. Normalmente se replica en dos copias.
![[Pasted image 20231129182407.png|400]]
### ♦ Acceso directo a una posición de un archivo
1. Determinar el bloque lógico dentro del archivo
2. Iterar FAT para encontrar el bloque físico
3. Determinar sector/es del bloque físico
**Ejemplo:** Transferir posición 14000 de F2
- Tamaño de bloque: 4KB (dos sectores de 2KB)
- S = primer sector de zona clusterizada
	Bloque lógico: $14000/4096=3,x$
	Bloque físico: $5$
	Sectores a transferir: $(S + 2*5) + (S+2*5+1)$
## 11.3. Sistema de archivos ext2
![[Pasted image 20231129183149.png]]
### ♦ Superbloque: 
- Número mágico (EF53) -> ID para identificar que es un superbloque
- Versión
- Cuenta de montajes y número máximo de montajes
- Tamaño de bloque, nº de bloques libres, nº de nodos-i libres, etc
### ♦ Descriptores:
Contiene las direcciones de comienzo de:
- Mapa de bits de bloques
- Mapa de bits de nodos-i
- Tabla de nodos-i
- Bloques del grupo
### ♦ Tabla de nodos-i
![[Pasted image 20231129183643.png]]
Los directorios en ext2 son archivos que contienen listas enlazadas de  registros de tamaño variable. Cada registro es un nombre de archivo y su contenido es:
![[Pasted image 20231129183933.png]]
### ♦ Gestión del espacio ocupado
![[Pasted image 20231129184117.png]]
Si ocupa más de 48KB (12 x 4KB), se pasa a usar el indirecto simple, doble o triple.
- El indirecto simple direcciona a una tabla cuyos registros son bloques.
- El indirecto doble direcciona a una tabla cuyos registros son indirectos simples.
- El indirecto triple direcciona a una tabla cuyos registros son indirectos dobles.
### ♦ Acceso directo
1. Determinar bloque lógico dentro del archivo
2. Encontrar la entrada correspondiente a dicho bloque lógico en nodo-i o bloque indirecto correspondiente (bloque físico)
3. Determinar sector/es físicos
**Ejemplo:** Transferir posición 58000 de un archivo
- Tamaño de bloque: 4KB (dos sectores de 2KB)
- Número de bits referencia a bloque: 32 (un bloque indirecto tiene 1024 referencias)
- S = Primer sector de zona de bloques del grupo
	Bloque lógico: 58000/4096=14.x
	Bloque físico: B14
	Sectores a transferir: S+2xB14+S+2xB14+1
### ♦ Enlace directo
Un mismo archivo puede aparecer múltiples veces en la estructura de archivos. La llamada link crea un enlace sobre un archivo y la unlink desenlaza. Los dos archivos tienen que estar en el mismo volumen, si no, existe el enlace simbólico. Si un fichero es un enlace simbólico a otro en otro volumen, se hace referencia en el primero pero no se copia. El enlace directo puede ser más parecido a los accesos directos de Windows.
## 11.4. Coherencia del sistema de archivos
La estructura del sistema de archivos puede quedar de forma inconsistente por fallos de alimentación o software o incluso malware. Pueden ocurrir varios errores:
### ♦ Contador de enlaces incorrecto
- **Descripción:** el contador de enlaces de un archivo en nodo-i no se corresponde con el nº real de veces que el archivo aparece en el sistema de archivos.
- **Cómo se detecta:** explorando el sistema de archivos y contando cuantas veces se referencia a cada nodo-i.
- **Solución:** sustituir valor erróneo en nodo-i por valor calculado en la exploración.
- **Posibles causas:** fallo de alimentación, cuelgue...
### ♦ Autorizaciones sin sentido
- **Descripción:** se detectan archivos con permisos absurdos.
- **Cómo se detecta:** explorando el sistema de archivos y comprobando permisos.
- **Solución:** cambiar permisos.
- **Posibles causas:** errores de programación, corrupción de la estructura de datos que implementa los permisos.
### ♦ Estados incoherentes de bloques
_Cada bloque debe estar o libre o asignado o defectuoso._
- **Descripción:** hay bloques que están o en ningún estado o en más de uno o asignados a más de un archivo.
- **Cómo se detecta:** explorando estructuras de gestión de espacio libre y/u ocupado.
- **Posibles causas:** fallo de implementación, cuelgue...
Los posibles estados incoherentes son:
- **Bloques perdidos:** bloques que no forman parte de ningún archivo pero no son libres. Se soluciona o convirtiendo los archivos y dando a elegir al usuario o marcando como libre.
- **Bloques en más de un estado:** bloques en más de un estado. Si está libre y en uso se marca como asignado (en FAT posiblemente se trunque), si está defectuoso y en uso se trunca el archivo y si está defectuoso y libre se comprueba si está defectuoso y se actúa en consecuncia.
- **Bloques asignados más de una vez:** se detecta explorando los bloques asignados y contando el número de veces que aparecen en un archivo. Se soluciona o truncando o asignando a uno u otro archivo.
## 11.5. Reserva de bloques
El acceso a los archivos también cumple el principio de localidad. Se puede mantener en memoria copia de los bloques que se están usando actualmente. Ante la necesidad de leer un bloque:
- Se comprueba si está en el buffer del disco.
- Si está, nos ahorramos lectura.
- Si no está, se lee y se carga en un bloque libre.
- Si la reserva se llena: se reemplaza con NRU o LRU.
### ♦ Formas de usar la reserva
- **Escritura directa:** Cuando se actualiza un bloque automáticamente se actualiza en el disco. El disco siempre está actualizado, pero la escritura no se beneficia de la reserva.
- **Escritura diferida:** Cuando se actualiza un bloque, sólo se actualiza su copia en memoria. El disco se actualiza al cerrar el archivo, al reemplazar el bloque en memoria, cuando el proceso lo solicita o al retirar el dispositivo/medio. Aquí la escritura si se beneficia de la reserva, pero no se pueden extraer discos ni apagar el ordenador arbitrariamente.
## 11.6. Concurrencia con archivos
Se da en cualquier sistema multiprogramado. El archivo encapsula servicios para controlar la concurrencia. Aperturas exclusiva y compartidaa, bloqueo y desbloqueo... Si se abre un archivo en modo incompatible/restringido o se intenta bloquear parte de un archivo bloqueado: error.

## 11.7. Transacciones
**Transacción:** conjunto de actualizaciones que se realizan todas o ninguna. Las primitivas son BEGIN_TRANSACTION, COMMIT y ROLLBACK.
### ♦ Propiedades de las transacciones
- **Serialidad:** El resultado final de realizar varias transacciones en paralelo debe coincidir con el resultado de ejecutarlas secuencialmente siguiendo algún orden.
- **Indivisibilidad:** Si se completa la transacción se realizan todos los cambios, si no, no se realiza ninguno. Como consecuencia de esto, otros procesos no pueden ver estados intermedios y **sólo ven el inicial** hasta que se completa la transacción.
- **Permanencia:** Si se acepta la primitiva END_TRANSACTION ,se garantiza que los cambios se hacen permanentes. Los fallos después del END_TRANSACTION no deben afectar.
### ♦ Realización de las transacciones
Hay dos formas de realizar transacciones:
- **Copia de información (ext2):** Se mantiene la información original y se hace una copia local para la transacción. Si se valida la transacción, se sustituye original por copia. Si no, se borra la copia.
  En ext2 se copian los bloques modificados de un archivo a bloques llamados bloques sombra. Al modificar, se eliminan los bloques originales si la modificación es exitosa.
- **Escritura anticipada:** Al iniciar la transacción se crea un archivo de log. ANTES de cada actualización se escribe en el log el valor original que se va a modificar y el valor por el que sustituye. Si la transacción se valida se borra el log. Si se cancela, se recorre el log en sentido inverso para deshacer todos los cambios.
# ==TEMA 12: Seguridad y protección del sistema de archivos==
## 12.1. Conceptos generales de seguridad
- **Seguridad del sistema:** garantizar que no se pueda  hacer uso indebido (uso sin autorización, robo de información, impedir el uso a otros usuarios).
- **Vulnerabilidad:** característica del sistema que es aprovechada para hacer un uso ilegítimo del mismo.
- **Exploit:** Algoritmo o procedimiento a seguir para usar una vulnerabilidad.
- **Ataque:** Acción ilícita sobre el sistema.
### ♦ Principios para diseñar sistemas seguros
1. Suponer que el diseño es público
2. Resultado por defecto de una comprobación: denegar
3. Comprobar permisos en vigor del usuario
4. Dar a cada proceso el mínimo privilegio para llevar a cabo su función
5. Establecer métodos sencillos y uniformes
## 12.2. Ataques contra la seguridad
Tipos de malware:
- Troyanos
- Bombas lógicas
- Virus
- Gusanos
Clasificación respecto a la actividad que llevan a cabo:
- DoS
- Ransomware
- Backdoors
- Spyware
### ♦ Troyano
Programa con apariencia inofensiva que contiene código malintencionado. Suelen ejecutarse por inocencia del usuario o automatizaciones (script de correo, "." al principio del PATH, phishing).
### ♦ Bombas lógicas
Código insertado en un programa que realiza alguna actividad ilícita. La bomba pasa por un periodo de inactividad hasta que se detona (actividad ilícita al detectar una determinada condición: fecha, introducir clave, etc)
### ♦ Virus
Fragmento de código que debe cumplir dos requisitos: se reproduce a sí mismo y lo hace infectando código. Puede acceder a cualquier recurso que pueda acceder el código infectado. El funcionamiento habitual es:
1. Desarrollo del virus
2. Utilizar un dropper para infectar máquina o primer ejecutable
3. Al ejecutarse el código infectado, el virus se ejecuta:
	- Se instala en memoria.
	- Infecta otros programas.
4. En algún momento lanza código útil (ilícito).
Tipos de virus:
- **De ejecutable (parásitos):** se anexan a los ejecutables. Constan de cargador y código útil.
- **Residente en memoria:** el virus permanece en memoria cuando acaba el programa infectado. Se ejecuta capturando una interrupción (supervisor!!).
- **Virus de arranque:** Infectan cargador software del S.O. en MBR. Si el cargador software + virus no caben en MBR: mueven el cargador a otro sitio.
- **Virus de controlador de dispositivo:** infectan un gestor de dispositivos.
- **Virus de macro:** infectan documentos y se ejecutan por parte de las apps que los manipulan.
- **Virus de código fuente:** localizan e infectan archivos de código fuente, muy específicos, por tanto, escasa repercusión.
Funcionamiento de los antivirus:
- Se consigue infectar un archivo señuelo (goal file).
- Se compara con el original y se identifica cadena característica.
- Se busca dicha cadena en archivos o memoria.
Los **virus polimórficos** son capaces de cambiar su cadena características en cada infección.
### ♦ Gusanos
Programas que se propagan copiándose a sí mismos de una máquina a otra a través de la red. Suelen explotar vulnerabilidades del S.O. o de procesos servidores. No modifican código (pero sí pueden modificar configs).

Los tipos de ataques son:
### ♦ Denegación de Servicio (DoS)
Impedir el uso del sistema. La forma más frecuente es borrado de información, encriptado de información, desconfiguración de hardware.
### ♦ Secuestradores (ransomware)
Ataque DoS condicional: se ofrece al usuario recuperar el sistema a cambio de dinero. Encriptado de información con clave pública (se ofrece la privada a cambio de dinero).
### ♦ Backdoor
Servidor instalado en el sistema que permite el control del mismo de manera remota. Se usa para robar información, instalar y/o ejecutar programas, usar el sistema para otros ataques, etc.
### ♦ Spyware
Programas que monitorizan la actividad del usuario. Actividad de usuarios en la web, keyloggers, etc.
## 12.3. Comprobación de la identidad del usuario
Permitir acceso al sistema sólo a usuarios autorizados. Se suele comprobar de distintas formas:
1. Información secreta
2. Elementos físicos
3. Técnicas biométricas
4. Servidor de autenticación
### ♦ Información secreta
El sistema pide dicha información al usuario y si es correcta permite acceso. Es simple y barato pero puede ser comunicada/averiguada. Los ataques pueden ser:
- Falso cliente de conexión
- Ingeniería social
- Uso de diccionario (fuerza bruta): se puede evitar limitando nº de reintentos, con un delay para reintento o usando contraseñas que no estén en diccionario.
- Packet Sniffer
### ♦ Elementos físicos
Identificación mediante chips, lectores NFC, etc. No hay que almacenar ningún tipo de clave pero se necesitan dispositivos físicos que pueden ser duplicados.
### ♦ Técnicas biométricas
Se usan características del usuario para autenticar, como huella, iris, capilares de la retina, cara, etc. 
### ♦ Servidor de autenticación
La autenticación se delega a un sistema externo (AaaS, Authentication as a Service). Se establece conexión segura con el sistema de autenticación, el servidor autentica al usuario e informa al sistema sobre la identidad.
- **Directorio LDAP (Lightwieht Directory Access Protocol):** el directorio LDAP es un conjuntno de objetos organizados de forma lógica y jerárquica (en forma de árbol). Almacena usuario y contraseña y alguna información adicional.
- **OAuth (Open Authorization):** Estándar abierto que permite compartir información de autenticación. Proporciona una API estándar.
## 12.4. Protección y control de acceso
Una vez que el usuario es identificado y ha iniciado sesión, sólo debe acceder a los recursos que esté autorizado. El sistema informático se compone de **sujetos** y **objetos**.
![[Pasted image 20231211170645.png]]
El **derecho de acceso** es la autorización para aplicar una operación sobre un objeto. El **dominio de protección** es el conjunto de derechos de acceso.
### ♦ Matriz de control de acceso
Matriz que relaciona conjunto de dominios con conjunto de objetos.
![[Pasted image 20231211170955.png]]
La matriz de control de acceso es robusta y potente pero es poco práctica ya que en un sistema real sería enorme además de ineficiente al tener muchas entradas vacías.
### ♦ Jerarquías de acceso
Establecer derechos de accesos organizados jerárquicamente. Existe un dominio _root_ que tiene acceso a **TODO** el sistema. A partir de ese se crean subdominios. Es un sistema simple que a penas consume recursos, pero es menos flexible y **no respeta el principio de mínimo privilegio**.
### ♦ Listas de control de acceso
Cortar matriz de control de acceso por columnas. 
**Lista de control de acceso (ACL) de un objeto:** relación de los derechos de acceso que sobre dicho objeto tienen los distintos sujetos.
![[Pasted image 20231211172027.png]]
![[Pasted image 20231211172045.png]]
Se asocia una lista de control de acceso para cada archivo. Se guarda la referencia a la lista en entrada de directorio o en nodo-i. El propietario puede manipular la lista. El S.O. comprueba la lista por cada acceso a un archivo. Posibilidad de usar * como usuario comodín.
# ==TEMA 13: Virtualización==
## 13.1. Visión general
Colocando una capa de software sobre el hardware obtenemos una máquina ampliada con interfaz de más alto nivel.
![[Pasted image 20231213175230.png]]
Los hipervisores tienen varias funciones:
- Ejecución simultánea de varios S.O.
- Explotación de mainframes.
- Cloud computing.
- Depurado de S.O.
- Reducción de consumo energético.
- Despliegue de aplicaciones dentro de MVs.
## 13.2. Hipervisores tipo I
- Se ejecutan directamente sobre el hardware anfitrión. Tienen que resolver parte de la funcionalidad del S.O. 
- Simulan los dispositivos, la memoria y los procesadores. 
- Simulan la interfaz de usuario (propia VRAM que se vuelca en memoria del anfitrión).
- Simulan adaptador de red: hay distintas opciones...
	- Compartir dirección de red del anfitrión
	- Mapear la dirección de red de las máquinas virtuales sobre el adaptador anfitrión
	- Asignar direcciones privadas mediante NAT a las MVs
- Simulan unidades de disco: se mapean sobre...
	- Una unidad o partición en el anfitrión.
	- Un disco virtual (archivo en anfitrión).
- Simulan la memoria:
	- Cada máquina su memoria mediante paginación.
- Simulan el procesador:
	- Reparte tiempo de procesador entre las MVs.
	- Simula un estado de CPU por cada MV.
	- Impide el acceso al hardware anfitrión.
### ♦ Soporte hardware para virtualización: Interl VT-x / AMD-V
El procesador tiene tos modos de funcionamiento:
- **Root:** Modo para hipervisor, irrestringido.
- **Non-root:** Modo para sistema huésped. Algunas instrucciones provocan trancisión a root.
Para cada MV el hipervisor crea una VMCS (Virtual Machine Control Structure). 
- Esta VMCS contiene el estado e información de control de cada MV. Si tiene varios procesadores, una VMCS para cada uno.
- En el VMCS se pueden definir dispositivos a los que puede acceder directamente la MV (**device passthrough**) y dispositivos simulados (al intentar usarlos se transfiere control al hipervisor). El device passthrough requiere una unidad IOMMU en CPU. Funciona de la siguiente forma:
	- Hipervisor inicia modo VMX con VMXON
	- Para cada MV se crea los VMCS
	- Lanza las MVs con VMLAUNCH
	- Cada vez que un procesador virtual ejecuta una instrucción no permitida en su VMCS, transfiere control a hipervisor y se simula
	- El hipervisor termina con VMXOFF
## 13.3. Hipervisores tipo II
Se ejecuta como un proceso más dentro del S.O. anfitrión. Simula una o varias MVs interpretando las instrucciones que deben ejecutar.
![[Pasted image 20231213183657.png]]
Interpreta las instrucciones una a una. Hipervisor analiza bloques básicos antes de ejecutar. Cada bloque se procesa de la siguiente forma:
- Las no privilegiadas se ejecutan.
- Las privilegiadas se sustituyen por llamadas al hipervisor.
### ♦ Comparación
- **Hipervisor tipo I:** eficiente. 
- **Hipervisor tipo II:** flexible.
## 13.4. Paravirtualizadores
¿Y si el S.O. invitado no tuviera instrucciones privilegiadas? Se podria modificar el source code del S.O. invitado para sustituir instrucciones privilegiadas por llamadas al hipervisor. El paravirtualizador ofrece una API al S.O. invitado. El inconveniente es que el invitado debe estar compilado para una MV. Para ejecutarlo se usa el HAL (Hardware Abstraction Layer):
- La interfaz entre el kernel y el hardware se encapsula en una capa VMI (Virtual Machine Interface). 
- VMI ofrece una API única al kernel.
- Su implementación se puede enlazar con librerías.
- Nueva interfaz: paravirt-ops (VMWare, XEN, IBM, RedHat).
La ventaja de la paravirtualización es la eficiencia. Lo malo es que no es transparente para el S.O. invitado. La solución es que el hipervisor híbrido trabaje como paravirtualizador.
## 13.5. Hipervisor híbrido
El hipervisor tipo II es la opción más flexible, se instala sobre S.O. anfitrión. Pero ya que hoy en día el hardware siempre soporta virtualización es más eficiente un tipo I.
Un hipervisor instalado sobre un S.O. anfitrión puede instalar parte del hipervisor...
- como módulo en Linux.
- como driver en Windows.
- y aprovechar el soporte hardware.
### ♦ Virtualización anidada
Si el hipervisor es capaz de virtualizar VMCS e IOMMU, una máquina virtual puede a su vez convertirse en máquina anfitrión. Cada S.O. se convierte en un nivel.
## 13.6. Contenedores
PROBLEMA: Las apps actuales no son un sólo ejecutable. Normalmente constan de...
- API del S.O.
- MV (Java, Python,etc)
- Librerías (.NET, DLLs)
- Servidor web
Ejemplo app web:
- PHP
- Apache con PHP 5.5
- MySQL 5.5
- Mongodb1.5, gd y msql (extensiones de PHP).
Pueden haber incompatibilidades de versiones. La solución es desplegar la app mediante máquinas virtuales.
![[Pasted image 20231213191025.png]]
En realidad sólo necesitamos:
- Sistema de archivos para alojar los archivos del entorno de ejecución, programas y datos.
- Interfaz de conexión a la red.
- Kernel.
Los contenedores se almacenan en un repositorio. Las máquinas que van a ejecutar las apps permiten:
- Descargar, instalar y gestionar contenedores.
- Instanciar contenedores.
- Cada contenedor está aislado.
No se pueden ejecutar apps que dependan de hardware:
- Imposible tiempo real y videojuegos.