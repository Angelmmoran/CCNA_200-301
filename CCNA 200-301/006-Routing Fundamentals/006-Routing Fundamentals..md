[[001-Comandos Básicos]] [[005-Comandos Routing]]
# ¿Que es routing?
##### Es el proceso que usan los routers para determinar el camino que el paquete debe tomar en la red para llegar a su destino.
-> Los routers guardan sus destinos conocidos en una Routing Table.
-> Cuando un router recibe un paquete, mira esta tabla para buscar la mejor ruta.

Hay dos tipos (métodos) principales de routing (enrutamiento):
		**Dinámico**: Los routers comparten información de enrutamiento entre ellos de forma automática y construyen sus routing tables.
		**Estático**: Un ingeniero/admin configura las rutas manualmente.
		
Una ruta le dice al router:
 -> Para mandar el paquete al destino X debes enviarlo al hop Y (next-hop: siguiente router en la ruta).
 -> Si la dirección X está conectada directamente al router, envía el paquete al destino final.
 -> Si el destino final es el router en si mismo, no envíes el paquete.
 -> Si la ruta no existe, el router dropea el paquete, nunca hace flood.

![[Pasted image 20260913093136.png]]

---

 **Configuración R1**: ![[Pasted image 20260913094101.png]]

**Tabla rutas R1**:
![[Pasted image 20260913094228.png]]
 - Ruta conectada (C) es una ruta a la red conectada conectada a la interfaz.
	 - R1 G0/2 IP = 192.168.1.1/24
	 - Network Address = 192.168.1.0/24
	 - Provee rutas a todos los host de la interfaz (192.168.1.10, 192.168.1.100, etc)
	 - Viendo esta tabla de rutas podemos saber que:
		 - Si R1 necesita enviar un paquete a algún host en 192.198.1.0/24 lo tiene que enviar a la interfaz G0/2.
- Ruta local (L) es la ruta a la dirección IP exacta configurada en la interfaz.
	- La máscara /32 se usa para especificar la IP exacta de la interfaz.
	- /32 significa que los 32 bits están fijados y no pueden cambiar.
	- Aunque la interfaz G0/2 de R1 está configurada como 192.168.1.1/24, la ruta L es  192.168.1.1/32. Indica SOLO esta dirección, ni incluye, por ejemplo 192.168.1.2.
	- Con esto, R1 sabe que si recibe un paquete a esta dirección es para el mismo.

![[Pasted image 20260913095859.png]]

![[Pasted image 20260913100036.png]]

## Selección de ruta

![[Pasted image 20260913172118.png|700]]

Si R1 recibe un paquete con destino 192.168.1.1, ¿a que IP lo envía? 
		- 192.168.1.0/24
			 o
		- 192.168.1.1/32
Ambas direcciones IPs pueden parecer que son el destino. Para resolver esto, el router escogerá la IP que coincida de manera **mas específica**.
La ruta hacía la IP 192.168.1.0/24 incluye 256 IPs distintas (192.168.1.0-192.168.1.255).
La ruta hacia la IP 192.168.1.1./32 incluye **SOLO** la IP 192.168.1.1.
			-> Esta ruta es mas específica.
**Mas específica** significa la ruta coincidente con el **prefijo fijado mas largo**.
![[Pasted image 20260913173037.png]]

#### Otros ejemplos de selección de ruta:

![[Pasted image 20260913174515.png]]

### Resumen:

- Un Router almacena la información sobre destinatarios en su **routing table**.
	-> Cuando reciben un paquete, miran esta tabla para encontrar la mejor ruta posible.
- Cada **ruta** en la tabla es una instrucción:
	-> Para llegar al destino en la red X, manda el paquete al **next-hop** Y.
	-> Si la dirección está **directamente conectada** (C), manda el paquete a ese destino.
	-> Si la dirección es **tu propia IP** (L), recibe tu mismo el paquete.
	-> Si la dirección **NO** coincide con ninguna, dropea el paquete (no flood).
- Cuando configuras una IP en una interfaz y la pones como **"enable"**, dos rutas son añadidas automáticamente al la **routing table**:
	- **Connected (C)**: Una ruta conectada a esa interfaz.
		-> Si la IP de la interfaz es 192.168.1.1/24, la ruta será 192.168.1.0/24.
	- **Local (L)**: Una ruta con la dirección IP **exacta** del Router.
		-> Si la IP de la interfaz es 192.168.1.1/24, la ruta será 192.168.1.1/32.
- Una ruta **coincide** con el destino si la IP de destino del paquete es parte de la red de la ruta especificada:
			- > Un paquete a 192.168.1.60 coincide con la ruta 192.168.1.1/24 pero no con 192.168.0.0/24.
- Si un Router recibe un paquete cuyo destino coincide con varias rutas posibles, escogerá la mas **específica** (la ruta coincidente con el prefijo fijado mas largo).






