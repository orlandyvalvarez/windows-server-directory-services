# Windows Server Directory Services

Laboratorio práctico de implementación y administración de servicios de infraestructura Windows utilizando VMware Workstation.

El proyecto consiste en la construcción de un entorno de laboratorio aislado basado en Windows Server 2025, Active Directory Domain Services, DNS, DHCP y Group Policy. El objetivo fue implementar un dominio funcional, integrar un equipo cliente Windows 10 Pro y comprobar mediante pruebas reales la comunicación, resolución DNS, asignación automática de red, autenticación y aplicación de políticas centralizadas.

Todo el laboratorio fue desarrollado sobre una red virtual independiente mediante VMware VMnet1, evitando cualquier interacción con la red física utilizada por el equipo.

## Entorno de laboratorio

| Componente                   | Configuración           |
| ---------------------------- | ----------------------- |
| Plataforma de virtualización | VMware Workstation      |
| Servidor                     | Windows Server 2025     |
| Cliente                      | Windows 10 Pro          |
| Red virtual                  | VMware VMnet1 Host-Only |
| Red                          | 192.168.78.0/24         |
| Dominio                      | alvarez.local           |
| NetBIOS                      | ALVAREZ                 |
| Controlador de dominio       | DC01-LAB-WS             |
| IP del servidor              | 192.168.78.10/24        |
| Cliente                      | WIN10-CLIENTE01         |
| IP obtenida por DHCP         | 192.168.78.101          |

## Arquitectura de red

El laboratorio fue construido sobre VMware VMnet1 configurado como red Host-Only. La red virtual permite la comunicación entre las máquinas virtuales y el equipo host, pero permanece separada de la red física.

```text
                         VMware VMnet1
                        192.168.78.0/24
                               |
                +--------------+--------------+
                |                             |
                |                             |
        DC01-LAB-WS                    WIN10-CLIENTE01
      Windows Server 2025               Windows 10 Pro
        192.168.78.10                  192.168.78.101
                |                             |
                |                             |
        Active Directory                Usuario de dominio
        DNS / DHCP / GPO                ALVAREZ\orlandy
                |
          alvarez.local
```

No se configuró una puerta de enlace en esta red debido a que el laboratorio no requiere acceso hacia Internet ni hacia otras redes.

## Objetivos del laboratorio

El laboratorio fue desarrollado para implementar y comprobar un entorno básico de administración de infraestructura Windows.

Se implementó Windows Server 2025 como controlador de dominio, se instaló Active Directory Domain Services, se configuró DNS, se crearon unidades organizativas, usuarios y grupos de seguridad, se integró un cliente Windows 10 Pro al dominio, se implementó DHCP y se creó una política de grupo para controlar el acceso al Panel de control.

Finalmente, se realizaron pruebas para comprobar el funcionamiento de los diferentes servicios.

## Configuración del servidor

El servidor fue configurado con el nombre:

```text
DC01-LAB-WS
```

El sufijo DNS principal utilizado fue:

```text
alvarez.local
```

La configuración IPv4 del servidor quedó establecida de forma estática:

```text
IP:        192.168.78.10
Máscara:   255.255.255.0
Gateway:   No configurado
DNS:       127.0.0.1
```

La dirección IP estática permite que el servidor mantenga una dirección conocida dentro de la infraestructura del dominio.

## Active Directory Domain Services

Se instaló y configuró Active Directory Domain Services en Windows Server 2025.

El dominio implementado es:

```text
alvarez.local
```

El controlador de dominio corresponde a:

```text
DC01-LAB-WS.alvarez.local
```

También se configuró el nombre NetBIOS del dominio:

```text
ALVAREZ
```

La estructura organizativa creada en Active Directory es:

```text
alvarez.local
|
+-- IT
|
+-- HR
|
+-- Ventas
```

Dentro de la administración de usuarios se crearon los grupos de seguridad:

```text
IT-Admins
IT-Users
```

Se creó el usuario de prueba:

```text
Orlandy Vilorio
```

con la cuenta de dominio:

```text
ALVAREZ\orlandy
```

El usuario fue asociado al grupo:

```text
IT-Users
```

También se realizó el primer inicio de sesión utilizando las credenciales del dominio y se comprobó posteriormente la identidad mediante `whoami`.

## DNS

DNS fue implementado como parte de la infraestructura del controlador de dominio.

El cliente Windows 10 utiliza al controlador de dominio como servidor DNS:

```text
192.168.78.10
```

Se realizaron pruebas de resolución utilizando:

```cmd
nslookup alvarez.local
```

y:

```cmd
nslookup dc01-lab-ws.alvarez.local
```

Las consultas permitieron comprobar que el cliente podía resolver correctamente el dominio y el nombre del controlador de dominio.

La correcta resolución DNS fue necesaria para completar la integración del cliente con Active Directory.

## Configuración del cliente Windows 10

Se implementó una máquina virtual independiente con Windows 10 Pro debido a que la edición Home no permite la integración con un dominio de Active Directory.

El equipo fue configurado con el nombre:

```text
WIN10-CLIENTE01
```

La máquina virtual fue conectada exclusivamente a:

```text
VMnet1
```

El cliente fue configurado inicialmente para utilizar el servidor:

```text
192.168.78.10
```

como servidor DNS.

Después de comprobar la conectividad y resolución DNS, el equipo fue incorporado correctamente al dominio:

```text
alvarez.local
```

El inicio de sesión con la cuenta de dominio fue comprobado mediante:

```cmd
whoami
```

Resultado:

```text
alvarez\orlandy
```

Esto confirmó que el equipo cliente estaba autenticando correctamente contra Active Directory.

## DHCP

Se instaló y configuró el rol DHCP en Windows Server.

El ámbito creado fue:

```text
DHCP-LAB
```

Utilizando la red:

```text
192.168.78.0/24
```

El rango configurado fue:

```text
192.168.78.100 - 192.168.78.200
```

La máscara utilizada fue:

```text
255.255.255.0
```

Las opciones DHCP configuradas incluyeron el servidor DNS:

```text
192.168.78.10
```

y el dominio:

```text
alvarez.local
```

No se configuró una puerta de enlace porque el laboratorio está diseñado como una red aislada y no requiere salida hacia otras redes.

El ámbito DHCP fue activado y posteriormente el cliente Windows 10 fue configurado para obtener automáticamente su configuración de red.

La asignación fue comprobada mediante:

```cmd
ipconfig /all
```

El cliente recibió:

```text
IPv4:          192.168.78.101
Máscara:       255.255.255.0
DHCP Server:   192.168.78.10
DNS Server:    192.168.78.10
Dominio:       alvarez.local
Gateway:       No configurado
```

También se comprobó la renovación de la configuración mediante:

```cmd
ipconfig /release
ipconfig /renew
```

Esto permitió verificar que Windows Server estaba funcionando como servidor DHCP para el cliente.

## Group Policy

Se implementó una política de grupo mediante Group Policy Management.

La política creada fue:

```text
GPO-IT-Restriccion-Panel
```

La GPO fue vinculada a la unidad organizativa:

```text
IT
```

La configuración aplicada se encuentra en:

```text
Configuración de usuario
→ Directivas
→ Plantillas administrativas
→ Panel de control
```

La política configurada fue:

```text
Prohibir el acceso al Panel de control y a Configuración del PC
```

La política fue establecida como:

```text
Habilitada
```

Después de realizar la configuración, se actualizó la directiva en el cliente mediante:

```cmd
gpupdate /force
```

Posteriormente se verificó la aplicación de la política utilizando:

```cmd
gpresult /r
```

El resultado confirmó que la GPO:

```text
GPO-IT-Restriccion-Panel
```

fue aplicada al usuario:

```text
Orlandy Vilorio
```

La salida de `gpresult` mostró además que la última aplicación de directivas se realizó desde:

```text
DC01-LAB-WS.alvarez.local
```

## Pruebas realizadas

### Prueba de conectividad

Se comprobó la comunicación entre el cliente y el controlador de dominio mediante:

```cmd
ping 192.168.78.10
```

La prueba confirmó la comunicación dentro de la red virtual.

### Prueba de resolución DNS

Se realizaron consultas mediante:

```cmd
nslookup alvarez.local
```

y:

```cmd
nslookup dc01-lab-ws.alvarez.local
```

Las pruebas confirmaron la resolución de nombres mediante el servidor DNS del dominio.

### Prueba de autenticación

Se verificó la cuenta utilizada en el cliente mediante:

```cmd
whoami
```

Resultado:

```text
alvarez\orlandy
```

### Prueba del nombre del equipo

Se verificó el nombre del cliente mediante:

```cmd
hostname
```

Resultado:

```text
WIN10-CLIENTE01
```

### Prueba de DHCP

Se verificó la configuración recibida automáticamente mediante:

```cmd
ipconfig /all
```

El cliente obtuvo correctamente una dirección dentro del ámbito DHCP configurado en Windows Server.

### Prueba de actualización de políticas

Se ejecutó:

```cmd
gpupdate /force
```

para forzar la actualización de las políticas de grupo.

### Verificación de aplicación de GPO

Se ejecutó:

```cmd
gpresult /r
```

El resultado confirmó la aplicación de:

```text
GPO-IT-Restriccion-Panel
```

La política fue aplicada desde:

```text
DC01-LAB-WS.alvarez.local
```

## Resultado final

El laboratorio quedó funcionando como un entorno de infraestructura Windows basado en dominio.

Windows Server 2025 opera como controlador de dominio y proporciona los servicios de Active Directory, DNS y DHCP. El equipo Windows 10 Pro se encuentra integrado al dominio `alvarez.local`, obtiene su configuración de red mediante DHCP, utiliza el controlador de dominio como servidor DNS y permite la autenticación mediante cuentas de Active Directory.

También se implementó una política de grupo vinculada a la OU `IT` y se comprobó mediante `gpresult` que la política fue recibida correctamente por el usuario de prueba.

El entorno permanece aislado mediante VMware VMnet1, por lo que las pruebas se realizaron sin modificar ni depender de la red física.

## Evidencias

La documentación del proyecto incluye capturas correspondientes a las principales etapas de implementación y validación.

```text
evidencias/
|
+-- 01-vmware-vmnet1.png
+-- 02-server-ip.png
+-- 03-active-directory.png
+-- 04-dns.png
+-- 05-dhcp.png
+-- 06-cliente-dominio.png
+-- 07-dhcp-client.png
+-- 08-gpo-creada.png
+-- 09-gpupdate.png
+-- 10-gpo-aplicada.png
```

Las evidencias permiten comprobar visualmente la configuración de VMware, la infraestructura del servidor, Active Directory, DNS, DHCP, la integración del cliente al dominio y la aplicación de Group Policy.

## Tecnologías utilizadas

```text
Windows Server 2025
Windows 10 Pro
Active Directory Domain Services
DNS
DHCP
Group Policy
VMware Workstation
VMware VMnet1
IPv4
PowerShell / CMD
```

##
