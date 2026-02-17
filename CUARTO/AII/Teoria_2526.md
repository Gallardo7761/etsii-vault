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