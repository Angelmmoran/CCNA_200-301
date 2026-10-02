[[001-Comandos Básicos]][[003-Comandos IPv4]][[005-Comandos Routing]]
Hasta ahora hemos usado FLSM (Fixed). Todas las subredes usan el mismo prefix lenght (ej. subnnetting clase c en 4 subnets usando /26)

VLSM es el proceso de crear subnets de diferentes tamaños para hacer mas eficiente la network addressing.

![[Pasted image 20260921094451.png]]

### Pasos para hacer subnetting con VLSM

1- Asignamos la subred mas grande al principio (Tokio LAN A).
2- Asignamos la segunda mas larga.
3- Repetimos hasta que todas las subredes han sido asignadas.

En este ejemplo, el orden será:

Tokio LAN A, Toronto LAN B, Toronto LAN A, Tokio LAN B, Point-to-point connection.

### Tokio LAN A

Network address: 192.168.1.0/25
Broadcast address: 192.168.1.127/25
First usable address: 192.168.1.1/25 (+1 a network address)
Last usable address: 192.168.1.126/25 (-1 a broadcast address)
Total de usable host: 126

Para esta LAN, usaremos /25. Esto deja 7 bits para hosts, lo que significa 128 direcciones, que son 126 hosts. Necesitamos 110. A partir de ahí, dividimos la IP en binario en las dos porciones, y sacamos el resto de direcciones:

**11000000.10101000.00000001.0**0000000
	192         168          1                 0

Network address. Cambiamos toda la porción de host a 0, y luego cambiamos a decimal:

**11000000.10101000.00000001.0**0000000
	192         168           1             0

Añadimos 1 a la Network address y obtenemos la First usable address (.00000001)

**11000000.10101000.00000001.0**0000001
	192        168            1            1

Cambiamos todos los hosts bits a 1, y tenemos la broadcast:

**11000000.10101000.00000001.0**1111111
	192        168            1            127

Quitamos 1 de la Broadcast y obtenemos la Last usable address (.11111110)

**11000000.10101000.00000001.0**1111110
	192        168            1            126

Para determinar el número de hosts, contamos el número de bits reservados para los hosts (7) y aplicamos la fórmula: 

2^7 -2 = 126 hosts por subred.

### Toronto LAN B

Asignamos la siguiente red (después de la Broadcast de Tokio LAN A) 192.168.1.128 a Toronto LAN B. Pero, ¿que prefijo tenemos que usar? Necesitamos 45 hosts. Tenemos que usar /26

Network address: 192.168.1.128/26
Broadcast address: 192.168.1.191/26
First usable address: 192.168.1.129/26 (+1 a network address)
Last usable address: 192.168.1.190/26 (-1 a broadcast address)
Total de usable host: 62 hosts.

**11000000.10101000.00000001.10**000000
	192     168            1              128

Cambiando todos los bits de hosts a 1, obtenemos la broadcast. Con esta y con la network tenemos el resto de respuestas. 

**11000000.10101000.00000001.10**111111
	192.     168.           1.             191
### Toronto LAN A

Asignamos la siguiente a la broadcast de Toronto LAN B, 192.168.1.192, y necesitamos 29 hosts, por lo que usamos /27. Hacemos el mismo proceso y sacamos todas las otras direcciones.

Network address: 192.168.1.192/27
Broadcast address: 192.168.1.223/27
First usable address: 192.168.1.193/27 (+1 a network address)
Last usable address: 192.168.1.222/27 (-1 a broadcast address)
Total de usable host: 30 hosts.

**11000000.10101000.00000001.110**00000

Cambiando todos los bits de hosts a 1, obtenemos la broadcast. Con esta y con la network tenemos el resto de respuestas. 

**11000000.10101000.00000001.110**11111
	192.         168.        1.           223

### Tokio LAN B

Asignamos la siguiente a la broadcast de Toronto LAN A, 192.168.1.224, y necesitamos 8 hosts, por lo que usamos /28. Hacemos el mismo proceso y sacamos todas las otras direcciones.

Network address: 192.168.1.224/28
Broadcast address: 192.168.1.239/28
First usable address: 192.168.1.225/28 (+1 a network address)
Last usable address: 192.168.1.238/28 (-1 a broadcast address)
Total de usable host: 14 hosts.


**11000000.10101000.00000001.1110**0000
	192.         168.        1.            224

Cambiando todos los bits de hosts a 1, obtenemos la broadcast. Con esta y con la network tenemos el resto de respuestas. 

**11000000.10101000.00000001.1110**1111
	192.     168.           1.             239


### Point-to-point 

Asignamos la siguiente a la broadcast de Tokio LAN B, 192.168.1.240, y necesitamos 2 hosts, por lo que usamos /30. Hacemos el mismo proceso y sacamos todas las otras direcciones.

Network address: 192.168.1.240/30
Broadcast address: 192.168.1.243/30
First usable address: 192.168.1.241/30 (+1 a network address)
Last usable address: 192.168.1.242/30 (-1 a broadcast address)
Total de usable host: 2 hosts.

**11000000.10101000.00000001.111100**00
	192      168           1              240

Cambiando todos los bits de hosts a 1, obtenemos la broadcast. Con esta y con la network tenemos el resto de respuestas.

**11000000.10101000.00000001.111100**11
192      168           1              243


![[Pasted image 20260921103811.png]]