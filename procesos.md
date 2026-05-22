# Procesos

Un proceso es un trabajo realizado por el sistema operativo ordenado por el propio sistema operativo o por el usuario que va a necesitar los recursos de este, ya sea CPU, memoria RAM…

Los procesos pueden ser de diferente tipo, pueden ser *daemon* ejecutados por debajo del sistema operativo y no pensados para interactuar con el usuario, pueden ser padres e hijos si dependen entre ellos, pueden ser *zombie* si han perdido al padre que los controlaba…

Linux los etiqueta a todos con un `PID` una identificación numérica para poder referirnos a ellos, el primer proceso, el cual arranca el sistema operativo llamado `init` es el que empieza la lista con `001`

`ps`

Para poder ver el listado de procesos del terminal en el que estamos en ese momento, usaremos `ps` sin ninguna otra opción.

Si lo que queremos es ver los procesos que están ocurriendo en todo el sistema operativo, no solo en nuestro terminal, entonces usamos `ps x`

Todo proceso tiene un estado, normalmente `STAT` en los listados de `ps` definido con letras abreviando el significado de cada uno, son los siguientes.

| `R` | Corriendo o listo para correr |
| --- | --- |
| `S` | Dormido, normalmente esperando a un evento |
| `D` | Dormido de manera ininterrumpida, esperando un evento, la diferencia con respecto a `S` es que este tipo de procesos no puede matarse con `kill` |
| `T` | Proceso detenido para relanzarse más adelante |
| `Z` | Proceso *zombie* es decir, proceso que se ha quedado sin padre |
| `<` | Proceso de alta prioridad, generalmente toma más tiempo de CPU |
| `N` | Proceso de baja prioridad y por tanto poco consumo de CPU |

Otra opción popular de `ps aux` el cual nos da aún más información, incluida la relacionada a otros usuarios del sistema.

Si lo que queremos ver es información detallada referente a un único proceso, tenemos que usar la opción `uw` seguido del `PID` que queremos.

```bash
ps uw  1
USER  PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root    1  0.1  0.0 167236 12796 ?        Ss   15:51   0:10 /sbin/init splash

```

`top`

Un problema que presenta `ps` es que muestra la información de los procesos en el momento en que este es lanzado, con `top` podemos tener la información ordenada por consumo y actualizada cada 3 segundos.

`job`

Un `job` es un proceso lanzado por el usuario en el terminal, y puede estar en *foregorund* o en *background* el primero es cuando el control del terminal lo tiene el `job` el segundo es cuando el control del terminal regresa al usuario al lanzar este.

Para ver el listado de `jobs` usamos el comando con el mismo nombre.

Podemos tener un comando en *foreground* con `xlogo` para terminar este hacemos `Ctrl + C`

```bash
# Esperara hasta cerrar el job
xlogo
```

O pasarlo al *background* añadiendo `&` a continuación del comando.

```bash
xlogo &

# Podemos hacerlo con muchos a la vez
xlogo & gedit &
```

Un proceso que se queda en *background* es inmune a las acciones del usuario para terminarlo como `Ctrl + C` si queremos aplicar esta acción, antes tenemos que llevarlo de vuelta al *foreground* con `fg` pasándole el `id` del `job` asignado con `%`

```bash
jobs
[2] xlogo

# Lo enviamos al background
xlogo &

# Lo traemos de vuelta al foreground
fg %2
```

`Ctrl + Z`

Con `Ctrl + Z` paramos un comando, y lo enviamos al *background* es decir, hace dos operaciones diferentes, si una vez que esté en el *background* lo queremos re-lanzar desde ahí, usaremos `bg` de la misma manera que usábamos `fg` pasándole el identificador del `job`

```bash
jobs
[2] xlogo

# Paramos el proceso y lo enviamos al background
Ctrl + Z

# Re-lanzamos el proceso desde el background
bg %2
```

**Prioridad de procesos**

En los procesos de Linux existe un concepto llamado *nice* que define lo “nice” que es un proceso en base a su consumo de CPU, si es muy “nice” es que consume poco, y si es poco “nice” es que consume mucho.

Para modificar el *nice* usaremos los comandos `nice` para lanzar un programa y `renice` para modificarlo con su `PID` teniendo en cuenta que solo el administrador puede modificar el *nice* de los procesos dejando al usuario solo la posibilidad de añadir más *nice* (quitar consumo de CPU) a los procesos que posee.

Los valores deben estar comprendidos entre `-20` y `19` siendo `0` el estado normal, sin modificar.

```bash
# Lanzamos un programa con -10 de prioridad de CPU
nice -n 10 my_program

# Modificamos el programa con PID 1234 para que use más CPU (sudo)
sudo renice -n -10 1234
```

**Señales**

Para poder enviar señales a los procesos usamos el comando `kill` seguido de la señal que queremos enviar y el `PID` del proceso en cuestión. Algunas de estas señales se envían automáticamente cuando ocurren ciertos eventos como por ejemplo, cerrar el terminal.

Todos los procesos sobre los que no somos propietarios, requerirán `sudo`

| Número | Nombre | Explicación |
| --- | --- | --- |
| 1 | HUP | “Hang up” se envía para terminar un proceso, ocurre cunado cerramos el terminal. |
| 2 | INT | “Interrupt” se envía para terminar un proceso, ocurre cuando hacemos `Ctrl + C` |
| 9 | KILL | “Kill” se envía para terminar un programa sin dejar que antes guarde su estado, es decir, cerrará el programa sea como sea. |
| 15 | TERM | “Terminate” se envía para terminar un programa, el estado por defecto de `kill` |
| 18 | CONT | “Continue” se envía para reanudar un proceso que estaba parado, se envía automáticamente con `fg` y `bg` |

La señal que se envía con `kill` puede ser el número, el nombre o el nombre seguido del prefijo `SIG`

```bash
kill -INT 1234

kill -9 1234

kill -SIGTERM 1234
```

`nohup`

Como se ha comentado anteriormente, un proceso puede terminarse cuando el usuario cierra el terminal en el que estaba corriendo, ya que recibe la señal `HUP` esto se puede evitar si lanzamos el comando precedido con `nohup`

```bash
# Aunque cerremos el terminal el proceso continuara
nohup xlogo
```

**Lanzar una señal a múltiples procesos**

Para lanzar una señal a múltiples procesos usamos el comando `killall` esto va perfecto cuando queremos terminar con todos los procesos relacionados.

```bash
killall firefox
```

**Reiniciar o apagar el equipo**

Para reiniciar el equipo podemos usar `reboot` o `shutdown` y para apagar el equipo debemos usar `shutdown` tanto uno como otro debe usarse con `sudo`

```bash
# Reiniciar el equipo
sudo reboot
sudo shutdown -r now

# Apagar el equipo
sudo shutdown -h now

# Apagar el equipo dentro de 20 minutos
sudo shutdown -h +20
```