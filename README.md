# Comandos para ejecutar apps graficas con **ssh -X**

#### Introduccion

**ssh -X nos permite ejecutar aplicaciones graficas 
de nuestros servidores remotos (que normalmente no tienen **Interfaz
 grafica**) en nuestra maquina local.

<br>

#### Comandos:
Esto fue probado en un Oracle linux como servidor 
y un Archlinux como maquina host, asi que espero que funcione en
todos los sistemas derivados de Unix

#### 1. **Configuracion en maquina local**

En nuestra maquina local: ingresamos a la ruta: `/etc/ssh`

Abrimos el archivo: `vim sshd_config` y buscamos las siguientes
lineas con estas configuraciones:

```shell
  88   │ X11Forwarding yes
  89   │ X11DisplayOffset 10
  90   │ X11UseLocalhost yes
```
Luego revisamos la variable **$DISPLAY**:
```shell
echo $DISPLAY

# si no nos arroja nada, haremos lo siguiente:
export DISPLAY=localhost:0.0

# para comprobar usaremos:
echo $DISPLAY
```

Ejecucion del servicio **xhost**:

```shell
xhost +
# nos debe de arrojar algo como esto:
access control disabled, clients can connect from any host

```

Reinicio de servicios:

```shell
sudo systemctl restart sshd --now
sudo systemctl restart {lightdm/gdm} --now 
# dependiendo de cual display manager tengas

```


<br>
<br>

#### 2. Configuracion en maquina remota (servidor)

Configuracion del archivo `/etc/ssh/sshd_config`:

```shell
AllowAgentForwarding yes
#AllowTcpForwarding yes
#GatewayPorts no
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost no  # Este es importante que este configurado en "no"
#PermitTTY yes
```

Instalacion de **xauth**: `sudo yum install xauth`

Reinicio de servicio: `sudo systemctl restart sshd --now`
