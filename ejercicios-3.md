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
Lo desordeno con sort -R


```
nguerrero@nguerrero-VirtualBox:~$ cd genes
nguerrero@nguerrero-VirtualBox:~/genes$ sort -R gene-2.bed > desordenado.bed
nguerrero@nguerrero-VirtualBox:~/genes$ ls
desordenado.bed  gene-1.bed  gene-2.bed
```

```
  GNU nano 5.6.1                   desordenado.bed                             
1       6230133 6230191 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6239952 6240378 GENE00000025907
1       6206227 6206270 GENE00000025907
1       6222341 6228319 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6234229 6234311 GENE00000025907
1       6206197 6206270 GENE00000025907
1       6234229 6234399 GENE00000025907
1       6234229 6234399 GENE00000025907
1       6238262 6238384 GENE00000025907
1       6223599 6223745 GENE00000025907
```

Los ordeno de nuevo  y se ordena em orden ascendente segun los valores de la segunda columna

```
nguerrero@nguerrero-VirtualBox:~/genes$ sort < desordenado.bed > desord1
nguerrero@nguerrero-VirtualBox:~/genes$ ls
desord1  desordenado.bed  gene-1.bed  gene-2.bed
nguerrero@nguerrero-VirtualBox:~/genes$ nano desord1
```

```
    GNU nano 5.6.1                       desord1                                 
1       6206197 6206270 GENE00000025907
1       6206227 6206270 GENE00000025907
1       6222341 6228319 GENE00000025907
1       6223599 6223745 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6227940 6228049 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6229959 6230073 GENE00000025907
1       6230133 6230191 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6233961 6234087 GENE00000025907
1       6234229 6234311 GENE00000025907
1       6234229 6234399 GENE00000025907
1       6234229 6234399 GENE00000025907
1       6238262 6238384 GENE00000025907
1       6239952 6240378 GENE00000025907
```

Y para ordenarlos en orden descendente usamos la funciones reverse r- del manual y aparecen ordenados en orden decreciente según la segunda columna

```
nguerrero@nguerrero-VirtualBox:~/genes$ sort -r gene-2.bed
1	6239952	6240378	GENE00000025907
1	6238262	6238384	GENE00000025907
1	6234229	6234399	GENE00000025907
1	6234229	6234399	GENE00000025907
1	6234229	6234311	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6230133	6230191	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6223599	6223745	GENE00000025907
1	6222341	6228319	GENE00000025907
1	6206227	6206270	GENE00000025907
1	6206197	6206270	GENE00000025907
```

  Para ordenarlos según la tercera columna usamos el comando sort -k
leyendo el manual, dice que se pueden ordenar dependiendo de una `key` que interpreto como una clave, como clave en bases de datos y explica que debemos introducir un numero `n` según el `field`, que leyendo el ejercicio interpreto como columna, al poner entonces `-k3` se ordenan según
la tercera columna en orden creciente.
```
nguerrero@nguerrero-VirtualBox:~/genes$ sort -k3 gene-2.bed 
1	6206197	6206270	GENE00000025907
1	6206227	6206270	GENE00000025907
1	6223599	6223745	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6227940	6228049	GENE00000025907
1	6222341	6228319	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6229959	6230073	GENE00000025907
1	6230133	6230191	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6233961	6234087	GENE00000025907
1	6234229	6234311	GENE00000025907
1	6234229	6234399	GENE00000025907
1	6234229	6234399	GENE00000025907
1	6238262	6238384	GENE00000025907
1	6239952	6240378	GENE00000025907
```

## Ejercicio 2

Cuáles son y cuántos tipos distintos de "features" hay en `Drosophila_melanogaster.BDGP6.28.102.gtf` y en `Homo_sapiens.GRCh38.102.gtf.gz`? Nota: para trabajar con ficheros .gunzip sin descomprimir puedes usar `zcat`.

### Respuesta ejercicio 2
 Para el archivo Drosophila_melanogaster.BDGP6.28.102.gtf
 
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

## Ejercicio 3

Recuerdas `covid-samples.fasta`? Localízalo en tu HOME, y extrae, usando un pipeline, los nombres de las secuencias contenidas en este fichero. Luego saca la primera palabra de cada una, ordénalas y guárdalas en un fichero `covid-seq-names.txt`.

### Respuesta ejercicio 3


## Ejercicio 4

Encuentra, usando una sola línea, el número de usuarias diferentes que tienen al menos una carpeta a su nombre en el '/home' de CPG3.

### Respuesta ejercicio 4





