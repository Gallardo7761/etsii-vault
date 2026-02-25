# <mark style="background: #FFF3A3A6;">Introducción a la Recuperación de Información y Sistemas de Recomendación</mark>
## <mark style="background: #ADCCFFA6;">Conceptos básicos</mark>
La **Recuperación de Información (IR)** trata de representar, almacenar y organizar datos no estructurados. Es básicamente el proceso de buscar información en una colección de documentos mediante una **consulta (query)**. Hay dos paradigmas:
- **Recuperación:** tenemos clara la información que queremos recuperar y vamos a la página de inmediato.
- **Búsqueda:** hay que "navegar" hasta encontrar lo que queremos.
## <mark style="background: #ADCCFFA6;">Recuperación de la Información</mark>
- **Primera generación:** modelos booleanos, vectoriales y probabilísticos. Ej: Lycos.
- **Segunda generación:** toda la web es un grafo donde los vértices son los documentos y las aristas enlaces entre ellos. Ej: Google.
- **Tercera generación:** Open research
### <mark style="background: #FFB86CA6;">Conceptos básicos</mark>
- Hay una **necesidad de información** que se expresa como una query en **formato libre**.
- La query **codifica** la información.
- La query es un **documento** que se comparará con los documentos en la colección.
- **Efectividad** VS **Eficiencia**
- Funciones de similaridad
- Buscar **secuencial** o **paralelo**?
### <mark style="background: #FFB86CA6;">Taxonomía de la búsqueda web</mark>
Las queries se pueden clasificar en 3 clases:
- **Navegacional:** quiero ir a una web y no sé la URL, busco una palabra y navego para encontrar la web (~20%).
- **Informacional:** quiero adquirir información de una página en la que sé que existe (~50%).
- **Transaccional:** quiero realizar una actividad comercial en una web (~30%).
### <mark style="background: #FFB86CA6;">Problemas</mark>
-  Colección de datos **muy** grande que es dinámica, auto-organizada y hiperenlazada. 
- Consultas **muy cortas**.
- Usuarios **no sofisticados** en tecnología.
- **Dificultad para juzgar la relevancia** y rankear resultados.
- **Sinonimia** y **ambigüedad**.

A la hora de elegir una estrategia, depende de varios factores:
- La **naturaleza** de la información que se quiere buscar
- La **estructura** del **contenido del repositorio**
- Las **herramientas de búsqueda** disponibles
- La **habilidad** del usuario para usar herramientas de búsqueda
### <mark style="background: #FFB86CA6;">Búsqueda de información y toma de decisiones</mark>
Están estrictamente relacionadas. O bien buscamos información para tomar decisiones, o tomamos decisiones respecto a qué información consideramos o cuándo parar de buscar (debido a la sobrecarga de información de la web).
## <mark style="background: #ADCCFFA6;">Sistemas de Recomendación</mark>
Se basa en 3 cosas:
- **Predicción de puntuación:** el sistema debe ser capaz de puntuar un item que el usuario no ha puntuado (númerica: regresión, discreta: clasificación).
- **Ranking:** el sistema debe ser capaz de calcular una puntuación para cada item y entonces rankearlo respecto a esa puntuación.
- **Selección:** el sistema debe de disponer de un modelo que seleccione los $N$ items más relevantes.
### <mark style="background: #FFB86CA6;">Sistemas colaborativos</mark>
El sistema intentará **predecir** la opinión que tendrá el usuario en diferentes items y será capaz de recomendar "el mejor" item basado en los **gustos previos del usuario** y de **otros usuarios parecidos a este**.
#### <mark style="background: #D2B3FFA6;">Matriz de rating</mark>
![[Pasted image 20260217115003.png|450]]
#### <mark style="background: #D2B3FFA6;">Sistemas colaborativos y Google</mark>
Los motores de búsqueda no son sistemas de recomendación, PERO, tienen varias similaridades:
- Los dos **rankean** items
- El ranking se basa en las **opiniones de usuarios**

Para rankear páginas se cuentan los **inlinks** o enlaces que referencian dichas páginas. Las páginas no son igual de importantes.

El voto de cada link es **proporcional** a la importancia de la **página fuente**. Si la página $P$ con importancia $x$ tiene $n$ enlaces de salida, cada enlace obtiene $x/n$ votos.
#### <mark style="background: #D2B3FFA6;">Sistemas de Recomendación VS Motores de búsqueda</mark>
- Los SR cogen técnicas de los IR (filtro basado en contenido por ejemplo). 
- Los IR tratan con **repositorios grandes de datos no estructurados** mientras que los SR están enfocados a **tópicos únicos**. 
- Los IR están recibiendo cada día más **personalización**. 
- Los IR **localizan contenido relevante** para el usuario, que debería ser capaz de evaluar dicha relevancia.
- Los RS **diferencian contenido relevante** para el usuario, que no tiene conocimiento para evaluar la relevancia.
# <mark style="background: #FFF3A3A6;">Recuperación de información booleana</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
Usar un modelo booleano significa que la información necesita ser traducido a operadores booleanos.
**Colección:** conjunto de documentos.
**Objetivo:** recuperar documentos con información relevante para las necesidades del usuario y que lo ayuda a completar una tarea.
### <mark style="background: #FFB86CA6;">Precisión y cobertura</mark>
**Precisión:** fracción de los documentos recuperados que son relevantes.
**Cobertura:** fracción de los documentos relevantes que se recuperan.
![[Pasted image 20260224110141.png|500]]
### <mark style="background: #FFB86CA6;">Matriz de incidencia término-documento</mark>

|               | Antony and Cleopatra | Julius Caesar | The Tempest | Hamlet | Othello | Macbeth |
| ------------- | -------------------- | ------------- | ----------- | ------ | ------- | ------- |
| **Antony**    | 1                    | 1             | 0           | 0      | 0       | 1       |
| **Brutus**    | 1                    | 1             | 0           | 1      | 0       | 0       |
| **Caesar**    | 1                    | 1             | 0           | 1      | 1       | 1       |
| **Calpurnia** | 0                    | 1             | 0           | 0      | 0       | 0       |
| **Cleopatra** | 1                    | 0             | 0           | 0      | 0       | 0       |
| **mercy**     | 1                    | 0             | 1           | 1      | 1       | 1       |
| **worser**    | 1                    | 0             | 1           | 1      | 1       | 0       |
Tenemos un vector booleano par cada término. 
#### <mark style="background: #D2B3FFA6;">Para colecciones más grandes...</mark>
Con $N = 1\times 10^6$ de documentos de ~$1000$ palabras (6GB de datos considerando cada palabra 6B). Suponemos $M = 500\times 10^3$ términos distintos. La matriz sería de $N\times M$.
## <mark style="background: #ADCCFFA6;">2. Índice invertido</mark>
Para cada término $t$, debemos almacenar una lista de documentos que contengan $t$. Como lista asociada a $t$ se suele usar una **lista enlazada**.
### <mark style="background: #FFB86CA6;">Construcción del índice</mark>
1. **Tokenizar:** "limpiar" el documento para quedarnos con _tokens_ que no son más que palabras o términos libres de signos de puntuación.
2. **Módulos lingüísticos:** los hay más complejos y más simples (_stemmer_). Un ejemplo podría ser poner todo en minúsculas, quitar plurales, etc.
3. **Indexar:** se añaden nodos (**docID**) a la lista enlazada asociada a $t$ que serían los documentos donde aparece $t$. Los pasos para indexar son:
	1. Tokenizar.
	2. Ordenar.
	3. Construir el diccionario de términos: lista enlazada de postings.
### <mark style="background: #FFB86CA6;">Haciendo queries</mark>
Si queremos un documento en el que salgan **Brutus** y **Caesar** (Brutus AND Caesar), el algoritmo pone un puntero al inicio de las dos listas enlazadas, si coinciden los **docID** se añaden los dos y se incrementan los punteros, y si no, se incrementa el más pequeño.
![[Pasted image 20260224112101.png|300]]
#### <mark style="background: #D2B3FFA6;">Optimización de queries</mark>
Si se necesita hacer la AND entre varios términos, se empieza con el que tenga la lista enlazada de postings más pequeña. En caso de las OR, se estima el tamaño sumando las frecuecias de los documentos.
### <mark style="background: #FFB86CA6;">Qué no puede hacer la RI booleana</mark>
- No se pueden buscar frases. Ej: Universidad de Sevilla.
- Proximidad: PALABRA1 cerca de PALABRA2
- Zonas: (autor = JKR) AND (text contains Harry Potter)
## <mark style="background: #ADCCFFA6;">3. Mejora del algoritmo merge pointers</mark>
Se pueden usar punteros de salto.
![[Pasted image 20260224113246.png|400]]
La idea es poder avanzar varios postings si uno de los dos punteros es muy pequeño en comparación al otro. Hay que balancear, si ponemos muchos punteros de salto podemos saltar muchas veces pero habría demasiadas comparaciones. Si ponemos pocos hay menos comparación pero los saltos son menos exitosos debido a la distancia entre postings. Una heurística común para postings de longitud $L$ es $\sqrt(L)$.
## <mark style="background: #ADCCFFA6;">4. Estructuras de datos para índices invertidos</mark>
### <mark style="background: #FFB86CA6;">Tablas hash</mark>
Cada término se hashea. La búsqueda es más rápida que en un árbol ($O(1)$), pero:
- No hay forma de encontrar variantes (judgement/judgment).
- No se puede buscar prefijos
- Si el vocabulario crece, necesitamos **rehashear todo**.
### <mark style="background: #FFB86CA6;">B-Trees</mark>
Cada vértice tiene un número de hijos en el intervalo $[a,b]~~a,b\in N$. Resuelven el problema de buscar por prefijo. Es más lento $O(log~M)$ al buscar y además hay que rebalancear el árbol.
# <mark style="background: #FFF3A3A6;">Recuperación de información vectorial</mark>
## <mark style="background: #ADCCFFA6;">1. Introducción</mark>
Es un buen sistema para usuarios expertos que sepan usar operadores lógicos para formar expresiones complejas en sus queries. El modelo ranked/vectorial tiene varias ventajas:
- El sistema devuelve unos resultados ordenados respecto a la query hecha.
- Se suelen hacer las queries en formato de texto libre **PERO NO ES OBLIGATORIO**.
## <mark style="background: #ADCCFFA6;">2. Estableciendo el ranking</mark>
Necesitamos aplicar una puntuación a los documentos en función de cuanto se parezca a la query del usuario. Hay varias formas de asignar puntuación.
### <mark style="background: #FFB86CA6;">1. Coeficiente de Jaccard</mark>
Es una medida comúnmente usada de intersección de dos conjuntos $A$ y $B$.
$$
jaccard(A,B)=|A\cap B|~~/~~|A\cup B|
$$
$$
jaccard(A,A) = 1
$$
$$
jaccard(A,B)=0~~if~~A\cap B =0
$$
#### <mark style="background: #D2B3FFA6;">Problemas</mark>
- No tiene en cuenta la frecuencia del término en el documento.
- Términos **raros** son más informativos que los comunes, y tampoco los tiene en cuenta. 
- Necesitamos normalizar.
### <mark style="background: #FFB86CA6;">2. Modelo bolsa de palabras</mark>
No tiene en cuenta el orden. _John is quicker than Mary_ y _Mary is quicker than John_ tendrían los mismos vectores.
## <mark style="background: #ADCCFFA6;">3. Frecuencias</mark>
### <mark style="background: #FFB86CA6;">Frecuencia del término tf</mark>
La frecuencia del término $tf_{t,d}$ de un término $t$ en un documento $d$ está definida por el número de veces que $t$ aparece en $d$.

Se suaviza con el logaritmo:
$$
w_{t,d}=
\begin{cases}
    1+\log{tf_{t,d}}~,~~~~\text{si}~~ tf_{t,d} > 0 \\
    0,~~~~\text{e.o.c}
\end{cases}
$$

### <mark style="background: #FFB86CA6;">Frecuencia del documento df</mark>
La frecuencia del documento del término $t$, $df_t$ es el número de documentos que contienen a $t$. Es una medida inversa de la "informatividad" de $t$ (cuanto más pequeña mejor). $df_t \leq N$. Definimos la **frecuencia inversa del documento** de $t$ como:
$$
idf_t=\log{(\frac{N}{df_t})}
$$
#### <mark style="background: #D2B3FFA6;">Efecto del idf en el ranking</mark>
No tiene efecto en el ranking, es el mismo para todas las palabras.
### <mark style="background: #FFB86CA6;">Peso tf-idf</mark>
$$
W_{t,d}=(1+\log{tf_{t,d}})\times\log{(\frac{N}{df_t})}
$$
### <mark style="background: #FFB86CA6;">Puntuación</mark>
$$
Score(d,q)=\sum_{t\in q\cap  d} tf_{t,d}\times idf_t
$$
## <mark style="background: #ADCCFFA6;">4. Documentos y queries como vectores</mark>
Tenemos un espacio vectorial |V|-dimensional. Son vectores muy dispersos con muchos 0's. Los términos son los ejes del espacio y los documentos son puntos o vectores en este espacio.

Hacemos lo mismo con las queries, representarlas como vectores en el espacio.
### <mark style="background: #FFB86CA6;">Formalizando la proximidad entre vectores en el espacio</mark>
#### <mark style="background: #D2B3FFA6;">Distancia Euclídea</mark> 
No es buena idea puesto que si dos vectores se superponen (porque son el mismo documento) pero tienen distinto módulo va a salir que hay distancia de un documento consigo mismo.
#### <mark style="background: #D2B3FFA6;">Mejor: usar el ángulo</mark>
Para usar el ángulo tenemos que normalizar los vectores volviéndolos unitarios y entonces podemos comparar la proximidad mediante el ángulo entre ellos.