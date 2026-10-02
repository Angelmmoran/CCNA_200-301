[[001-Comandos Básicos]] [[003-Comandos IPv4]]

![[Pasted image 20260911072748.png]]

Para leer este cuadro, empezamos arriba a la izquierda; y leemos hacia la derecha y luego hacia abajo (version, ILH, DSCP, ECN, Total Lenght, Identification...)

- **Version**: Longitud 4 bits. 
		- Identifica la versión de IP en binario (en este caso 4 asique siempre será 0100)
- **IHL** (Internet Header Length): Longitud 4 bits.
		- Indica la longitud total del header. La última parte (options) es variable.
		- Identifica la longitud del header en incrementos de 4 bytes (valor 5 = 5x4 = 20 bytes).
		- El valor mínimo es 5 (empty option field), el valor máximo es 15 (15x4 = 60 bytes).
- **DSCP** (Differentiated Serrvices Code Point): Longitud 6 bits.
		- Se utiliza para QoS (Quality of Service), para priorizar información prioritaria para evitar retrasos o lag.
- **ECN** (Explicit Congestion Notification): Longitud 2 bits.
		- Notifica congestión de red entre endpoints sin dropear el paquete.
		- Es opcional porque ambos endpoints y la infraestructura deben soportarlo.
- **Total Lenght**: Longitud 16 bits.
		- Indica la longitud total del paquete (L3 header + L4 segment).
		- Mide la longitud en bytes (pero no en incremental de 4, ya indica el valor total).
		- Tiene un valor mínimo de 20 y un valor máximo de 65.535 (valor max 16 bits).
- **Identification**: Longitud 16 bits.
		- Si el paquete debe ser fragmentado, este campo identifica a que paquete pertenece el fragmento.
		- Un paquete se fragmenta si su longitud es mayor que el MTU (Maximun Transmission Unit) que es normalmente 1500 bytes.
- **Flags**: Longitud 3 bits.
		- Se usa para controlar/identificar fragmentos. Los 3 bits se dividen así:
					- Bit 0: Reservado, siempre es 0.
					- Bit 1: Dont fragment (DF) bit. Indica que el paquete no debe ser fragmentado (si su valor es 1).
					- Bit 2: More Fragments (MF) bit. Si su valor es 1, indica que hay mas fragmentos, si es 0 indica que es el último fragmento.
- **Fragment Offset**: Longitud 13 bits.
		- Se usa para indicar la posición del fragmento dentro del paquete original.
		- Permite ordenar el paquete aunque llegue desordenado.
- **Time To Live**: Longitud 8 bits.
		- Los paquetes con un TTL 0 son dropeados.
		- Se usa para evitar loops infinitos.
		- Originalmente debía indicar el tiempo de vida del paquete en segundos. Actualmente, indica el número de saltos (hops) permitidos. Cada vez que el paquete pasa por un router, este número disminuye en uno. 
- **Protocol**: Longitud 8 bits.
		- Indica el protocolo del L4PDU encapsulado. Normalmente tiene uno de estos valores:
					- 6 para TCP.
					- 17 para UDP.
					- 1 para ICMP (ping).
					- 89 OSPF (Open Shorter Path First).
- **Header Checksum**: Longitud 16 bits.
		- Checksum usado para comprobar errores en el header de IPv4.
		- Cuando un router recibe un paquete, calcula el checksum y lo compara con este campo, si no son iguales, el router dropea el paquete. 
		- El checksum se calcula con un algoritmo.
- **Source / Destination IP Address**: Longitud 32 bits.
		- Source = Dirección IPv4 que envía el paquete.
		- Destination = Dirección IPv4 a la que va dirigida el paquete.
- **Options**: Longitud 0-320 bits.
		- Es un campo opcional. Raramente usado.
		- Si IHL es mayor que 5, quiere decir que en este campo hay información.