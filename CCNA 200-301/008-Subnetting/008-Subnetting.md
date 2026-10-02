[[001-Comandos Básicos]]

Un problema con las asignaciones de IPs es la cantidad de direcciones que se pierden y no pueden usarse. Ejemplo:

Una compañía X necesita IPs para 5000 hosts. Una IP de clase C no da los suficientes hosts, por lo que habría que asignarle una IP tipo B. Las direcciones B admiten unos 65.000 hosts, lo que dejaría aproximadamente 60.000 direcciones IPs sin usar y perdidas que no pueden asignarse a otra compañía.

Para solucionar este problema, existe CIDR (Classless Inter-Domain Routing). CIDR elimina los requerimientos de las clases A, B y C de reservar 8, 16 y 24 bits para la porción de red respectivamente. Esto permite separar las redes en porciones mas pequeñas para evitar el malgaste de IPs.
Estas redes mas pequeñas se llaman "subnets."

### ¿Como funciona CIDR?

Sencillamente se asignan bits de la porción de host a la porción de red. Esto reduce exponencialmente la cantidad de hosts permitidos en la red, haciendo que se malgasten menos IPs.

Para calcular la cantidad de IPs asignables en una red, usamos esta fórmula: 
			2^n -2 Siendo n el numero de bits de la porción de host. 
El -2 corresponde a la IP broadcast y la IP de la propia red, que no pueden ser asignadas a ningun host.

### Subnets/Hosts (Class C)

| Prefijo | Número de subredes | Número de hosts en cada subred |
| ------- | :----------------: | :----------------------------: |
| /25     |         2          |              126               |
| /26     |         4          |               62               |
| /27     |         8          |               30               |
| /28     |         16         |               14               |
| /29     |         32         |               6                |
| /30     |         64         |               2                |
| /31     |        128         |             0 (2)              |
| /32     |        256         |             0 (1)              |
Cada bit que coges de la parte de host, duplica el número de subredes. Cada bit que dejamos en la porción de host, duplica el número de hosts, pero hay que quitarle 2 en cada paso. 

### Subnets/Hosts (Class B)

| Prefijo | Número de subredes | Número de hosts en cada subred |
| ------- | :----------------: | :----------------------------: |
| /17     |         2          |             32766              |
| /18     |         4          |             16382              |
| /19     |         8          |              8190              |
| /20     |         16         |              4094              |
| /21     |         32         |              2044              |
| /22     |         64         |              1022              |
| /23     |        128         |              510               |
| /24     |        256         |              254               |
| /25     |        512         |              126               |
| /26     |        1024        |               62               |
| /27     |        2048        |               30               |
| /28     |        4096        |               14               |
| /29     |        8162        |               6                |
| /30     |       16384        |               2                |
| /31     |       32768        |             0 (2)              |
| /32     |       65536        |             0 (1)              |
#### ¿Cuantas direcciones asignables hay en estas IPs?

|      CIDR      | Bits de host | IPs disponibles | Bits asignados a la porción de red |
| :------------: | :----------: | :-------------: | :--------------------------------: |
| 203.0.113.0/25 |      7       |       126       |                 1                  |
| 203.0.113.0/26 |      6       |       62        |                 2                  |
| 203.0.113.0/27 |      5       |       30        |                 3                  |
| 203.0.113.0/28 |      4       |       14        |                 4                  |
| 203.0.113.0/29 |      3       |        6        |                 5                  |
| 203.0.113.0/30 |      2       |        2        |                 6                  |
| 203.0.113.0/31 |      1       |        0        |                 7                  |
| 203.0.113.0/32 |      0       |       -2        |                 8                  |
### Ejemplo de calculo de 203.0.113.0/25

**El número detrás de / indica la cantidad de bits reservados para la porción de red**

La máscara de red de esta IP es 255.255.255.128, que en binario es:

11111111.11111111.11111111.**1**0000000

Ese 1 extra en la porción de host es el bit que hemos asignado a la porción de red. En esta red, hay un total de 7 bits disponibles para los hosts. Cada número que aumenta después de / coge un bit mas de esta porción.
Si seguimos la fórmula, 2^7 -2 = 126 IPs disponibles.
En el ejemplo que seguimos en el vídeo de Jeremy´s IT Lab, se busca reducir las direcciones al mínimo para una red de solo dos Routers. En ese ejemplo se llega hasta /32 la cual solo tiene una dirección útil. Tanto /31 como /32 solo se usan para point-to-point.

---
## ¿A que subred pertenece la IP 192.168.5.57/27?

Este tipo de preguntas son frecuentes dentro del examen. Nos dan la IP de un host (192.168.5.57/27) y tenemos que averiguar la subred. El proceso es sencillo:

Primero, pasamos la IP a Binario:

11000000.10101000.00000101.00111001
	192       168            5             57

Sabemos que es una subred /27, que coge 3 bits de la porción del host. Los bits 001 del último octeto pertenecen a la red. Cambiamos el resto de bits por 0, lo devolvemos a decimal, y tenemos la subred.

11000000.10101000.00000101.00100000
	192       168            5             32

### Subnetting de una IP clase A

PC1 tiene la IP 10.217.182.223/11.
Identifica lo siguiente para la subred de PC1:
1) Network address: 10.192.0.0
2) Broadcast address: 10.223.255.255
3) First usable address: 10.192.0.1
4) Last usable address: 10.223.255.254
5) Numer of usable host addresses: 2.097.150

##### ¿Cómo calculamos esto?
Primero, pasamos a binario e identificamos a porción de red y la porción de host:

**00001010.11**011001.10110110.11011111
	10        217            182          223

La porción de red son los 11 primeros bits (/11) y los restantes son para host.

Network address. Cambiamos toda la porción de host a 0, y luego cambiamos a decimal:

00001010.11000000.00000000.00000000
	10           192           0               0

Añadimos 1 a la Network address y obtenemos la First usable address (.00000001)

Cambiamos todos los hosts bits a 1, y tenemos la broadcast:

00001010.11011111.11111111.11111111
	10          223           255           255

Quitamos 1 de la Broadcast y obtenemos la Last usable address (.11111110)

Para determinar el número de hosts, contamos el número de bits reservados para los hosts (21) y aplicamos la fórmula: 

2^21 -2 = 2.097.150 hosts por subred.


