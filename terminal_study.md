# Estudio del terminal

Cada vez que introducimos algo en el terminal, en el momento en que pulsamos _enter_ este lo interpreta y lo traduce, es decir, el texto introducido se transforma. Esto solo funciona (al menos de momento con los conocimientos que poseo, con el comando `echo` )

El carácter `*` por ejemplo, es interpretado por el _shell_ como “cualquier cosa”.

```bash
// Devuelve todo lo que haya en el directorio actual
echo *

// Podemos filtrar de diferentes formas
echo D*
echo *s
echo [[:upper:]]*
```

El carácter `~` representa la _home_ del usuario, es decir, su carpeta personal.

```bash
echo ~
// /home/user
```

### Operaciones aritméticas

Con la expresión `$((  ))` podemos realizar operaciones matemáticas en el propio _shell_ el único detalle que tenemos que tener en cuenta es que no podemos utilizar número decimales, solo enteros.

```bash
echo $(( 2 + 2 )) // 4

// Podemos usar paréntesis
echo $(( (2 + 2) * 5 ))
```

### Expansiones de corchetes

Las expansiones de corchetes consisten en tres partes, el inicio, el final y la parte intermedia, que puede estar compuesta a su vez de puntos suspensivos o de cadenas de carácteres. El terminal interpretará esto como un listado a iterar y generando su parte correspondiente en cada iteración.

```bash
echo test-{A,B,C}
echo test-{1..10}
echo test-{001..010}
```

Ten en cuenta que cada expansión de corchetes es un bucle, por lo que dos expansiones serán dos bucles.

```bash
// Esto muestra 100 resultados
echo test-{1..10}-{1..10}
```

### Variables

Podemos acceder a diferentes variables disponibles en el sistema usando `$` seguido del nombre de la variable.

```bash
echo $USER
```

Para ver el número de variables disponibles en el sistema tenemos que utilziar `printenv`

### Sustitución de comandos

Podemos utilizar comandos dentro de expresiones para que sirvan como argumentos para otros comandos, por ejemplo:

```bash
ls -l $(wich cp)

// Incluso usar pipes
file $(ls -d /usr/bin/* | grep zip)
```

### Comillas

Cuando pasamos una cadena de carácteres por el shell esta es tratada como comandos independientes salvo que usemos las comillas dobles. Teniendo en cuenta que las expresiones de variables, aritméticas y de sustitución de comandos, seguirán existiendo.

```bash
// Esto es interpretado como tres comandos independientes
echo my file name.txt

echo "my file name".txt

// Esto seguirá funcionando como si nada
echo "$USER $(( 2 + 2 )) $(wich cp)"
```

Si lo que queremos es que todo sea tratado como texto tenemos que utilizar las comillas simples.

```bash
// Imprimirá "my var is $USER"
echo 'my var is $USER'
```

Si queremos ignorar el tratamiento de solo un carácter, tendremos que usar `\` pudiendo además usar este para utilizar algunos de los carácteres especiales de programación general, como el salto de línea, la tabulación, etc.

```bash
echo "My sum is \$$(( 2 + 2 ))"

echo "Theres is a \t extra space here"
```
