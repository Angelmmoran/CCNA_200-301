DTP y VTP son protocolos específicos de Cisco que fueron diseñados y desarrollados por Cisco, y que solo funciona en sus dispositivos. Actualmente estos dos conceptos se encuentran fuera del temario oficial de CCNA 200-301, pero aún así hay algunas preguntas en algunos exámenes sobre estos temas. Esta página será menos detallada, solo cubre los detalles básicos para saber como funcionan estos protocolos para poder responder las preguntas básicas que quedan en algunos exámenes.

## 1. DTP - Dynamic Trunking Protocol
Permite a los switchs de Cisco determinar dinámicamente el estado de su interfaz, entre *access* o *trunk* sin necesidad de configuración manual. DTP está activado por defecto en todos los switchs de Cisco.
Por seguridad, este protocolo debe ser desactivado y debemos configurar las interfaces manualmente. DTP puede ser explotado por atacantes. 
La opción *dynnamic* es la que configura DTP
![](../999-Imágenes/Pasted%20image%2020261012184457.png)

**auto**:  **NO** intentará formar un trunk de forma activa. Sin embargo, si otro switch le solicita crear un trunk, lo hará. Creará un trunk con un switchport en alguno de estos modos:
	- *switchport mode trunk*
	- *switchport mode dynamic desirable*

**desirable**: Intentará formar un trunk con otro switch de Cisco. Lo hará si está conectado a otro switchport con alguno de los siguientes modos:
	-*switchport mode trunk*
	- *switchport mode dynamic desirable*
	- *switchport mode dynamic auto*

### Tabla resumen de las conexiones según configuración

| Administrative mode | Trunk | Dynamic Desirable | Access | Dynamic Auto |
| :------------------ | :---: | :---------------: | :----: | :----------: |
| Trunk               | Trunk |       Trunk       |   X    |    Trunk     |
| Dynamic Desirable   | Trunk |       Trunk       | Access |    Trunk     |
| Access              |   X   |      Access       | Access |    Access    |
| Dynamic Auto        | Trunk |       Trunk       | Access |    Access    |

Por ejemplo, un switch en modo *dynamic desirable* formará conexiones trunk con cualquier otro switch excepto con los que estén en modo *access*.

## VTP - VLAN Trunking Protocol
VTP permite configurar VLANs en un VTP server switch centra, y otros switches (VTP clients) sinctronizarán sus bases de datos de VLAN con este servidor.
Está diseñado para redes muy grandes, con muchas VLANs, para no tener que configurar cada VLAN en cada switch.
Sin embargo, a penas se usa, y no se recomienda su uso. Uno de los motivos es porque, si conectas un switch antiguo con un *Revision number* mayor al de tu red (y el dominio de VTP coincide) todos los switches del dominio se actualizarán a esa versión.

VTP usa el campo *Configuration Revision* con un valor numérico para controlar la última versión de la base de datos. Si se hace un cambio, este número aumenta en 1, y avisa al resto de la red para que se actualicen.

Hay tres versiones, 1, 2 y 3; y tres modos: server, client y transparent.
	- server: puede añadir, modificar y borrar VLANs. Guardan la base de datos en NVRAM.
	- client: no puede añadir, modificar o borrar VLANs. No guardan la base de datos en NVRAM.
	- transparent: no participan en VTP domain (no se actualizan). Pueden cambiar, añadir y borrar VLANs pero no envían la actualización.

