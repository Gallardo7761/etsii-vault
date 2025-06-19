# <mark style="background: #CAFECAFE;">1. Push-Pull / Open-Drain</mark>
### Push-Pull
![[Screenshot_2025-05-20-22-44-15-771_com.myscript.nebo.png]]

### Open-Drain
![[Screenshot_2025-05-20-22-46-01-773_com.myscript.nebo.png]]
# <mark style="background: #CAFECAFE;">2. Mapa de memoria</mark>
![[Screenshot_2025-05-20-22-47-13-012_com.myscript.nebo.png]]
Explicar:
- MD
- MI
- 4GB máx
- 512MB - 32MB
- Usables los 128KB?
# <mark style="background: #CAFECAFE;">3. Timers basados en comparador</mark>
![[Screenshot_2025-05-20-22-54-00-843_com.myscript.nebo.png]]
En el OCR se pone el valor al que queremos que el comparador "salte" cuando llegue. Se puede generar una onda PWM con este tipo de timer, poniendo en el OCR el tiempo en alto (o en bajo) cada vez que se de la igualdad en el comparador.
# <mark style="background: #CAFECAFE;">4. Timers captura de eventos</mark>
![[Screenshot_2025-05-20-23-17-28-779_com.myscript.nebo.png]]
El contador de CLK cuenta hasta overflow. En el ICR se guarda el valor del contador cuando ocurre un evento (flanco). Cada vez que el evento seleccionado ocurre, se copia el valor del contador al ICR, activándose una flag (e interrupciones si están habilitadas) avisando al sistema de que ha ocurrido. Funciona básicamente como un cronómetro preciso hardware para medir el tiempo entre dos sucesos. También puede funcionar como fuente extra de interrupciones. Puede ser más complejo:
- Evento cada N flancos
- ICR sustituido por FIFO (event queue)
# <mark style="background: #CAFECAFE;">5. Serial Peripheral Interface (SPI)</mark>
![[Screenshot_2025-05-21-00-40-48-110_com.myscript.nebo.png]]
Líneas:
- MISO (Master-In Slave-Out)
- MOSI (Master-Out Slave-In)
- CLK (Clock)
- SS (Slave Select)
Se conecta el MOSI con el MISO (quién es el Master va implícito). Se usa Open-Drain para poder conectar MOSI con MISO.
# <mark style="background: #CAFECAFE;">6. Variables y constantes SSEE VS Propósito General</mark>
En los SSEE se divide la memoria en ROM y RAM. Las variables puede que se carguen en RAM, pero al reiniciar el microcontrolador, como es memoria volátil se borra. Como las variables globales y constantes se inicializan en ROM (haciendo que al ejecutar, exista la variable pero no el valor), hay que usar una función de _startup_ antes del `main(void)` para inicializarlas, cosa que no pasa en los SGP ya que el SO se encarga de cargar código y valores en RAM.
# <mark style="background: #CAFECAFE;">7. Watchdog Timers</mark>
Mecanismo híbrido SW/HW para detectar fallos. Se basa en que el código de los SSEE es cíclico. Esto permite que cada cierto tiempo el sistema limpie la cuenta del Watchdog Timer, evitando que se produzca un reset, y produciéndose este si el sistema se quedase colgado, no pudiendo limpiar dicha cuenta. Consideraciones:
- Hay que elegir un tWDT adecuado para que ni se produzcan resets innecesarios ni cuelgues largos. 
En nuestro STM32 existe el IWDT (Independent), que funciona con un CLK independiente y el WWDT (Window), que funciona en una ventana temporal programable. Adicionalmente, es recomendable no iniciar el WDT en el periodo de desarrollo ya que hace más difícil el debugging.
# <mark style="background: #CAFECAFE;">8. Descriptores, para qué sirven, tipos</mark>
Son estructuras de datos usadas para describir y comunicar información sobre el dispositivo al driver y al SO. Tienen un formato (códigos numéricos) determinado por los estándares USB.
- **Device descriptor:** proporciona información básica sobre el dispositivo USB. Al enumerar un dispositivo, el driver USB solicita este descriptor para saber cosas como la versión USB, el número de clase (HID, Gamepad, Mouse...), el Product ID, el Vendor ID, etc.
- **Configuration descriptor:** describe las configuraciones varias que pueda tener el dispositivo. Contiene la cantidad de energía necesaria, así como una lista de descriptores de interfaz. Permite al SO seleccionar la configuración adecuada.
- **Interface descriptor:** describe una interfaz específica. Proporciona información sobre el número de interfaz, el número de configuraciones alternativas de esa interfaz, la clase de la interfaz, el protocolo, etc.
- **Endpoint descriptor:** un _endpoint_ es un punto de conexión lógica en una interfaz USB, donde los datos se transfieren entre el dispositivo y el host. Describe los parámetros del endpoint como dirección, tamaño del paquete de datos, tipo de transferencia, atributos, etc.
# <mark style="background: #CAFECAFE;">9.       SOTR: Semáforos, interbloqueos, inversión de prioridades</mark>
Los semáforos son técnicas de sincronización para tareas paralelas (o pseudoparalelas con round-robin) cuando acceden a recursos compartidos (como puede ser un recurso hardware o una variable global). Los hay binarios (0,1) o contadores (0,1,2...). Pueden servir tanto para interrupciones como para sincronizar:
- **Interrupciones:** semáforo ocupado hasta que se lance la interrupción. Cuando se lanza, se libera el semáforo, y la tarea bloqueada esperando puede continuar. El semáforo, si se usa para sincronizar interrupciones, lo suelta una entidad distinta a la que lo coge, a diferencia de para recursos compartidos que lo suelta quien lo coge.
- **Recursos compartidos:** se especifica un número de _slots_ al crearlo. Tiene una cola que se va llenando y la tarea los irá procesando por orden según se acabe con los anteriores.

Un **interbloqueo** es cuando dos tareas tienen un recurso compartido cada una y cada una requiere acceso al recurso de la otra, esperan infinitamente. Se puede solucionar con semáforos con un _timeout_, ya que al expirar el _timeout_ una de las dos terminará de ejecutarse y luego irá la otra.

La **inversión de prioridades** ocurre cuando una tarea de baja prioridad ocupa un semáforo y una de alta prioridad está esperando ese semáforo porque necesita el mismo recurso que la de baja prioridad. Se soluciona:
- heredando la prioridad, haciendo que la de baja prioridad sea temporalmente de la misma prioridad que la alta
- haciendo uso de tareas portero
# <mark style="background: #CAFECAFE;">10. PWM 8b del C8051F340: ¿valor en el registro PCA0CPHn? Para PWM con duty-cycle del 50%, ¿valor del PCA0CPHn? Si se usa un puente en H para un motor DC24V ¿valor para que le lleguen 6V? ¿Usar el timer de la figura para generar tonos distintos? ¿Parámetros de sonido controlables y cómo?</mark>
![[Pasted image 20250521013913.png]]
Como el comparador es de 8b, tendremos 256 ciclos PWM. El contador y comparador van al biestable. 
- El Comparador en set se carga con el valor de tiempo en bajo. 
- Reset cambia desde donde comenzó a contar. 
- OCR: valor para cambiar el pin CICLO = LECTURA + TIEMPO

**a) Valor PCA0CPHn**
Es un registro de sincronización que espera a que la recarga sea mayor a la cuenta actual para que la cuenta no pase el valor, evitando hacer un pulso más largo de la cuenta.

**b) Invertirlo**
TPWM = TH + TL = 256
Simplemente restamos a 256 el TL, pasando el TL a ser TH.

**c) Duty-cycle 50%**
nº de ticks del timer a la mitad y cargar el PCA0CPHn con ese valor:
- PCA0PCHPn $\leftarrow$ 256/2

**d) Puente H 24V -> Motor sólo ve 6V?**
$V_{EFECTIVA}=\frac{T_H}{T_{PWM}}\times{V_{CC}}$
24V es el 100%
6V es el 25% = 1/4
Por tanto tenemos que:
$$
\begin{equation}
6\text{V} = \frac{\text{T}_H}{256} \times 24\text{V} \Rightarrow \text{T}_H = \frac{6}{24} \times 256 = 64
\end{equation}
$$

**e) Timer para generar tonos**
No se puede, el tono lo da el PWM. Para cambiar el tono se cambiaría $F_{CLK}$ con un _prescaler_.

**f) Parámetros de sonido controlables y como**
El tono es la frecuencia y el volumen el $T_H$. El máximo volumen se consigue con un _duty-cycle_ del 50%, ya que el volumen es cuánto se desplaza la membrana hacia delante y hacia atrás.
# <mark style="background:#CAFECAFE;">EXTRA: Estados tarea</mark>
![[Pasted image 20250521015738.png]]
# <mark style="background:#CAFECAFE;">EXTRA: Bucle de scan</mark>
![[Pasted image 20250521015758.png]]