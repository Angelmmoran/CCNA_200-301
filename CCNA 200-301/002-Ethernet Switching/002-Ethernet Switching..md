[[001-Comandos Básicos]]
[[002-Comandos Ethernet Switching]]

![[Pasted image 20260825131044.png]]

---

PREAMBLE:

- Longitud: 7 bytes (56 bits)
- Alterna 1's and 0's
- 10101010 * 7x
- Permite a los dispositivos sincronizar sus relojes

SFD : ‘Start Frame Delimiter’

- Longitud: 1 byte(8 bits)
- 10101011
- Marca el final del PREAMBLE y el inicio del resto del Frame

DESTINATION AND SOURCE

- Indica el dispositivo emisor y el receptor
- MAC = ’Media Access Control’

TYPE / LENGTH

- 2 bytes (16-bit) field
- Un valor de 1500 o menos indica la longitud del paquete encapsulado.
- Un calor de 1536 o mas indica el tipo de paquete encapsulado (la longitud se indicará de otra forma)
- IPv4 = 0x0800 (hexadecimal) = 2048 en decimal
- IPv6 = 0x86DD (hexadecimal) = 34525 en decimal

ETHERNET TRAILER contiene:

- FCS (‘FRAME CHECK SEQUENCE’)
- Longitud de 4 bytes (32 bits) 
- Detecta información corrupta ejecutando un algoritmo 'CRC' sobre la información recibida (CRC = "Cyclic Redundancy Check")


---
### MAC Address Table

Cada switch aprende de forma dinámica las direcciones MAC de los dispositivos que tiene conectados, usando la sección "SOURCE MAC ADDRESS" de los Frames que recibe. Las direcciones MAC que no estén activas durante 5 minutos se borran de esta tabla de forma automática.


---
### ARP request

Un "ARP request" se utiliza para conocer la dirección MAC (capa 2) de una dirección IP conocida (capa 3)

Consiste de dos mensajes:
- ARP REQUEST (Broadcast. Inunda todos los puertos)
- ARP REPLY (Unicast. Solo va a la dirección que hizo el request)
