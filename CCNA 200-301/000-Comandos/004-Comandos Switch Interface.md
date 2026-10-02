#comandosSwitchInterface


| Comando                             | Descripción                                                                                                              |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| show interfaces status              | Muestra el estado de las interfaces                                                                                      |
| speed (velocidad)                   | Fuerza la velocidad máxima de la interfaz. Para ver las velocidades disponibles podemos usar ?                           |
| duplex (tipo)                       | Fuerza el tipo de duplex. Usamos ? para ver las opciones disponibles                                                     |
| description (texto)                 | Escribir una descripción de la interfaz                                                                                  |
| interface range f0/5 - 12           | Para configurar varias interfaces a la vez. En este ejemplo configuraríamos de la 05 a la 12. (Desde Global config mode) |
| interface range f0/5 - 6, f0/9 - 12 | Configura las interfaces 05, 06, 09 - 12, dejando 07 y 08 sin modificar.                                                 |
