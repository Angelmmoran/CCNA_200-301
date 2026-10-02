#comandosEthernetSwitching 

|                         Comando                         |                                 Descripción                                 |
| :-----------------------------------------------------: | :-------------------------------------------------------------------------: |
|                 ping "dirección IP/MAC"                 |               Envía un Ping a la dirección IP o MAC indicada                |
|                         arp -a                          |                       Muestra la tabla ARP (Windows)                        |
|                        show arp                         |                  Muestra la tabla ARP (dispositivo CISCO)                   |
|                 show mac address-table                  |                 Muestra la tabla de direcciones MAC (CISCO)                 |
|             clear mac address-table dynamic             |                     Limpia la tabla de direcciones MAC                      |
| clear mac address-table dynamic address "direccion MAC" |                  Elimina solo la "dirección MAC" indicada                   |
|  clear mac address-table dynamic interface "interfaz"   | Elimina todas las direcciones MAC asociadas a la interfaz (puerto) indicada |
