# ==TEMA 1: Introducción a la Ingeniería del Software==
## 1.1. El software. Definición y características.
Es un conjunto de instrucciones que ejecutan una tarea en un computador. Presenta carac terísticas como:
- Intangibilidad.
- Se desarrolla, no se fabrica.
- No se estropea, pero si se queda obsoleto.

| ![[bañera_small.png]] | ![[software_ideal_small.png]] | ![[software_real_small.png]] |
| ---------- | ---------- | ---------- |
| <center>HARDWARE</center> | <center>SOFTWARE IDEAL</center> | <center>SOFTWARE REAL</center> |
### ♦ Tipos de software.

![[tipos-de-software.png]]
### ♦ ¿Qué es una ingeniería?
Es una disciplina que, haciendo uso de conocimientos científico-tecnológicos, plantean soluciones a problemas prácticos reales. Las actividades propias de un ingeniero son por ejemplo: concebir, inventar, desarrollar, etc.

Es importante también, entender el vocabulario (interfaz, clase, variable, etc.), instrumentación (Java, Python, etc.), herramientas (Eclipse, Visual Studio Code, etc.), hacer uso de buenas prácticas (PMBOK, ITIL, CMMI, etc.) y la metodología (SCRUM, RUP, XP, AUP, etc.).

## 1.2. Ingeniería del software.
Establecimiento y uso de principios fundamentales de la ingeniería, con objeto de desarrollar de forma económica software fiable y eficiente.
### ♦ Definición de proyecto
Un proyecto es un esfuerzo temporal que se lleva a cabo para crear un producto, servicio o resultado único. Existen los siguientes tipos:
- **Productivo:** produce beneficios.
- **Público:** dedicado a la sociedad y financiado por empresas.
- **Social:** también dedicado a la sociedad pero no necesariamente financiado por empresas.
- **De vida:** el resultado es la satisfacción personal.
- **Científico:** utilizar datos para generar información para otros ámbitos.
### ♦ Etapas de un proyecto
![[pdsa.png]]
En la etapa "Plan" se establecen unos objetivos y actividades que serán necesarios posteriormente en la etapa "Do", donde se realizan dichos objetivos. Luego se verifica si se han cumplido los objetivos en la etapa "Check" y por último, se toman decisiones respecto a eso en la etapa "Act".
### ♦ Proyecto de software
Esfuerzo temporal para crear un único producto o servicio software realizado por personas y que debe ser limitado en tiempo y coste. Además se tiene que planificar, ejecutar y controlar. En un proyecto de software hay varios roles:
- **Director de proyecto:** toma las decisiones sobre el proyecto.
- **Ingeniero de requisitos:** también llamado analista. Interactúa con los usuarios finales del proyecto.
- **Equipo de desarrollo:** consta de desarrolladores backend, frontend, IU, BDD, etc.
- **Equipo de calidad:** verifican la calidad del software y de la documentación.
- **Cliente:** responsable de la financiación del proyecto.
- **Usuario:** es el usuario final al que está destinado el proyecto.
- **Responsable TIC:** es el responsable del entorno tecnológico del cliente.

## 1.3. Normas y estándares
### ♦ Ciclo de vida del software (ISO/IEC/IEEE 12207:2017)
- **Proceso de acuerdo:** se determina si hace falta adquirir otro software para el desarrollo correcto del proyecto.
- **Procesos organizacionales:** infraestructura, RRHH, etc.
- **Procesos de gestión técnica:** planear, configurar, etc.
- **Procesos técnicos:** diseño, implementación, etc.
### ♦ CMMI-DEV (2010)
- Mejora y evaluación de procesos para el desarrollo, mantenimiento, y operación de sistemas software.
- Desarrollado por el SEI (Software Engineering Institute).
- A veces se requiere un nivel mínimo de certificación CMMI.
- Existen cinco niveles de madurez: 
  Inicial -> Administrado -> Definido -> Administrado cuantitativamente -> Optimización

## 1.4. Productos de la ingeniería del software
Los productos previos al comienzo del proyecto son:
- Petición de propuestas.
- Pliego de Prescripciones Técnicas (documento oficial que contiene información necesaria para que el objeto del contrato se ejecute satisfactoriamente).
- Oferta.
- Contrato.
Se debe dejar claro...
- Las necesidades a satisfacer.
- Los entregables (plan de proyecto, seguimiento, código fuente).
- Presupuesto.
- Restricciones técnicas.
- Penalizaciones por retrasos.

## 1.5. Mantenimiento del software
Se encarga de proporcionar un mantenimiento y gestión de incidencias post-entrega.
- El mantenimiento se encarga de mejorar, adaptar o corregir el software en explotación. Tiene un coste muy alto.
- La gestión de incidencias se encarga de identificar errores, priorizar, diagnosticar, escalar y resolver la incidencia y devolver el producto a su operativa normal.
### ♦ Tipos de mantenimiento (Métrica 3)
- **Evolutivo (60%):** incorporar nuevos requisitos o cambios.
- **Correctivo (17%):** corregir errores.
- **Adaptativo (18%):** adaptación a cambios tecnológicos.
- **Perfectivo (5%):** mejorar la calidad.

## 1.6. Calidad del software
![[coste-correccion.png]]

## 1.7. Gestión de la configuración
Se encarga de identificar e informar sobre nuevos cambios. Las actividades básicas son control de versiones, control de cambios, auditoría, generación de informes, determinar items a configurar...
- **Baseline:** estado del proyecto a partir del cual se modifican items.
- **CMS/CMDB:** Configuration Management System/Database.

<div style="page-break-before:always"></div>

# ==TEMA 2: El ciclo de vida del software==
## 2.1. Concepto de ciclo de vida
Procesos, actividades y tareas involucradas en el desarrollo, operación y mantenimiento de un producto.
![[ciclo_vida_concepto.png|350]]

## 2.2. Ciclo de vida clásico
![[ciclo_vida_cascada.png|350]]
- Cada fase comienza al terminar la anterior.
- Asume que se conocen todos los requisitos.
- Se tarda mucho en disponer del software.
- Es mejor que no seguir ningún ciclo.
- Más fácil de planificar (ideal).

## 2.3. Ciclos evolutivos
Cuanto mayor es el proyecto menor es la probabilidad de éxito. Existen varios ciclos evolutivos.
- **Incremental:** se repiten varios en cascada y se suele aplicar a desarrollos de gran tamaño.
- **Iterativo:** las primeras versiones pueden ser prototipos desechables. Los usuarios dan feedback a los prototipos.
- **Basado en prototipos:** no es exclusivo del iterativo. Habitualmente son prototipos de UI. Se usa como herramienta para feedback y se evalúa si merece la pena. Se pueden enfocar de forma desechable, evolutiva o mixta.

## 2.4. Ciclo de vida en los métodos ágiles
Son evolutivos con iteraciones de corta duración en las cuales se incorporan nuevos requisitos.
### ♦ El manifiesto ágil de 2001...
- **Individuos e interacciones sobre procesos y herramientas:** La gente es el principal factor de éxito de un proyecto. Es mejor construir un buen equipo que un buen entorno.
- **Software que funcione sobre una documentación detallada:** Los documentos deben ser cortos y centrarse en lo fundamental y sólo deben crearse si son necesarios de forma inmediata.
- **Colaboración con el cliente:** Debería existir una interacción constante entre el cliente y el equipo de desarrollo, ya que esto marcará el avance del proyecto.
- **Respuesta al cambio:** La habilidad a adaptarse a los cambios que surjan durante el proyecto también condicionará el mismo, por lo que la planificación debe ser flexible.
### ♦ Técnicas de apoyo para los métodos ágiles
- **Refactorización:** Mejorar el código sin cambiar su funcionalidad.
- **Pruebas automáticas:** Testeos programados en vez de a mano.
- **Integración continua:** Automatización de la compilación y ejecución de pruebas automáticas.
- **Gestión de la configuración:** Diseñada para la integración continua.

## 2.5. Metodología SCRUM
Es la metodología ágil más usada actualmente. Está basada en iteraciones de 30 días, llamadas sprints. Durante un sprint se produce código potencialmente entregable y no se admiten ni cambios de requisitos ni de miembros del equipo. Hay algunos conceptos importantes relacionados con la metodología SCRUM.
- **Agile meeting:** reuniones cortas y frecuentes donde cada miembro expone lo que ha hecho, los problemas que ha tenido y lo que va a hacer próximamente.
- **Backlog:** lista priorizada de tareas, que reemplazan a los diagramas de Gantt. Existen backlogs de producto y de iteración.

## 2.6. Ciclo de vida del Proceso Unificado
Es un proceso iterativo e incremental propuesto por los creadores de UML. Se definen 6 fases: inicio, elaboración, construcción, transición, producción y retirada. En cada fase se producen una o más iteraciones y se obtiene una versión evaluable del software.

## 2.7. Ciclo de vida en Métrica 3
Es la metodología oficial de las Administraciones Públicas en España. Permite aplicar varios ciclos de vida. Sus procesos básicos son:
- **Plan de Sistemas de Información (PSI):** Requisitos y arquitectura de información.
- **Desarrollo de sistemas de información:**
	- Estudio de viabilidad del sistema (EVS): Económicos, técnicos, legales...
	- Análisis del Sistema de Información (ASI): Obtener especificaciones del SI.
	- Diseño del Sistema de Información (DSI): Definir cómo va a ser el SI.
	- Construcción del Sistema de Información (CSI): Construcción y pruebas.
	- Implantación y Adaptación del Sistema (IAS) : Entrega, adaptación y pruebas.
- **Mantenimiento de Sistemas de Información (MSI)**

## 2.8. Pruebas en el ciclo de vida
- **El modelo en V:** asocia un tipo de pruebas a cada producto de cada fase según su nivel de abstracción.
![[Pasted image 20230919181949.png|500]]

## 2.9. Conceptos importantes
### ♦ Ingeniería inversa
A veces es necesario mantener software legacy de los que no hay ni código fuente ni documentación. Se suele "ir hacia atrás" para obtener el diseño analizando el código.
### ♦ Reingeniería
Se utiliza la información obtenida por la ingeniería inversa para aplicar cualquier tipo de mantenimiento. El mantenimiento preventivo del efecto 2000 ha sido el mayor esfuerzo hasta la fecha.

<div style="page-break-before:always"></div>

# ==TEMA 3: Introducción a los Sistemas de Información==
## 3.1. Conceptos básicos
### ♦ ¿Qué es un sistema?
Un sistema es un conjunto de componentes que interactúan entre sí para lograr objetivos comunes. Un sistema interactúa con un entorno del que toma entradas y al que devuelve salidas.
- Las entradas de un sistema pueden ser datos, energía, materia, etc. 
- Las salidas pueden ser información, energía, materia, etc. 
- Los objetivos controlan como se realizan las transformaciones. 
- Casi todos los sistemas son realimentados.
### ♦ Datos, información y conocimiento
- **Datos:** hechos o cifras que tienen existencia propia e independiente. Por sí solos no son relevantes o irrelevantes sino que hay que procesarlos.
- **Información:** conjunto de datos procesados con significado, relevancia y propósito.
- **Conocimiento:** mezcla de experiencias concretas, valores, información y juicio basado en la experiencia que permite evaluar e incorporar nuevas experiencias e información.
### ♦ ¿Qué es un sistema de información?
Un sistema de información es un sistema diseñado para recoger, almacenar, procesar y distribuir información sobre el estado de su entorno y soportar las operaciones, la gestión y la toma de decisiones de la organización de la que forma parte.

Un sistema de información ayuda a tomar decisiones estratégicas de competitividad, tácticas de negocio y a llevar a cabo los procesos de negocio.
## 3.1. Conceptos básicos
Un sistema informático es un sistema compuesto por hardware, software y personal encargado de su operación y mantenimiento.
### ♦ Diferencias entre un sistema de información e informático
Un sistema de información puede incluir un sistema informático excepto si es totalmente manual (periódico, bibliotecas antiguas, etc.). A su vez, un sistema informático no tiene por qué ser parte de un sistema de información (consolas por ejemplo).

## 3.2. Funciones de un sistema de información
- **Memoria:** mantiene una representación interna del estado del entorno del sistema.
- **Informativa:** proporciona información sobre el estado del entorno del sistema.
- **Activa:** acciones que modifican el estado del entorno del sistema.
Estas tres funciones se pueden realizar de las siguientes formas:
- **Bajo demanda:** los usuarios solicitan el servicio.
- **Autónomamente:** el sistema realiza la función al detectar cambios en su entorno.

## 3.3. Componentes de un sistema de información
Dentro de los recursos se engloban recursos humanos, de software, de hardware, de redes y de datos. Las actividades del sistema son:
- Controlar el rendimiento.
- Procesar entradas para generar salidas.
- Guardar (o no) recursos de datos.

## 3.4. Tipos de sistemas de información
Se clasifican normalmente en función del servicio que ofrecen. Los de apoyo a la gestión son DSS (Decision Support System) y  MIS (Management Information System), mientras que los de apoyo a la operación son los TPS (Transaction Processing System).
### ♦ Procesamiento de transacciones (TPS)
Las transacciones son tareas que se llevan a cabo dentro de la organización y aportan nueva información. El objetivo de un TPS es capturar y procesar los datos recogidos sobre la transacción realizada. Es importante cumplir las reglas ACID:
- **Atomicity:** las operaciones o son completadas y procesadas totalmente o son descartadas y no tienen efecto.
- **Consistency:** las operaciones no pueden violar la integridad de los datos.
- **Isolation:** cada operación debe ser independiente y se debe llevar a cabo sin que afecte a otras transacciones.
- **Durability:** el resultado de una operación debe ser persistente en el tiempo.
Los TPS recogen la mayoría de los datos. Un fallo grave en uno de estos puede paralizar la organización. Una BDD sólo almacena información mientras que un TPS además ofrece una interfaz gráfica, servidor de transacciones, etc.
### ♦ Información de gestión (MIS)
Proporcionan información operacional y táctica. Transforman datos e información para cumplir con los requisitos. Generan informes para gerentes de nivel medio. Ej: control de inventario.
### ♦ Apoyo a toma de decisiones (DSS)
Usan información de los TPS, MIS y externa. Proporcionan informes y análisis de forma flexible. Para no sobrecargar los TPS suelen usar _Data Warehouses_ y cada vez más, el _Big Data_.

<div style="page-break-before:always"></div>

# ==TEMA 4: Legislación==
## 4.1. Jerarquía normativa
Las leyes se definen como documentos legales jurídicos escritos que prevalecen frente a cualquier fuente normativa. Dentro de las leyes existe una jerarquía, toda norma que contradiga a la de rango superior carece de validez. Una vez se aprueba una ley, pasa a formar parte del ordenamiento jurídico (conjunto del derecho de una sociedad).
<table style="margin-top:10px;" border=1>
	<tr>
		<td colspan=4>
			Derecho Comunitario
		</td>
	</tr>
	<tr>
		<td colspan=4>
			Constitución
		</td>
	</tr>
	<tr>
		<td>
			Leyes orgánicas
		</td>
		<td>
			Leyes ordinarias
		</td>
		<td>
			Decretos legislativos
		</td>
		<td>
			Decretos-ley
		</td>
	</tr>
	<tr>
		<td colspan=4>
			Reglamentos
		</td>
	</tr>
	<tr>
		<td colspan=4>
			Convenios colectivos
		</td>
	</tr>
	<tr>
		<td colspan=4>
			Contratos de trabajo
		</td>
	</tr>
	<tr>
		<td colspan=4>
			Usos y costumbres
		</td>
	</tr>
</table>

## 4.2. Ámbito Europeo
### ♦ Reglamento General de Protección de Datos (RGPD)
Entró en vigor el 25 de mayo de 2018. Referido a la protección de las personas físicas en lo que respecta al tratamiento de datos personales y a la libre circulación de estos. En el artículo 4 del RGPD se incluye lo siguiente:
- **Datos personales:** Toda información sobre una persona física identificada o identificable siendo esta última una persona identificable directa o indirectamente por un identificador.
- **Tratamiento:** Cualquier operación o conjunto de operaciones realizadas sobre datos personales o conjuntos de datos ya sean automatizadas o no.
- **Elaboración de perfiles:** Toda forma de tratamiento automatizado de datos personales con objetivo de evaluar distintos aspectos personales o predecir otros aspectos.
- **Seudonimización:** El tratamiento de datos personales de manera que no se puedan atribuir a un interesado sin utilizar información adicional.
### ♦ Obligaciones
- Tratamiento leal, legal y transparente de los datos.
- Limitación del fin, datos y almacenamiento.
- Establecer medidas de protección suficientes.
- Registros de violación de seguridad de los datos.
- Privacidad en diseño. 
- Transferencias a terceros con garantías.
- Delegado de protección de datos. $^{(1)}$
- Derechos ARCO-POL. $^{(2)}$
- Obtener el consentimiento expreso de los usuarios. $^{(3)}$
### ♦ Delegado de protección de datos$^{(1)}$
Es necesario que el tratamiento lo lleve a cabo una autoridad u organismo público a excepción de si es un procedimiento judicial. Las funciones son informar y asesorar, supervisar el cumplimiento del RGPD, ofrecer asesoramiento, cooperar con la AEPD (autoridad de control) y actuar como punto de contacto.

Otras características son que no podrá ser destituido ni sancionado, deberá mantener la confidencialidad, deberá facilitar los recursos necesarios, etc.
### ♦ Derechos ARCO-POL$^{(2)}$
- Derecho de Acceso.
- Derecho de Rectificación.
- Derecho de Cancelación.
- Derecho de Oposición.
- Derecho de POrtabilidad.
- Derecho de Limitación.

### ♦ Supuestos para evitar pedir consentimiento$^{(3)}$
Según el RGPD, hay ciertos supuestos en los que no sería necesario obtener el consentimiento del usuario.
- Cuando el tratamiento sea necesario para un contrato.
- Por la aplicación de alguna ley.
- Para proteger intereses vitales del interesado.
- Cuando el tratamiento sea necesario para satisfacer intereses legítimos.
### ♦ Sanciones del RGPD
Multas de hasta 10M € o 2% del volumen de negocio global del último año:
- Por vulnerar las obligaciones.
Multas de hasta 20M € o 4% del volumen de negocio globa del último año:
- Por vulnerar los principios básicos de tratamiento.
- Por vulnerar los derechos de los interesados.
- Por transferir datos a países ajenos a la UE que no tengan un nivel adecuado de garantía.
- Incumplimiento de una resolución.

## 4.3. Ámbito Nacional
### ♦ Constitución
- Artículo 18.
- Sentencia del TC 94/1998.
- Sentencia del TC 292/2000.
### ♦ Nueva LOPD (LOPDGDD)
Deroga la antigua LOPD. Su objetivo es adaptar el ordenamiento jurídico español al RGPD.

<div style="page-break-before:always"></div>

# ==TEMA 5: Requisitos para Sistemas de Información==
## 5.1. Introducción
Un requisito es una condición o capacidad que un usuario o un sistema necesita para resolver un problema o tener éxito en el entorno.
![[Pasted image 20230922164516.png|500]]

## 5.2. Historias de usuario
Son la propuesta de las metodologías ágiles para la especificación de los requisitos. Se escriben desde el punto de vista del usuario del sistema y usando su vocabulario. Se suele usar el formato propuesto por Mike Cohn:
```TOML
TITULO
#----#
Como [tipo de usuario],
quiero [servicio],
para [razón]
```

## 5.3. Requisitos generales
No todas las historias de usuario están al mismo nivel de detalle, por ello se crea una especie de "jerarquía". 
```TOML
GESTIONAR EL ALMACÉN
#------------------#
Como [encargado del almacén],
quiero [gestionar el almacén],
para [contribuir al buen funcionamiento]
```
Dado que es muy general, se complementa con una o varias historias más, desglosando la historia principal para detallar mejor la implementación de la solución.

## 5.4. Requisitos de información
Describen qué información se debe almacenar sobre un concepto relevante para poder cumplir objetivos. También que datos son específicos del concepto. 
```TOML
INFORMACIÓN PRÉSTAMOS BIBLIOTECA
#------------------------------#
Como [director de la biblioteca],
quiero [disponer de la siguiente información sobre los préstamos],
- El socio que realiza el préstamo
- El libro o libros prestados
- La fecha del préstamo
- Para cada libro:
	- La fecha de devolución prevista
	- La fecha de devolución efectiva, si se ha producido
```

## 5.5. Reglas de negocio
Definen reglas o políticas que son importantes para los usuarios y deben ser respetadas. Suelen ser inestables ya que pueden cambiar si cambia la política de la empresa/entidad.

## 5.6. Requisitos funcionales
En general, definen los servicios que los usuarios desean que el sistema les ofrezca.

## 5.7. Requisitos no funcionales
Describen aspectos relacionados con la calidad: usabilidad, rendimiento, seguridad, etc.

## 5.8. Pruebas de aceptación
- No sólo describen cómo validar que el sistema satisface los requisitos.
- También añaden detalle a las historias de usuario sin complicarlas.
- Lo ideal es que se programen para autoejecutarse.
- Se asocian a uno o más requisitos (trazabilidad).

<div style="page-break-before:always"></div>

# ==TEMA 6: Introducción al modelado conceptual==
## 6.1. Introducción
El modelado conceptual es una técnica de análisis de requisitos y diseño de bases de datos.
Como técnica de análisis:
- Ayuda a identificar problemas en los requisitos antes de comenzar el desarrollo.
Como técnica de diseño de bases de datos:
- Permite representar de forma abstracta conceptos y hechos relevantes del dominio y transformarlos en un esquema de base de datos.

## 6.2. Notación UML: Diagramas de clases

```plantuml
!theme vibrant
class Profesor {
	nombre
	apellidos
	uvus
	departamento
}


class Alumno {
	nombre
	apellidos
	uvus
	anyo_inscr
	matricula
}

class Evaluacion {
	tipo_evaluacion
	nota
	preguntas
	tipo_continuidad
}

class Grupo {
	hora
	dia
	alumnos
}

Profesor "1" --> "3..*" Evaluacion : elabora
Profesor "1" --> "1..*" Grupo : atiende
Profesor "1" --> "1..*" Alumno : supervisa
Alumno "1" --> "0..*" Evaluacion : esEvaluado
Alumno "1" --> "1" Grupo : estaInscrito
Alumno "1" --> "1..*" Alumno : esCompañero
```

### ♦ Clase entidad
Representa un concepto relevante del dominio del que se debe almacenar información. Se nombra mediante un sustantivo en singular.
### ♦ Atributo de una clase entidad
Son propiedades asociadas a un concepto del dominio que se debe almacenar. Se nombran mediante un sustantivo en singular. Los valores deben ser atómicos.
### ♦ Notación para las clases entidades UML
No se suele especificar el Tipo excepto en los enumerados. Mediante [0..1] se indica que el atributo es opcional, es decir que cuando no se conoce su valor se representa por un valor nulo.
### ♦ Asociación entre clases entidades
Representa una relación entre dos o más conceptos relevantes. Se nombra mediante un verbo en tercera persona del singular. Debe formar una frase con sentido. 
### ♦ Rol de un extremo de una asociación
Papel que juega cada una de las clases. Sólo es necesario indicarlo en las asociaciones de una clase consigo misma o cuando existe más de una asociación entre dos clases.
### ♦ Generalización/especialización
A veces algunos de los conceptos presentan entre ellos relaciones tipo _es-un_, que suelen tener atributos y asociaciones comunes. Para que no pase esto se crean superclases y subclases. Por ejemplo:
```plantuml
class Persona {
	
}

class Vehiculo {
	matricula
	numeroBastidor
	modelo
}

class Seguro {
	compañia
	numeroPoliza
	tipo
	precio
}

class Turismo {
	plazas
}

class Camion {
	tonelaje
	ejes
}

class Motocicleta {
	cilindrada
}

Persona --> Vehiculo : esPropietariaDe
Vehiculo --> Seguro : tiene
Turismo --|> Vehiculo
Camion --|> Vehiculo
Motocicleta --|> Vehiculo
note "{completa, disjunta}" as N
Turismo .. N
Camion .. N
Motocicleta .. N
```

La superclase contiene todos las propiedades que heredan las subclases. Todas las instancias de las subclases **también** son instancias de las superclases.
### ♦ Notación para la generalización en UML
- {completa}: las instancias de la superclase deben ser instancias de al menos una subclase.
- {incompleta}: puede haber instancias de la superclase que no sean instancias de ninguna subclase
- {disjunta}: las instancias de una superclase pueden ser instancias de una sola subclase.
- {solapada}: las instancias de una superclase pueden ser instancias de una o más subclases.
### ♦ Composición
Representa el _ser-parte-de_ o _estar-compuesto-por_.
- Una parte pertenece sólo a un todo.
- Una parte no puede existir sin que exista el todo.
- La eliminación del todo implica la eliminación de todas sus partes.
- Es transitiva y asimétrica.
- Puede ser recursiva.
### ♦ Agregación
- Una parte puede pertenecer a uno, varios o ningún todo.
- Una parte **sí** puede existir sin pertenecer a un todo.
- La eliminación del todo no implica la eliminación de las partes.
	- Transitiva, asimétrica y recursiva.

## 6.3. Notación UML: Diagramas de objetos
Un objeto es una instancia de una clase. Un enlace es una instancia de una asociación.
Pasos recomendados:
1. Analizar la información sobre el dominio del problema (glosario) y los requisitos.
2. Identificar posibles entidades y atributos.
3. Identificar posibles asociaciones.
4. Construir incrementalmente el modelo conceptual e identificar las multiplicidades de las asociaciones.
5. Identificar clasificaciones entre entidades con propiedades (atributos y/o asociaciones) comunes.
6. Identificar composiciones entre entidades.
7. Añadir las restricciones que no puedan expresarse gráficamente.
8. Validar con posibles escenarios mediante diagramas de objetos.
9. Registrar todos aquellos problemas semánticos que deban ser aclarados con clientes y usuarios.

<div style="page-break-before:always"></div>

# ==TEMA 7: Introducción a las Bases de Datos y al Modelo Relacional==
## 7.1. Introducción
Un sistema de gestión de bases de datos (SGBD) es un sistema informático que:
- Implementa las funciones de memoria e informativa de un sistema de información.
- Almacena grandes volúmenes de datos.
- Gestiona el acceso concurrente a los datos.
- Mantiene la integridad semántica de los datos.
- Controla el acceso a los datos.

## 7.2. Conceptos básicos
### ♦ Sistemas pre-relacionales (antes de 1970)
- Basado en archivos de texto plano.
- Bases de datos jerárquicas: IMS.
- Bases de datos en red: CODASYL.
### ♦ Sistemas relacionales (desde 1970)
- Experimentales: RDMS, Ingres, …
- Comerciales: Oracle, DB2, MS SQL Server
- Open Source: PostgreSQL, MySQL, MariaDB, SQLite, ...
### ♦ Sistemas post-relacionales (desde 1990)
- Orientados a objetos: Gemstone, db4o, ...
- Objeto-relacionales: SQL3
- NoSQL: Cassandra, MongoDB, Redis, ...
### ♦ El modelo relacional
Surge en 1970 por E. F. Codd. Es un modelo sencillo y flexible basado en teoría de conjuntos y lógica de predicados. 
- Una **relación** está compuesta por una intensión y una extensión. La **intensión** define el conjunto de atributos que toma valores en el dominio. La **extensión** es un conjunto de tuplas cada una formada por conjuntos de pares (atributo, valor) de forma que a cada atributo se le asigna un valor. El número de atributos se denomina **grado** y el de tuplas **cardinalidad**.
- El valor **nulo (`null`)** significa que no se conoce el valor del atributo. Esto implica la introducción a una **lógica trivaluada**.

| A     | B    | A OR B | A AND B | NOT A |
| ----- | ---- | ------ | ------- | ----- |
| true  | null | true   | null    | false |
| false | null | null   | false   | true  |
| null  | null | null   | null    | null  |

## 7.3. Claves
### ♦ Superclaves
Una superclave es un subconjunto de atributos (descriptor) de una relación que identifica de manera única a las tuplas de dicha relación. Sigue un criterio de unicidad:
- Un descriptor es único si dos tuplas no pueden tener el mismo descriptor.
- Toda superclave cumple este criterio.
### ♦ Clave candidata
En cualquier relación siempre existe una superclave formada por la tupla entera ya que no se puede repetir la misma. Una relación puede tener varias superclaves. Según el criterio de Minimalidad:
- Una Superclave es Mínima si al eliminar cualquier atributo de su descriptor deja de cumplir el criterio de Unicidad.
- Las Superclaves que cumplen este criterio se denominan Claves Candidatas (cumplen Unicidad y Minimalidad).
Se deben evitar las superclaves que no son Claves Candidatas.
### ♦ Candidatas, Primarias [ PK ] y Alternativas [ AK ]
- **Claves candidatas:** Una relación tiene al menos una clave candidata, aunque puede tener varias.
- **Clave primaria (Primary Key):** Clave candidata seleccionada arbitrariamente como mecanismo de identificación.
- **Clave Alternativa (Alternative Key):** Cualquier clave candidata no seleccionada como primaria.
### ♦ Claves ajenas (Foreign Keys) [ FK ]
Las claves ajenas son conjuntos de atributos de una relación cuyos valores deben coincidir con los de la clave primaria de otra relación. Es la forma de representar las asociaciones del modelo conceptual. Se debe evitar ponerles nombres distintos a la clave primaria.

## 7.4. Integridad
- **Integridad de la entidad:** Ningún atributo de la clave primaria (PK) puede tener un valor nulo.
- **Integridad referencial:** Todos los atributos de una clave ajena deben tomar valores que coincidan con la clave primaria o bien, nulos.

<div style="page-break-before:always"></div>

# ==TEMA 8 (b): Transformación de MC en MR==
## 8.1. El desarrollo dirigido por modelos
Enfoque de desarrollo en el que se van generando productos mediante transformación de modelos hasta llegar al codigo fuente. En el desarrollo de sistemas con BBDD relacionales, se puede transformar de MC a MR y luego a código SQL.

## 8.2. Transformación de entidades
### ♦ Regla general
- Cada entidad se transforma en una relación con su mismo nombre en plural.
- Cada atributo de la entidad se transforma en un atributo de la relación con su nombre.
Se debe tener en cuenta:
- Definir dominios para los atributos en función de la semántica de los atributos correspondientes. 
- Añadir un sufijo **Id** como clave primaria (PK).
### ♦ Identificadores de objetos (Id)
Son atributos artificiales que se añaden para poder identificar un objeto de otro sin depender de las claves semánticas. Normalmente se delega la generación al SGBD. Se suelen nombrar con el sufijo Id.
### ♦ Claves semánticas
Son atributos claves en el dominio del problema. A veces se usan como Id porque realmente lo son en el dominio (NIF, NSS, etc.).

## 8.3. Transformación de asociaciones
### ♦ Regla general
- Las asociaciones 1:N se representan con la clave ajena en la relación de la entidad del rol N.
- Las asociaciones 1:1 se representan con una clave ajena en cualquiera de las relaciones.
- Las asociaciones M:N se representan con una relación auxiliar con claves ajenas a las dos.
A tener en cuenta:
- Si un rol está **{ordenado}** hay que añadir un atributo (si no existe ya) que especifique el orden.

## 8.4. Transformación de clasificaciones
Hay tres estrategias:
- **Una relación para cada clase**
	![[Pasted image 20231003181401.png|500]]
- **Una relación para cada subclase concreta**
	![[Pasted image 20231003181500.png|500]]
- **Una única relación para toda la jerarquía (con discriminante)**
	![[Pasted image 20231003181538.png|500]]
- **Una única relación para toda la jerarquía (con valores booleanos)**
	![[Pasted image 20231003181812.png|500]]

## 8.5. Ejemplos
### ♦Estrategia I
![[Pasted image 20231003182313.png]]
### ♦ Estrategia II
![[Pasted image 20231003182355.png]]
### ♦ Estrategia III
![[Pasted image 20231003182632.png]]

<div style="page-break-before:always"></div>

# ==TEMA 8(a): Normalización de modelos relacionales==
## 8.1. Anomalías de manipulación
- La calidad de un modelo relacional depende, entre otros factores, de las anomalías de manipulación que presente. 
- La forma de asegurar la calidad de un modelo relacional frente a las anomalías de manipulación es comprobar que está al menos en tercera forma normal (3FN).
Los tipos de anomalías son:
- **Datos redundantes:** el nombre y el cargo de cada empleado se repita tantas veces como inmuebles gestione, malgastando espacio. 
- **Riesgos de incoherencia:** la redundancia de datos implica el riesgo de que se vuelvan incoherentes si no se actualizan todas las ocurrencias a la vez.
- **Anomalías de inserción:** hasta que un empleado no gestione un inmueble no se puede registrar en el sistema de información. 
- **Anomalías de actualización:** si un empleado cambia de cargo hay que actualizarlo múltiples veces en lugar de hacerlo una sola vez.
- **Anomalías de eliminación:** si un empleado deja de gestionar inmuebles, sus datos desaparecen del sistema de información. 
- **Problemas de consulta:** ¿Cómo se podrían conocer todos los inmuebles de un determinado propietario?

## 8.2. Dependencias funcionales
Si **R** es una relación y **X** e **Y** son dos subconjuntos de los atributos de **R**:
- X determina funcionalmente a Y.
- Y depende funcionalmente de X.
Si y sólo si...
- Siempre que dos tuplas tienen los mismos valores de X, también tiene los mismos de Y.
	$\forall~t_1,t_2~\in~extension(R)·(t_1·X=t_2·X)\Rightarrow (t_1·Y=t_2·Y)$
	Se denota como $X\rightarrow Y$.
Nunca dos tuplas con los valores de X pueden tener valores distintos a los de Y.

## 8.3. Formas normales
Son condiciones basadas en las dependencias funcionales, que debe cumplir un modelo relacional para estar exento de anomalías. Las formas normales son 1FN, 2FN, 3FN (Codd), FN, 4FN, 5FN (Boyce-Codd). Cada FN incluye a la anterior.
### ♦ Primera forma normal (1FN)
- Una relación está en 1FN si en cada tupla se le asigna a cada atributo un solo valor del dominio sobre el que está definido. 
- Esto implica la ausencia de grupos repetidos.  Ejemplo 1FN:
### ♦ Segunda forma normal (2FN)
- Atributos no primos (no forman parte de ninguna clave candidata) completamente dependientes de las claves candidatas.
- Normalmente se representan varias entidades y asociaciones así que no está en 2FN.
- Siempre se puede transformar un modelo que no esté en 2FN a uno que sí sin perder información.
### ♦ Tercera forma normal (3FN)
Una relación está en 3FN si está en 2FN y ningún atributo no primo depende transitivamente de ninguna clave candidata. Se justifica de la siguiente manera:
- Todos los atributos no primos deben representar un hecho sobre la clave, toda la clave y nada más que la clave.* 
- Normalmente una relación no está en 3FN porque está representando varias entidades asociadas a la vez.

# ==TEMA 9: Álgebra Relacional==
## 9.1. Introducción
El álgebra relacional es un conjunto de operadores sobre relaciones que permiten expresar consultas sobre un MR. Estos devuelven relaciones derivadas, por lo que pueden anidarse.
### ♦ Nombres de atributos
Se prefijan con el nombre de la relación a la que pertenecen cuando pueda haber ambigüedad: R.a = atributo a de la relación R.
### ♦ Relaciones compatibles
Son relaciones cuyas intensiones comparten los mismos dominios, de modo que cada atributo tiene su correspondiente en la otra relación. Se pueden aplicar operadores de unión, diferencia e intersección. Si se quiere aplicar los operadores en relaciones no compatibles, hay que usar el operador renombrado para hacerlas compatibles:
$T=\rho_{a/b}(R)$ ; el atributo R.b pasa a nombrarse como T.a.
## 9.2. Operadores conjuntistas
![[Pasted image 20231024175052.png]]

## 9.3. Operadores relacionales
♦ **Selección:** $T\leftarrow \sigma_f(R)$  
- $I(T)=I(R)$
- $E(T)={r\in E(R)|f(r)}$ 
- $f$ es una fórmula bien formada sobre $I(R)$
♦ **Proyección:** $T\leftarrow\Pi_B(R)$, siendo $B\subseteq I(R)$ 
- $I(T)=B$
- $E(T)={t|\forall b\in B\land\exists r\in E(R)\land t.b=r.b}$ 
♦ **NatJoin:** $T\leftarrow R \bowtie S$, siendo $I(R)\cap I(S)=C\neq\emptyset$   
- $I(T)=I(R)\cup I(S)$
- $E(T)={r\cup S|r\in E(R)\land s\in E(S)\land\forall c\in C\vDash r.c=s.c}$ 
♦ **División:** $T\leftarrow R\div S$, siendo $I(S)\subset I(R)$
- $I(T)=I(R)-I(S)$
- $E(T)={t|\forall s\in E(S)\vDash ts\in E(R)}$ 
♦ **Agregación:** $\gamma_G^F(R)$
- $G\subset I(R)$
	- Determina el número de particiones por cada valor $g\in G$. Si $G=\emptyset$ la partición es toda la relación.
- $F$ función de agregación definida sobre $R$
	- $F$ no puede ser  vacío. Si $G$ es vacío sólo hay una partición (relación entera).
	- Las funciones de agregación (count, sim, min, max, avg, ...) tienen un sólo parámetro.
	- $F$ puede contener atributos de $G$. En este caso $F$ es la función identidad.

# ==TEMA 10: Introducción a SQL==
## 10.1. Introducción
Se pueden realizar varias operaciones:
- **DDL (Data Definition Language):** gestión del esquema de la base de datos (`CREATE`, `ALTER`, `DROP`).
- **DML (Data Manipulation Language):** gestión de los datos (`INSERT`, `UPDATE`, `DELETE`).
- **DCL (Data Control Language):** control de acceso y permisos (`GRANT`, `REVOKE`).
- **DQL (Data Query Language):** gestión de consultas (`SELECT`).
- **TCL (Transaction Control Language):** gestión de transacciones (`COMMIT`, `ROLLBACK`, `TRANSACTION`).
### ♦ Transformación MR $\rightarrow$ SQL
- Se crea una tabla por cada relación del MR.
- Es necesaria la creación de PK y FK.
- El resto de restricciones se definen mediante:
	- `CHECK <condición> `
	- `UNIQUE`
	- `NOT NULL`
	- En caso de que no se pueda hacer `CHECK` $\rightarrow$ 
	  `STORED PROCEDURE + TRIGGERS`

## 10.2. Lenguaje de definición de datos (DDL)
### ♦ Claves
- **Primarias:** `PRIMARY KEY(tablaId)`
- **Alternativas:** `UNIQUE(atributo)` o `UNIQUE(atributo1,atributo2)`
- **Ajenas:** 
	`FOREIGN KEY(atributo)`
	`REFERENCES OtraTabla(otraTablaId)`
		`ON DELETE`:
				- `RESTRICT`
				- `CASCADE`
				- `SET NULL`
				- `SET DEFAULT`
		`ON UPDATE`: ...
### ♦ Reglas de negocio
- `atributo TIPO DEFAULT(valor)`
- `CHECK(expresion)`
- `CONSTRAINT nombre CHECK(expresion)`
### ♦ Tipos de datos
- **Numéricos:** `TINYINT, BOOLEAN, SMALLINT, MEDIUMINT, INT, BIGINT, DECIMAL, FLOAT, DOUBLE, BIT...`
- **Cadenas:** `CHAR, VARCHAR, TINYTEXT, TEXT, MEDIUMTEXT, LONGTEXT, ENUM...`
- **Binarios:** `BINARY, VARBINARY, TINYBLOB, BLOB, MEDIUMBLOB, LONGBLOB...`
- **Fechas:** `DATE, TIME, DATETIME, YEAR, MONTH, DAY...`
- **Geometría:** `POINT, LINESTRING, POLYGON, MULTIPOINT...`
### ♦ INSERT INTO
`INSERT INTO <Tabla> <Columna/s> VALUES <Valor/es>`
### ♦ UPDATE 
`UPDATE <Tabla> SET <Valor/es> WHERE <Condición>`
### ♦ DELETE
`DELETE FROM <Tabla> WHERE <Condición>`
## 10.3. Lenguaje de consulta de datos (DQL)
### ♦ SELECT
`SELECT <Columna/s> FROM <Tabla/s> WHERE <Condición>`
- **`BETWEEN`:** rango
- **`IN`:** valores concretos
- **`LIKE`:** un tipo de regex usando `_` para un carácter concreto y `%` para un String cualquiera. Por ejemplo, nombres que tengan 'o' en la segunda posición: `_o%`.
- **`ORDER BY`:** ordenar la selección ascendentemente `ASC` o descendentemente `DESC`.
- **Producto cartesiano:** en SQL de la forma: `SELECT * FROM <Tablas>`. Devuelve una relación con todas las posibles relaciones.
- **`NATURAL JOIN`:** se puede hacer de dos formas
```SQL
SELECT nombre, salario, fechaInicial, departamento
FROM Empleados E, Departamentos D
WHERE E.departamentoId=D.departamentoId;
```
```SQL
SELECT nombre, salario, fechaInicial, departamento
FROM Empleados NATURAL JOIN Departamentos;
```
- **`LEFT/RIGHT JOIN`:** se obtienen las tuplas de la tabla que se especifique a la izquierda o a la derecha.
```SQL
SELECT nombre, salario, fechaInicial, nombreDep, localidad
FROM Empleados E
LEFT/RIGHT JOIN Departamentos D
	ON E.departamentoId=D.departamentoId;
```
- **`UNION`:** devuelve la unión de dos o más selecciones.
- **`EXISTS`:** devuelve una selección en la que existe (`EXISTS`) o no (`NOT EXISTS`) una selección.
## 10.4. Consultas complejas
### ♦ Consultas complejas (Agregados)
- **`COUNT`:** 
```SQL
SELECT COUNT(*), MIN(salario), MAX(salario), AVG(salario), SUM(salario)
FROM Empleados;
```
- **`GROUP BY`:** 
```SQL
SELECT departamentoId,
	COUNT(*),
	AVG(salario) salarioMedio,
	AVG(salario * (1+comision)) salarioConComision,
	SUM(salario) gastoSalarios
FROM Empleados
GROUP BY departamentoId;
```
- **`HAVING`:** 
```SQL
SELECT departamentoId,
	COUNT(*) numeroTuplas,
	AVG(salario) salarioMedio,
	AVG(salario * (1+comision)) salarioConComision,
	SUM(salario) gastoSalarios
FROM Empleados
GROUP BY departamentoId HAVING numeroTuplas > 1;
```
- **`ALL, ANY`:** 
```SQL
SELECT * FROM Empleados
WHERE salario >
ALL/ANY (SELECT AVG(salario)
	FROM Empleados
	GROUP BY departamentoId);
```

# ==TEMA 11: SQL Avanzado==
## 11.1. Tipos de objetos
### ♦ Procedimientos (stored procedures)
Es un programa almacenado en la BDD. Se ejecuta directamente en el motor SGBD. Puede implementar lógica. No se pueden usar desde un `SELECT` sino desde un `CALL`.
### ♦ Funciones (stored functions)
Es un procedimiento que devuelve un valor, por tanto se pueden usar en los `SELECT`. Como las funciones de otros lenguajes, se les puede pasar parámetros de entrada.
### ♦ Disparadores (triggers)
Es código asociado a eventos. Es una forma de validar de forma compleja datos y reglas de negocio. Uso: 
```SQL
CREATE [OR REPLACE]
	[DEFINER = { user | CURRENT_USER | role | CURRENT_ROLE}]
	TRIGGER [IF NOT EXISTS] trigger_name trigger_time trigger_event
	ON tbl_name FOR EACH ROW [{ FOLLOWS | PRECEDES}] other_trigger_name
	trigger_statement
```
- trigger_time = {BEFORE, AFTER}
- trigger_event = {INSERT, UPDATE, DELETE}
- FOLLOWS / PRECEDES other_trigger = Añade other_trigger antes o después
### ♦ Cursores (cursors)
Son iteradores sobre resultados devueltos por `SELECT`. NO es aleatorio, es decir va uno por uno en los registros devueltos por el `SELECT`. Tampoco se reordenan los datos.  Uso: `DECLARE cursor_name FOR select_instruction`
### ♦ Palabras reservadas para los objetos
- `BEGIN END`: Define un conjunto de instrucciones.
- `CASE`: Case clásico semejante a otros lenguajes. Uso: `CASE <var> ... WHEN <var> = <value>`.
- `DECLARE CONDITION`: Declarar condición.
- `DECLARE HANDLER`: Detector de errores. Uso: `DECLARE CONTINUE/EXIT/... HANDLER FOR ERROR#<code>`
- `DECLARE <var>`: Declarar variable.
- `FOR`: Recorrer un agregado.
- `GOTO`: Ir a una etiqueta.
- `IF`: Control de flujo.
- `:labels`: Declarar una etiqueta.
- `LOOP`: Bucle "manual". Uso: `LOOP ... END`.
- `REPEAT LOOP:` Indicación de cuando se repite el `LOOP`.
- `RETURN`: Devuelve un valor en las funciones.
- `SELECT INTO`:
- `WHILE`: Otro bucle.
### ♦ Delimitadores
Las instrucciones en un bloque `BEGIN-END` están separadas por un delimitador de inicio y otro de fin.

# ==TEMA 12: Gestión de Transacciones==
## 12.1. Concepto de transacción
Una transacción es una secuencia de operaciones DML que se comporta como una unidad lógica de trabajo. Los puntos de sincronismo (synchronization points) definen el estado de la transacción: `START TRANSACTION`, `COMMIT`, `ROLLBACK`.
Las transacciones deben cumplir las características ACID (Atomicity Consistency Isolation Durability).
- **Atomicity:** si se confirman (`COMMIT`), se ejecutan todas, si no (`ROLLBACK`), no se ejecuta ninguna.
- **Consistency:** A partir de un estado consistente $S_0$ se deja la BDD en otro estado consistente $S_1$.
- **Isolation:** se realiza como si fuera la única transacción.
- **Durability:** Si se confirma, los cambios se hacen persistentes y si se cancela, el estado debe ser el inicial.

## 12.2. Estados de una transacción
Una transacción accede a elementos de datos (Gránulos) para leerlos  (Read) o modificarlos (Write). Un gránulo modificado en memoria (Buffer Pool) que no se ha escrito en la BDD es una página sucia (Dirty Page).
![[Pasted image 20231114180832.png|550]]

## 12.3. Problemas típicos de concurrencia
### ♦ Lost Update
Unas transacciones sobrescriben las actualizaciones de otras.
![[Pasted image 20231114181537.png|500]]
### ♦ Dirty Read
Se lee un valor que está siendo modificado por otra transacción no finalizada que podría cancelarse o fallar.
![[Pasted image 20231114181833.png|500]]
### ♦ Non-repeatable read
Se obtienen las lecturas diferentes del mismo valor durante la misma transacción.
![[Pasted image 20231114182046.png|400]]
### ♦ Phantom read
Se obtienen diferentes conjuntos de tuplas {X} durante la misma transacción.
![[Pasted image 20231114182521.png|450]]

## 12.4. Planes de ejecución
- **Secuencialmente:** una transacción detrás de otra, pero reducen el rendimiento del SGBD.
- **Concurrentemente:** Se ejecutan concurrentemente pero puede haber algo de secuencialidad.

## 12.5. Estrategias o algoritmos para evitar problemas de concurrencia
- **Bloqueos:**
Protocolo de bloqueo en dos fases: expansión y reducción. Se bloquea la escritura y la lectura. Se pueden dar deadlocks.
- **Ordenación por marcas de tiempo (timestamps):**
Se ordena según el orden temporal.
- **Múltiples versiones del mismo dato:**
Se ordena por timestamps multiversión.

## 12.6. Niveles de aislamiento
- **READ_UNCOMMITED:** permite leer datos no confirmados.
- **READ_COMMITED:** sólo permite leer datos confirmados de otras, pero si se repite la lectura en la misma transacción, pueden desaparecer datos eliminados por otra transacción.
- **REPEATABLE_READ:** garantiza, al menos, que las filas se acaban de leer en la misma transacción. (MariaDB)
- **SERIALIZABLE:** garantiza la secuenciabilidad de la transacción con todas las demás concurrentes.

## 12.7. Fallos en la BDD - Recuperación
### ♦ Fallos del medio
Fallos en los componentes hardware del sistema. Para recuperar los datos se hace una recuperación en frío, por lo que hay que parar el sistema.
### ♦ Fallos software
Son fallos creados por las transacciones. Se suele llevar un registro (log) para restablecer el último punto estable del sistema.

# ==EJERCICIOS DE REPASO==
## 28/11/23
$\Pi\rightarrow proyecci\acute{o}n\rightarrow$ `SELECT` 
$\rho\rightarrow renombrado$
$\sigma\rightarrow selecci\acute{o}n\rightarrow$ `SELECT`
$\gamma^F_G\rightarrow agrupaci\acute{o}n$  

### 1. Platos y precios de la BD
##### $\Pi_{plato,precio}(Platos)$ 
```sql
SELECT plato, precio 
FROM Platos;
```

### 2. Platos y precios menos del plato de Gambas
##### $\Pi_{plato,precio}(\sigma_{plato<>,'Gambas'}(Platos))$
```SQL
SELECT plato, precio 
FROM Platos WHERE plato <> 'Gambas';
```

### 3. Precio de los platos que conforman cada menú
##### $\Pi_{menu,tamannio,plato,precio}(Menu\bowtie Platos)$
```SQL
SELECT Menus.menu,tamannio,plato,precio 
FROM Menus JOIN Platos ON (Menus.menu = Platos.menu);
```

### 4. a) Precio de los menús (Suma de sus platos)
##### $\gamma^{menu,sum(precio)}_{menu}(Platos)$
```SQL
SELECT menu, SUM(precio)
FROM Platos
GROUP BY menu;
```

### 4. b) Apartado a) + tamaño
##### $\gamma^{menu,tamannio,sum(precio)}_{menu}(Menus\bowtie{Platos})$ 
```SQL
SELECT menu, tamannio, SUM(precio) AS total
FROM Menus NATURAL JOIN Platos
GROUP BY menu;
```

### 5. Precio y tamaño de menus con más de 2 platos
##### $\Pi_{menu,tamannio,total}(\rho_{sum(precio)/total}(\sigma_{count(plato)>2}(\gamma^{menu,tamannio,count(plato)}_{menu}(Menus\bowtie{Platos}))))$  
```SQL
SELECT menu, tamannio, SUM(precio) AS total
FROM Menus JOIN Platos ON (Menus.menu = Platos.menu)
GROUP BY menu
HAVING (COUNT(plato) > 2);
```

### 6. Platos que le gustan a Rocío
##### $\Pi_{plato}(\sigma_{persona='Rocio'}(Gourmets))$
```SQL
SELECT plato 
FROM Gourmets
WHERE persona = 'Rocio';
```

### 7. Platos que no le gustan a Rocío
##### $\Pi_{plato}(Platos)-\Pi_{plato}(\sigma_{persona='Rocio'}(Gourmets))$
```SQL
SELECT plato
FROM Platos
WHERE plato NOT IN (
	SELECT plato 
	FROM Gourmets
	WHERE persona = 'Rocio'
);
```

### 8. Menús que le gustan a Rocío 
##### $\Pi_{menu}(\sigma_{persona='Rocio'}(Platos\bowtie{Gourmets}))$
```SQL
SELECT menu
FROM Platos NATURAL JOIN Gourmets
WHERE persona = 'Rocio';
```

### 9. Menu, tamaño, precio total de los menús con precio mayor a 20.0€
##### $\Pi_{menu,tamannio,sum(precio)}(\sigma_{sum(precio)>20.0}(\gamma_{menu}^{menu,tamannio,sum(precio)}(Menus\bowtie{Platos})))$
```SQL
SELECT menu, tamannio, SUM(precio)
FROM Menus NATURAL JOIN Platos
GROUP BY menu
HAVING (SUM(precio) > 20.0);
```
## 5/12/23
### 1. Dado el siguiente trigger indique las afirmaciones correctas
```SQL
DELIMITER //
CREATE OR REPLACE TRIGGER Ejemplo03
BEFORE UPDATE OR INSERT ON Usuarios FOR EACH ROW
BEGIN
	DECLARE usuario ROW Usuarios;
	SELECT * INTO usuario
		FROM (SELECT * FROM Usuarios
			WHERE edad >= (SELECT edad FROM Usuarios 
				ORDER BY edad)) AS X;
	IF(usuario.edad > OLD.edad) THEN
		SIGNAL '45000' SET MESSAGE_TEXT = 'No cumple...';
	END IF;
END //
DELIMITER ;
```
a) El trigger se lanzaría antes de cada INSERT o UPDATE de la tabla Usuarios.
b) La variable usuario almacena una fila de la tabla Usuarios.
c) Se mostraría un mensaje de error si la edad calculada es mayor que la edad actual del usuario registrado.
d) Si hay más de un usuario registrado 
e)
### 2. Qué devuelve esta vista
```SQL
CREATE OR REPLACE VIEW Ejemplo01 AS
	SELECT U.usuarioId, U.nombre, U.genero, U.edad, 
			U.email, A.aficionId, A.aficion, 
				UA.usuarioAficionId
		FROM Usuarios U
		LEFT JOIN UsuariosAficiones UA ON 
			(U.usuarioId=UA.usuarioId)
		LEFT JOIN Aficiones A ON
			(UA.aficionId=A.aficionId);
```
Devuelve los usuarios que tienen aficiones.

### 3. Para esta vista, ¿es correcta la consulta?
```SQL
CREATE OR REPLACE Ejemplo01 AS
	SELECT U.usuarioId, U.nombre, U.genero, U.edad, U.email
		A.aficionId, A.aficion, UA.usuarioAficionId
		FROM Usuarios U
		LEFT JOIN UsuariosAficiones UA ON 
			(U.usuarioId=UA.usuarioId)
		LEFT JOIN Aficiones A ON
			(UA.aficionId=A.aficionId);
```
```SQL
SELECT *
FROM Ejemplo01
GROUP BY usuarioId;
```
No, ya que se agrupan por valores no agrupables.

### 4. Para esta vista, ¿es correcta la consulta?
```SQL
CREATE OR REPLACE Ejemplo01 AS
	SELECT U.usuarioId, U.nombre, U.genero, U.edad, U.email
		A.aficionId, A.aficion, UA.usuarioAficionId
		FROM Usuarios U
		LEFT JOIN UsuariosAficiones UA ON 
			(U.usuarioId=UA.usuarioId)
		LEFT JOIN Aficiones A ON
			(UA.aficionId=A.aficionId);
```
```SQL
SELECT usuarioId, nombre, aficion, COUNT(aficionId) as tot
FROM Ejemplo01
GROUP BY usuarioId;
```
No, por el mismo error de antes. Habría que agrupar por usuarioId y aficionId o quitar aficion del SELECT.

### 5. Para esta vista, ¿es correcta la consulta?
```SQL
CREATE OR REPLACE Ejemplo01 AS
	SELECT U.usuarioId, U.nombre, U.genero, U.edad, U.email
		A.aficionId, A.aficion, UA.usuarioAficionId
		FROM Usuarios U
		LEFT JOIN UsuariosAficiones UA ON 
			(U.usuarioId=UA.usuarioId)
		LEFT JOIN Aficiones A ON
			(UA.aficionId=A.aficionId);
```
```SQL
SELECT aficion, COUNT(aficionId) AS tot
FROM Ejemplo01
GROUP BY aficion;
```
Sí, sería correcta, ya que muestra la cantidad de personas que tienen cada afición.

-----------------------------------------------------

```SQL
SELECT aficionId, aficion, AVG(edad) as prom
FROM Usuarios
	NATURAL JOIN UsuariosAficiones
	NATURAL JOIN Aficiones
GROUP BY aficionId
HAVING prom > (
	SELECT AVG(P.prom)
	FROM (
		SELECT aficionId, aficion, AVG(edad) as prom
		FROM Usuarios
			NATURAL JOIN UsuariosAficiones
			NATURAL JOIN Aficiones
		GROUP BY aficionId
	) AS P
);
```

MAS FÁCIL:

```SQL
CREATE OR REPLACE VIEW AficionesAvgEdad AS
	SELECT aficionId, aficion, AVG(edad) as promedio
	FROM Usuarios
	NATURAL JOIN UsuariosAficiones
	NATURAL JOIN aficiones
	GROUP BY aficionId;

SELECT aficionId, aficion, promedio
FROM AficionesAvgEdad
WHERE promedio > (
	SELECT AVG(promedio) FROM AficionesAvgEdad
);
```
 $\Pi_{aficionId,aficion,promedio}(\sigma_{promedio > (\sigma_{AVG(prom)}(\rho_{prom/AVG(edad)}(\gamma_{aficionId}^{aficionId,AVG(edad)}(Usuarios\bowtie UsuariosAficiones\bowtie Aficiones))))}(\rho_{promedio/AVG(edad)}(\gamma_{aficionId}^{aficionId,aficion,AVG(edad)}(Usuarios\bowtie UsuariosAficiones\bowtie Aficiones)))$

