# <mark style="background: #FFF3A3A6;">TEMA 1: Introducción a la lógica</mark>
## <mark style="background: #ADCCFFA6;">Lógica proposicional</mark>
Se utiliza un conjunto de variables atómicas (T ó F, V ó F) llamadas **variables proposicionales**. Se usan unas conectivas (u operadores) los cuales son: 
- Negación ($\neg ...$)
- Disyunción ($... \lor ...$)
- Conjunción ($... \land ...$)
- Implicación ($... \implies ...$)
- Doble implicación ($... \iff ...$)
Una forma de dar significado a las conectivas son las **Tablas de Verdad**. 
![[Pasted image 20240913105735.png|450]]
Este tipo de problemas se llaman problemas SAT (o de satisfacción de restricciones). Son NP-Completos. El método de las tablas de verdad, es exponencial ($2^n$).
### Consecuencia Lógica
Una fórmula $F$ es consecuencia lógica de un conjunto **finito** de fórmulas $U$ y se denota $U\models F$, si se verifica que $F_1\land...\land F_n\rightarrow F$ es una **tautología**.
#### <mark style="background: #BBFABBA6;">def.</mark>
<hr>

**TAUTOLOGÍA:** Siempre es verdad

<hr>
### Ejemplo
<hr>
_Si continúa la investigación, surgirán nuevas evidencias. Si surgen nuevas evidencias, entonces varios dirigentes se verán implicados. Si varios dirigentes están implicados, los periódicos dejarán de hablar del caso. Si la continuación de la investigación implica que los periódicos dejen de hablar del caso, entonces, el surgimiento de nuevas evidencias implica que la investigación continúa. La investigación no continúa. Por tanto, no surgirán nuevas evidencias._

$p$: Continúa la investigación
$q$: Surgen nuevas evidencias
$r$: Varios dirigentes se verán implicados
$s$: Los periódicos dejarán de hablar del caso

Por tanto:

$\{p\implies q,q\implies r, r\implies s,(p\implies s)\implies (q\implies p), \neg p\}$

En conclusión: $!q$
<hr>

### Fórmulas equivalentes 
Dos fórmulas $F_1$ y $F_2$ se dicen **equivalentes** si tienen la misma tabla de verdad, o si $F_1 \iff F_2$ es una tautología.

### Forma Normal Conjuntiva
Una fórmula se dice que está en FNC (ó clausal) si es conjunción de disyunciones de literales (producto de sumas).

### <mark style="background: #FFB86CA6;">Algoritmo DPLL</mark>
Este algoritmo permite calcular los modelos, lo cual no es suficiente para saber si es una tautología o no, porque hace falta también saber el número de modelos. Sin embargo, si que sirve por si mismo para saber cuando algo no es satisfactible (todas las ramas acaban en contradicción).
#### <mark style="background: #ABF7F7A6;">Nota</mark>
<hr>

$\{p,q,r\},\{\neg{p},q,r\},\{p,\neg{q}\},\{p,r\},\{p\}$ 
que es igual que 
$\{p\lor{q}\lor{r}\}\land\{\neg{p}\lor{q}\lor{r}\}\land\{p\lor{\neg{q}}\}\land\{p\lor{r}\}\land\{p\}$

<hr>

![[Pasted image 20240913113533.png|450]]
$\{□\}$ son _cláusulas vacías_ o contradicciones

### Otro ejemplo
<hr>
$(F_1\land ...\land F_n)\implies F\in~TAUT$
$\{F_1,...,F_n,\neg F\}\notin SAT$  
<hr>

### Otro ejemplo
<hr>
$p\equiv$ Todos los hombres son mortales
$q\equiv$ Sócrates es un hombre
$r\equiv$ Sócrates es mortal

Es insuficiente para representar el problema por la independencia de las frases y por la aparición de **objetos**, **propiedades** y **cuantificadores**. Aparece la **Lógica de Primer Orden (LPO)**
<hr>
## <mark style="background: #ADCCFFA6;">Lógica de Primer Orden</mark>
<span style="color:red;">NO existen métodos "mecánicos" para LPO</span>

Siguiendo el ejemplo de Sócrates:
$s\equiv$ Sócrates
$H(x)\equiv$ Es un hombre
$M(x)\equiv$ Es mortal

Por tanto:

$$
\left.
\forall{x}~(H(x)\implies{M(x)})\atop
H(s)
\right\}\longrightarrow{
	\left.
	\forall{x}~(H(s)\implies{M(s)})\atop
	H(s)
	\right\}\longrightarrow{
		\left.
		p\implies{q}\atop
		p
		\right\}\models{q}\longrightarrow{
			\{p\implies{q},p,\neg{q}\}
		}
	}
}
$$
### Ejemplo: problema de las n reinas
<hr>
$r_{ij}\equiv$ en la casilla $[i,~j]$ está una reina
$r_{ij}\implies\neg{r_{i1}}\land ... \land\neg{r_{in}}$
O también:
$r_{ij}\implies\land|k\neq{j}~~~~\neg{r_{ik}}$
<hr>

# <mark style="background: #FFF3A3A6;">TEMA 2: Problemas de Satisfacción de Restricciones</mark>
Son problemas que básicamente extienden la LP/LPO. Proporciona más capacidad para resolver problemas, pero a su vez hace dicha resolución más compleja.
## <mark style="background: #ADCCFFA6;">1. Definir problemas CSP</mark>
Un problema CSP viene dado por la terna $(X,D,C)$ donde:
- $X = \{x_1,...,x_n\}$ es un conjunto de variables
- $D = \langle D_1,...,D_n\rangle$ es un vector de dominios ($D_i$ contiene a todos los posibles valores que puede tomar $x_i$)
- $C$ es un conjunto finito de restricciones (valores $k-arios\equiv \{x_{i1},...,x_{ik}\}$). Cada restricción se define sobre un conjunto de $k$ variables de $X$ por medio de un predicado que restringe sus valores.

Una **asignación de variable** es un par $(x,a)$ (o $x=a$) que representa asignar el valor $a$ a la variable $x$. Una **asignación de un conjunto de variables** sería un conjunto de tuplas ordenadas $((x_i,a_i),...,(x_k,a_k))$ donde cada par asigna el valor $a_i\in D_i$ a $x_i$.

Una tupla es **localmente consistente** si satisface todas las restricciones que tienen que ver con ella.
Un CSP es **consistente** si tiene al menos una solución. Normalmente se intenta encontrar:
- Una solución.
- Todas las soluciones.
- Una solución óptima (usando una **función objetivo**).
## <mark style="background: #ADCCFFA6;">2. Métodos de resolución</mark>
### <mark style="background: #FFB86CA6;">Fuerza bruta</mark>

```
Fuerza-Bruta
   As = D₁ × … × Dₙ
   Para cada A ∈ As:
      Si A es consistente
         Devolver A 
         Parar
   Devolver false
```

El inconveniente obvio de este método es que puede requerir una cantidad excesiva de memoria inicial para almacenar el conjunto completo de las asignaciones y una cantidad excesiva de tiempo para encontrar la solución en el caso peor.
### <mark style="background: #FFB86CA6;">Backtracking (BT)</mark>
![[Pasted image 20240920111350.png|500]]
Las asignaciones parciales se extienden solamente cuando **NO** son inconsistentes. Al encontrar una asignación inconsistente se "poda" o deja de explorar ese espacio de asignaciones.

Una posible implementación (en pseudocódigo) sería:
```
BT(A, U)
   Si A es completa
      Devolver A
   Seleccionar una variable x de U
   U ← U − {x}
   Para cada v ∈ D(x):
      Si {x = v} es consistente con A
         res ← BT(A ∪ {x = v}, U)
         Si res ≠ false
            Devolver res
   Devolver false
```

Existen algunas estrategias para la optimización de BT:
- **Valores Mínimos Restantes (MRV):** se elige la variable no asignada con menos valores restantes válidos, ya que es posible que se quede sin valores válidos antes y que de lugar a un retroceso.
- **Valor Menos Restrictivo (LCV):** se selecciona el valor (de variable) que elimine el menor número de valores de los dominios de los valores sin asignar restantes.
- **Min-conflicts:** ordena los valores de acuerdo a los conflictos en los que estos están involucrados con las variables no asignadas. Esta heurística asocia a cada valor de la variable actual, el número total de valores en los dominios de las futuras variables adyacentes que son incompatibles con él. El valor seleccionado es el asociado a la suma más baja.
# <mark style="background: #FFF3A3A6;">TEMA 3: Espacios de estados</mark>
Esencialmente un espacio de estados es una 4-tupla $(X,S,G,T)$, donde:
- $X$ es el conjunto de estados
- $S\subseteq X$ es un conjunto no vacío de estados iniciales
- $G\subseteq X$ es un conjunto no vacío de estados finales
- $T:X\rightarrow P(X)$ es una función de transición 
El problema consiste en decidir si, partiendo desde los estados de $S$, se puede $G$ aplicando $T$.
### <mark style="background: #FFB86CA6;">Problema: misioneros-caníbales</mark>
Se podría representar matemáticamente de la siguiente forma:
$(m_D,~c_D,~m_I,~c_I,~b)~o~tambien:~(m_D,~c_D,b)$ ya que $m_D+m_I=3$ y $c_D+c_I=3$

$S=(m_D,~c_D,b)$

$b=0:$
- $(m,c)$
- $0\leq m\leq m_I$
- $0\leq c\leq c_I$
- $1\leq m+c \leq 2$
$a(m,c)\rightarrow S'=(m_I-m,~c_I-c,~1)$ 

$b=1:$
- $(m,c)$
- $0\leq c \leq 3·m_I$
- $0\leq c \leq 3-c_I$
- $1\leq m+c \leq 2$
$a(m,c)\rightarrow S'=(m_I+m,~c_I+c,~0)$

$valid(S):$
- $m_I\geq c_I$
- $3-m_I\leq 3-c_I$

<hr>

$S\longrightarrow valido(S):$
- $Sí\implies calcular~y~aplicar~A(S)$
- $No\implies S'=S$

### <mark style="background: #FFB86CA6;">Problema: todos los dígitos del rey</mark>
Sea el conjunto $S=\{0,...,9\}$, insertar los símbolos de los operadores aritméticos ($\times +-/$) entre ellos para que la expresión resultante se evalúe como $100$. 
$S=(+,~·,~-,~+,~+,...)$

## <mark style="background: #ADCCFFA6;">1. Posibles algoritmos</mark>

```jl
function Generate-X-from(S)
   Fr = S
   X = Fr
   while(Fr ∩ G = ∅)
      Fr = T(Fr)
      X = X ∪ Fr
   return X
end
```
1. Parte de la frontera inicial `Fr = S`
2. Se aplican todas las $T$ posibles
3. Se acumula `Fr` en `X` para no perder los estados obtenidos
4. Si en `Fr` no hay elementos de `G` se vuelve a empezar hasta que lo haya y se retorne

Una mejora del algoritmo anterior es añadiendo una selección:

```jl
function Selected-Search-from(s0)
   Fr = {s0}
   X = Fr
   while(Fr ∩ G = ∅)
      sel_s = select(Fr) 
      Fr = Fr ∪ T(sel_s) - {sel_s}
      X = X ∪ Fr
   return X
end
```
Donde `select()` sería una función que selecciona el estado a expandir en este paso. Por ejemplo se puede tomar `select(C) = rand(C)`, y se convertiría en búsqueda aleatoria.

### <mark style="background: #FFB86CA6;">Búsquedas no informadas (DFS, BFS)</mark>

```jl
function bfs(vecinos::Function, s_0, G)
      # lista de tuplas que almacena el vértice actual y el camino hacia él
      bfsCola = [(s_0, [])]
      # conjunto de visitados
      visitados = Set()
      # mientras la cola esté llena
      while length(bfsCola) != 0
         # se asigna a s, camino_s el valor de la tupla actual
         s, camino_s = pop!(bfsCola)
         # si s es final, termina
         if s in G
	        # devuvelve el camino de llegada al vértice final
            return camino_s, push!(visitados, s)
         end
         # si s no es final
         if s in visitados
            continue
         end
         # s se mete en el conjunto de visitados
         push!(visitados, s)
         # para cada acción y vértice se mete en la cola
         # el vértice y el camino hasta él (camino anterior
         # más la nueva acción)
         for (a_ss', s') in vecinos(s)
            push!(bfsCola, (s', push!(camino_s, a_ss') ) )
         end
      end
      return :nopath
end
```

DFS y BFS son lo que se llaman **búsquedas ciegas**.

### <mark style="background: #FFB86CA6;">Búsqueda informada: A*</mark>
A* lo que hace en cada paso básicamente es:
$g(s)=coste(s)+h^*(s)$
Y además guarda un "historial" y mantiene la cola abierta.
Con todo esto, intenta elegir el camino mínimo más óptimo, siendo el final del algoritmo **no el vértice final** sino **cuando el coste es mínimo**.
#### <mark style="background: #D2B3FFA6;">EJERCICIO A*</mark>
$A\rightarrow B:1$
$A\rightarrow D:1$
$A\rightarrow C:1$
$B\rightarrow D:2$
$C\rightarrow D:1$
$C\rightarrow F:2$
$D\rightarrow E:2$
$D\rightarrow F:1$
$E\rightarrow F:1$
$E\rightarrow G:1$
$F\rightarrow G:2$
$G\rightarrow H:2$
$F\rightarrow H:3$

| Nodo        | $A$ | $B$ | $C$ | $D$ | $E$ | $F$ | $G$ | $H$ |
| ----------- | --- | --- | --- | --- | --- | --- | --- | --- |
| **h(Nodo)** | $4$ | $4$ | $3$ | $3$ | $2$ | $3$ | $2$ | $0$ |
 
