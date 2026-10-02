[[001-Comandos Básicos]][[008-Subnetting]]
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