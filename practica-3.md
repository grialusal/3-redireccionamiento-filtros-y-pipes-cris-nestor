# Sesión III - Redireccionamiento, filtros y pipes

Herramientas computacionales para bioinformática: UNIX, expresiones regulares y shell script

======================================

Nota: Enlaza al directorio con GTFs que creaste en la anterior sesión antes de comenzar. 

* * *

### 1. Redirecciones

#### 1.1. Redirección de la salida
Como has podido comprobar, la salida de las órdenes que ejecutamos en la shell van a la pantalla, que es la salida por defecto del sistema. 
Sin embargo, este comportamiento estándar se puede cambiar, por ejemplo, pidiendo que una orden guarde en un fichero lo que normalmente mostraría por pantalla. Esto en jerga "unixera" se llama __redireccionar__, y se emplea el caracter "mayor que" (`>`) para conseguirlo.  

Aquí redireccionamos la salida del comando `ls` para que guarde lo que sacaría por pantalla en un fichero llamado `resultados`.

```
abenito@cpg3:~/sesion-iii$ ls -l
total 12
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 gtfs
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-1
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-2
abenito@cpg3:~/sesion-iii$ ls -l > resultados
abenito@cpg3:~/sesion-iii$ ls -l
total 16
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 gtfs
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-1
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-2
-rw-r--r-- 1 abenito abenito  233 ene  5 14:46 resultados
```

Si ahora exploramos los contenidos de este fichero con la orden `cat` (con__cat__enate), nos sale:

```
abenito@cpg3:~/sesion-iii$ cat resultados
total 12
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 gtfs
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-1
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-2
-rw-r--r-- 1 abenito abenito    0 ene  5 14:46 resultados
```

**Ejercicio: prueba a ver el contenido de los ficheros gtf usando `cat`**

Si quisiéramos volcar el contenido de la pantalla a un fichero pero sin destruir su contenido (añadiéndolo al final), emplearemos el caracter "mayor que" dos veces (`>>`). Por ejemplo, para añadir la fecha actual, haríamos:

```
abenito@cpg3:~/sesion-iii$ date >> resultados
abenito@cpg3:~/sesion-iii$ more resultados
total 12
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 gtfs
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-1
drwxr-xr-x 2 abenito abenito 4096 ene  5 14:46 pruebas-2
-rw-r--r-- 1 abenito abenito    0 ene  5 14:46 resultados
lun ene  8 09:28:40 CET 2021
```

Como has podido comprobar, `date` sin argumentos devuelve la fecha actual en un formato por defecto especificado por el sistema y tu locale. 

**Ejercicio: Consulta el manual para ver cómo podrías sacar la fecha con el siguiente formato: `viernes, 08/01/21.`**

**Ejercicio: usa las sintaxis de comillas invertidas con `date` para crear un directorio llamado directorio-{diadelasemana}, donde {diadelasemana} será el dia de hoy (cuando se ejectuta el comando).** 


#### 1.2. Redirección de los errores

El canal de errores puede ser redirigido de manera análoga anteponiendo un 2 antes del símbolo "mayor que", así: `2>`. 

```
abenito@cpg3:~/sesion-iii$ ls noexisto 2> errores
abenito@cpg3:~/sesion-iii$ more errores
ls: no se puede acceder a 'noexisto': No existe el fichero o el directorio
```


#### 1.3. Fichero nulo 
En UNIX, existe un fichero especial que todo lo que se envía a él desaparecerá. Este fichero es `/dev/null`. 
Por ejemplo, si tenemos un programa o comando que es un poco pesado y no queremos que nos moleste con _warnings_ o _errores_, direccionaremos su `stderr` a `/dev/null`. 

Esto es muy útil cuando no queremos que la pantalla se nos llene de mensajes irrelevantes. Por ejemplo, si intentamos buscar todos los ficheros .gtf que existan en el directorio `/home`:

```
abenito@cpg3:~/sesion-iii$ find /home -name "*.gtf"
find: ‘/home/verogmartos’: Permiso denegado
find: ‘/home/alba’: Permiso denegado
find: ‘/home/gualtierolugato’: Permiso denegado
find: ‘/home/arqcarmel’: Permiso denegado
find: ‘/home/theron’: Permiso denegado
find: ‘/home/elenanavarro’: Permiso denegado
find: ‘/home/lihuengd’: Permiso denegado
find: ‘/home/amptoboso’: Permiso denegado
/home/abenito/sesion-iii/pruebas-2/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_3.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_1.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_bueno.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_2.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_4.gtf
/home/abenito/sesion-iii/gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-ii/pruebas-2/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_3.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_1.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_bueno.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_2.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_4.gtf
/home/abenito/sesion-ii/gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
find: ‘/home/laudupri/.ipython/profile_default/security’: Permiso denegado
find: ‘/home/laudupri/.ipython/profile_default/pid’: Permiso denegado
find: ‘/home/laudupri/.local’: Permiso denegado
find: ‘/home/josanc’: Permiso denegado
find: ‘/home/grader-python-bio/.local’: Permiso denegado
find: ‘/home/jorgeruizramirez’: Permiso denegado
find: ‘/home/lost+found’: Permiso denegado
find: ‘/home/lauraalvarezalvarez’: Permiso denegado
find: ‘/home/anaromero’: Permiso denegado
find: ‘/home/carlosp’: Permiso denegado
find: ‘/home/adrianambroa’: Permiso denegado
find: ‘/home/sagoji’: Permiso denegado
/home/seqview_user/annotations/Caenorhabditis elegans/gff/c_elegans.PRJNA13758.WS256.canonical_geneset.gtf
find: ‘/home/msandreu’: Permiso denegado
find: ‘/home/jibri’: Permiso denegado
find: ‘/home/alejandro’: Permiso denegado
find: ‘/home/giselcp’: Permiso denegado
find: ‘/home/rodri’: Permiso denegado
find: ‘/home/rafalopez’: Permiso denegado
find: ‘/home/agarmar/.ipython/profile_default/security’: Permiso denegado
find: ‘/home/agarmar/.ipython/profile_default/pid’: Permiso denegado
find: ‘/home/agarmar/.local’: Permiso denegado
```

Vemos, como era de esperar, que nos da un montón de errores relativos a nuestra falta de permisos para inspeccionar carpetas personales de otros usuarios. Para "filtrar" estos mensajes bastaría con hacer:

```
abenito@cpg3:~/sesion-iii$ find /home -name "*.gtf" 2> /dev/null
/home/abenito/sesion-iii/pruebas-2/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_3.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_1.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_bueno.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_2.gtf
/home/abenito/sesion-iii/pruebas-1/Drosophila_4.gtf
/home/abenito/sesion-iii/gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-ii/pruebas-2/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_3.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_1.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_bueno.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_2.gtf
/home/abenito/sesion-ii/pruebas-1/Drosophila_4.gtf
/home/abenito/sesion-ii/gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
/home/seqview_user/annotations/Caenorhabditis elegans/gff/c_elegans.PRJNA13758.WS256.canonical_geneset.gtf
```

Mucho mejor! Lo mismo podríamos hacer para analizar sólo los mensajes de error (redireccionando sólo la salida estándar con `comando 1> /dev/null`). 
Finalmente, si queremos volcar el contenido del `stdout` y el `stderr` al mismo fichero, usaremos `comando > fichero 2>$1`. Aquí le estaríamos diciendo a UNIX: "Envía la salida de errores al mismo sitio donde te he dicho que envíes la salida estándar". 

#### 1.4. Redirección de la entrada
Ya hemos visto que podemos usar la orden `cat` para inspeccionar el contenido de los ficheros que le pasemos como argumento. Vamos a reparar un poco más en cómo funciona esta orden invocándola sin argumentos:

```
abenito@cpg3:~/sesion-iii$ cat
Hola
Hola
Qué tal?
Qué tal?
eco
eco
```

La orden cat, invocada sin argumentos, toma todo lo que le llega por la entrada estándar (el teclado) y lo vuelca en la salida estándar (la pantalla), repitiendo todo lo que le tecleamos. Para salir de este bucle, tenemos que interrumpir su ejecución usando la combinación de teclas `Ctrl-C` (esta combinación de teclas "mata" el proceso que está en primer plano de manera "abrupta"). 

Podemos usar `cat` como editor rudimentario, simplemente redireccionando su salida estándar a un fichero. Crea un directorio `pruebas-cat` y añade las líneas siguientes:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ cat > provincias
Salamanca
Palencia
Segovia
León
Valladolid
Ávila
Burgos
Soria
Zamora
[[Pulsa Ctrl-D]]

abenito@cpg3:~/sesion-iii/pruebas-cat$ cat provincias
Salamanca
Palencia
Segovia
León
Valladolid
Ávila
Burgos
Soria
Zamora
```

Cuando terminamos de editar con `cat` es necesario emplear la combinación `Ctrl-D`, que significa "fin de fichero/transmisión", que se interpreta como parada no anormal o no abrupta. Con esto te aseguras que los contenidos del buffer son volcados a la salida indicada (cosa que no ocurre con `Ctrl-C`). 

`cat` pertenece a la familia de órdenes UNIX conocidas como __filtros__. Los filtros operan todos de manera similar, toman algo (normalmente texto, también llamado "stream") por la entrada estándar, hacen algo con dicho stream, y lo sacan por la salida estándar: 

```
======================================
Entrada estándar --> FILTRO --> Salida estándar
======================================
```

`cat` es el filtro más sencillo que existe, ya que no hace _nada_ con los datos. Existen sin embargo otros filtos más interesantes, como `sort`, que va a ordenar lo que le pasemos por la entrada estándar:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ sort
viernes
sábado
martes
lunes
[[Pulsa Ctrl-D]]
lunes
martes
sábado
viernes
```

Así visto, `sort` tiene utilidad, pero no demasiada, ya que tenemos que introducir lo que se quiere ordenar por teclado en vivo y en directo. ¿Cómo podríamos hacer que ordenase otras cosas? Pues sustituyendo su entrada estándar por otra cosa. ¿Cómo se consigue esto? Con el caracter "menor que" `<`. Vamos a ordenar la lista de las provincias que creamos antes: 

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ sort < provincias
Ávila
Burgos
León
Palencia
Salamanca
Segovia
Soria
Valladolid
Zamora
```

¿Qué está ocurriendo aquí?

```
======================================
Salamanca					Ávila
Palencia					Burgos
Segovia						León
León						Palencia
Valladolid 	--> sort -->	Salamanca
Ávila						Segovia
Burgos						Soria
Soria						Valladolid
Zamora						Zamora
======================================
```

Aquí estamos redireccionando la entrada estándar de sort a un fichero que contiene las provincias de CyL. El filtro sort recibe este "stream", hace la tarea para la cual fue diseñado (ordenar), y lo saca por la salida estándar (pantalla). 


**Ejercicio:** En el ejemplo anterior, hemos redireccionado la entrada de sort para sacarla por su salida estándar. ¿Cómo haríamos para redireccionar también su salida a un fichero `provincias-ordenadas`? 


### 2. Filtros básicos
#### 2.1. head y tail 
Si como te dije, probaste a inspeccionar algunos ficheros usando la orden `cat`, quizás diste con algún fichero suficientemente largo como para llenar tu terminal de caracteres (algo bastante molesto), impidiéndote tener una idea de sus contenidos. Para suplir esta carencia de `cat`, tenemos los comandos `head` y `tail`, que nos permitirán explorar el principio y el final de un fichero, respectivamente.

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ head provincias_ordenadas
Ávila
Burgos
León
Palencia
Salamanca
Segovia
Soria
Valladolid
Zamora
```

Por defecto, `head` mostrará las 10 primeras líneas del fichero (en este caso todas), pero podemos controlar cuántas sacará con la opción `-n`:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ head -n3 provincias_ordenadas
Ávila
Burgos
León
```

Por su parte, `tail`, está pensado para mirar el final del fichero. Funciona de forma muy parecida a `head`:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ tail -n3 provincias_ordenadas
Soria
Valladolid
Zamora
```

`tail` es muy útil para quitar encabezados, por ejemplo en ficheros .csv. Si a la opción `-n` le pasamos un número con un signo positivo `+` delante, nos mostrará el fichero empezando en esa línea: 

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ tail -n+3 provincias_ordenadas
León
Palencia
Salamanca
Segovia
Soria
Valladolid
Zamora
```


**Ejercicio:** Inspecciona algunos ficheros .gtf, .fasta y .bed de tu HOME usando `head` y `tail` (encuéntralos antes usando `find`). 


#### 2.2. cut, paste, uniq
Estos filtros sirven para quedarse con parte de la información o reformatearla. `cut` sirve para quedarnos con una parte de la información que aparezca en cada línea. Por ejemplo, si quisiéramos quedarnos con los tres primeros caracteres de las provincias de CyL:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ cut -c 1-3 provincias_ordenadas
Áv
Bur
Le�
Pal
Sal
Seg
Sor
Val
Zam
```

El resultado no es exactamente el que esperábamos (fíjate en lo que ha extraído para Ávila y León). Esto es un problema de codificación, y sucede porque la mayoría de herramientas GNU no entienden UTF-8, que es la codificación de nuestro fichero. Veremos cómo solucionar este problema con otras herramientas en futuras clases. 

A pesar de este problema, `cut` sigue siendo muy útil para trabajar con datos tabulares como .bed y .gtf, que será el principal uso que le demos. 
Estos ficheros tienen la particularidad de que formatean sus datos en columnas usando normalmente el caracter "invisible" de tabulación `\t`. 
Gracias a esto, `cut` va a poder extraer datos de estos ficheros usando la opción `-f`. Vamos a probarlo con nuestros genes de pega para sacar todos los rangos contenidos en el fichero: 

```
abenito@cpg3:~/sesion-iii/genes$ head -n 3 gene-1.bed
1	6206197	6206270	GENE00000025907
1	6223599	6223745	GENE00000025907
1	6227940	6228049	GENE00000025907

abenito@cpg3:~/sesion-iii/genes$ cut -f 2-3 gene-1.bed
6206197	6206270
6223599	6223745
6227940	6228049
6229959	6230073
6230003	6230005
6233961	6234087
6234229	6234311
6206227	6206270
6227940	6228049
6229959	6230073
6230003	6230073
6233961	6234087
6234229	6234399
6238262	6238384
6214645	6214957
6227940	6228049
6229959	6230073
6230003	6230073
6233961	6234087
6234229	6234399
6238262	6238464
6239952	6240378

```

Por defecto, `cut` va a interpretar el tabulador como el separador por defecto de nuestros datos. Pero podemos especificar el que queramos con la opción `-d`. Aquí va un ejemplo sencillo: 

```
abenito@cpg3:~/sesion-iii$ cat > separar
esta es una línea que quiero separar
<ctrl-d> 

abenito@cpg3:~/sesion-iii$ cut -f 2-4 -d " " separar
es una línea
```

Como orden complementaria a `cut`, existe `paste`. Esta orden toma una línea del fichero y le añade lo que queremos, por ejemplo, para unir nuestros ficheros .bed horizontalmente usando un tabulador, haríamos:

```
abenito@cpg3:~/sesion-iii/genes$ paste -d "\t" gene-1.bed gene-2.bed
1	6206197	6206270	GENE00000025907	1	6206197	6206270	GENE00000025907
1	6223599	6223745	GENE00000025907	1	6223599	6223745	GENE00000025907
1	6227940	6228049	GENE00000025907	1	6227940	6228049	GENE00000025907
1	6229959	6230073	GENE00000025907	1	6222341	6228319	GENE00000025907
1	6230003	6230005	GENE00000025907	1	6229959	6230073	GENE00000025907
1	6233961	6234087	GENE00000025907	1	6233961	6234087	GENE00000025907
1	6234229	6234311	GENE00000025907	1	6234229	6234311	GENE00000025907
1	6206227	6206270	GENE00000025907	1	6206227	6206270	GENE00000025907
1	6227940	6228049	GENE00000025907	1	6227940	6228049	GENE00000025907
1	6229959	6230073	GENE00000025907	1	6229959	6230073	GENE00000025907
1	6230003	6230073	GENE00000025907	1	6230133	6230191	GENE00000025907
1	6233961	6234087	GENE00000025907	1	6233961	6234087	GENE00000025907
1	6234229	6234399	GENE00000025907	1	6234229	6234399	GENE00000025907
1	6238262	6238384	GENE00000025907	1	6238262	6238384	GENE00000025907
1	6214645	6214957	GENE00000025907	1	6227940	6228049	GENE00000025907
1	6227940	6228049	GENE00000025907	1	6229959	6230073	GENE00000025907
1	6229959	6230073	GENE00000025907	1	6233961	6234087	GENE00000025907
1	6230003	6230073	GENE00000025907	1	6234229	6234399	GENE00000025907
1	6233961	6234087	GENE00000025907	1	6239952	6240378	GENE00000025907
1	6234229	6234399	GENE00000025907
1	6238262	6238464	GENE00000025907
1	6239952	6240378	GENE00000025907
```

Finalmente, `uniq` es un filtro muy sencillo que sólo quita ocurrencias repetidas de una línea en un stream de texto. Además, con la opción `-c` nos contará cuántas veces aparece cada una, y con la opción `-d` nos dará sólo las líneas que están repetidas (útil para encontrar duplicados)

```
abenito@cpg3:~/sesion-iii/genes$ cut -f 3 gene-1.bed > gene-1-ends
abenito@cpg3:~/sesion-iii/genes$ cat gene-1-ends
6206270
6223745
6228049
6230073
6230005
6234087
6234311
6206270
6228049
6230073
6230073
6234087
6234399
6238384
6214957
6228049
6230073
6230073
6234087
6234399
6238464
6240378

abenito@cpg3:~/sesion-iii/genes$ uniq -c gene-1-ends
      1 6206270
      1 6223745
      1 6228049
      1 6230073
      1 6230005
      1 6234087
      1 6234311
      1 6206270
      1 6228049
      2 6230073
      1 6234087
      1 6234399
      1 6238384
      1 6214957
      1 6228049
      2 6230073
      1 6234087
      1 6234399
      1 6238464
      1 6240378
```

#### 2.3. wc
Un filtro muy usado en manejo de ficheros de texto es `wc` (word count). `wc` nos dar'a el número de líneas, palabras y caracteres de la entrada.
Una prueba con un .gtf:

```
abenito@cpg3:~/sesion-iii$ wc gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
   543511  13371522 140785169 gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
``` 

Aunque normalmente usaremos la opción "-l", que nos da sólo el número de líneas:

```
abenito@cpg3:~/sesion-iii$ wc -l gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
543511 gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf
```


### 3. Pipes
Hagamos un alto en el camino antes de seguir aprendiendo filtros para describir uno de los conceptos de UNIX que en su día más contribuyeron al éxito de este sistema operativo: los __data pipes__ (o tuberías). Vamos a explicar lo que son.

Cuando más arriba te pedía que ordenases el archivo provincias con `sort` y volcases el resultado a otro fichero, seguramente lo resolvieses así:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ sort < provincias > provincias_ordenadas
```

Aunque esto es completamente válido, eso de redireccionar la entrada estándar de esta manera es un poco "raro" de ver en la naturaleza. 
Lo que normalmente haremos será "enchufar" la salida de nuestros comandos a la entrada de otros para producir un resultado final que busquemos (de ahí lo de "pipe"). Este efecto deseado lo vamos a conseguir con el carácter de tubería `|` y va a permitir que nos ahorremos un montón de ficheros intermedios que tendríamos que borrar (lo que nos ha pasado al presentar `cut` y `paste`). 

En general, la manera de encadenar comandos mediante tuberías será la siguiente:

```
comando_1 entrada.txt | comando_2 | comando_3 | ... | comando_n > salida.txt
```

Mediante el uso de tuberías, es muy sencillo conseguir lo que ya hemos antes, por ejemplo para el ejemplo de las provincias:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ cat provincias | sort
Ávila
Burgos
León
Palencia
Salamanca
Segovia
Soria
Valladolid
Zamora
```

La salida estándar de `cat` (los contenidos del fichero provincias) se enchufa a la entrada de sort, que ordena las líneas y las saca por su salida estándar que en este caso es la pantalla. Aunque podríamos conectarla a la entrada de otro comando: 

```
abenito@cpg3:~/sesion-iii/pruebas-cat$  cat provincias | sort | cut -c1,3
�v
Br
L�
Pl
Sl
Sg
Sr
Vl
Zm
```

Si volvemos al ejemplo de los genes, podemos generar la siguiente salida análoga a la del apartado anterior:

```
abenito@cpg3:~/sesion-iii/genes$ cat gene-1.bed | cut -f 3 | sort | uniq -c | sort -nr
      5 6230073
      3 6234087
      3 6228049
      2 6234399
      2 6206270
      1 6240378
      1 6238464
      1 6238384
      1 6234311
      1 6230005
      1 6223745
      1 6214957
```

Pero espera! No es exactamente igual! Qué ha pasado??? Bueno, resulta que `sort` no funciona bien si no se le pasan los datos ordenados.
Además, por defecto, también tomará el orden lexicográfico (`10<2`). Para que ordene números hay que decírselo con `-n`. Otro detalle es que `sort` ordena de menor a mayor. Si queremos que ordene en el otro sentido, debemos pasarle la opción `-r`. 


#### tee
A veces, en nuestros pipelines, necesitaremos escribir ficheros intermedios a disco. Estos ficheros intermedios pueden ser útiles para debuguear un pipeline o cuando queremos salvar datos intermedios si nuestro pipeline tarda mucho en terminar (podríamos comenzar a hacer otros tipos de análisis en estos residuales sin esperar a que termine todo). 

`tee` sirve específicamente este propósito: no hace nada más que desdoblar la salida en dos, a un fichero y a su salida estándar. Aquí lo usamos para ver qué saca `cut -f 3` en el anterior ejemplo:

```
abenito@cpg3:~/sesion-iii/genes$ cat gene-1.bed | cut -f 3 | tee columns.txt | sort | uniq -c | sort -nr
      5 6230073
      3 6234087
      3 6228049
      2 6234399
      2 6206270
      1 6240378
      1 6238464
      1 6238384
      1 6234311
      1 6230005
      1 6223745
      1 6214957
abenito@cpg3:~/sesion-iii/genes$ cat columns.txt
6206270
6223745
6228049
6230073
6230005
6234087
6234311
6206270
6228049
6230073
6230073
6234087
6234399
6238384
6214957
6228049
6230073
6230073
6234087
6234399
6238464
6240378
```

### 4. Más filtros

#### 4.1. more y less
`more` saca las líneas página a página, que iremos mostrando pulsando la tecla espacio.
Pruébalo con el fichero `Drosophila_melanogaster.BDGP6.28.102.gtf`. 

`less` es una versión _"en esteroides"_ de `more`. Admite muchas más opciones y además no carga todo el fichero en memoria para mostrarlo (útil para navegar ficheros muy grandes). Casi siempre vamos a preferir usar `less` a `more`. Para navegar un fichero usando `less`, usaremos los siguientes atajos de teclado:

| Atajo     | Acción                                      |
|-----------|---------------------------------------------|
| q   		  | Salir                            |
| Espacio   | Página siguiente                            |
| b         | Página anterior                             |
| g         | Primera línea                               |
| G         | Última línea                                |
| j         | Ir hacia abajo línea por línea              |
| k         | Ir hacia arriba línea por línea             |
| /\<patrón> | Búsqueda hacia abajo de la cadena \<patrón>  |
| ?\<patrón> | Búsqueda hacia arriba de la cadena \<patrón> |


Además de para mirar el contenido de archivos, `less` es muy útil para analizar la salida de los comandos cuando estemos programando nuestros pipelines. `less` va a capturar el contenido del último comando y va a pausar la ejecución (cuando se llene el terminal) para que puedas ir viendo la salida progresivamente. 


#### 4.2. grep

Uno de los filtros más usados es `grep`. grep va a dejar pasar todo lo que case con el primer argumento y filtrará lo demás. 

Si necesitas encontrar un patrón textual en un fichero, `grep` va a operar _más rápido_ que cualquier cosa que puedas escribir en Python, R, o incluso otras herramientas de línea de comandos como `awk` o `sed` (que veremos en las próximas sesiones). Es realmente rápido. Apróximadamente __cinco__ veces más rápido que la alternativa más rápida. O sea, _rapidísimo_. 

Esto es debido a que está diseñado para hacer una sola tarea muy muy bien: buscar líneas en un fichero que casan con un patrón. Existen otras alternativas con más opciones, pero esta versatilidad afecta negativamente a su aficiencia en la tarea de búsqueda. 

Si la velocidad es una preocupación (por ej., cuando estemos manejando ficheros muy grandes, y/o gran cantidad de ficheros), las herramientas UNIX son, normalmente, las que dan la implementación más veloz (l@s hackers de los 70 eran un@s verdader@s máquinas). 

`grep` admite dos argumentos: uno, el patrón a buscar, que será una cadena o una expresión regular, y dos, el archivo o archivos donde se quiere buscar. 

Comencemos por un ejemplo sencillo: veamos qué provincias contienen al menos una letra "v" minúscula: 

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ grep "v" provincias
Segovia
Ávila
```

O aquellas que _no_ contienen una letra "v" minúscula (opción -v): 

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ grep -v "v" provincias
Salamanca
Palencia
León
Valladolid
Burgos
Soria
Zamorafiltro de o
```

Las comillas rodeando lo que queremos buscar no son necesarias, pero las emplearemos de todas formas para evitar que la shell trate de interpretar algún caracter por error. 

Con la opción `-c` nos da simplemente la cuenta de ocurrencias que ha encontrado:

```
abenito@cpg3:~/sesion-iii/pruebas-cat$ grep -c "v" provincias
2
```


Una opción muy útil es `--color`, que resaltará las ocurrencias en texto en nuestro terminal. Pruébala.

También, vamos a poder usar expresiones regulares con `grep`. Por ejemplo aquí vamos a sacar un .gtf en formato .bed:

```
abenito@cpg3:~/sesion-iii/gtfs$ grep -v "^#" Drosophila_melanogaster.BDGP6.28.102.gtf | cut -f 1,4,5 | head
3R	567076	2532932
3R	567076	2532932
3R	567076	567268
3R	835376	835491
3R	835378	835491
3R	835378	835380
3R	869486	869548
3R	869486	869548
3R	895786	895893
3R	895786	895893
```

Aquí le digo que saque aquellas líneas que __no__ comienzan con el caracter `#` (header). Luego extraigo las columnas 1, 4 y 5 y visualizo las 10 primeras líneas.

Sin embargo, si usamos más columnas...

```
abenito@cpg3:~/sesion-iii/gtfs$ grep -v "^#" Drosophila_melanogaster.BDGP6.28.102.gtf | cut -f 1-8 | head
3R	FlyBase	gene	567076	2532932	.	+	.
3R	FlyBase	transcript	567076	2532932	.	+	.
3R	FlyBase	exon	567076	567268	.	+	.
3R	FlyBase	exon	835376	835491	.	+	.
3R	FlyBase	CDS	835378	835491	.	+	0
3R	FlyBase	start_codon	835378	835380	.	+	0
3R	FlyBase	exon	869486	869548	.	+	.
3R	FlyBase	CDS	869486	869548	.	+	0
3R	FlyBase	exon	895786	895893	.	+	.
3R	FlyBase	CDS	895786	895893	.	+	0
```

...a veces no es sencillo visualizar bien qué dato pertenece a qué columna! 

Podemos usar el comando `column -t` para mostrar las columnas mejor separadas. Aunque cuidado! no uses `column` para formatear datos y pasárselos a otros comandos. `column` está pensado para visualizar datos en el terminal. Los ordenadores normalmente prefieren usar datos delimitados por tabuladores porque son más fáciles de parsear. Sin embargo los humanos preferimos usar un tamaño variable de espacios en la mayoría de ocasiones. 


```
abenito@cpg3:~/sesion-iii/gtfs$ grep -v "^#" Drosophila_melanogaster.BDGP6.28.102.gtf | cut -f 1-8 | head | column -t
3R  FlyBase  gene         567076  2532932  .  +  .
3R  FlyBase  transcript   567076  2532932  .  +  .
3R  FlyBase  exon         567076  567268   .  +  .
3R  FlyBase  exon         835376  835491   .  +  .
3R  FlyBase  CDS          835378  835491   .  +  0
3R  FlyBase  start_codon  835378  835380   .  +  0
3R  FlyBase  exon         869486  869548   .  +  .
3R  FlyBase  CDS          869486  869548   .  +  0
3R  FlyBase  exon         895786  895893   .  +  .
3R  FlyBase  CDS          895786  895893   .  +  0
```

El próximo día seguiremos explorando las opciones de `grep`, y veremos en más detalle los dos lenguaje de expresiones regulares más famosos que usa, POSIX Basic Regular Expressions (BRE) y Extended Regular Expressions (ERE). Si lo deseas, puedes ir echando un ojo [aquí](https://en.wikipedia.org/wiki/Regular_expression#Standards).

### Órdenes de la shell vistas en esta sesión:

`cat`: muestra el contenido de ficheros, filtro nulo

`date`: muestra la fecha y hora actuales

`sort`: filtro que ordena las líneas que se pasen por la entrada

`head`: filtro que muestra las primeras líneas de un fichero

`tail`: fltro que muestra las últimas líneas de un fichero

`cut`: filtro para seleccionar columnas

`paste`: filtro para concatenar columnas de dos ficheros

`uniq`: filtro que elimina líneas repetidas

`wc`: filtro para contar líneas, palabras y caracteres

`more`: mostrar el contenido de la entrada página a página

`less`: inspección avanzada del stream de texto

`grep`: filtro para seleccionar líneas de acuerdo a una cadena o expresión regular 

`column`: ayuda a visualizar datos tabulares


