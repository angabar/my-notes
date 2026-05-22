# Información

Que es un comando? un comando puede ser una de estas 4 cosas:

1. Un programa ejecutable, compilado o interpretado.
2. Un comando interno del shell, también llamados "built-in"
3. Una función del shell
4. un alias

`type`

El comando `type` devuelve el tipo de comando que pasamos como argumento, si es un ejecutable, un alias...

`wich`

Nos indica la ruta del ejecutable que le pasemos como argumento, solo admite estos, por lo que los alias o built-in fallarán. Muy últil si tenemos varios ejecutables al mismo tiempo instalados en el sistema.

`help`

Nos proporciona ayuda e instrucciones de un built-in. Los corchetes en un comando nos indican opcionalidad y el pipe, descarte.

```bash
# cd puede ser precedido de -L o -P, en caso de ser precedido de -P podrá, además ser precedido por -e, pero no es posible -L -e
cd [-L | [-P [-e]]]
```

`--help`

Muchos ejecutables permiten la opción `--help` con la que poder obtener información útil de este, no siempre estará disponible.

`man`

Seguido de un comando, muestra el manual de este, es mejor leerlos con `less` para poder paginarlo. Los manuales se dividen en secciones numéricas, podemos complementar el comando con este y un texto a buscar para obtener más información.

```bash
man pwd 5 pw
```

`apropos`

Si queremos buscar en un manual y no sabemos la sección en la que empezar, podemos usar `apropos` para pasarle una cadena de texto y que busque en el manual las coincidencias.

`whatis`

Devuelve la primera sección de un manual, normalmente la descripción, de un comando, es algo escueto, definitorio.

`info`

Proporciona información de un comando, una alternativa más moderna a `man` con opción de hipervínculos.

`alias`

Permite crear una secuencia de comandos o comando en una varibale "X".

```bash
# Que no haya espacios entre el igual
gohome="cs /home"
```

`unalias`

Elimina un alias creado con anterioridad.

**Notas adicionales del tema**

Podemos lanzar muchos comandos seguidos en paralelo usando `&` y en serie, uno después del otro con `;`
