[[001-Comandos Básicos]] [[005-Comandos Routing]]

Vamos a enviar un paquete desde PC1 a PC4:

![[Pasted image 20260913183330.png]]

Endhost como PC1 y PC4 pueden enviar paquetes directamente a destinatarios conectados a su misma red. 
Para enviar paquetes fuera de su red local, deben enviarlos a su **"default gateway"**. 

![[Pasted image 20260913183937.png]]

 "Default gateway" también se conoce como "default route". 
	 -> Es una ruta a 0.0.0.0/0 que incluye todas las direcciones desde 0.0.0.0 a 255.255.255.255.
Los endhost normalmente no necesitan conocer mas rutas a parte de "default route". Solo tienen que saber que si quieren enviar un paquete fuera de su red local deben enviarlo a esa dirección.

##### Route table R1
![[Pasted image 20260913184505.png|443]]

PC1 envía el paquete encapsulado a R1.
R1 desencapsula el paquete y mira a que dirección IP tiene que enviarlo. 
Al revisar su tabla de direcciones, no encuentra un ruta específica para la IP de destino.
Como el router no tiene una ruta para esta IP, debe dropear el paquete. 

### Para solucionar esto, usamos Static Route
Cada router debe tener rutas para comunicarse con PC1 y con PC2, para asegurar la comunicación entre ambos endhost. Si esto no es así, por ejemplo, PC1 podrá comunicarse con PC4 pero no al revés. 
Para permitir a PC1 comunicarse con PC4, y viceversa, debemos configurar las "static routes":

![[Pasted image 20260913185428.png]]

### ¿Como configuramos esto?

Desde la CLI del Router:
Para configurar la static route: ![[Pasted image 20260913185747.png|334]]
*ip route 192.168.4.0 255.255.255.255.0 192.168.13.3* -> ruta a R3.
Ahora R1 sabe que para enviar paquetes a 192.168.4.0 (red de R4) tiene que enviarlo a 192.168.13.3 (R3). R1 no necesita saber mas, R3 se encargará de buscar a donde enviar ahora el paquete para llegar al destino.

Ahora vamos a configurar R3: ![[Pasted image 20260913190323.png|340]]
*ip route 192.168.1.0 255.255.255.0 192.168.13.1* -> ruta a R1.
*ip route 192.168.4.0 255.255.255.0 192.168.34.4* -> ruta a R4.
Ahora R3 sabe que para enviar paquetes a 192.168.1.0 (red de R1) tiene que enviarlo a 192.168.13.1 (R1), y para enviar paquetes a 192.168.4.0 (red de R4) tiene que enviarlo a 192.168.34.4 (R4).

Ahora vamos a configurar R4: ![[Pasted image 20260913190937.png|345]]
*ip route 192.168.1.0 255.255.255.0 192.168.34.3* -> ruta a R3.
Ahora R4 sabe que para enviar paquetes a 192.168.1.0 (red de R1) tiene que enviarlo a 192.168.34.3 (R3). R4 no necesita saber mas, R3 se encargará de buscar a donde enviar ahora el paquete para llegar al destino.

### ¿Como configurar "default route"?

"Default route" es una ruta a 0.0.0.0/0
	-> Es la ruta menos específica, incluye todas las IPs de destino posibles.
Si un router no tiene una ruta mas específica que esta, mandará el paquete a la "default route".
Es la ruta usada para enviar trafico a internet.

![[Pasted image 20260913192703.png]]

Para configura la "default route" en esta red; e indicar al router que todo el tráfico que no esté dirigido a 192.168.12.0/24 o 192.168.13.0/24 lo envíe a esta "default route"; desde la CLI, en global config:
*ip route 0.0.0.0 0.0.0.0 203.0.113.2*
