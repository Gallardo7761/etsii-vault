# <mark style="background: #FF5582A6;">ASM / C</mark>
### a) ¿Qué hace el código?
Recorre el array `arrayx` (empezando por la posición 1) de tamaño `N_ITER` almacenando en la posición `i - 1` la suma del elemento `i` más el `i + 1` entre la constante `f11`.
### b) ¿Cómo se puede escribir en alto nivel?
```C
#define N_ITER 25

float arrayx[N_ITER];
const float f11 = 21.0;

for(int i = 1; i < N_ITER; i++)
{
	arrayx[i - 1] = ( arrayx[i] + arrayx[i + 1] ) / f11; 
}
```
### c) ¿Son paralelizables directamente?
Sí porque no hay dependencias reales.
```C
#define N_ITER 25

float arrayx[N_ITER];
const float f11 = 21.0;

for(int i = 5; i < N_ITER; i+=5)
{
	arrayx[i - 1] = ( arrayx[i] + arrayx[i + 1] ) / f11;
	arrayx[i - 2] = ( arrayx[i + 1] + arrayx[i + 2] ) / f11;
	arrayx[i - 3] = ( arrayx[i + 2] + arrayx[i + 3] ) / f11;
	arrayx[i - 4] = ( arrayx[i + 3] + arrayx[i + 4] ) / f11;
	arrayx[i - 5] = ( arrayx[i + 4] + arrayx[i + 5] ) / f11; 
}
```
### d) Como la BTB predice siempre tomado ¿cuándo acierta?
La BTB acertará en todas las iteraciones del bucle excepto en la última porque predice salto tomado y no debería tomarlo porque el bucle ha acabado.
![[Pasted image 20241022134454.png]]
### e) Si la BTB de un GPP es muy sofisticada ¿acertará en los saltos?
...
# <mark style="background: #D2B3FFA6;">Visual Studio</mark>
La función más paralelizable es **`pp4()`** ya que sólo tiene una operación de cada tipo (**`LD, SF, ADDF, SLTI, ADDI`**) y no hay saltos condicionales en el bucle.
```C
double pp4()
{
	int i;
	for (i = 0; i<N_ITER; i++)	{
		a[i] = a[i] + 8.0;
	}
	return a[N_ITER - 1];
}
```

La función que más se parece (parecer en términos de mismo tipo de instrucciones) es **`pp2()`**.
```C
double pp2()
{
	int i;
	for (i = 0; i<N_ITER; i++)	{
		a[i] = 3.14 / sqrt(b[i]+c[i]);
	}
	return a[N_ITER - 1];
}
```

### <mark style="background: #FFB8EBA6;">Máquina en la que se ejecuta el código real:</mark>

![[Pasted image 20241022144429.png|400]]

### <mark style="background: #FFB8EBA6;">Comparativa</mark>

| `pp2()`                                                                                                                                | `nuestro`                                                                                                                                                                             |
| -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| double pp2()<br>{<br>	int i;<br>	for (i = 0; i<N_ITER; i++)	{<br>		a[i] = 3.14 / sqrt(b[i]+c[i]);<br>	}<br>	return a[N_ITER - 1];<br>} | # define N_ITER 25<br><br>float arrayx[N_ITER];<br>const float f11 = 21.0;<br><br>for(int i = 1; i < N_ITER; i++)<br>{<br>	arrayx[i - 1] = ( arrayx[i] + arrayx[i + 1] ) / f11; <br>} |
| Este código tiene:<br>- 2x LDF<br>- 1x STF<br>- 1x ADDF<br>- 1x DIVF<br>- 1x constante                                                 | Nuestro código tiene:<br>- 2x LDF<br>- 1x STF<br>- 1x ADDF<br>- 1x DIVF<br>- 1x constante                                                                                             |
|                                                                                                                                        |                                                                                                                                                                                       |
### <mark style="background: #FFB8EBA6;">Duración de CPI en los bucles</mark>

| Release                              | Debug                                |
| ------------------------------------ | ------------------------------------ |
| ![[Pasted image 20241022144832.png]] | ![[Pasted image 20241022150214.png]] |

(Todo para config1 y config2)
5. GFLOPS (o MIPS)
6. CPI (pp2 pp4) y Ac
7. Cronograma