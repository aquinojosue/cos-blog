# Configuración de topologías
## Topología Casa Matriz (Central)
La topología consta de dos servidores en la vlan DMZ (FreePBX y DNS), un firewall mediante pfSense, vpcs para simular hosts y un contenedor docker que simula un host con navegador web, además de una máquina virtual con Windows que simula un cliente con funcionamiento VoIP.
![topologia-central](_assets/configuracion-topologias/topologia-central.png)

### Configuración de las máquinas virtuales (FreePBX, DNS, pfSense y cliente Windows)
Las siguientes configuraciones fueron realizadas desde VirtualBox:
- Servidor FreePBX
    - Memoria ram: 1gb
- Adaptadores de red: 1
    - Hilos del CPU: 1
- Servidor DNS (Debian)
    - Memoria ram: 512mb
    - Adaptadores de red: 1
    - Hilos del CPU: 1
- pfSense
    - Memoria ram: 512mb
    - Adaptadores de red: 4
    - Hilos del CPU: 1

Todas las configuraciones se realizaron con un disco duro virtual de 8gb cada uno.

Para poder agregar una máquina virtual de virtualbox a GNS3 primero hay que entrar a la ventana de preferencias (Edit -> Preferences ó Ctrl+Shift+P). Una vez adentro se dirigirá al apartado VirtualBox VMs.
![](_assets/configuracion-topologias/001.png)
Seleccionamos el botón “New”
![](_assets/configuracion-topologias/002.png)
Seleccionamos “Run this VirtualBox VM on my local computer” (Correr esta máquina virtual de Virtualbox en mi computadora local) y presionamos “Next”.
![](_assets/configuracion-topologias/003.png)
Seleccionamos la máquina virtual que deseamos agregar en GNS3 y presionamos Finalizar. Al salir nos aparecerá de nuevo la ventana de preferencias y presionamos “OK”.
![](_assets/configuracion-topologias/004.png)

Una vez realizado podremos observar que la máquina virtual se encuentra como un dispositivo para agregar a nuestra topología. Para poder agregarla a nuestra topología antes tenemos que editar una opción. Para esto damos clic derecho en nuestra máquina virtual y seleccionamos “Configure template”
![](_assets/configuracion-topologias/005.png)

Dentro de la ventana de configuración, seleccionaremos la pestaña “Network”. Seleccionamos la casilla que dice “Allow GNS3 to use any configured VirtualBox adapter”. En el caso que se esté configurando la máquina virtual de pfSense, también se tiene que agregar los adaptadores necesarios, desde la casilla marcada con verde (en nuestro caso, seleccionaremos 4).
![](_assets/configuracion-topologias/006.png)

Presionamos OK y la máquina virtual ya estará lista para incorporarse y ser conectada en la topología de GNS3.

### Configuración de contenedor Docker (gns3-webterm)
El contenedor gns3-webterm simula un host con capacidades de navegador, se recomienda el uso de este contenedor frente a una máquina virtual si se planea utilizar solamente para conectarse a un servidor web, ya sea localmente o en internet.
Para instalar el contenedor realizaremos los siguientes comandos (en el caso que se tenga instalado GNS3 en una partición de Linux con docker instalado y configurado correctamente).

```console
docker pull gns3/webterm
```

Una vez completada la instalación del contenedor, desde la ventana de preferencias de GNS3, seleccionaremos el apartado “Docker containers”. Adentro de este apartado seleccionaremos el botón “New”.
![](_assets/configuracion-topologias/007.png)

Una vez adentro del asistente de Docker, seleccionaremos la opción donde hayamos instalado el contenedor. En este caso, se instaló en un servidor gns3 aparte, por lo cual seleccionaré “run this docker container on a remote computer”. Una vez seleccionada la opción daremos clic en “Next”.
![](_assets/configuracion-topologias/008.png)

Seleccionamos “existing image” y de la lista, seleccionamos el contenedor a ingresar en gns3. En este caso se seleccionó el contenedor “docker.io/gns3/wenterm:latest”. Una vez seleccionado el contenedor presionamos “Next”.
![](_assets/configuracion-topologias/009.png)

Asignamos un nombre a nuestro contenedor y presionamos “Next” para continuar.
![](_assets/configuracion-topologias/010.png)

Ingresamos el número de adaptadores de red que queramos utilizar o dejamos el número por defecto (1). Presionamos “next” para continuar.

![](_assets/configuracion-topologias/011.png)

Ingresamos un comando que se quiera realizar una vez el contenedor se inicialice o dejamos en blanco y presionamos “next” para continuar.

![](_assets/configuracion-topologias/012.png)

Seleccionamos la manera de ingresar al contenedor, se selecciona VNC para poder tener un entorno gráfico debido al uso de un navegador. Seleccionamos “next” para continuar.
![](_assets/configuracion-topologias/013.png)

Ingresamos las variables de entorno a utilizar o dejamos en blanco si no se desea poner alguna. Presionamos “finish” para finalizar el asistente.

![](_assets/configuracion-topologias/014.png)

Una vez terminado el asistente, podremos ver que el contenedor docker fue agregado correctamente. Para efectos de este caso, todos los webterm se conectarán mediante DHCP, por lo tanto se tiene que editar un dato más, así que cuando se agregue un webterm a la topología, se realizará la siguiente configuración. Primero daremos clic derecho al dispositivo y seleccionaremos la opción “Edit Config”

![](_assets/configuracion-topologias/015.png)

Dentro de esta configuración, se descomentarán las últimas dos líneas, necesarias para configurar el dispositivo con DHCP. una vez terminado daremos clic en “Save”.
![](_assets/configuracion-topologias/016.png)