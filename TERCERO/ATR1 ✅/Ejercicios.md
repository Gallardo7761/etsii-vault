## 4.11 
```
enero.examen.atr1.edu. IN SOA 
	serverd.enero.examen.atr1.edu.admin.enero.examen.atr1.edu.
		{ 2022011300
		  60000
		  15000
		  100000
		20000 }
enero.examen.atr1.edu.          IN NS serverd. enero.examen.atr1.edu.
enero.examen.atr1.edu.          IN NS dns-server.otrodominio.es.
serverd. enero.examen.atr1.edu. IN A 129.2.32.3
prueba.enero.examen.atr1.edu.   IN A 190.1.3.32
www.enero.examen.atr1.edu.      IN A 180.2.33.2
```
**a) Indique la información DNS que almacenará el servidor “defaultserver.midominio.es.” en su caché DNS al resolver la consulta. Nota: Utilice el formato IP_XXX, donde XXX representa el hostname del servidor y la etiqueta del dominio autoritativo que gestiona del servidor, para representar la dirección IP de dicho servidor. Esto es, para representar la dirección IP de “ejemplo.midominio.org.” se usará “IP_ejemplo.midominio”.**

Glue Record:
```
enero.examen.atr1.edu.         IN NS serverd.enero.examen.atr1.edu.
enero.examen.atr1.edu.         IN NS dnsserver.otrodominio.es.
serverd.enero.examen.atr1.edu. IN A 192.2.32.3
```

