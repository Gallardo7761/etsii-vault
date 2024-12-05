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
# <mark style="background: #FFF3A3A6;">TEMA 4: Optimización</mark>
A diferencia de las búsquedas como A*, no se tienen todos los nodos hijos disponibles, si no que se escoge uno aleatoriamente. Se intenta minimizar la "energía" de nodo en nodo.
## <mark style="background: #ADCCFFA6;">1. Templado simulado</mark>

![[Pasted image 20241009084536.png|600]]
Este método casi no tiene memoria, ya que al pasar a un vecino "se olvida" de lo anterior, sólo almacena el mejor nodo que ha visto.
1. **Función de energía:** que mide la calidad de una solución.
2. **Temperatura:** nº de saltos que puede dar.
Una posible implementación en pseudocódigo:
```julia
Algoritmo: Templado Simulado (T₀, s₀, N, γ < 1, ϵ > 0)
    T = T₀
    while T > ϵ
        Repeat N:
            s₁ = Genera_vecino(s₀)
            ∆E = E(s₀) − E(s₁)
            if ∆E > 0
                s₀ = s₁
            else
                con probabilidad exp(∆E/T): s₀ = s₁
        T = T ⋅ γ
    return s₀
```
Las iteraciones del algoritmo están perfectamente acotadas ya que dependen de $T$, de $\gamma$, y de $N$.
### Describe de forma general cómo se puede usar templado simulado para un problema de satisfacción de restricciones
:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:·:
### $s=(x_{1}=a_{1},\dots,x_{n}=a_{n})\forall a_{i}\in D_{i}$
### $S=D_{1}\times D_{2}\times\dots\times D_{n}$
### $v(s)=s~cambiada~en~una~posicion~al~azar$
### $E(s)=Nº~restricciones~de~C~que~no~se~cumplen$
## <mark style="background: #ADCCFFA6;">2. Optimización por Enjambre de Partículas (PSO)</mark>
Se tienen $N$ partículas denotadas $\{1,...,N\}$ donde cada partícula tiene una posición $x_{i}$
y una velocidad $v_{i}$. 
1. Cada partícula es atraída hacia la mejor localización que ella, personalmente, ha encontrado: $x_{i}^{pb}$ (componente cognitivo)
2. Cada partícula es atraída hacia la mejor localización que ha sido encontrada **globalmente**: $x^{gb}$ (componente social)

![[Pasted image 20241011121847.png|400]]
La fuerza con que las partículas son empujadas a cada dirección se basa en la atracción a $x_{i}^{pb}$ y $x^{gb}$.
# <mark style="background: #FFF3A3A6;">TEMA 5: Búsquedas con incertidumbre</mark>
### <mark style="background: #FFB86CA6;">Juego formal</mark>
Un juego formal es una variante de un espacio de estados $(S,P,A,T,g,U)$ donde:
- $S:$ es el conjunto de estados comenzando por el estado inicial $s_{o}\in S$
- $P:$ es el conjunto de jugadores $P=\{1,\dots ,n\}$
- $A:$ son las acciones
- $T:$ es la función de transferencia $T:S\times A\rightarrow S$, que indica cómo cambian los estados.
- $g:$ indica si un estado del juego es terminal (goal) $g:S\rightarrow\{true,false\}$
- $U:$ es una función de utilidad $U:S\times P\rightarrow \rm I\! R$ de estados terminales que indica lo bueno que es un estado terminal para cada jugador.
## <mark style="background: #ADCCFFA6;">1. Algoritmo Minimax</mark>
Los pasos son:
1. Generar el árbol de juego a partir del estado actual hasta llegar a un estado terminal.
2. Se calculan los valores de la función de evaluación para  cada nodo terminal.
3. Se evalúan los nodos superiores a partir del valor de los inferiores. Según si estos pertenecen a un nivel MAX o MIN, se elegirán los valores mínimos y máximos representando los valores del jugador y del oponente.
4. Se repite el paso 3 hasta llegar al nodo superior (estado actual).
5. Se selecciona la jugada-nodo directamente accesible desde el actual que maximiza el valor de la evaluación.
![[minimax-algorithm-animation.gif]]
El problema de esta búsqueda es que el número de estados puede ser exponencial. Si $r$ es cuántos hijos tiene cada nodo y $m$ el nivel de profundidad, la complejidad en tiempo es del orden $O(r^m)$ y en espacio del orden $O(rm)$. 
### <mark style="background: #FFB86CA6;">Poda alfa-beta</mark>
La idea es que cada nodo se analiza teniendo en cuenta el valor que por el momento tiene el nodo y el valor que por el momento tiene su padre, lo que determina en cada momento un intervalo $(\alpha,\beta)$ de posibles valores que podría tomar el nodo. El intervalo tiene un significado depende del nodo:
- En nodos $MAX$: $\alpha$ es el valor del nodo actual (que tendrá ese valor o superior) y $\beta$ es el valor del padre (que tendrá ese valor o inferior).
- En nodos $MIN$: $\beta$ es el valor del nodo actual (que tendrá ese valor o inferior) y $\alpha$ es el valor del padre (que tendrá ese valor o superior).
La poda se produce si en algún momento $\alpha\geq\beta$ y en ese momento ya no se analizan los sucesores restantes.
## <mark style="background: #ADCCFFA6;">2. Monte Carlo Tree Search</mark>
### <mark style="background: #FFB86CA6;">Problema de las tragaperras múltiples</mark>
Hay $K$ máquinas y cada una suelta una "riqueza" cada vez que se juega. Las riquezas son variables aleatorias $\{X_{i,n}:1\leq i\leq K,n~\geq 1\}$, donde $i$ es el índice de cada máquina y la $n$ la tirada.

En estas condiciones una **estrategia (policy)** es un algoritmo $A$ que elige la siguiente máquina basándose en la secuencia de jugadas anteriores y los premios recibidos. Si $T_i(n)$ es el número de veces que el algoritmo $A$ ha seleccionado la máquina $i$ durante las primeras $n$ jugadas, la pérdida por no haber elegido siempre la mejor máquina posible viene dada por:
$$
\begin{equation}
\mu^*n-\sum\limits_{i=1}^K{\mu_{i}E[T_{i}(n)]} 
\end{equation}
$$
donde $\mu^*=max_{1\leq i\leq K}\mu_{i}$ 

Se puede usar una estrategia, llamada **UCB (Upper Confidence Bound)** que construye intervalos estadísticos para cada máquina, que es esencialmente la confianza basada en la media de ganancias dentro de un radio dado por $\sqrt{\frac{2\ln{n}}{T_i(n)}}$.
### <mark style="background: #FFB86CA6;">Aplicación a búsquedas</mark>

![[mcts.png|600]]
1. **Selección:** se realiza mientras tengamos las estadísticas necesarias para tratar cada estado alcanzado como un problema de tragaperras múltiples. Comenzando por el raíz se selecciona recursivamente el estado más urgente (de acuerdo a UCB) hasta alcanzar un estado terminal o que no está completamente extendido.
2. **Expansión:** para cuando ya no se puede aplicar Selección. Se elige aleatoriamente una posición sucesora no visitada y se añade un nuevo estado al árbol de estadísticas.
3. **Simulación:** partiendo del estado recién añadido se simula una partida completa, ya sea al azar o con una heurística. Se obtiene un valor (premio, recompensa, etc) que determina la utilidad de esa rama para el jugador.
4. **Actualización:** con el estado final alcanzado de la fase anterior se actualizan las estadísticas de todas las posiciones previas visitadas durante la simulación que se ejecutó a partir del nuevo estado (incluyendo la cuenta de ganancias). 
![[mcts.gif]]

# <mark style="background: #FFF3A3A6;">TEMA 6: Fundamentos de ML</mark>
Básicamente se trata de crear algoritmos capaces de generalizar comportamientos y reconocer patrones a partir de unos datos de entrada. De la forma:
$$
\begin{equation}
f:D\rightarrow r
\end{equation}
$$
sea $f$ una función que dados unos datos $D$ da un resultado $r$. Hace falta saber, **cómo** se manipulan los datos, **qué** datos se cogen y cómo calcular los **errores**. ML es una mezcla de técnicas de álgebra (para representación vectorial de los parámetros de $f$) y optimización.
### <mark style="background: #FFB86CA6;">Aprendizaje supervisado</mark>
Se trata de $D\rightarrow ML\rightarrow f~aprox~D$ (o minimizar el error entre $D$ y $f$). Los datos se suelen dividir en dos bloques, uno de **entrenamiento** ($D_{train}$), y otro de **validación** ($D_{val}$) para calcular el error empírico (ya que esos datos "no los ha visto"). También puede haber un tercer bloque de **test** ($D_{test}$) para usarse luego de repetir varias veces el entrenamiento.

# <mark style="background: #FFF3A3A6;">TEMA 8: Redes neuronales</mark>

Dado un dataset $D=\{(\vec{x},y)\}$ hay que encontrar una función $f\rightarrow f(\vec{x})~\textasciitilde~y~\forall~(\vec{x},y)\in D$. Hay dos posibles espacios:
- **Lineal:** $f(x)=mx+n$
- **Polinómica:** $f(x)=a_0+a_1x+a_2x^2+\dots+a_nx^n$ que se puede aproximar mediante sus parámetros $(a_0,a_1,a_2,\dots,a_n)$
### <mark style="background: #FFB86CA6;">Funcionamiento básico</mark>
![[neurona.gif]]
1. **Entrada de datos:** La capa de entrada recibe un vector numérico.
2. **Peso y sesgo:** Cada conexión entre las neuronas de una capa y la siguiente tiene un peso asociado, y cada neurona de la capa siguiente tiene un sesgo (constante ajustable)
3. **Suma ponderada:** En cada neurona consideramos la suma ponderada de los valores de las neuronas entrantes a ella, así como de su sesgo.
4. **Función de activación:** Se aplica una función no lineal sobre los valores que tiene como objetivo cambiar los valores de manera no lineal.
5. **Capa de salida:** Los valores calculados en la capa de salida son el cálculo final.
![[Pasted image 20241115112854.png|400]]
Para esta red, que es muy simple, hacen falta **44 parámetros** (32 pesos y 12 sesgos).
## <mark style="background: #ADCCFFA6;">1. Teorema de aproximación universal</mark>
Supongamos:
- $K\subset ℝ^d$
- $f$ una función coste arbitraria en $C(K,ℝ)$
- $\epsilon\in ℝ^d$ arbitrario
- $\sigma$ función continua no constante, acotada y creciente
Entonces:
$\exists n\in N / b_i\in ℝ,~v_i\in ℝ~y~w_i\in ℝ^d~\forall i\in \{1,\dots,d\}$
tales que se cumple la desigualdad:
$\max\limits_{x\in K}|φ(x)-f(x)|<\epsilon$
## <mark style="background: #ADCCFFA6;">2. Funciones de coste</mark>
### <mark style="background: #FFB86CA6;">Error/coste cuadrático</mark>
$C_2(W,b):=\frac{1}{2|T|}\sum\limits_{x\in T}{||y(x)-a(x)||_2^2}$ 
Encontrar la mejor red es encontrar la red con el **menor error posible**.
## <mark style="background: #ADCCFFA6;">3. Reconocimiento de dígitos</mark>

# <mark style="background: #FFF3A3A6;">TEMA 9: Algoritmos de clustering</mark>
Básicamente un algoritmo de clustering tiene como objetivo agrupar los datos de un dataset según la similaridad entre ellos, de forma que los que estén en un mismo grupo (cluster) sean más similares que los que no estén en dicho grupo.
## <mark style="background: #ADCCFFA6;">1. Axiomatización del clustering</mark>

**Distancias**
Normalmente, para poder hablar de similaridad se suele acudir a algún tipo de distancia.
Sea $X=\{1,\dots,n\}$ un conjunto de datos. Una función $d:X\times X\rightarrow \rm I\! R$ es una función distancia si:
- $d(x_i,x_i)\geq 0~\forall~x_i\in X$
- $d(x_i,x_j)>0\iff x_i\neq x_j~\forall~x_i,x_j\in X$
- $d(x_i,x_j)=d(x_j,x_i)~\forall~x_i,x_j\in X$

**k-Clustering**
Un k-Clustering, $C=\{C_1,\dots,C_k\}$ de $X$ es una k-partición de $X$. $C_i\cap C_j=\emptyset$ para $i\neq j$ y $\cup^{k}_{i=1}{C_i}=X$ 

### <mark style="background: #FFB86CA6;">Axiomas</mark>
- **Invarianza por Escala:** Un algoritmo de clustering no debe dar resultados distintos si se aplica una escala al conjunto de puntos.
- **Consistencia:** Un algoritmo no debe dar resultados distintos si las distancias dentro de cada cluster se reducen o si las distancias entre clusters aumentan.
- **Riqueza:** La función de agrupación debe ser lo suficientemente flexible como para producir _cualquier_ partición arbitraria del conjunto de datos de entrada.
## <mark style="background: #ADCCFFA6;">2. Clasificación de algoritmos de clustering</mark>
Según la forma en que los clusters se relacionan entre si y con los objetos del dataset, podemos hacer una primera clasificación:
- **Clustering Blando:** los puntos pertenecen a los clusters según un grado de pertenencia.
- **Clustering Duro:** cada punto sólo pertenece a un solo cluster.
Pero hay clasificaciones más finas como:
- **Partición estricta:** cada punto pertenece exactamente a un cluster.
- **Partición estricta con outliers:** puede haber puntos que no pertenezcan a ningún cluster (outliers).
- **Clustering con superposiciones:** un punto puede pertenecer a más de un cluster.
- **Clustering jerárquico:** los clusters se ordenan jerárquicamente de forma que los puntos de un cluster también están en el cluster padre.
## <mark style="background: #ADCCFFA6;">3. Principales algoritmos</mark>
### <mark style="background: #FFB86CA6;">K-medias (modelo de centroides)</mark>
![[Sin título.png|300]]
Donde colocar los $k$ centroides para dividir en $k$ bolsas. El algoritmo intenta minimizar la función potencial:
$$
\begin{equation}
\sum_i\sum_j d(x_{ij},~c_i)^2
\end{equation}
$$
Un pseudo-código para k-medias podría ser:
```
K-MEANS(D,K) [D: Dataset, K > 0]
    C = {c1,...,ck} ⊆ D (cualesquiera)
    Asignar Di = {P ϵ D: d(P,ci) < d(P,cj), j ≠ i}, i = 1...K
    Mientras algún ci cambie:
        Calcular ci = centroide(Di), i=1...K
        Recalcular Di, i = 1...K 
    Devolver {D1,...,DK}
```
K-medias es aceptable si se pueden dividir las bolsas mediante rectas, en cuanto no se pueda, empieza a fallar.
### <mark style="background: #FFB86CA6;">DBSCAN (modelo de densidad discreto)</mark>
![[Pasted image 20241127091731.png|300]]

Se basa en densidades para calcular las bolsas, y marca como outliers aquellos puntos que no superan un umbral designado. El algoritmo recibe dos datos de entrada que determinan la densidad mínima:
- $\epsilon$ que fija el radio de los entornos que se mirarán alrededor de los puntos
- $MinPts$ que establece el número mínimo de puntos que debe haber en el entorno para que se considere que supera el umbral.
Un posible pseudo-código:
```
DBSCAN(D, ε, MinPts) [D: Dataset; ε > 0; MinPts >0]
    C = ∅
    Para cada P ϵ D no visitado:
        Marcar P como visitado 
        N = E(P, ε) 
        Si tamaño(N) < MinPts:
            marcar P como OUTLIER 
        si no:
            C = C ⋃ {creaCluster(P, N, ε, MinPts)}
    Devolver C 
```
Donde:
```
creaCluster(P, N, ε, MinPts) [P ϵ D; N entorno de P; ε > 0; MinPts > 0]
    Cl = {P}
    Para cada P' ϵ N:
        Si P' no ha sido visitado:
            marcar P' como visitado
            N' = E(P', ε)
            Si tamaño(N') ≥ MinPts:
                N = N ⋃ N'
        Si P' no está en ningún cluster:
            Cl = Cl ⋃ {P'}
    Devolver Cl
```
### <mark style="background: #FFB86CA6;">AGNES (modelo jerárquico)</mark>
![[Pasted image 20241127091902.png|300]]
Va construyendo una jerarquía creciente de clusters desde los más pequeños hasta el mayor cluster posible. Un posible pseudo-código:
```
AGNES(D,N) [D: Dataset, N > 0]
    Cls = { {P} : P ϵ D} (un cluster unitario para cada P ϵ D)
        Mientras |Cls| > N:
            Sean C1, C2 en Cls: d(C1,C2) = min {d(Ci,Cj): Ci,Cj ϵ Cls}
            C12 = C1 ⋃ C2
            Cls = Cls - {C1,C2} ⋃ {C12}
    Devolver Cls
```
El algoritmo va midiendo las distancias entre los propios clusters, llamadas Métodos de Conexión. Los más habituales son:
1. **Conexión completa:** $d_{max}(C_1,C_2)=max\{d(x_1,x_2):x_1\in C_1,~x_2\in C_2\}$
2. **Conexión simple:** $d_{min}(C_1,C_2)=min\{d(x_1,x_2):x_1\in C_1,~x_2\in C_2\}$
3. **Conexión media:** $d_{mean}(C_1,C_2)=mean\{d(x_1,x_2):x_1\in C_1,~x_2\in C_2\}$
4. **Conexión centroide:** $d_{cent}(C_1,C_2)=d(c_1,c_2),~donde~c_i=centroide(C_i)$
## <mark style="background: #ADCCFFA6;">4. Métricas para clustering</mark>
### <mark style="background: #FFB86CA6;">Medidas de calidad</mark>
- **Validación interna:** no requieren información adicional, solo miden haciendo uso de mediciones que se pueden obtener de los mismos puntos originales del conjunto. Por ejemplo el potencial o el índice de Davies Bouldin son ejemplos de validaciones internas.
  $$
\begin{equation}
I_{DB}=\frac{mean\{d(x,x'):x,x'\in c\}_{c\in C}}{mean\{d(x,x'):x\in c,x'\in c\}_{c\notin C}}
\end{equation}
  $$
- **Validación externa:** se debe construir una matriz de confusión 2x2 que indique los falsos positivos y negativos y los 'true' positivos y negativos. Teniendo en cuenta esto, hay varias medidas que se pueden usar:
	- Precisión: $Pr=\frac{T_P}{T_P+F_P}$
	- Recall: $R=\frac{T_P}{T_P+F_N}$
	- F-métrica: $F=\frac{Pr*R}{Pr+R}$
	- Pureza: $U=\sum_i{p_i(\max_j{\frac{p_{ij}}{p_i}})}$  
# <mark style="background: #FFF3A3A6;">TEMA 10: Árboles de decisión</mark>
Un árbol de decisión está formado por un conjunto de nodos de decisión (interiores) y de nodos-respuesta (hojas):
- Un nodo de decisión se asocia a uno de los atributos y tiene 2 o más ramas hacia sus posibles valores.
- Un nodo-respuesta está asociado a la clasificación que se quiere proporcionar, y devuelve la decisión respecto a la entrada.
## <mark style="background: #ADCCFFA6;">1. ID3</mark>
### <mark style="background: #FFB86CA6;">Entropía</mark>
$$
\begin{equation}
E(S)=-P\log_2(P)-N\log_2(N)
\end{equation}
$$
donde $P$, $N$ y $S$ son la proporción de ejemplos positivos y negativos y el conjunto de muestras respectivamente

### <mark style="background: #FFB86CA6;">Aplicación de la entropía a ID3</mark>
Se puede calcular la entropía asociada a cada atributo $X$ de la siguiente forma:
$$
\begin{equation}
E(T,X)=\sum\limits_{c\in X}{p(c)E(T,D_c)}
\end{equation}
$$
Y la ganancia (atributo que mejor responde) de información que aportaría dividir respecto a los valores de ese atributo viene dada por:
$$
\begin{equation}
Gain(T,X)=E(T,D)-E(T,X)
\end{equation}
$$
## <mark style="background: #ADCCFFA6;">2. Extensiones de ID3</mark>
El algoritmo ID3 tiene algunos problemas:
- Está bien definido para atributos con un rango finito de valores, pero lo está para atributos que tienen, por ejemplo, un rango continuo (infinito).
- No siempre da el mejor árbol posible, ya que el algoritmo es voraz y optimiza cada caso por separado (mejor resultado local).
- No se comporta bien con ejemplos para los que no se conoce algún valor de algún atributo.
### <mark style="background: #FFB86CA6;">Tratamiento de información faltante</mark>
La solución más directa es hacer uso de los ejemplos en los que el atributo que se está estudiando **SI** tiene un valor conocido.
### <mark style="background: #FFB86CA6;">Tratamiento de atributos con datos continuos</mark>
- Si el atributo depende de varias dimensiones, o no es un conjunto ordenado, se puede aplicar clusterización para poder saber en que clusters se puede dividir de forma eficiente y entonces usar como pregunta la pertenencia a un clúster u otro.
- El otro caso, es más sencillo: se consideran $N$ preguntas siendo $N$ el número de ejemplos (y estos toman los valores $\{v_1,\dots v_n\}$).