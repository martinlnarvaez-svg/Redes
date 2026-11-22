## **Definiciones** 
- Una *red* es un conjunto de dispositivos interconectados a traves de distintos medios con el principal objetivo de transmitir informacion y compartir recursos y/o servicios
- Un *protocolo* es un conjunto de reglas que define el formato y orden de los mensajes que se intercambian y las acciones que se llevan a cabo en la transmision y/o recepcion de un mensaje u otro evento
- Un *PDU* (Protocol Data Unit) es el tipo de dato que utiliza cada capa
- La *encapsulacion* consiste en tomar el PDU de la capa anterior a la actual, agregarle informacion (headers) para convertirla en un nuevo PDU
- Un *User Agent* es una interfaz de usuario entre el usuario y la aplicacion de red

## **Funcion**
- La funcion de la capa de aplicacion es proveer servicios para que las aplicaciones de usuario puedan comunicarse entre si, ya sea que esten en una misma computadora o en computadoras distintas, la idea es hacer la comunicacion mas amigable y user-friendly. Su PDU es el Dato

## HTTP
### HTTP vs HTML
- HTML es un lenguaje usado para estructurar paginas web mientras que HTTP es un protocolo para comunicar clientes con servidores
- HTTP usa por defecto el puerto 80
- HTTPS usa por defecto el puerto 443
### Requerimientos HTTP
Los requerimientos son pedidos que se hacen de un cliente a un servidor e incluyen el formato: MetodoHTTP RecurosHTTP/VersionHTTP. Por ejemplo: GET /hola/test.html HTTP/1.1
### Respuestas HTTP <br>
Las respuestas son lo que el servidor le responde al cliente, y tienen el siguiente formato: HTTP/Version CodDeRespuesta. Por ejemplo: HTTP/1.1 200 OK
### Comando curl <br>
Realiza peticiones de varios protocolos URL. Cuando el HTML de la URL tiene recursos externos, el navegador hara HTTP requests separados de forma transparente para traerlos y tiene los siguientes parametros: <br>
-I: Realiza una solicitud HTTP usando el metodo HEAD, es decir no muestra el BODY, unicamente los HEADERS <br>
-H: Agregar headres personalizados al request del tipo clave:valor <br>
-X: Permite especificar el metodo HTTP (GET, PUT POST, etc) <br>
-S: Silencia el progreso y no muestra mensajes de error <br>
-V: Muestra informacion detallada del request, headers de la respuesta, informacion de la conexion, etc
### Versiones de HTTP <br>
- 1.0: Usa texto ASCII, tiene conexiones no persistentes, osea, por cada request se crea una nueva conexion TCP que se termina cuando el servidor contesta. 1 IP -> 1 dominio
- 1.1: Usa texto ASCII, tiene conexiones persistentes, osea, se pueden enviar muchos requests en una misma conexion TCP de forma secuencial. 1 IP -> N dominios
- 2.0: Usa binario (dificil de leer). Multiplexing, similar a 1.1 pero ademas los requests se realizan de forma concurrente. No hay que esperar a que uno haya terminado de enviarse para enviar el siguiente. 1 IP -> N dominios

#### Como saber si se trata de HTTP 1.0 o 1.1?
En HTTP 1.0 puede usarse la cabecera HOST pero es poco comun, mientras que en HTTP 1.1 siempre se usa esta cabecera. <br>
En HTTP 1.0 la cabecera Connection tiene por defecto el valor close, mientras que e HTTP 1.1 tiene por defecto el valor keep-alive
#### Codigos HTTP <br>
Valores que se envian en una respuesta HTTP que representan lo que ocurrio con el requerimiento, si fue exitoso, si fallo por alguna razon, etc <br>
**1XX**: Informacion de estado <br>
**2XX**: Exito <br>
**3XX**: Redireccion <br>
**4XX**: Error de cliente <br>
**5XX** Error del servidor
#### Headers HTTP <br>
Son datos con el formato clave:valor que se intercambian en los requerimientos y respuestas HTTP <br>
Accept: application/jason/text/html -> Tipos de datos que el cliente espera como respuesta <br>
Content-Type: application/json -> Tipos de datos que se enviaron <br>
Authorization: Bearer -> Credenciales de autenticacion que envia el cliente <br>
User-Agent: curl/7.88.1 -> Datos del cliente que realizo el request <br>
Location: https://abc.com/index.php -> Indica la nueva URL a la que el cliente deberia redireccionarse <br>
Server: Apache2 -> Datos del servidor <br>
Content-Lenght -> Tamaño de la respuesta en bytes <br>
Host: www.abc.com -> Especifica el dominio e interes, para evitar ambiguedades. Muy util cuando un dominio tiene alojados mas de 1 sitio <br>
Date -> Indica fecha, hora y timezone en que se envio la respuesta <br>
Last- Modified: Indica fecha, hora y timezone de la ultima vez que se modifico (en el servidor) el recurso solicitado <br>
If- Modified-Since -> el servidor retorna el recurso solicitado SOLO si fue modificado DESPUES de la fecha ingresada <br>
Set-Cookie -> El servidor envia una cookie al cliente <br>
Cookie -> El cliente le envia una cookie al servidor
### GET vs POST <br>
La diferencia principal es que el GET envia los datos ingresados en el formulario a traves de la URL via querys (?clave=valor) mientras que el POST envia los datos ingresados en el body del request, por ende es mas seguro <br>
Otras diferencias:
- Los POST no se cachean, los GET pueden cachearse
- Los POST no permanecen en el historial del browser, los GET si
- Los POST no tienen limites de longitud en cuanto a los datos a enviar, los GET si, ya que las URLs (donde los queary parameters se colocan) tienen limites de longitud
### SSH
Permite ejecutar una conexion remota con un servidor y ejecutar comandos. La data va cifrada. Por defecto usa el puerto 22
## DNS 
DNS es un protocolo que traduce nombre de dominio a direcciones IP, que son identificadores numericos separados por puntos. La idea es que no tengamos que buscar sitios web mediante su IP, sino por sus nombre y que el DNS los traduzca. Por defecto usa el purto 53 usa UDP <br>
Ademas de ser un protocolo, tambien es un sistema distribuido de forma jerarquica por todo el mundo
### Root servers
Son servidores DNS que estan al principio de la jerarquia de servidores DNS, son los primeros que los resolvers consultan y tienen informacion de los Top Levels Domains
### Top Levels Domains
- Se dividen en 2 tipos: Generic y Country Code <br>
	Generic: Indican el proposito general de la pagina. ".com", ".org", etc <br>
	Country Code: indican de que pais es el dominio. ".arg", ".es", etc
### Servidores autoritativos
Son aquellos que poseen informacion original, actualizada y oficial de los registros DNS de un dominio, los caches de servidores no autoritativos pueden tener informacion desactualizada de un dominio, y pueden tardar bastante en actualizarse. Se dividen en 1 primario y N-1 secundarios. El primario copia su informacion a los secundarios regularmente, esto se conoce como transferencia de zona
### Resolver
Es un componente de software integrado en el SO que resuelve todas las consultas DNS. Se comunica con otros servidores DNS via internet y cuando recibe una consulta, primero chequea su cache. Si no encuentra lo pedido ahi, realiza un consulta al servidor de DNS configurado como local
### Consultas DNS recursivas vs iterativas
Las consultas recursivas son aquellas que el resolver resuelve por completo una vez que el cliente le realiza una solicitud. Si el resolver tiene la respuesta en su cache la devuelve al cliente rapidaente. SI no, va preguntando recusivamente servidor por servidor segun lo que le responden hasta traer la respuesta autoritativa. <br>
	Cuando un cliente quiere saber una IP, le realiza una consulta recursiva a su resolver local <br>
Las consultas iterativas son aquellas donde el resolver le da al cliente una respuesta segun lo que tiene en su cache. Si su cache esta vacia, devuelve una referencia a otro servidor DNS. El cliente es responsable por ende de hacer las subsecuentes querys segun la referencia que el resolver le devolvio <br>
	Cuando el resolver local necesita saber una IP, realiza consultas iterativas empezando por los root servers <br>
Si esta en cache o esta en el servidor autoritativo entonces la respuesta no es recursiva, es iterativa
### Respuestas DNS autoritativas vs no autoritativas
Las respuestas autoritativas son las que retornan efectivamente la direccion IP del dominio solicitado, y esta respuesta proviene del servidor autoritativo de ese dominio, y si no, de algun otro servidor que aun posea la IP en su cache. Las respuestas no autoritativas provienen de servidores DNS que no son autoritativos
### Delegacion de dominios
Cuando tenemos subdominios como "example.com" - "test.example.com" hay que delegar, test.example.com tendra sus propios servidores autoritativos separados de los de example.com. <br>
Se crean registros NS en el servidor autoritativo de example.com que apuntan a los servidores autoritativos de test.example.com. <br>
En el servidor autoritativo para el dominio example.com tenemos: <br>
	test.example.com IN NS ns1.test.example.com<br>
	ns1.test.example.com IN A 192.168.1.0 
### Flags DNS<br>
**aa**: authoritative answer<br>
**rd**: recursion desired<br>
**ra**: recursion accepted<br>
**qr**: query/response
### Codigos de respuesta DNS<br>
**NOERROR**: La query fue exitosa (el dominio existe) pero el registro solicitado puede no existir<br>
**NXDOMAIN**: El dominio solicitado no existe<br>
**SERVFAIL**: Hay un problema tecnico con los servidores DNS. Podria ser un firewall<br>
**REFUSED**: El servidor rechazo la consulta por motivos de seguridad o falta de autorizacion
### Registros DNS <br>
**A**: Mapea un nombre de dominio a una direccion IPv4<br>
**AAAA**: Idem A pero a IPv6<br>
**PTR**: Mapea una direccion IPv4 a un nombre de dominio<br>
**MX**: servidores de email responsables de recibir emails (no enviar) para un nombre de dominio. Cuanto mas bajo el valor, mas prioridad tiene el MX<br>
**TXT**: Informacion de nombre de dominio<br>
**SRV**: Servicios disponibles para un nombre de dominio<br>
**NS**: Servidores autoritativos para un nombre de dominio<br>
**CNAME**: Alias de un nombre de dominio<br>
**SOA**: Informacion administrativa de una zona DNS. Sirve para configurar y sincronizar a los servers DNS secundarios. Muestra, entre otras cosas, cual de los servidores autoritativos es el primario
## SMTP <br>
Simple Mail Transfer Protocol es un protocolo que se usa cuando un cliente envia un email a su mail server y tambien cuando un servidor de mail le manda un email a otro servidor de mail. Esto es porque SMTP puede actuar como cliente o servidor. Por defecto usa el purto 25 y usa TCP<br>
### Pop3
Protocolo simple, cuando el usuario quiere acceder a sus emails los descarga y estos se borran del servidor, no guarda estado, por ende es eficiente. Al no soportar estado no permite la sincronizacion a traves de multiples dispositivos. Por defecto usa el puerto 110 y TCP<br>
### IMAP
 Protocolo mas complejo que permite al usuario leer, borrar, y organizar sus emails en carpetas como si fuera un filesystem, guarda estado. Al guardar el estado se puede sincronizar a traves de multiples dispositivos. Usa el puerto 143 y usa TCP
### Encoding de imagenes
Tanto SMTP como POP3 e IMAP necesitan encodificar las imagenes para transformarlas a texto al enviar o recibir un email que las contenga, ya que trabajan unicamente con contenido de texto plano <br>
## FTP 
Protocolo que permite transferir archivos, necesita 2 conexiones TCP, una de control y otra de datos. Tiene 2 modos, activo y pasivo, que difieren en como se maneja la conexion de datos.<br>
	Activo: El cliente abre uno de sus puertos para la conexion de datos, y el servidor se conecta al cliente, puede ser inseguro, puede generar problemas de firewall/NAT y no se suele usar hoy en dia.<br>
	Pasivo: El servidor abre uno de sus puertos para la conexion de datos, y el cliente se conecta al servidor, suele ser seguro, no suele causar problemas de firewall/NAT y es el mas usado actualmente