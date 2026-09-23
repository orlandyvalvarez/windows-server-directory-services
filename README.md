# Windows Server Directory Services

Laboratorio práctico de implementación y administración de servicios de infraestructura Windows utilizando VMware Workstation.

El proyecto consiste en la creación de un entorno de dominio basado en Windows Server, con Active Directory, DNS, DHCP y políticas de grupo. El laboratorio se desarrolla en una red virtual aislada para realizar las pruebas sin afectar la red física.

## Entorno

| Componente             | Configuración       |
| ---------------------- | ------------------- |
| Plataforma             | VMware Workstation  |
| Servidor               | Windows Server 2025 |
| Cliente                | Windows 10 Pro      |
| Red                    | VMware VMnet1       |
| Dominio                | alvarez.local       |
| Controlador de dominio | DC01-LAB-WS         |
| IP del servidor        | 192.168.78.10/24    |
| Cliente                | WIN10-CLIENTE01     |

## Topología

```text
                    VMnet1
               192.168.78.0/24
                      |
          +-----------+-----------+
          |                       |
    DC01-LAB-WS             WIN10-CLIENTE01
 Windows Server 2025          Windows 10 Pro
    192.168.78.10              Cliente
          |
     alvarez.local
```

## Objetivos

* Implementar Windows Server como controlador de dominio.
* Instalar y configurar Active Directory Domain Services.
* Configurar DNS para el dominio.
* Crear unidades organizativas, usuarios y grupos.
* Integrar un equipo Windows 10 Pro al dominio.
* Implementar DHCP.
* Crear y aplicar políticas de grupo mediante GPO.
* Comprobar la comunicación y autenticación entre servidor y cliente.
* Documentar las configuraciones y pruebas realizadas.

## Active Directory

El dominio utilizado para el laboratorio es:

```text
alvarez.local
```

El controlador de dominio es:

```text
DC01-LAB-WS
```

La estructura organizativa implementada es:

```text
alvarez.local
|
+-- IT
|
+-- HR
|
+-- Ventas
```

Se configuraron grupos de seguridad para la administración de usuarios:

```text
IT-Admins
IT-Users
```

Usuario de prueba:

```text
ALVAREZ\orlandy
```

## Configuración de red

El controlador de dominio utiliza una dirección IP estática:

```text
IP:        192.168.78.10
Máscara:   255.255.255.0
Gateway:   No configurado
DNS:       127.0.0.1
```

El laboratorio utiliza VMware VMnet1 en modo Host-Only. De esta manera, las máquinas virtuales pueden comunicarse entre ellas dentro de una red independiente de la red física.

## DNS

DNS se configuró como parte de la infraestructura del dominio.

El cliente utiliza el controlador de dominio como servidor DNS:

```text
192.168.78.10
```

Se realizaron pruebas de resolución mediante:

```cmd
nslookup alvarez.local
```

y:

```cmd
nslookup dc01-lab-ws.alvarez.local
```

La resolución permitió al cliente localizar los servicios del dominio y completar correctamente la integración con Active Directory.

## Unión del cliente al dominio

El equipo:

```text
WIN10-CLIENTE01
```

fue incorporado al dominio:

```text
alvarez.local
```

La autenticación con una cuenta de dominio fue comprobada mediante:

```cmd
whoami
```

Resultado:

```text
alvarez\orlandy
```

Esto confirma que el cliente utiliza Active Directory para la autenticación del usuario.

## DHCP

Se implementó el servicio DHCP en Windows Server para proporcionar automáticamente la configuración de red a los equipos clientes.

El ámbito DHCP utiliza la red:

```text
192.168.78.0/24
```

La configuración incluye:

* Rango de direcciones IP.
* Máscara de subred.
* Exclusiones necesarias.
* Servidor DNS.
* Dominio `alvarez.local`.

El cliente Windows 10 fue configurado para obtener automáticamente su dirección IP y posteriormente se verificó la asignación mediante:

```cmd
ipconfig /all
```

La renovación de la configuración DHCP fue comprobada mediante:

```cmd
ipconfig /release
ipconfig /renew
```

## Group Policy

Se implementaron políticas de grupo mediante Group Policy Management.

Las políticas fueron vinculadas a las unidades organizativas correspondientes y probadas desde el equipo cliente.

La aplicación de las políticas fue comprobada mediante:

```cmd
gpupdate /force
```

y:

```cmd
gpresult /r
```

Estas pruebas permitieron verificar que las configuraciones definidas en el controlador de dominio fueran recibidas correctamente por el equipo cliente.

## Pruebas realizadas

### Conectividad

```cmd
ping 192.168.78.10
```

Se comprobó la comunicación entre el cliente y el controlador de dominio.

### Resolución DNS

```cmd
nslookup alvarez.local
```

Se verificó la resolución del dominio.

### Autenticación

```cmd
whoami
```

Resultado:

```text
alvarez\orlandy
```

### Nombre del equipo

```cmd
hostname
```

Resultado:

```text
WIN10-CLIENTE01
```

### Configuración DHCP

```cmd
ipconfig /all
```

Se verificó la configuración obtenida mediante DHCP.

### Actualización de políticas

```cmd
gpupdate /force
```

### Verificación de GPO

```cmd
gpresult /r
```

Se verificó la aplicación de las políticas configuradas desde Active Directory.

## Resultados

El laboratorio permitió implementar un entorno funcional de administración centralizada basado en Windows Server.

Se configuró un controlador de dominio con Active Directory y DNS, se incorporó un equipo Windows 10 Pro al dominio, se implementó DHCP y se aplicaron políticas mediante Group Policy.

Las pruebas realizadas confirmaron:

* Comunicación entre servidor y cliente.
* Resolución DNS.
* Autenticación mediante Active Directory.
* Integración del equipo cliente al dominio.
* Asignación automática de configuración de red mediante DHCP.
* Aplicación de políticas de grupo.

## Evidencias

Las principales evidencias del laboratorio incluyen:

* Configuración IP del servidor.
* Configuración de VMnet1.
* Active Directory Users and Computers.
* Unidades organizativas.
* Usuarios y grupos.
* Configuración DNS.
* Configuración DHCP.
* Unión del cliente al dominio.
* Inicio de sesión con usuario de dominio.
* Configuración obtenida mediante DHCP.
* Configuración de Group Policy.
* Resultado de `gpupdate`.
* Resultado de `gpresult`.
* Pruebas de conectividad y resolución DNS.

