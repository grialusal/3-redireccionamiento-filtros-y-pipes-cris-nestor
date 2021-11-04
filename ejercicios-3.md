# Sesión III - Redireccionamiento, filtros y pipes

Herramientas computacionales para bioinformática: UNIX, expresiones regulares y shell script

Edita esta plantilla en formato markdown [Guía aquí](https://guides.github.com/features/mastering-markdown/) como se pide en el guión. 
Cuando hayas acabado, haz un commit de tus cambios y súbelos al repositorio antes de la fecha de entrega señalada. 

======================================


## Ejercicio 1
`sort -R` (random) desordena las líneas de un fichero. Prueba a desordenar el fichero `gene-2.bed` y crea un nuevo fichero llamado `gene-2-desordenado.bed`.

Trata ahora de ordenar este fichero de acuerdo a los siguientes criterios: 
1. Sin cortar los elementos
2. En orden descendente
3. Usando a la vez la tercera y la segunda columna (en este orden de prioridad). Consulta el manual para ver la opción -k. 

### Respuesta ejercicio 1

Examinando el archivo gene-2,bed con  nano tiene este aspecto

```
GNU nano 5.6.1                      gene-2.bed                               
1       6206197 6206270 GENE00000025907
1       6223599 6223745 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6222341 6228319 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6234229 6234311 GENE00000025907
1       6206227 6206270 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6230133 6230191 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6234229 6234399 GENE00000025907
1       6238262 6238384 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6234229 6234399 GENE00000025907
1       6239952 6240378 GENE00000025907
```
Usamos el flag -R de sort e examinamos el resultado con cat:

```
nguerrero@nguerrero-VirtualBox:~/genes$ sort -R gene-2.bed > gene-2-desordenado.bed
nguerrero@nguerrero-VirtualBox:~/genes$ cat gene-2-desordenado.bed 
1  6238262  6238384  GENE00000025907
1  6206227  6206270  GENE00000025907
1  6222341  6228319  GENE00000025907
1  6234229  6234311  GENE00000025907
1  6233961  6234087  GENE00000025907
1  6233961  6234087  GENE00000025907
1  6233961  6234087  GENE00000025907
1  6206197  6206270  GENE00000025907
1  6223599  6223745  GENE00000025907
1  6234229  6234399  GENE00000025907
1  6234229  6234399  GENE00000025907
1  6230133  6230191  GENE00000025907
1  6239952  6240378  GENE00000025907
1  6229959  6230073  GENE00000025907
1  6229959  6230073  GENE00000025907
1  6229959  6230073  GENE00000025907
1  6227940  6228049  GENE00000025907
1  6227940  6228049  GENE00000025907
1  6227940  6228049  GENE00000025907
```
  Para ordenarlos en orden descendente se usa el flag reverse -r. Para ordenarlos según la tercera columna usamos el comando sort -k
leyendo el manual, dice que se pueden ordenar dependiendo de una `key` que interpreto como una clave, como clave en bases de datos y explica que debemos introducir un numero `n` según el `field`, que leyendo el ejercicio interpreto como columna, al poner entonces `-k3` se ordenan según
la tercera columna. Y los datos de la segunda columna aparecen también ordenados en orden decreciente debido a la propia naturaleza de los datos. Parece que también disminuyen progresivamente al igual que los datos de la columna 3.
```
nguerrero@nguerrero-VirtualBox:~/genes$ sort -k3 -r gene-2-desordenado.bed
1  6239952  6240378  GENE00000025907
1  6238262  6238384  GENE00000025907
1  6234229  6234399  GENE00000025907
1  6234229  6234399  GENE00000025907
1  6234229  6234311  GENE00000025907
1  6233961  6234087  GENE00000025907
1  6233961  6234087  GENE00000025907
1  6233961  6234087  GENE00000025907
1  6230133  6230191  GENE00000025907
1  6229959  6230073  GENE00000025907
1  6229959  6230073  GENE00000025907
1  6229959  6230073  GENE00000025907
1  6222341  6228319  GENE00000025907
1  6227940  6228049  GENE00000025907
1  6227940  6228049  GENE00000025907
1  6227940  6228049  GENE00000025907
1  6223599  6223745  GENE00000025907
1  6206227  6206270  GENE00000025907
1  6206197  6206270  GENE00000025907
```

### Comentario: Simplemente faltaría añadir `,3n` a `-k3` para que ordene numéricamente los datos, además faltaría añadir -k2,2n para que ordene los datos en primer lugar teniendo en cuenta primero la segunda columna y en caso de empate teniendo en cuenta el orden de la tercera columna, la opción correcta sería: `sort -k3,3n -k2,2n -r`. Por lo demás todo perfecto, muy bien :)
### Nota: 2.3/2.5

## Ejercicio 2

Cuáles son y cuántos tipos distintos de "features" hay en `Drosophila_melanogaster.BDGP6.28.102.gtf` y en `Homo_sapiens.GRCh38.102.gtf.gz`? Nota: para trabajar con ficheros .gunzip sin descomprimir puedes usar `zcat`.

### Respuesta ejercicio 2
 Para el archivo Drosophila_melanogaster.BDGP6.28.102.gtf, generamos el siguiente pipelina: primero cortamos para tomar la información del tercer campo, que en este caso es la tercera columna con la orden 'cut -f3' luego lo ordenamos con el comando 'sort' porque el comando 'uniq' trabaja sobre archivos ordenados.  Tras ordenarlo usamos el comando grep que filtra (retirando, gracias al flag '-v') las líneas que empiezan con '#' y son parte del encabezamiento y no hay features en el encabezamiento. Y por último usamos el comando 'uniq' que nos dará sólo una copia por de cada uno de los valores de la columna, y uso el flag -c para que me diga el número de copias.
 
```
nguerrero@cpg3:~$ cut -f 3 /home/nguerrero/gtfs/Drosophila_melanogaster.BDGP6.28.102.gtf | sort | grep -v "^#" | uniq -c
 161508 CDS
      4 Selenocysteine
 188169 exon
  46462 five_prime_utr
  17807 gene
  30595 start_codon
  30590 stop_codon
  33453 three_prime_utr
  34920 transcript
```
Para el archivo Homo_sapiens.GRCh38.102.gtf.gz, uso el mismo pipeline colocando zcat al principio. Y me aparece el resultado, pero el cursor está a la espera de algo con ejecuto el pipeline, pero funciona cuando lo cierre con CTRL+ D, me imagino que se debe al funcionamiento zcat
```
nguerrero@cpg3:~/gtfs$ zcat Homo_sapiens.GRCh38.102.gtf.gz | cut -f3 | sort | grep -v "^#" | uniq -c
# Debo pulsar CTRL + D
 789946 CDS
    117 Selenocysteine
1429266 exon
 157872 five_prime_utr
  60675 gene
  90054 start_codon
  82918 stop_codon
 167723 three_prime_utr
 232024 transcript
 ```
 
### Comentario: simplemente faltaría emplear `wc -l` para que cuente las líneas y nos muestre el número de freatures diferentes que hay en cada archivo. Por lo demás todo correcto, muy bien :)
### Nota: 2.4/2.5

## Ejercicio 3

Recuerdas `covid-samples.fasta`? Localízalo en tu HOME, y extrae, usando un pipeline, los nombres de las secuencias contenidas en este fichero. Luego saca la primera palabra de cada una, ordénalas y guárdalas en un fichero `covid-seq-names.txt`.

### Respuesta ejercicio 3

Primero buscamos el archivo, para tener la ruta del archivo

```
ccalvo@cpg3:/home$ find /home -name "covid-samples.fasta" 2> /dev/null
/home/gtfs/covid-samples.fasta
```
Se trata de un archivo fasta, estos tipo de formate tiene una primera línea que comienza por '>' que es el encabezado con el nombre y otras informaciones sobre la secuencia y después de un ENTER aparece la secuencia, como vamos a trabajar con los encabezados en este ejercicios examinemos uno a modo de ejemplo para conocer la estructura (copiado de nano):

```
>MW186669.1 |Severe acute respiratory syndrome coronavirus 2 isolate SARS-CoV-2/human/EGY/Cairo-sample 19 MOH/2020, complete genome
```
Observamos que comienza por una valor que consta de dos letras seguido de un número con un decimal y tras él un espacio y la misma barra que usamos para crear el pipeline. Parece que eso puede ser un nombre o clave primaria de una base de datos. Y aparecería separado del resto de información del encabezado por "|".

Creo un pipeline comenzando con el comando 'grep' que va a tomar sólo las líneas del encabezado (que comienzan por > ) gracias a que le adicionamos "^>". El resultado de eso le diremos que extraiga con el comando 'cut' el primer campo '-f1', estando los campos separados por"|", lo que nos deja sólo con las claves de los nombres y por último le pedimos que nos lo ordene y guardé en un archivo, que visualizamos con 'more' para verificar.
```
nguerrero@cpg3:~$ grep "^>" /home/gtfs/covid-samples.fasta |cut -f1 -d "|" | sort > covid-seq-names.txt
nguerrero@cpg3:~$ more covid-seq-names.txt 
>MW181431.1 
>MW186669.1 
>MW186829.1 
>MW186830.1 
```

### Comentario: simplemente añadir que os ha faltado sacar toda la línea que empieza por > ya que ese es el nombre de la secuencia, sin embargo en la solución pusisteis que si solo ponemos `grep ">"` nos saca toda la línea, a sí que todo correcto, muy bien :)
### Nota: 2.5/2.5

## Ejercicio 4

Encuentra, usando una sola línea, el número de usuarias diferentes que tienen al menos una carpeta a su nombre en el '/home' de CPG3.

### Respuesta ejercicio 4

Usó el siguente pipeline que me da el valor de 35:
```
nguerrero@cpg3:/home$ ls -l | sort | grep  -v "^total"  | cut -c 15-33 | uniq | wc -l
35
```
Primero le pedimos que nos muestre el contenido del directorio home en formato lista **ls -l |** , después que lo ordene para poder usar el comando para mostrar una sola copia ' uniq -c' **sort |**, como Ls -l nos da el número total de elementos en una línea adicional lo contaría como un usuario adicional en  la cuenta, por lo que lo eliminamos con el comando 'grep'  **grep  -v "^total"  |**, y ahora tomamos de cada fila los caracteres que van desde las posiciones 15 a la 33 que coincide con los nombres de usuario **cut -c 15-33 |**, le decimos que muestre sólo una línea y elimine posibles copias **uniq |** y que cuente el número de líneas **wc -l**

.# Esta linea tiene la desventaja de que si se adicionan varios nuevos usuarios con más 19 de caracteres con los finales diferentes, al cortar del modo que lo hice los interpretaría como la misma línea y tendríamos un resultado final de la cuenta erróneo.

### Comentario: Todo correcto, muy bien :)
### Nota: 2.5/2.5

### Nota total: 9.7
