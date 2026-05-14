# Redirecciones

Los comandos que lanzamos en Linux pueden devolver dos tipos de resultados:

1. `stdout` también llamada salida estándar, es la usada para devolver los resultados de lo que se supone ha de hacer el comando en un comportamiento normal.
2. `stderr` también llamada salida de error, es la usada para devolver el estado o los errores del comando.

Cuando hacemos referencia a lo que introduce el usuario por pantalla, normalmente a través del teclado, hablamos de entrada estándar o `stdin`

**Redireccionamientos**

Por defecto tanto el `stdout` como el `stderr` utilizan la pantalla para devolver los resultados, pero eso podemos cambiarlo usando el operador `>` este lo que hace es enviar los datos de lo que devuelva el comando a un archivo.

```bash
ls /usr > output.txt
```

Ten en cuenta que si el comando lanza un error, es decir, usa `stderr` el operador `>` no hará nada, ya que este solo funciona con `stdout` 

**Append**

Por defecto, el operador `>` re-escribe el archivo desde el principio, borrando todo lo que hubiese antes, si lo que queremos es que acumule lo que ya tenía en lugar de re-escribir, tenemos que usar el operador `>>`

En el caso de que queramos usar múltiples comandos y combinar la salida de todos ellos en un único archivo podemos usar `{}` ya que con el vamos a poder lanzar múltiples comandos y tratar la salida de todos ellos de manera unificada.

```bash
{ comando1; comando2; comando3; } > log.txt
```

**Redirigir errores**

Desafortunadamente, para redirigir errores no tenemos un comando específico, tenemos que hacer uso de referencias, una referencia es un identificador del tipo de operación que devuelve el comando, existen tres, `0` para `stdinput` el `1` para el `stdout` y el `2` para el `stderr`

Podemos combinarlo con `>` para devolver lo que queramos.

```bash
ls /lksdjfl 2> error.txt
```

Si además queremos guardar en el archivo, tanto el error como la salida normal del comando, tenemos que hacer una combinación un poco extraña.

```bash
ls /usr > log.txt 2>&1
```

La operación `2>&1` quiere decir, envía el resultado de error, donde habías enviado la salida estándar. Como es necesario que esta salida estándar exista previamente, es importante que el orden de las operaciones sea correcto, y no guardar por ejemplo antes del error y después la salida estándar.

```bash
// Esto no va a funcionar
ls 2>&1 > log.txt
```

**Ignorar errores**

Si queremos lanzar un comando ignorando los errores que puedan suceder en el proceso, podemos usar un directorio especial en Linux para ello `/dev/null` todo lo que sea redirigido ahí será destruido.

```bash
ls /usr 2> /dev/null
```

**Redireccionamiento de la entrada de información**

Con Bash podemos redirigir la entrada de información por parte del usuario, uno de los comandos más usados para ello es `cat` este nos permite mostrar por pantalla el contenido de un archivo de texto. Aunque lo que realmente lee `cat` son bytes, por lo que vamos a poder abrir cualquier archivo en realidad, otra cosa es que se entienda.

```bash
cat users.txt
```

Otro punto interesante de `cat` es que permite la fusión de diferentes archivos y mostrar por pantalla la unión creada.

```bash
cat users.txt users2.txt
```

Como `cat` lee bytes, no texto como tal, podemos unir archivos multimedia.

```bash
cat mov1.mpg mov2.mpg mov3.mpg > result.mpg
```

Podemos también usarlo como procesador de texto si no le pasamos ningún argumento, además si no le pasamos nada y usamos el comando `>` podemos tener un generador de texto en el terminal.

```bash
cat > test.txt
```

El operador `<` indica que el argumento a continuación ha de ir al comando que lancemos, que es exactamente lo mismo que hace `cat` si no lo pasamos, tenerlo en cuenta solo como dato informativo.

```bash
cat < story.txt

// Hace lo mismo que
cat story.txt
```

**Pipelines**

Una *pipeline* es un operador que nos permite pasar la salida de un comando, a la entrada de otro, siempre y cuando el comando destino admita `stdin`

```bash
ls /usr | less
```

Esto sería idéntico a:

```bash
// Ejecutamos y copiamos el contenido de la salida de ls
ls /usr

// Lo pegamos como argumento en less
less (aquí lo copiado por ls)
```

`sort`

Una característica muy potente de las *pipelines* es que pueden concatenarse entre ellas permitiendo combinar sus efectos en un único resultado. Veamos el ejemplo de `sort` la cual nos permite ordenar un conjunto de textos.

```bash
ls /usr | sort | less
```

`uniq`

El comando `uniq` elimina duplicados de una lista ordenada, es importante este dato porque en realidad elimina duplicados adyacentes, si la lista no está ordenada es posible que algunos no se eliminen. Si queremos que en lugar de devolver el listado sin duplicados, devuelva los duplicados, le pasamos la opción `-d`

```bash
// Elimina duplicados
ls /usr | sort | uniq | less

// Muestra los duplicados
ls /usr | sort | uniq -d | less
```

En los ejemplos anteriores se hace uso de `less` para mostrar los resultados de manera paginada, pero no es necesario, los resultados saldrán igualmente por pantalla sino usamos `less` al final de cada operación.

`wc`

El comando `wc` (word count) nos permite obtener el número de líneas, palabras y bytes (en este orden) de un conjunto de textos. Si lo que queremos es limitar el número para ver solo las líneas, usamos la opción `-l`

Contar solo líneas es especialmente útil si lo que queremos es contar listados de resultados.

```bash
ls /usr | sort | uniq | wc -l
```

`grep`

El comando `grep` permite buscar una cadena de carácteres coindiciente dentro de uno o varios archivos de texto, si se pasa como *pipeline* entonces filtrará sobre ese listado resultante.

```bash
ls /usr | sort | uniq | grep abc
```

Algunas opciones que maneja `grep` son:

- `-i` normalmente `grep` es sensible a mayúsculas y minúsculas, con esta opción, hacemos que ignore este criterio y muestre todo lo que coincida.
- `-l` le dice a `grep` que devuelva solo los nombres de los archivos donde ha habido coincidencias, en ligar de mostrar también las líneas de resultado.
- `-v` hace que `grep` devuelva solo lo que no coincide.
- `-w` hace que `grep` haga la coincidencia solo con palabras completas, no con partes de texto.

`head` `tail`

Estos dos comandos nos permiten visualizar solo las 10 primeras líneas o las 10 últimas líneas de texto de un archivo de texto. Podemos especificar el número de líneas concreto con la opción `-n`

```bash
head -n 20 log.txt

tail -n 15 log.txt

ls /usr | sort | tail -n 5
```

Una opción muy potente de `tail` es que nos permite ver en directo que ocurre en un archivo que pasemos como argumento con la opción `-f`

```bash
tail -f logs.txt
```

`tee`

El comando `tee` nos permite bifurcar una salida de un comando al `stdout` y a un archivo al mismo tiempo, es especialmente útil si queremos ver el estado de un *pipe* en un momento dado.

```bash
ls /usr/bin | sort | tee state.txt | grep zip
```

Una última cosa, los comando que hemos visto en última instancia se han ejemplificado siempre en un *pipe* pero eso no tiene porque ser siempre así, de hecho, todos estos comandos pueden ser utilizados perfectamente por separado actuando sobre archivos.

```bash
grep zip test.txt

wc test.txt
```