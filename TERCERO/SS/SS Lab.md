# Práctica 1
<hr>
### Ejercicio 1.2.
El código debería detectar una pulsación de tecla (`_kbhit`) dentro del _timer_ de 10s e imprimirla por consola si existe (`_getch`) y en caso contrario, imprimir que no se ha pulsado ninguna.
### Ejercicio 1.3.
No. Devuelve 0 siempre ya que la variable tecla está definida como variable local dentro del bucle `for` en `smt4497-P1.cpp`.
### Ejercicio 1.4.
Una refactorización para que quede más limpio el código es usar un **fichero de inclusión de proyecto**

_smt4497-P1.h_
```C
#pragma once

#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <conio.h>
#include <windows.h>

int esperaPulseTecla(int toutMs);
```
### Ejercicio 1.5.
El modo `release` es más rápido que el `debug` al usar la optimización del compilador, haciendo desaparecer algunas funciones y por tanto algunos breakpoints.
### Ejercicio 1.8.
Cuenta mientras no pulses una tecla. Al llegar a 2147483647 se desborda.