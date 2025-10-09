# ==TEMA 1: Esquemas iterativos y recursivos==
Tipos de ejercicio:

## 1.1. Notación teórica
- **Rangos:** `[a,b), [a,b] -> [1,5) = 1,2,3,4`
	- **Progresión aritmética:** La diferencia entre dos términos consecutivos es siempre la misma. Ejemplo: 1,3,4,7,10,13,16...
	- **Progresión geométrica:** La diferencia entre dos términos varía. Ejemplo: 1,2,4,8,16,32...
- **Tuplas:** `t = (t0, t1, ..., tn-1) -> Records (inmutables)`
- **Listas:** `ls = [e0, e1, ..., en-1] -> |ls|, ls[i, j], ls[i], []`
- **Conjunto:** `ss = {e0, e1, ..., en-1} -> |ss|, {}`
- **Diccionarios:** `m = {k0:v0, k1:v1, ..., kn-1:vn-1} -> m[ki]`
- **Multiconjuntos:** `ms = {e0:m0, e1:m1, ..., en-1:mn-1}`
- **Agregados de datos:** `d -> reales, virtuales`
	- **Reales:** en todo momento podemos disponer de cualquier elemento. Almacenados en MEMORIA.
	- **Virtuales:** sus elementos se proporcionan bajo demanda, es decir, inicialmente está vacío. Ejemplo: `Stream<>, Iterator<>`

## 1.2. Factorías de secuencias
- **Range:** 
	Se trata de una progresión aritmética.
	`range(a,b) = (a, e -> e < b, e -> e + 1)`
- **Iterate:** 
	Se trata de una progresión geométrica donde `e0` es el valor inicial, `g` es el `Predicate` y `nx` la "función next".
	`iterate(e0, g, nx) = (e0, e -> g(e), e -> nx(e))`

## 1.3. Operaciones sobre secuencias
- **Prefijo:**
	Se queda con los consecutivos que cumplen un `Predicate`.
	`s2 = s1.takeWhile(p);`
- **Sufijo:** 
	Descarta los consecutivos que cumplen un `Predicate`.
	`s2 = s1.dropWhile(p);`
- **Concatenar:** 
	Concatena dos secuencias.
	`concat(s1, s2);`
- **Enumerate:**
	Parejas de elementos de una `Sequence` y sus posiciones.
	`(e: i)`

## 1.5. Transformación de algoritmos
- **Paso 1:**
	Identificar los elementos principales (`e0`, `b0`, `nx`, `g` …).
- **Paso 2:**
	Pasar del método original a la llamada inicial y recursiva.

Al usar la "función cortocircuito" aparece el acumulador secuencial:
`a = (b0,  (b,e) -> c(b,e),  b -> r(b),  b -> d(b))`
Y un acumulador simple es cuando no tiene ni "función retorno" ni "función cortocircuito":
`a = (b0,  (b,e) -> c(b,e)`

## 1.6. Acumuladores
- **`allMatch(p): `**`(true,  (b,e) -> p(e),  b -> b,  b -> !b)`
	$b_0$ = `true` ; `c(b,e)`= `p(e)` ; no hay `r(b)` ; `d(b)`= `!b`
	_Ejemplo:_ `IntStream.range(15,75).filter(e->e%3==2).allMatch(e->e%2==1)`
		`s = (15, e -> e < 75, e -> e + 1)`
		`a = (true, c(b,e) -> b = e % 2 == 1, b -> b, b -> !b)`
	```java
	Integer e = 15;
	Boolean b = true;
	while(e < 75 && !d(b)) { // d(b) = !b entonces queda !!b = b
		if(e % 3 == 2) {
			b = e % 2 == 1;
		}
		e = e + 1;
	}
	return b; // no hay r(b)
	```
- **`anyMatch(p):`**`(false,  (b,e) -> p(e),  b -> !b, b -> b)`
- **`noneMatch(p):`**`(false,  (b,e) -> p(e),  b -> b,  b -> b)`
- **`sum():`**`(0,  (b,e) -> b + e)`
- **`count():`**`(0,  (b,e) -> b + 1)`
- **`joining(sp,pf,sf):`**
	`B = (String,Boolean), E = R = String`
	`a = ((pf, true), ((b1,b2),e) -> c((b1,b2),e), (b1,b2) -> b1 + sf, b -> false)`
	```java
	String b1 = "*";
	Boolean b2 = true;
	Integer i = 0;
	while(i < ls.size()) {
		if(b2) {
			b1 += ls.get(i);
			b2 = false;
		} else {
			b1 += ":"+ls.get(i);
		}
		i++;
	}
	return b1 + "#";
	```
- **`findLast:`**``
- **`ToList, ToSet, ToMultiset:`**``
- **`AllDifferents, Frequency:`**``
- **`GroupsSize:`**`s.collect(Collectors.groupingBy()`
- **`GroupingList, GroupingSet:`**`s.collect(Collectors.groupingBy()`
- **`GroupingReduce:`**`s.collect(Collectors.groupingBy()`

Ejemplo3Java8
```java
s = (0, i -> i < ls.size(), i -> i + 1);
B = Boolean;
a = (true, (b,i) -> ls.get(i) % 2 == 1, b -> b, b -> !b);
```

## 1.7. El tamaño del problema
- El tamaño se denota mediante la letra $n$ , siendo $n >= 0$ . 
- $n = f(\vec{x})$ , siendo $\vec{x}$ las propiedades del problema. (Ejemplo: en problemas de secuencias, $n = b - a$ , siendo $a$ y $b$ los extremos de la secuencia)
- Nos interesará calcular el tiempo en función del tamaño (complejidad): $T(n)$ .
- En un algoritmo bien construido, el tamaño disminuirá a medida que el algoritmo avanza hacia la solución.
- A partir de la versión funcional, $n$ se obtiene muy fácilmente.

## 1.8. Diseño de algoritmos iterativos
Obtener un algoritmo a partir de un enunciado. Hay varias estrategias de diseño:
- **Algoritmo iterativo:** 
	1. Hallar la secuencia `s` y el acumulador `a` para el problema.
	2. Generalizar el problema.
- **Algoritmo recursivo:** 
	1. Encontrar una definición a partir de un conjunto de propiedades.
	2. Definir una función de partición.
	3. Generalizar el problema.
	4. Definir una restricción E/S.
### ♦ Algoritmos iterativos - Estrategia 1
Obtenidos `s` y `a` implementamos el algoritmo en estilo:
Funcional $\rightarrow$ Aplicando los esquemas $\rightarrow$ Imperativo $\rightarrow$ Recursivo Final
Es necesario que la secuencia `s` sea finita y que si `s` no es indexable, la transformación se realice mediante iteradores.
#### Ejemplo 1: Número primo más pequeño mayor a un número n.
$e_0~=~n~\%~2~==~0~?~n~+~1~:~n~+~2$ 
$nx(e)=e\rightarrow e+2$ 
$g(e)=e\rightarrow true$ 
```java
Long siguientePrimo1(Long n) {
	Long e0 = n%2 == ? n+1 : n+2;
	return Stream.iterate(e0, e -> e+2)
			.filter(e -> Math2.esPrimo(e))
			.findFirst()
			.get();
}
```
$s=(n\%2~==~0~?~n+1~:~n+2,~e\rightarrow true,~e\rightarrow e+2$
$a=(null,~b=esPrimo(e)~?~e~:~null,~b\rightarrow e,~b\rightarrow b!=null)$
#### Ejemplo 2: Dado un fichero de números enteros en cada línea separados por comas y espacios, sumar los que sean primos.
```csv
numeros.csv
1,2,3,4
5,6,7,8
9,10,11,12
13,14,15,16
```

```java
Integer sumaPrimos(String file) {
	IntStream st = Streams.file(file)
					.flatMap(e -> Streams.split(e,"[ ,]"))
					.mapToInt(e -> Integer.parseInt(e))
					.filter(e -> Math2.esPrimo(e));
	return st.sum();
}
```
### ♦ Algoritmos iterativos - Estrategia 2
Consiste en concretar un estado y sobre él definir un invariante. Un estado es una tupla (record) cuyas componentes dan la información necesaria sobre cómo progresa el algoritmo hacia la solución del problema. 
Definir la tupla-estado se denomina generalizar el problema. El algoritmo pasa de un estado inicial a un estado final mediante una sucesión de estados. El invariante previamente definido es el predicado que debe cumplirse en todos los estados. 
#### Ejemplo 1: Encontrar el n-ésimo termino de la sucesión de Fibonacci.
$$
fb(n)=
\left
\lbrace
\begin{array}{c}
d_0,~n=0 \\ d_1,~n=1 \\ a*fb(n-1)+b*fb(n-2),~n>1
\end{array}
\right.
$$
#### Ejemplo 2: Dada una lista `ls`, ordenada con respecto a un orden, y un elemento `e`, devolver el índice de `e` en `ls` ó `-1` si no está.
```java
Integer n = ls.size();
(i,j,k) = (0, n, n/2);
while(j-i > 0 && ls.get(k)!=e) {
	(i,j,k) = ver (1)
}
return r((i,j,k));
```
$$
^{(1)}~(i,j,k) = 
\left
\lbrace
\begin{array}{c}
(k,j,\frac{k+j}{2}), ls[k] < e \\
(i,k,\frac{i+k}{2}), ls[k] > e
\end{array}
\right.
$$

## 1.9. Diseño de algoritmos recursivos
### ♦ Algoritmos recursivos - Estrategia 1
Dado un conjunto de propiedades, diferenciar aquellas necesarias y suficientes que proporcionan una definición recursiva. Es necesario determinar las propiedades que determinan los casos base y las que permiten llegar a ellos. Hay que tener en cuenta que algunas propiedades pueden necesitar ser reescritas.
$$
\binom{n}{k} =
\left
\lbrace
\begin{array}{c}
1,~~~k, n-k = 0 \\ n,~~~k,n-k=1 \\ \binom{n-1}{k-1}+\binom{n-1}{k},~~~k>1
\end{array}
\right.
$$
```java
public static BigInteger binom(Long n, Long k) {
	BigInteger r;
	if(k==0 || n-k==0) {
		r = BigInteger.ONE;
	} else if(k==1 || n-k==1) {
		r = new BigInteger(n.toString());
	} else if(k>1) {
		r = binom(n-1,k-1) + binom(n-1,k);
	}
	return r;
}
```
### ♦ Algoritmos recursivos - Estrategia 2
Asumimos que el tipo $D$ tiene un método `size()` que indica su tamaño.
```java
ls = [2,4,6,8,10]
View1<Integer> v1 = List2.view1(ls);
// v1 = (2, [4,6,8,10])
View2<Integer> v2 = List2.view2(ls);
// v2 = ([2,4], [6,8,10])
View2E<Integer> v3 = List2.view2e(ls);
// v3 = (6, [2,4], [6,8,10])
```
### ♦ Algoritmos recursivos - Estrategia 3
- **Opción 1:**

- **Opción 2:**
Consideramos dos esquemas según el tipo de problema, aplicables únicamente a secuencias indexables.
	- **Generalización sufija (reduce y vencerás):** Si `s` es la secuencia original, cada nuevo subproblema `(i,s)` lo conforman los elementos de `s` cuyos elementos cumplen $\geq$ `i`.
	- **Generalización central (divide y vencerás):** Si `s` es la secuencia
### ♦ Algoritmos de ordenación: Bandera Holandesa
Sea una lista `List<T>` y un elemento tipo `T` llamado pivote. Se establecen 3 "banderines" `a`, `b` y `c`. Los banderines `a` y `b` se van desplazando hacia la derecha y `c` hacia la izquierda. Cuando `b` y `c` se encuentran, se ha terminado. $BH\rightarrow\Theta(n)$
### ♦ Algoritmos de ordenación: QuickSort
Los intervalos son $[0,a)$, $[a,b)$, $[b,n)$. Tiene una complejidad $\Theta(n·\log{n})$ oscilante entre eso y $n^2$ en el peor de los casos (si los pivotes caen en los extremos al ser aleatoriamente escogido).
### ♦ Algoritmos de ordenación: MergeSort
Los intervalos son $[0,a)$, $[a,b)$, $[b,n)$. Tiene una complejidad $\Theta(n·\log{n})$ oscilante entre eso y $n^2$ en el peor de los casos (si los pivotes caen en los extremos al ser aleatoriamente escogido). $MS\rightarrow\Theta(n·\log{n})$
### ♦ Algoritmos de ordenación: K-ésimo
Dada una lista se quiere saber la posición de un elemento `e` si la lista estuviera ordenada. A la lista se le aplica la Bandera Holandesa $T(n)=n+T(\frac{n}{2})$ .

# ==TEMA 2: Recursividad múltiple==
## 2.1. Recursividad múltiple sin memoria
$$
f(a,b)=
\left
\lbrace
\begin{array}{c}
a+b,~a<2~∧~b<3 \\ a*b,~a<3~∨~b<4 \\ f(a-1,b/2),~a<5~∨~b<5 \\ f(a/2,b-3),~e.o.c
\end{array}
\right.
$$
```java
Long f(Integer a, Integer b) {
	Long r = null;
	if(a < 2 && b < 3) {
		r = a + b;
	} else if(a < 3 || b < 4) {
		r = a * b;
	} else if(a < 5 || b < 5) {
		r = f(a-1, b/2) + f(a-2, b/4);
	} else {
		r = f(a/2, b-3) -  f(a/3, b-1);
	}
	return r;
}
```    

## 2.2. Recursividad múltiple con memoria
```java
Long fInit(Integer a, Integer b) {
	return f(a, b Map2.empty());
}

Long f(Integer a, Integer b, Map<IntPair, Long> m) {
	if(m.containsKey(IntPair.of(a,b))) {
		r = m.get(IntPair.of(a,b));
	} else {
		if(a < 2 && b < 3) {
			r = a + b;
		} else if(a < 3 || b < 4) {
			r = a * b;
		} else if(a < 5 || b < 5) {
			r = f(a-1, b/2, m) + f(a-2, b/4, m);
		} else {
			r = f(a/2, b-3, m) -  f(a/3, b-1, m);
		}
		m.put(IntPair.of(a,b), r);
	}
	return r;
}
```

# ==TEMA 3: Análisis de la complejidad y eficiencia==
## 3.1. Análisis de la complejidad
Analizar la complejidad de un algoritmo es el estudio del tiempo que tarda en ejecutarse en función del tamaño de problema que se está resolviendo. Se representa mediante $T(n)$, que es continua y creciente, siendo $n$ el tamaño del problema. Interesa analizar la complejidad cuando el tamaño del problema tiende a infinito.  Hay varias relaciones de equivalencia:
- **Igualdad:** $f(n)=_\infty g(n)\leftrightarrow 0 < \lim\limits_{n\to\infty}\frac{h(n)}{g(n)}<\infty$
- **Relación de equivalencia:** 
	$\Theta(g(n))=\{f(n)|f(n)=_\infty g(n)\}$ 
	$\Theta(g(n))=\{f(n)|\lim\limits_{n\to\infty}\frac{f(n)}{g(n)}=0$
- **Relación de orden:** $f(n)<_\infty g(n)\leftrightarrow \lim\limits_{n\to\infty}\frac{f(n)}{g(n)}=0$ 
- **Propiedades:** $\Theta(af(n)+bg(n))=\Theta(f(n))~si~f(n)>\infty g(n)$
Se denominan **instrucciones elementales** a la asignación, a evaluar expresiones lógicas y aritméticas, declaración de tipo de variable y a cualquier combinación de una/varias de las anteriores. Un **bloque básico** es una secuencia finita de instrucciones elementales. La complejidad de un bloque básico es de orden constante $\Theta(1)$.

## 3.2. Complejidad de sumatorios
![[Pasted image 20231009184410.png|400]]
## 3.3. Operaciones con enteros (Integer, Long, BigInteger)
- Número de dígitos $d(n)$ de un entero $n$: $\Theta(d(n))=\log{n}$
- Complejidad de la suma y multiplicación para enteros cortos: $\Theta(1)$
- Complejidad de la suma de enteros largos: $\Theta(d)$
- Complejidad de la multiplicación de dos enteros largos: $\Theta(d^k)$ sea $k=2$ para la multiplicación normal, $k=1.58$ para el algoritmo de _Karatsuba_ y $k=1.46$ para el de Tom-Cook.
- Complejidad de la multiplicación de un entero largo por un entero corto: $\Theta(d)$
### ♦ Potencia
- Propiedades:
$a^n=a^{n-1}$ 
$T(n)=T(n-1)+1$ : `BigInteger * Integer/Long`
$T(n)=T(n-1) + n^k$ : `BigInteger * BigInteger`

$a^n=({a^{n/2}})^2$ , si $n$ es par
$a^n=a·({a^{n/2}})^2$ , si $n$ es impar
- Si $a^n$, $a$ y $n$ son enteros cortos:
	- No hay caso mejor y peor
	- Primera opción: $\Theta(n)$
	- Segunda opción: $\Theta(\log{n})$
- Si 
#### ♦ Ejemplo 1
```java
Integer alg(Integer a) {
	Integer r= 0, i=1;
	while(i<=a) {
		r+=i*i;
		i+=2;
	}
	return r;
}
```
$\sum_{i\in pa(1,2)}^{n}~1=\sum_{x\in pa(1,2)}^{n}~x^d·\log^p{x}\Leftrightarrow 1=x^d·\log^p{x}\Leftrightarrow d=0, p=0$ 

Lo cual quedaría en: $T(n)_{bucle}=_\infty \frac{1}{2(0+1)}·n^{d+1}·\log^0{n}=\frac{n}{2}=_\infty n$
#### ♦ Ejemplo 2
```java
Integer alg(Integer a) {
	Integer r= 0, x=1;
	while(x<=a) {
		r+=x*x;
		i+=2;
	}
	return r;
}
```
Para $d=0, p=1;~T_{bucle}=\log{n}$ 
#### ♦ Ejemplo 3
```java
Integer alg(Integer a) {
	Integer r = 0, x = a;
	while(x>0) {
		for(int y = 0; y < a; y++) {
			r += x*y;
		}
		x = x/2;
	}
	return r;
}
```
$T_{bucleWhile}=\sum_{y\in pa(0,1)}^{n}~1=_\infty n$ 
$T_{bucleFor}=\sum_{x\in pa(n,1/2)}^{1}~n=n·\sum_{x\in pg(1,2)}^{n}~1=n·[~\sum_{x\in pg(1,2)}^{n}~x^d·\log^p{x}~]$
$\Leftrightarrow 1=x^d·\log^p{x}\Rightarrow d=0, p=0 \Rightarrow T_{bucleFor}=n·[~\log^{d+1}{n}~]=n·\log{n}$   
#### ♦ Ejemplo 4
```java
Integer alg(Integer e) {
	Integer r = null;
	if(e<=2) {
		r = e;
	} else {
		r = alg(e-1) + e*e;
	}
	return r;
}
```
$T(n)=T(n-1)+1=a·T(n-b)+n^d·\log^p{n}\Rightarrow a=1, b=1, d=0, p=0\Rightarrow \Theta(T(n))=n^{0+1}·\log^0{n}=n$
#### ♦ Ejercicio
<hr>

```java
Integer alg(Integer a, Integer b) {
	Integer r = a;
	if(b<=1) {
		r+=b;
	} else {
		r+=alg(a*4,b-2)+alg(a*4,b-2);
	}
	return r;
}
```
1) $n = b$

2) $T(n)=T(n-2)+T(n-2)+1=2·T(n-2)+1=a·T(n-b)+n^d·\log^p{n}\rightarrow$ $\rightarrow a=2,~b=2,~d=0,~p=0$ 

3) $\Theta(T(n))=2^\frac{n}{2}·\log^0{n}=\sqrt{2}^n$  
#### ♦ Ejercicio
<hr>

```java
Integer alg(Integer a, Integer b) {
	Integer r = a;
	if(b<=1) {
		r+=b;
	} else {
		r+=alg(a*4,b-2)*2;
	}
	return r;
}
```
1) $n=b$

2) $T(n)=T(n-2)+1=1*T(n-2)+1=a·T(n-b)+n^d·\log^p{n}\rightarrow$
	$\rightarrow a=1,~b=2,~d=0,~p=0$

3) $\Theta(T(n))=n^{0+1}·\log^0{n}=n$ 
#### ♦ Ejercicio
<hr>

```java
Integer alg(Integer a, Integer b) {
	Integer r = a;
	if(b<=1) {
		r+=b;
	} else {
		r+=alg(a*4,b-2)*alg(a/2,b/2)-alg(a+1,b/2);
	}
	return r;
}
```
1) $n=b$

2) $T(n)=3·T(n-2)+1=a·T(\frac{n}{b})+n^d·\log^p{n}\rightarrow$
	$\rightarrow a=3,~b=2,~p=d=0$

3) $\Theta(T(n))=n^{\log_2{3}}$ 
#### ♦ Ejercicio
<hr>

```java
Integer alg(Integer a, Integer b) {
	Integer r = a;
	if(b<=1) {
		r+=b;
	} else {
		r+=alg(a*4,b/2)*alg(a/2,b/4)-alg(a+1,b/3);
	}
	return r;
}
```
1) $n=b$

2) $T(n)=T(\frac{n}{2})+T(\frac{n}{3})+T(\frac{n}{4})+1$ 
	$T_1(n)=3T(\frac{n}{4})+1$
	$T_2(n)=3T(\frac{n}{2})+1$ 

3) $3T(\frac{n}{4})+1=a·T(\frac{n}{b})+n^d·\log^p{n}\rightarrow$
	$\rightarrow a=3,~b=4,~d=p=0$
	$\Theta(T_1(n))=n^{\log_4{3}}$
	$3T(\frac{n}{2})+1=a·T(\frac{n}{b})+n^d·\log^p{n}\rightarrow a=3,~b=2,~d=p=0$
	$\Theta(T_2(n))=n·\log_2{3}$ 
#### ♦ Ejercicio
<hr>

```java
	Integer alg(Integer a, Integer b) {
		Integer r = a;
		if(b<=1) {
			r+=b;
		} else {
			for(int i = 1, i<=b*b; i*=2) {
				r+=a;
			}
			r+=alg(a*2,b-2)+alg(a/2,b-1);
			for(int j = 0; j*j<b; j++) {
				r+=b;
			}
		}
		return r;
	}
```
1) $n=b$

2) $T(n)=T_{bucle1}+T(n-2)+T(n-1)+T_{bucle2}+1$
	$T_{bucle1}=\sum_{i\in pg(1,2)}^{n^2}~1=n^d·\log^p{n}\rightarrow$
	$\rightarrow d=p=0\Rightarrow T_{bucle1}=\log^{0+1}{n^2}=\log{n^2}=2·\log{n}=\Theta(\log{n})$  
	$T_{bucle2}=\sum_{j\in pa(1,1)}^{\sqrt n}~1\rightarrow 1={\sqrt{n}^d}·\log^p{\sqrt{n}}\rightarrow d=p=0 \Rightarrow$
	$\Rightarrow T_{bucle2}=\frac{1}{1(0+1)}·{\sqrt{n}}^0+1·\log^0{\sqrt n}=\sqrt n$ 

3) $T_1(n)=2·T(n-2)+n^{\frac{1}{2}}\rightarrow a=2,~b=2,~d=\frac{1}{2},~p=0\Rightarrow$
	$\Rightarrow \Theta(T_1(n))=2^{\frac{n}{2}}·\log^0{n}={\sqrt{2}}^n$
	$T_2(n)=2·T(n-1)+n^{\frac{1}{2}}\rightarrow a=2,~b=1,~d=\frac{1}{2},~p=0$
	$\Theta(T_2(n))=2^{\frac{n}{1}}·\log^0{n}=2^n$
	$\Theta(2^{\frac{n}{2}})<_\infty\Theta(T(n))<_\infty\Theta(2^n)$       
#### ♦ Ejercicio
<hr>
**Sin memoria**
1) $n=k$
2) c. mejor => Último elemento de v = -a
    c. peor => Último elemento de v $\neq$ -a
3) $T(n)=T(n-1)+T(n-2)+T_{bucle}$
   $T_{bucle}=\sum_{i\in pg(1,3)}^{n}\sum_{j\in pa(1,2)}^{i}~1=\sum_{i\in pg(1,3)}^{n}~i\Leftrightarrow\sum_{i\in pg(1,3)}~i^d·\log^p{i}\Rightarrow d=1, p=0\Rightarrow T_{bucle}=\Theta(n)\Rightarrow$ $T(n-1)+T(n-2)+n\Rightarrow$
	   $T_1(n)=2T(n-2)+n\rightarrow\Theta({\sqrt{2}}^n)$ 
	   $T_2(n)=2T(n-1)+n\rightarrow\Theta(2^n)$    
**Con memoria**
1) $n=k$
2) Igual a sin memoria
3) $T(n)=T(n-1)+n\Rightarrow\Theta(T(n))=n^2$ 

# ==TEMA 4: Tipos recursivos==
Un ejemplo de tipo recursivo:
```java
interface Lista {
	Integer getPrimero();
	Lista getResto();
}
```
Un tipo recursivo es aquel que tiene una propiedad con valores del mismo tipo. Los tipos recursivos dan lugar a algoritmos recursivos específicos. Asumimos que cada tipo recursivo tiene un conjunto de subtipos disjuntos. El tipo puede definir un conjunto de predicados que indiquen de qué subtipo es.
```plantuml
hide methods
class Lista{}
class ListaPares {}
class ListaImpares {}
ListaPares -up-|> Lista
ListaImpares -up-|> Lista
```
### ♦ Árbol
Un árbol es un tipo de datos jerárquico, con una raíz y subárboles hijos. 
- Un árbol n-ario (`Nary<E>`) puede ser vacío, puede tener un elemento (que llamaremos etiqueta), o puede ser un árbol n-ario que tiene n hijos. Si un árbol no tiene padre, se llama raíz.
- Si un árbol binario (`Binary<E>`):
	- tiene etiqueta pero no tiene hijos, se llama hoja (`Leaf`). 
	- no tiene etiqueta ni hijos se llama vacío (`Empty`).
El tipo `Tree<E>` (árbol n-ario) y sus propiedades:
```java
sealed interface Tree<E> permits TEmpty<E>, TLeaf<E>, TNary<E> { 
	static Tree<String> parse(String s) { … } 
	static <R> Tree<R> empty() { … } 
	static <R> Tree<R> leaf(R label) { … } 
	static <R> Tree<R> nary(R label, List<Tree<R>> elements) { … } 
	int size(); 
	int height(); 
	Tree<E> copy(); 
	Tree<E> reverse(); 
	<R> Tree<R> map(Function<E, R> f); 
	String toString(); 
	Stream<Tree<E>> byDepth(); 
	Stream<TreeLevel<E>> byLevel();
	… 
}
```
El tipo `BinaryTree<E>` (árbol binario) y sus propiedades:
```java
sealed interface BinaryTree<E> permits BEmpty<E>, BLeaf<E>,	BTree<E> {
	static BinaryTree<String> parse(String s) { … }
	static <R> BinaryTree<R> empty() { … }
	static <R> BinaryTree<R> leaf(R label) { … }
	static <R> BinaryTree<R> binary(R label,BinaryTree<E> left,
		BinaryTree<E> right) { … }
	int size();
	int height();
	Tree<E> copy();
	<R> BinaryTree<R> map(Function<E, R> f);
	String toString();
	Stream<BinaryTree<E>> byDeph();
	Stream<BinaryTreeLevel<E>> byLevel();
	…
}
```
Si se quiere mapear los valores del árbol parseado desde un fichero se puede usar:
`BinaryTree.parse(s,Integer::valueOf)`
Se llama **tamaño** del árbol a su número de etiquetas (labels). Un árbol binario está **ordenado** si es vacío u hoja, o si la etiqueta es mayor que su hijo izquierdo y menor que su derecho. Un árbol binario es **equilibrado** si es vacío u hoja, o si sus hijos están equilibrados y sus alturas (camino más largo desde la raíz hasta una hoja) no difieren en más de una unidad.
Un **camino** es una secuencia de árboles en la que cada uno es padre del siguiente. Entre cada padre e hijo hay una **arista**. La **longitud de un camino** es el número de aristas. La **profundidad** de un árbol es la longitud de un camino desde la raíz hasta él.
Hay varias formas para recorrer árboles:
![[Pasted image 20231106183346.png|300]]
# ==TEMA 5:  Grafos==
## 5.1. Conceptos y tipos (librería JGraphT)
Un grafo es un conjunto de vértices unidos por aristas. Los vértices y aristas son tipos que pueden tener información propia (tipos `V` y `E`). Las aristas y grafos pueden ser dirigidos y no dirigidos y los grafos también pueden ser mixtos.
- **Camino:** Secuencia de vértices en el que cada uno está conectado al siguiente mediante una arista.
- **Longitud de camino:** Número de aristas.
- **Camino simple:** No tiene vértices repetidos a excepción del primero que puede ser igual al último.
- **Camino cerrado:** El primer vértice consigue con el último.
- **Bucle:** Arista que une un vértice consigo mismo.
- **Grafos simples:** Desde `V1` hasta `V2` **sólo** hay una arista. No puede tener bucles.
- **Multigrafos:** Más de una arista entre dos vértices.
- **Pseudografos:** Más de una arista entre dos vértices y puede tener bucles.
- **Grafo completo:** Grafo simple en el que hay una arista entre cada par de vértices.
- **Subgrafo:** `G1` es subgrafo de `G2` si los vértices y aristas de `G1` están incluidos en los vértices y aristas de `G2`.
## 5.2. Formatos de ficheros para grafos
- `.gdf`: Representa información asociada a los vértices y aristas.
- `.dot/.gv`: Representa información que se necesita para visualizarlo.
## 5.3. Vistas de grafos
Elementos inmutables para "consultar" subconjuntos de datos de los grafos. Por ejemplo:
- **`SubGraphView<V,E>`**
- **`CompleteGraphView<V,E>`**
- **`IntegerVertexGraphView<V,E>`**
## 5.4. Recubrimientos, componentes conexas y ciclos
Un **grafo conexo** es un grafo no dirigido en el que hay al menos un camino entre cada dos nodos. Una **componente conexa** de un grafo no dirigido es un subgrafo conexo.
Si hablamos de grafos dirigidos, un grafo fuertemente conexo es un grafo donde para cada par de vértices $u,v$ existe un camino $u\rightarrow v$ y otro $v\rightarrow u$. Un grafo débilmente conexo es un grafo en el cual al reemplazar cada arista dirigida por una no dirigida resulta un grafo no dirigido conexo.
Un **ciclo** es un camino cerrado donde no se repiten vértices, salvo el primero que coincide con el último (ciclo de Hamilton). Un **grafo acíclico** no tiene ciclos. Un **árbol** es un grafo simple, conexo y acíclico. Un **bosque** es un conjunto de árboles (grafo sin ciclos).
El **recubrimiento mínimo** es un subgrafo (bosque) que incluye todos los vértices del grafo y minimiza la suma total de los pesos de las aristas escogidas.
El **recubrimiento de vértices** consiste en escoger el mínimo número de vértices que tocan todas las aristas del grafo. 
## 5.5. Coloreado de grafos
Dado un grafo se define su **número cromático** como el mínimo número de colores necesario para dar un color a cada vértice de tal forma que dos vértices vecinos tengan colores distintos.
## 5.7. Camino mínimo
Dado un grafo y dos vértices encontrar el camino con longitud mínima entre ambos. Se resuelve o por Dijkstra ($\Theta(V^2)$) o por Floyd-Warshall ($\Theta(V^3)$).

### Ejercicio ejemplo
Dado un árbol n-ario de enteros, implementar un método recursivo que devuelva el camino de mayor longitud tal que:
1) No empiece en la raíz.
2) No termine ni en vacío ni en hoja.
3) Empiece y termine por un número par.
4) Contenga un número impar de números impares.
