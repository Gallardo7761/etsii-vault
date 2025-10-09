# Práctica 1
<hr>

### Métodos principales: 
```python
# Sea G = Graph()/DiGraph()
G.plot(figsize=int, layout='circular/tree', vertex_size=int, vertex_colors=dict, edge_colors=dict)
G.add_vertex(v)/vertices()/delete_vertex(v) # vertices
G.add_edge((src,dst))/edges()/delete_edge((src,dst)) # aristas
G.degree(vertice)/in_degree(vertice)/out_degree(vertice) # valencia/grado
G.neighbors(vertice) # vecinos/adyacentes
G.size() # nº aristas
G.order() # nº vertices
G.complement() # complementario
G.is_isomorphic(G2) # es isomorfo a G2? -> boolean
G.adjacency_matrix() # matriz adyacencia
G.subgraph(vertices) # subgrafo inducido

# Grafo manualmente
G1 = Graph({0:[1,2,3],2:[1,3,4],3:[4]})
G1 = Graph(M) # sea M su matriz de adyacencia
```

### Operaciones con grafos: 
```python
# Sean G1 y G2 grafos
G1 + G2 # G1 U G2
G1.join(G2) # G1 + G2
```

# Práctica 2
<hr>

### Digrafos para juegos (usando el kernel)
1. Digrafo con tantos vértices como posibles posiciones del juego.
2. Las posiciones ganadoras se corresponderán con vértices $\delta({v})=0$

Para obtener el kernel:
```python
# Sea DG = DiGraph()
kernel(DG) # kernel() definido en Practica 2
```

### Nº de independencia y Nº de clique
- **Nº independencia:** tamaño del mayor conjunto independiente de vértices
```python
S = G.independent_set() # mayor conjunto independiente
len(S) # numero de independencia
```
- **Nº de clique:** $n~/~C_n$
```python
G.clique_number() # numero de clique
G.cliques_maximal() # cliques maximales (no contenidos en otros cliques)
G.cliques_maximum() # cliques máximos (mayor cardinal)
```

# Práctica 3
<hr>

### Distancias y matriz de adyacencia
Sea $G=~Graph()$
Sea $M$ la matriz de adyacencia de $G$

Los elementos de $M^n$ indican que hay $m_{ij}$ caminos de longitud $n$ entre $v_i$ y $v_j$
### Distancias y elementos métricos
- **Distancia:** Longitud del camino más corto entre dos vértices.
```python
# Sea G = Graph()
G.distance(v1,v2)
```
- **Excentricidad:** Distancia de un vértice a su vecino más alejado.
```python
# Sea G = Graph()
G.eccentricity(v) # sin parametros -> lista de excentricidades
```
    - *Radio:* Menor excentricidad
    - *Diámetro:* Mayor excentricidad
- **Centro:** Conjunto de vértices cuya excentricidad es igual al radio.
```python
# Sea G = Graph()
G.center()
```
- **Periferia:** Conjunto de vértices de mayor excentricidad.
```python
# Sea G = Graph()
G.periphery()
```

### Conectividad
```python
# Sea G = Graph()
G.is_connected()/is_strongly_connected() # ver si es conexo/fuertemente conexo
G.connected_components()/strongly_connected_components() # lista de componentes conexas / fuertemente conexas
G.connected_component_containing_vertex(v) # a donde esta conectado v
```

### k-Conexión
```python
# Sea G = Graph()
G.vertex_connectivity() # conectividad por vértices
G.edge_connectivity() # conectividad por aristas
G.blocks_and_cut_vertices() # [bloques,vs_corte]
```

# Práctica 4
<hr>

### Árboles
```python
# Sea G = Graph()
G.is_tree() # ver si es arbol -> boolean
G.is_forest() # ver si es bosque -> boolean
```

### DFS, BFS, Kruskal, Dijkstra, digrafo_laberinto
Las funciones más importantes:
- `arbol_dfs(DG,inicial)` -> Árbol en profundidad (estudiar vertices de corte)
- `arbol_bfs(DG, v)` -> Árbol en anchura (camino mas corto desde v a cualquiera. Si G -> G.to_directed() = DG. 
- `digrafo_laberinto(filas,columnas,obstver,obsthor)` -> Digrafo que resuelve el laberinto
    - obstver: lista de columnas que tienen obstaculo a la derecha
    - obsthor: lista de columnas que tienen obstaculo debajo
- `Kruskal(G_ponderado)` -> Arbol de peso minimo
- `Dijkstra(G, inicial)` -> Caminos mas cortos desde el inicial
    
Para resolver un laberinto se usa `digrafo_laberinto` y luego `arbol_bfs`.

# Práctica 5
<hr>

### Multigrafo
```python
# Sea G = Graph()
G.allow_multiple_edges() # indicar que tendra aristas multiples
```

### Grafos Eulerianos
```python
# Sea G = Graph()
G.is_eulerian() # ver si es euleriano -> boolean
G.is_eulerian(path=True) # si False no es semieuleriano. Si lo es, devuelve v_i y v_f del recorrido.
G.eulerian_circuit(path=True) # circuito semieuleriano en caso de que lo sea
```
#### Si no es E ni Semi E, ¿numero de saltos?
Se convierte en multigrafo y se añaden aristas postizas (bucle `for` en Practica 5)

### Grafos Hamiltonianos
```python
# Sea G = Graph()
G.is_hamiltonian() # ver si es hamiltoniano -> boolean
G.hamiltonian_cycle() # ciclo hamiltoniano si G es H
G.hamiltonian_path() # camino hamiltoniano si G es Semi H
```

**DIRAC:** Un grafo de $n$ vértices ($n>3$) es hamiltoniano si cada vértice tiene valencia mayor o igual a $n/2$.
**ORE:** Un grafo de $n$ vértices ($n>3$) es hamiltoniano si la suma de valencias de cada pareja de vértices no adyacentes es mayor o igual que $n$.
### Coloracion vertices
```python
voraz(G,p=[]) # definido en Práctica 5
```
Si un grafo $G$ contiene un clique de $m$, $\chi(G)\geq m$ por tanto $clique(G)\leq\chi(G)$.

### Coloracion aristas
```python
# Sea G = Graph
G.chromatic_index() # numero cromatico por aristas
```
