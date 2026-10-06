[[001-Comandos Básicos]] [[006-Comandos VLANs]]

## ¿Qué es una LAN?

La definición básica de LAN es: Un grupo de dispositivos en una sola localización (casa, oficina...)
Una descripción mas específica es: 
Una LAN es un solo **Broadcast domain**, incluyendo todos los dispositivos en ese dominio.

Un **Broadcast domain** es un grupo de dispositivos que reciben un broadcast frame (dest MAC FFF.FFF.FFF) enviado por cualquiera de los miembros del dominio.

Si tenemos una red local (LAN) con varias Subredes conectadas a un solo switch, el problema al enviar un paquete broadcast es que el switch enviará este paquete a todas las subredes. Esto genera tráfico innecesario y puede saturar la red. De este problema nacen las VLANs, para separar estas redes en la Capa 2.

![Pasted image 20260929070228.png](<../999-Imágenes/Pasted image 20260929070228.png>)

## ¿Cómo configuramos las VLANs?

Esto se configura en el Swtich, mas específicamente en cada una de las interfaces. Una vez esté configurado, el Switch solo eviará los paquetes Broadcast dentro de la VLAN donde llegó.

IMPORTANTE: El Switch NO puede enrutar entre VLANs, tiene que enviar el paquete al Router, este se lo devuelve y lo envía a la VLAN correspondiente.

El comando *show vlan brief* muestra las vlans de un router y las interfaces de cada vlan. Esta es la configuración por defecto:

![Pasted image 20260929070927.png](<../999-Imágenes/Pasted image 20260929070927.png>)

VLANs 1, 102 - 105 vienen por defecto y no pueden ser borradas. 102 a 105 son para tecnologias que no son necesarias para el CCNA.

Este es el flujo de trabajo para asignar una VLAN a un grupo de interfaces: 

![Pasted image 20260929071617.png](<../999-Imágenes/Pasted image 20260929071617.png>)

Si usamos el comando *show vlan brief* vemos las VLANs que hemos configurado

![Pasted image 20260929072300.png](<../999-Imágenes/Pasted image 20260929072300.png>)

Para que sea mas sencillo de identificar, cambiamos los nombres con los comandos que vemos abajo. Primero entramos en la vlan correspondiente, luego asignamos un nombre con el comando *name*

![Pasted image 20261002123602.png](<../999-Imágenes/Pasted image 20261002123602.png>)

#### ¿Qué es un Trunk Port?
Se usa para gestionar el tráfico de varias VLANs en una sola Interfaz. 
El tráfico va por una sola interfaz entre ambos Switchs, para poder distinguir a que VLAN corresponde, el Switch usará VLANs Tags para diferenciar VLANs en un mismo Trunk Port.
El protocolo que se usa es 802.1Q y el Switch lo pone entre el source y el type/lentgh field del frame. Y tiene 4 bytes (32 bits),  2 para Tag Protoco Identifier (TPID) y 2 para Tag Control Information (TCI).
TCI puede dividirse en 3 sub-fields PCP, DEI y VID

| 16 bits | 3 bits | 1 bit | 12 bits |
| ------- | ------ | ----- | ------- |
| TPID    | TCI    | TCI   | TCI     |
|         | PCP    | DEI   | VID     |
#### TPID
16 bits (2 bytes)
Siempre es 0x8100. Indica que el frame tiene el tag 802.1Q

#### PCP (Priority Code Point)
3 bits. 
Se usa para CoS (priorización en redes congesitoandas)

#### DEI (Drop Eligibe Indicator)
1 bit. 
Se usa para indicar que el paquete se puede dropear si la red está congestionada.

#### VID (VLAN ID)
12 bits
Es el mas importante. Identifica la VLAN.
12 bits = 4096 VLANS (2^12)
VLANS 0 y 4095 no se pueden usar

#### Native VLAN
Por defecto, la VLAN nativa del Switch es VLAN 1. No se añaden Tags 801.1Q para Native VLAN. Se puede configurar manualmente otra.
Si se recibe un paquete sin tag, se entiende que pertenece a la Native VLAN. Al no tener Tags, es un sistema mas eficiente.
**ES MUY IMPORTANTE QUE LAS NATIVE VLAN SEAN LAS MISMAS EN TODOS LOS SWITCHS**
Por seguridad, se debe configurar una VLAN que no se usa.

Hay dos formas de configurar una VLAN Nativa:
- Usando encapsulation dot1q *vlan-id* native en una subinterfaz del router.
![Pasted image 20261006181922.png](<../999-Imágenes/Pasted image 20261006181922.png>)

- Configurar la dirección IP para la Vlan nativa en la interfaz física del router.
![Pasted image 20261006182638.png](<../999-Imágenes/Pasted image 20261006182638.png>)
#### Router on a Stick (ROAS)
Se llama así porque al usar solo una interfaz para conectarse al router, en el diagrama de red parece un palo.
Se usa para interVLAN routing, y podemos subdividir una sola interfaz física en sub-interfaces.
![Pasted image 20261002133047.png](<../999-Imágenes/Pasted image 20261002133047.png>)
Las tres sub-interfaces son lógicas, y están dentro de una sola interfaz física. No debemos configurar nada en el Switch. Solo tiene que tener G0/1 como trunk y asegurarnos que VLANs 10, 20 y 30 están autorizadas.

Esta es la config del router:
![Pasted image 20261002133323.png](<../999-Imágenes/Pasted image 20261002133323.png>)

Para entrar en la sub-interfaz, interface g0/0.10 para la sub-interfaz de la VLAN10
Luego configuramos la VLAN de esta sub-interfaz con *encapsulation dot1q 10*
Y configuramos la IP, la última utilizable de la VLAN y la máscara correspondiente.
Seguimos el mismo proceso para las otras dos sub-interfaces.

### Ejemplo de ruta 

![Pasted image 20261006180801.png](<../999-Imágenes/Pasted image 20261006180801.png>)

El PC1 de VLAN10 quiere comunicarse con el PC1 de VLAN30:

El paquete se envía a SW2.
SW2 envía el paquete a R1 a través de la interfaz G0/1 con el Tag VLAN10.
R1 lo recibe en su interfaz G0/0, identificando que llega en la subinterfaz G0/0.10 porque viene con el tan VLAN10.
El destino del paquete está en la subred 192.168.1.128/26, que está conectada a la interfaz G0/0.30 de R1.
R1 envía el paquete por su interfaz G0/0 a SW2 con el Tag VLAN30 (configurado en la subinterfaz G0/0.30).
SW2 lo envía a SW1 y SW1 a PC1.

## ¿Como funcionan los Switch de capa 3 o multicapa

Estos switches pueden hacer switching y routing. Se pueden asignar IPs a sus interfaces como a un router. Se pueden crear interfaces virtuales para cada VLAN. Se pueden configurar rutas. Se puede usar para inter-VLAN routing.

SVI (Switch Virtual Interface) son interfaces virtuales a las que puedes asignar una IP en un Switch multicapa. 
Confugura cada PC para usar SVI como Gateway Address y no el router.

#### Configuración de SVI 
![Pasted image 20261006184036.png](<../999-Imágenes/Pasted image 20261006184036.png>)

