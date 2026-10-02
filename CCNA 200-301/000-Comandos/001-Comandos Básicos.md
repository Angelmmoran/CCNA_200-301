#comandosBasicos

|  Comandos básicos CLI CISCO  |                               Descripción                               |
| :--------------------------: | :---------------------------------------------------------------------: |
|            enable            |                  Para entrar en "Privileged EXEC Mode"                  |
|             exit             |                       Para salir del modo actual                        |
|              ?               |                 Muestra todos los comandos disponibles                  |
|            conf t            |                      Entrar en modo configuración                       |
|      hostname (nombre)       |                Desde config t, cambia el nombre del host                |
|  enable password "password"  | Configura la contraseña "password" para proteger "Privileged EXEC mode" |
| show running-config (sh run) |             Muestra la configuración actual del dispositivo             |
|     show startup-config      |           Muestra la configuración de inicio del dispositivo            |
|            write             |          Guarda la configuración actual como "startup-config"           |
| service password-encryption  |   Encripta la contraseña configurada con "enable password "password""   |
|   enable secret "password"   |          Crea una contraseña mas segura que "enable password"           |
|         no "comando"         |                      Cancela el comando "comando"                       |
