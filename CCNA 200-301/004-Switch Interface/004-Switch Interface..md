[[001-Comandos Básicos]]
[[004-Comandos Switch Interface]]

Al acceder a la configuración de las interfaces de un switch, veremos el estado de estas. 
Pueden estar up / up o down / down.
A diferencia de los routers, los switches de CISCO no vienen con el comando "shutdown" de fábrica, por lo que nunca encontraremos un router con el estado "admistratively down" por defecto. Este estado solo se da cuando usamos el comando "shutdown"
Que un switch tenga el estado "down" significa que no está conectado a otro dispositivo.

#### Comando show interfaces status

![Pasted image 20260906110831.png](<../999-Imágenes/Pasted image 20260906110831.png>)

Port muestra cada interfaz 
Name muestra la descripcion 
Status muestra el estado de la interfaz
Vlan (explicado mas adelante)
Duplex muestra el tipo de duplex configurado en la interfaz (auto por defecto)
Speed muestra la velocidad máxima configurada (auto por defecto)
Type muestra el tipo de interfaz
(Ver *Comandos Switch Interface* para los comandos de configuración)

Por seguridad, debemos desactivar las interfaces que no están en uso 

![Pasted image 20260906111813.png](<../999-Imágenes/Pasted image 20260906111813.png>)

## Full y Half Duplex

**Half duplex**: El dispositivo NO puede enviar y recibir información al mismo tiempo. 
**Full duplex**: El dispositivo SI puede enviar y recibir información al mismo tiempo.

Half duplex casi no se usa en las redes modernas, ya que puede provocar colisiones de los paquetes si un dispositivo (como un Hub) recibe dos paquetes a la vez. El Hub repetiría el paquete a todas las interfaces, pudiendo provocar colisiones. Cada red conectada a un Hub se llama "Collision Domain"
Para evitar esto se usa el mecanismo CSMA/CD
##### CSMA/CD
Carrier Sense Multiple Access with Collision Detection
Antes de enviar un paquete, los dispositivos "escuchan" para ver si otro dispositivo no ha enviado un paquete. 
Incluso así, las colisiones pueden ocurrir. Si pasa, el dispositivo envía una "jammin signal" para indicar que ha ocurrido una colisión.
Cada dispositivo esperará un tiempo aleatorio antes de volver a enviar paquetes.
El proceso se repite.

Este es el sistema usado durante muchos años, pero es ineficiente comparado con los Switches. 


## Speed/Duplex Negotiation

Las interfaces que pueden trabajar a distintas velocidades vienen configuradas en modo auto en ambas.
Las interfaces en auto "anuncian" su capacidad a los otros dispositivos y negocian la mejor velocidad y duplex posible entre ellos.

##### ¿Que pasa si "Autonegotiation" está desactivada?

**Velocidad**: El Switch intentará "sentir" a que velocidad está operando el otro dispositivo. Si no consigue "sentirlo", usará la mas lenta disponible.

**Duplex**: Si la velocidad es 10 o 100 Mbs, usará Half Duplex. Si la velocidad es 1000 Mbs o mayor, usará Full Duplex.

#### Interfaces Errors

Esto lo vemos con el comando show interface. Esta información aparece en la parte de abajo. 

![Pasted image 20260906114013.png](<../999-Imágenes/Pasted image 20260906114013.png>)

