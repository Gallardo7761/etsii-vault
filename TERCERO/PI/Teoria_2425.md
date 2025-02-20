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
