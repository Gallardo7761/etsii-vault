# <mark style="background: #FFF3A3A6;">TEMA 9: Programación asíncrona y buenas prácticas</mark>
## <mark style="background: #ADCCFFA6;">1. Programación síncrona/asíncrona</mark>
JavaScript es _single-threaded_ y en principio síncrono. También puede tener comportamiento asíncrono (para por ejemplo consultar APIs). Soluciones asíncronas: callback, promises, async/await...
- **Comunicación síncrona (o bloqueante)**:
  Cuando hay comunicación síncrona entre dos entes A y B, si A envía un mensaje a B, deja de actuar hasta que B responde.
- **Comunicación asíncrona (o no bloqueante)**:
  Cuando hay comunicación asíncrona entre dos entes A y B, si A envía un mensaje a B, puede seguir actuando y B responderá cuando pueda.
### <mark style="background: #D2B3FFA6;">- Funciones callback</mark>
Una función callback es una función que se pasa como parámetro. 
![[Pasted image 20240312153525.png|500]]
Generalmente se usan para definir el código asociado a un evento (por ejemplo un botón cuando se haga click en él).
![[Pasted image 20240312153619.png|450]]
### <mark style="background: #D2B3FFA6;">- Promises</mark>
Antes se usaba un objeto tipo Promise para gestionar llamadas asíncronas. La promesa llamaba a una función callback ("`then`") cuando la operación asíncrona finalizaba con éxito, y a otra diferente ("`catch`") si fallaba. Como no es muy intuitivo, se introdujo en 2017 `async/await`.
### <mark style="background: #D2B3FFA6;">- Async/await</mark>
![[Pasted image 20240312154046.png]]
En un comportamiento síncrono, hasta que la API no responda no carga la página completa y se queda bloqueada "cargando". 
Con **async/await**, se puede solucionar esto convirtiendo las funciones en asíncronas. Las funciones marcadas con **async** se ejecutarán de forma paralela al hilo principal. 
![[Pasted image 20240312154300.png|500]]
Usando **await** se espera a la respuesta de la API, y se convierte en bloqueante, pero `loadPhotos()` sigue siendo asíncrona.
![[Pasted image 20240312154500.png|500]]
## <mark style="background: #ADCCFFA6;">2. Buenas prácticas</mark>
### - Consideraciones generales
- Usar siempre `;` tras cada instrucción.
- Usar `let`  para declarar variables en vez de `var`;
- Usar notación camelCase.
- Usar `{}` en lugar de `new Object()` y `[]` en lugar de `new Array()`.
- Usar siempre el mismo tipo de comillas, salvo casos excepcionales.
- Usar siempre la misma unidad de identación (4 espacios/1 tab).
- No confundir `for...of` para iterar arrays con `for...in` para iterar claves.
- Usar el comparador de igualdad `===` en lugar de `==` (y `!==` en vez de `!=`).
### - Modo estricto
- Usar siempre el modo estricto (`"use strict";`):
	- Impide usar variables no declaradas.
	- Impide que se pueda cambiar valores de elementos NaN o Infinity.
	- Impide definir un Object con valores repetidos.
	- Convierte en errores cosas que generalmente serían warnings.
### - Puntos de entrada
- Usar una función de entrada `main()`, que deberá ser **async** si tiene llamadas a API u operaciones asíncronas.
  ![[Pasted image 20240312155414.png|500]]
### - Módulos
Los módulos permiten importar y exportar funciones y variables de otros módulos. Se recomienda su uso ya que, si no, todo lo que sea definido globalmente en un archivo JS será accesible **desde cualquier otro archivo**.
Sólo es necesario enlazar un script (B) en el HTML ya que las sentencias import/export se encargan de cargar automáticamente el resto de archivos (A u otros) JS referenciados.
### - Organización del código
![[Pasted image 20240312155900.png|150]]
- La carpeta **js** contendrá todo el código JS incluidas las vistas (controladores para archivos html, ej. index.html -> index.js).
- La carpeta **api** contiene los módulos que se encargan de la comunicación con los endpoints de la API.
- La carpeta **libs** contiene librerías JS externas que usemos.
- La carpeta **renderers** contiene módulos para transformar objetos JS en entidades HTML.
- La carpeta **utils** contiene módulos con funciones de utilidad (como `include()`, o `parseHTML()`).
- La carpeta **validators** contiene módulos cuya función es validar datos recibidos en forms.
# <mark style="background: #FFF3A3A6;">TEMA 10: Manipulación del DOM y renders</mark>
## <mark style="background: #ADCCFFA6;">1. Modelo DOM (Document Object Model)</mark>
Modelo que se crea al cargar una página en el navegador. Contiene una jerarquía de elementos en forma de árbol. Con JS se puede:
- Cambiar los elementos HTML de la página 
- Cambiar los valores de los atributos de dichos elementos 
- Cambiar los estilos CSS asociados a los elementos Eliminar elementos y atributos existentes en el HTML 
- Añadir nuevos elementos y atributos HTML 
- Ejecutar funciones y scripts en respuesta a los eventos originados en los elementos HTML 
- Crear nuevos manejadores de eventos 
- Eliminar manejadores de eventos
En el DOM cada objeto es un nodo (_node_). Cada nodo tiene padre, excepto el raíz. Cada nodo puede tener $n$ hijos. Si dos nodos tienen el mismo padre son hermanos.
![[Pasted image 20240319154406.png|500]]
## <mark style="background: #ADCCFFA6;">2. Manipulación del DOM</mark>
DOM ofrece una API con métodos y propiedades para manipular elementos HTML. El primer paso es encontrar el elemento y a partir de ahí modificar. Algunos métodos son:
- `node.getElementById(id)`: Obtiene un elemento a partir de su ID.
- `node.getElementsByTagName(tagName)`: Obtiene un array de descendientes del nodo que sean de un tipo de etiqueta.
- `node.getElementsByClassName(className)`: Obtiene un array de descendientes del nodo con una clase determinada.
- `node.querySelectorAll(cssSelector)`: Obtiene un array de descendientes del nodo que concuerdan con un selector CSS.
- `node.classList`: Devuelve la lista de clases del nodo (mutable).
- `node.tagName`: Devuelve el tipo de etiqueta HTML del nodo.
- `node.style`: Proporciona acceso para modificar y consultar los estilos.
- `node.innerHTML`: Devuelve y permite modificar el contenido HTML de un nodo.
- `node.innerText`: Devuelve y permite modificar el texto de un nodo.
- `node.getAttribute("atributo")`: Permite consultar el valor de un atributo del nodo.
- `node.setAttribute(nombreAtributo, valor)`: Modifica el valor de un atributo.
- `document.createElement(nombreElemento)`: Crea un nuevo elemento HTML.
- `node.removeChild(elemento)`: Elimina un nodo hijo.
- `node.remove()`: Elimina el nodo.
- `node.append(elemento)`: Añade un elemento como **último** hijo.
- `node.prepend(elemento)`: Añade un elemento como **primer** hijo.
- `node.insertBefore(elemento, hermano)`: Añade un elemento como nodo hijo, por delante de un nodo hermano.
- `node.replaceChild(nuevo, antiguo)`: Reemplaza un nodo hijo con otro.
- `node.parentNode`: Devuelve el nodo padre.
- `node.firstChild`: Devuelve el primer hijo del nodo.
- `node.lastChild`: Devuelve el último hijo del nodo.
- `node.childNodes[i]`: Devuelve el i-ésimo hijo del array de hijos del nodo.
- `node.nextSibling`: Devuelve el siguiente hermano de un nodo.
- `node.previousSibling`: Devuelve el hermano anterior de un nodo.
El DOM tiene varias propiedades extra como:
- `document.URL`
- `document.body`
- `document.cookie`
- `document.doctype`
- `document.head`
- `document.inputEncoding`
- `document.readyState`
- `document.title`
### <mark style="background: #D2B3FFA6;">- NodeLists</mark>
Hay propiedades para acceder directamente a colecciones de objetos:
- `document.images`: Todas las imágenes del documento.
- `document.forms`: Todos los formularios del documento.
- `document.forms[X].elements`: Todos los campos del formulario X.
- `document.links`: Todos los `<a>` del documento.
- `document.scripts`: Todos los scripts.
## <mark style="background: #ADCCFFA6;">3. Renderizadores</mark>
Normalmente se necesita mostrar muchos elementos del mismo tipo, como imágenes o mensajes de éxito/error. O mostrar el mismo elemento de más de una forma diferente (miniatura o completa).
# <mark style="background: #FFF3A3A6;">TEMA 12: Llamadas AJAX mediante REST y JSON</mark>
## <mark style="background: #ADCCFFA6;">1. REST</mark>
Arquitectura cliente-servidor. No tiene estado ya que se basa en HTTP. Cacheable. Modelo en capas.
![[Pasted image 20240409154435.png]]
## <mark style="background: #ADCCFFA6;">2. Formatos de comunicación JSON</mark>
![[Pasted image 20240409154700.png]]
### <mark style="background: #D2B3FFA6;">- Tipos de datos</mark>
- **Number:** números decimales con signo, incluyendo notación exponencial.
- **Array:** elementos separados por "," y entre "[ ]". Pueden ser de cualquier tipo.
- **String:** secuencia de caracteres entre "". Los caracteres especiales se escapan con "\\".
- **Object:** colección no ordenada de pares K,V entre "{ }" y separados por ",".
- **null:** valores vacíos. Undefined en JS.
- **Boolean:** true/false.
## <mark style="background: #ADCCFFA6;">3. Implementación de llamadas asíncronas REST</mark>
### <mark style="background: #D2B3FFA6;">- AJAX (Asynchronous JavaScript And XML)</mark>
Inicialmente se usaba XML pero ahora JSON. El núcleo de AJAX es la API XMLHttpRequest o XHR. Algunas implementaciones de AJAX son XHR, fetch (nativas), jQuery, axios (externas).
### <mark style="background: #D2B3FFA6;">- Fetch</mark>
API nativa basada en Promises. La respuesta es una Promise. Cuando se resuelve la Promise se puede consultar el objeto Response.
### <mark style="background: #D2B3FFA6;">- jQuery</mark>
La librería incluye diversos métodos para AJAX. Basada en XHR. Facilita el código y es cross-browser pero es externa, y se usa FormData.
### <mark style="background: #D2B3FFA6;">- Axios</mark>
Basada en XHR pero usa funciones asíncronas. Como ventajas:
- Intercepta request y response (permite examinar y modificar las peticiones y respuestas HTTP).
- Permite cancelar requests.
- Múltiples librerías de terceros como "addons".
- Compatibilidad con muchos navegadores.
- Simplifica la lógica de gestión de respuestas y errores.
- Muy popular actualmente (incluso con Node.js).
La única desventaja es que es externa.
#### <mark style="background: #FFB86CA6;">NOTA:</mark> Al capturar errores hay que capturar `error.response.data.message` en el catch y no solo el objeto error.
## <mark style="background: #ADCCFFA6;">4. Módulos API</mark>
- `/js/api/common.js`: URL Base y configuración común para todas las peticiones.
- `/js/api/OBJETO.js`: módulo de acceso a API para un OBJETO (ej. Fotos).
# <mark style="background: #FFF3A3A6;">TEMA 13: Persistencia en cliente</mark>
## <mark style="background: #ADCCFFA6;">1. HTTP Cookies</mark>
Una cookie HTTP (o web cookies, Internet cookies, browser cookies) es un pequeño fragmento de información enviado desde un sitio web y que el navegador almacena en local mientras este navega por el sitio.
![[Pasted image 20240423154342.png|500]]
- **Cookies de sesión (in-memory, transient, non-persistent)**: Son volátiles y se almacenan en memoria y se borran al cerrar el navegador. No suelen tener fecha de expiración.
- **Cookies persistentes (tracking)**: Se almacenan incluso con el navegador cerrado. Tienen fecha de expiración.
Hay distintos tipos según su origen:
- **First-party cookies:** Las envía el sitio web que el usuario visita.
- **Third-party cookie:** Las envía un sitio web externo al visitado (likes, adSense, iframes).
Hay varios metadatos, como Domain, Path, Same-site, Secure, HTTPOnly, Max-Age, Size. Las cookies tienen varias finalidades como gestionar sesiones (productos añadidos al carrito, puntuaciones de juegos, auth cookies), tracking (páginas visitadas, datos de formularios, estadísticas) o preferencias del usuario (lenguaje, colores, divisa).
### <mark style="background: #D2B3FFA6;">- Problemas de seguridad</mark>
Las cookies forman parte de las request, por lo que hay que validarlas y contemplar la posibilidad de que formen parte de un ataque. **Nunca** se debe almacenar información sensible. Las third party cookies se pueden usar para tracking o para ataques XSRF. Sin embargo pueden ser útiles para integrar componentes de un sitio en otro (google maps, youtube, etc).
## <mark style="background: #ADCCFFA6;">2. Web Storage API</mark>
Permite al navegador almacenar pares clave/valor de forma más intuitiva. Más seguro que las cookies. Permite mayor tamaño de almacenamiento.
### <mark style="background: #D2B3FFA6;">- SessionStorage</mark>
Mantiene un área de almacenamiento separada para cada origen mientras el navegador esté abierto. Cuando se cierra se borran los datos. El límite es 5MB. Accesible mediante `Window.sessionStorage`.
### <mark style="background: #D2B3FFA6;">- LocalStorage</mark>
Mantiene un área de almacenamiento separada para cada origen incluso al cerrar el navegador. Almacena datos sin fecha de expiración o y sólo se borra mediante JS o borrando el caché del navegador. Límite 5MB. Accesible mediante `Window.localStorage`.
## <mark style="background: #ADCCFFA6;">3. Cache API</mark>
