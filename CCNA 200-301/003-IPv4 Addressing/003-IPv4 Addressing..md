[[001-Comandos Básicos]]
[[003-Comandos IPv4]]

Los Switches no separan diferentes redes, conectan y expanden redes dentro de la misma LAN. 
El dispositivo que separa redes es el Router, cada una con su dirección IP.

192.168.1.0/24 (255.255.255.0) 192.168.2.0/24 (255.255.255.0)

![Pasted image 20260831181613.png](<../999-Imágenes/Pasted image 20260831181613.png>)

Los routers tienen diferentes direcciones IPs para cada una de las interfaces que tienen conectadas. 

La dirección IP de la interfaz G0/0 es 192.168.1.254/24

La dirección IP de la interfaz G0/1 es 192.168.2.254/24

La porción Network de una IP es la misma para todos los dispositivos conectados a una LAN:
192.168.1.100 / 192.168.1.105 / 192.168.1.205
Estas IPs están dentro de la misma LAN, la porción Network de la IP es la misma 192.168.1; y la porción Host es distinta 100 / 105 / 205

Cuando un paquete Boardcast llega a un router, NO SIGUE SU CAMINO, se queda siempre dentro de la red LAN. La comunicación con dispositivos en otras LAN es distinta. 

---
Un Header de UPv4 contiene una un campo para la Source y Destination IP.
Este campo tiene 32 bits.

192.168.1.254 (Cada decimal contiene 8 bits) - 11000000 . 10101000 . 00000001 . 11111110

Cada decimal se llama Octeto.
Como el binario es difícil de leer, escribimos en el formato Dotted Decimal.

### ¿Que significa el /24 de las direcciones IPs de la imagen?

Una IP tiene una longitud de 32 bits, /24 significa que los primeros 24 (3 primeros octetos) bits de la IP representa la Network Portion de la IP, y los restantes 8 bits (4º octeto) representan los Endhosts (Host Portion):

192.168.1 -> es la Network Portion.
.254 -> es la Host Portion.

![Pasted image 20260901074752.png](<../999-Imágenes/Pasted image 20260901074752.png>)

La IP del PC1 del SW1 es 192.168.1.1
La IP del PC2 del SW1 es 192.168.1.2
La IP de la interfaz G0/0 del R1 (donde está conectado SW1) es 192.168.1.254

La IP del PC3 del SW2 es 192.168.2.1
La IP del PC3 del SW2 es 192.168.2.2
La IP de la interfaz G0/1 del R1 (donde está conectado el SW2) es 192.168.2.254

*SOLO* la porción de red de la IP es distinta entre las dos redes LAN.

## Clases de IPv4

| Clase | Primer Octeto |  Rango numérico del primero octeto  | Prefix (Network Portion) Length |
| :---: | :-----------: | :---------------------------------: | :-----------------------------: |
|   A   |   0xxxxxxx    |               0 -127                |               /8                |
|   B   |   10xxxxxx    |              128 - 191              |               /16               |
|   C   |   110xxxxx    |              192 - 223              |               /24               |
|   D   |   1110xxxx    |   224 - 239 (multicas addresses)    |               --                |
|   E   |   1111xxxx    | 240 - 255 (reserved / experimental) |               --                |
Con esta tabla, podemos identificar el tipo de IPv4 fijándonos en el primero octeto de la IP.
Por ejemplo, en la IP 172.16.254.1 si aislamos el primer octeto y compararlo con la tabla, sabemos que esta IP es Clase B porque 172 se encuentra en el rango 128 - 191.
Nos centraremos principalmente en las clases A, B y C.

Realmente, las IPs de tipo A solo se usan hasta 126, porque 127 está reservado para Loopback addresses. Si hacemos ping a cualquier IP dentro de ese rango, nuestro ordenador simplemente enviará y recibirá los paquetes del ping a si mismo.

![Pasted image 20260901081124.png](<../999-Imágenes/Pasted image 20260901081124.png>)

Todos los tiempos de ida a vuelta son 0ms, porque el tráfico no está yendo a ningún sitio, solo va y vuelve al mismo ordenador.

## NetMask

Las mascaras de red se escriben con toda la porción de red del tipo de IP con valor 1 y la porción de host con 0 (binario). Actualmente es común verlo representado solo con / y el número de bits disponibles en la porción de red. 

| Clase | Nomenclatura moderna | Nomenclatura clásica |            Valor en binario            |
| :---: | :------------------: | :------------------: | :------------------------------------: |
|   A   |         / 8          |      255.0.0.0       |  11111111 00000000 00000000 00000000   |
|   B   |         / 16         |     255.255.0.0      | 111111111 1111111111 00000000 00000000 |
|   C   |         / 24         |    255.255.255.0     |  11111111 11111111 11111111 00000000   |
Si la porción de Host es todo 0, es la dirección de red (el identificador de la red). Esta IP no puede asignarse a ningún Host, por lo que la primera IP útil es una por encima. La última IP del rango tampoco puede asignarse ya que es la IP de boardcast de la red. La última IP del rango se representa con .255 en toda la porción de host (o todo 1 en binario). Para calcular la última IP asignable del rango, cambiamos el último 1 de la porción de host por 0, o restamos 1 al último .255.

Para calcular el número de IPs disponibles para hosts en un rango de IPs, podemos usar esta fórmula:  2^n -2; siendo n el número de bits disponibles en la porción de host.


---
## Configuración dispositivos CISCO

##### Comando show ip interface brief
La columna Status se refiere al estado de la capa 1
La columna Protocol se refiere al estado de la capa 2

####  Comando ip address
Cuando escribamos la IP, también tenemos que escribir seguido la subnet mask.

---
# Decimal y Exadecimal

![Pasted image 20260901070936.png](<../999-Imágenes/Pasted image 20260901070936.png>)

El número 3249 en decimal (base 10) lo podemos desglosar como:
3294 =
(3 * 1000) + (3 en la posición de los miles)
(2 * 100) + (2 en la posición de los cienes)
(9 * 10) + (9 en la posición de los dieces)
(4 * 1) (4 en la posición de los unos)

Este número en Exadecimal (base 16) es CDE.
En esta base, cada posición vale 16 veces mas que la anterior.
El valor CDE se desglosa multiplicando cada letra por el peso de su posición.

E = 14. En la primera posición (x1) = 14

D = 13. En la segunda posición (x16) = 208

C = 12. En la tercera posición (x256) = 3072

14 + 208 + 3072 = 3294

---
# Como pasar decimal a binario 

La IP 192.168.1.254 en Binario es:

11000000 10101000 00000001 11111110 (cada bloque representa 8 bits)

### Vamos a desglosar este cambio:

#### 192 = 11000000

Binario es base 2, cada posición vale el doble que la anterior.

1 - 1 en la posición de los 128
1 - 1 en la posición de los 64
0 - 0 en la posición de los 32
0 - 0 en la posición de los 16
0 - 0 en la posición de los 8 
0 - 0 en la posición de los 4 
0 - 0 en la posición de los 2
0 - 0 en la posición de los 1

128 + 64 = 192

#### 168 = 10101000

1 - 1 en la posición de los 128
0 - 0 en la posición de los 64
1 - 1 en la posición de los 32
0 - 0 en la posición de los 16
1 - 1 en la posición de los 8 
0 - 0 en la posición de los 4 
0 - 0 en la posición de los 2
0 - 0 en la posición de los 1

128 + 32 + 8 = 168

#### 1 = 00000001

0 - 0 en la posición de los 128
0 - 0 en la posición de los 64
0 - 0 en la posición de los 32
0 - 0 en la posición de los 16
0 - 0 en la posición de los 8 
0 - 0 en la posición de los 4 
0 - 0 en la posición de los 2
1 - 0 en la posición de los 1

Este es muy simple, solo tenemos un valor, 1

#### 254 = 11111110

1 - 1 en la posición de los 128
1 - 1 en la posición de los 64
1 - 1 en la posición de los 32
1 - 1 en la posición de los 16
1 - 1 en la posición de los 8 
1 - 1 en la posición de los 4 
1 - 1 en la posición de los 2
0 - 0 en la posición de los 1

128 + 64 + 32 + 16 + 8 + 4 + 2 = 254

##### ¿Como sabemos que 192.168.1.254 = 11000000.10101000.0000000.11111110?

Así es como se pasa de Decimal a Binario.
Ejemplo, el número 221:

Primero escribimos los valores de los octetos:

128    64    32    16    8    4    2    1

Empezando por la izquierda, empezamos a restar el valor del octeto al número que queremos convertir. Si podemos restarlo sin quedar en negativo, ese octeto tendrá un valor de 1, si no podemos restarlo porque quedamos en negativo, tendrá un valor de 0. En el momento en el que ya no podamos quitar mas (porque hemos llegado a 0) todos los octetos que queden por la derecha tendrán un valor de 0.

221 - 128 = 93 (El octeto de 128 valdrá 1)

93 - 64 = 28 (El octeto de 64 valdrá 1)

28 - 32 = -4 (El octeto de 32 valdrá 0)

28 - 16 = 12 (El octeto de 16 valdrá 1)

12 - 8 = 4 (El octeto de 8 valdrá 1)

4 - 4 = 0 (El octeto de 4 valdrá 1)

Y como ya hemos llegado a 0, los octetos de 2 y 1 también valdrán 0.

Por lo que, nuestro número decimal 221 en binario es 11011100

# Como pasar binario a decimal 

Lo mas sencillo es escribir los valores de cada posición encima y sumar los que tengan un 1. Empezamos por la derecha, con un valor de 1 y lo vamos duplicando:

128  64  32   16    8    4    2    1
  1     0    0     0     1    1    1    1

Los valores que tienen un 1 son 128 + 8 + 4 + 2 + 1 = 143

Por lo que 143 = 10001111