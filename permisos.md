# Permisos

Los permisos en Linux se dividen en tres, usuarios, gurpos y el resto. El usuario es aquel que tiene como propiedad un archivo o directorio, el grupo es el conjunto de usuarios y el resto es el conjunto de usuarios que acceden al sistema sin pertenecer a un grupo ni ser propietarios del archivo o directorio.

Además de estos existen también otros usuarios creados por el sistema para el mantenimiento del mismo así como para tareas de administración.

Los usuarios vienen definidos en `/etc/passwd`

Los grupos vienen definidos en `/etc/group`

**Simbología**

Para representar estos permisos Linux define los siguientes símbolos:

- `-` Un archivo normal
- `d` Un directorio
- `l` Un enlace simbólico
- `c` Un archivo especial
- `b` Un bloque de archivos especiales

| **Usuario** | **Grupo** | **El resto** |
| ----------- | --------- | ------------ |
| `rwx`       | `rwx`     | `rwx`        |

| **Comando** | **Archivo**                                                                                                                    | **Directorio**                                                                                                  |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------- |
| `r`         | Puede ser abierto o leído                                                                                                      | Se puede listar el contenido pero necesita del permiso adicional de `x` para ver la información de cada archivo |
| `w`         | Puede ser escrito o modificado pero no puede ser borrado o renombrado salvo que `w` se defina en el directorio que lo contiene | Se pueden borrar, modificar los archivos del directorio siempre que `x` esté definido en este                   |
| `x`         | Permite la ejecución del archivo como programa, si es un script, tendrá que tener permisos de `r` también                      | Permite el acceso al directorio y poder realizar cualquier operación sobre este                                 |

`chmod`

Para definir los permisos de un archivo o directorio, usamos el comando `chmod` al cual le podemos pasar notación en octal o en formato simbólico.

**Octal**

Usa la numeración en base 8 para definir los permisos, los diferentes valores posibles se definen en la siguiente tabla.

| 0   | 000 | `---` |
| --- | --- | ----- |
| 1   | 001 | `--x` |
| 2   | 010 | `-w-` |
| 3   | 011 | `-wx` |
| 4   | 100 | `r--` |
| 5   | 101 | `r-x` |
| 6   | 110 | `rw-` |
| 7   | 111 | `rwx` |

```bash
// rw------x
chmod 601

// rwx-w---x
chmod 721
```

**Simbólico**

Otra forma más clara de definir los permisos es usando la notación simbólica, en esta tenemos 4 formas de definir la entidad sobre la que vamos a definir los permisos.

- `u` Usuario
- `g` Grupo de usuarios
- `o` El resto
- `a` Todos, es decir `ugo`

Tan solo tenemos que unir esto a los permisos que queremos añadir `+` quitar `-` o definir exatcamente esos `=`

```bash
chmod u+x

chmod u-x,o+r
```

`umask`

Con el comando `umask` podemos definir los permisos que tendrán por defecto los archivos y directorios que creemos a partir del momento en que lancemos el comando. Pasamos como argumento el número que queremos restar a `777` en directorios y a `666` en archivos.

```bash
umask 002

// Directorios: 777 - 002 = 775
// Archivos: 666 - 002 = 664
```

En el ejemplo anterior se nos quedarán unos permisos de `775` en directorios y `664` en archivos.

**Permisos especiales**

Los permisos especiales son aquellos permisos extra a los ya conocidos `rwx` estos pueden ser tres, en función de a quien afecten, usuario (4), grupo (2), otros (1).

```bash
setuid -> 4000
setgid -> 2000
sticky -> 1000
```

`setuid`

Este comando está pensado generalmente para programas ejecutables en los que en algún momento se necesitarán permisos de administrador, se define sustituyendo la `x` de la parte del usuario por una `s`

```bash
-rwsr-x---
```

Si por ejemplo queremos como usuario normal sin permisos modificar la contraseña de nuestra cuenta, esa acción, aunque no tengamos permisos de administración, necesitará de permisos de administración en algún momento para poder realizar la acción. Para esto precisamente se necesita este `id`

`setgid`

Este comando tiene dos posibles definiciones, cuando actua sobre un fichero y cuando actua sobre un directorio.

- Sobre un archivo: Hace que cuando se ejecute el programa lo haga usando los permisos del grupo del archivo, no del grupo del usuario, permitiendo dar más libertad de acción al usuario, pero sin llegar a otorgarle permisos de administrador. Por ejemplo, supongamos que un archivo tenga que realizar acciones en el sistema pertenecientes al grupo `audio` pero nosotros no estamos en ese grupo, pues bien si ejecutamos el programa con `g+s` entonces lo haremos como parte de ese grupo.

- Sobre un directorio: Hace que cuando se cree un archivo, se hereden a este archivo los permisos del grupo de la carpeta, no los permisos del grupo al que pertenece el usuario, esto nos permite tener más uniformidad a la hora de crear archivos en carpetas compartidas, sino estuviese esta opción, cada archivo creado pertenecería al grupo de la persona en cuestión, mientras que con esto queda más uniforme, todos los archivos creados por usuarios diferentes pertenecen al mismo grupo.

Funciona sustituyendo la `x` del grupo del grupo por una `s`

```bash
drwxrws---
```

`sticky`

Actua sobre directorios compartidos, hace que solo los usuarios propietarios de un archivo puedan borrarlo o renombrarlo, aunque todos los uaurios que accedan a este directorio tengan permisos de escritura y lectura (777).

Se usa sobretodo el carpetas de acceso global a todos los usuarios como `/tmp` y se define sustituyendo la última `x` (grupo otros) por una `t`

```bash
rwxrwxrwt
```

```bash
// Podemos definir estos permisos con octal
chmod 4677
chmod 2444
chmod 1777

// O con simbólicos
chmod u+s nombre_archivo
chmod g+s nombre_directorio_archivo
chmod +t nombre_archivo
```
