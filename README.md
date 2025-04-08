# parcial_bioinformatica

# Punto 1

## Procedimiento:

### 1

Fui a la página del **NCBI**, seleccioné **Nucleotid** y pegué cada uno de los números de accesos que aparecían en la tabla.

Usé el código:

```cp /mnt/c/Users/Admin/Downloads/seqpar*```

para pasar los archivos de mi PC local a Ubuntu.

Luego de esto los concatené de la siguiente forma:

```cat *.fasta > sequences.fasta```

y los moví al cluster de la siguiente manera:

```
scp -i bio.pt.pem -P 53841 *.fasta bio.pt@loginpub-hpc.urosario.ed
u.co:/home/bio.pt/data/Parcial1/parcial1_DavidLeonardoPrietoNino/
```
Esto lo hice para pasar no sólo el archivo concatenado, sino los otros archivos fasta que había descargado del **NCBI** por si los necesitaba

### 2

Para cambiar los nombres usé el siguiente código:

```
sed -E 's/^(>\w+\.\w) (\w+) (\w+).*/\1_\2_\3/' sequences.fasta > secuencias.fasta
```

La expliación es: usé sed para cambiar los nombres dentro del archivo, el -E es para activar las expresiones regulares, con 's/^ le digo cómo inicia la línea que quiero encontrar, con (>\w+\.\w)
agrupo los primeros caracteres (por ejemplo >AB201258.1), dejo un espacio por el espacio que hay en los nombres, y los siguientes grupos que son (\w+) es para agrupar las otras partes del nombre,
siempre dejando el espacio por el espacio que hay entre estos. Luego, pongo .* para que se seleccione el resto de la línea sin importar que tenga sin agarrar los nucleótidos de las secuencias.
Al final digo en que archivo está (sequences.fasta) y señalo el archivo que quiero crear para guardar estas modificaciones (> secuencias.fasta)

### 4

Primero generé la base de datos de la siguiente forma:

```
salloc
module load blast/2.7.1
makeblastdb -in secuencias.fasta -dbtype nucl -parse_seqids -out blastsec -title "Misticetos"
```

Luego realicé un Blast con el querry:

```
blastn -query query.fasta -task megablast -db blastsec -outfmt 7 -word_size 7 -out blastballenas -num_threads 1
```
### 5

Según lo obtenido, la mejor identificación fue de AP006472.1_Balaena_mysticetus, esto dado que posee un 100% de identidad, tiene una longitud de alineamiento de 13 (de las más altas en las
que tienen 100% de identidad), tiene 0 gaps y 0 mistmatches, su evalue es de 2.3 (es de los más cercanos a 0 de aquellos que tienen 100% de similitud e implica que tiene mayor soporte), 
su bitscore es de 25.1 (de los más altos con la identidad del 100%, lo que indica que tiene un alto soporte) y finalmente sos q. start son de 498 y 510 respectivamente, lo que le da un mayor 
soporte a esta identifiación con respecto a las demás.

# Punto 2

## Procedimiento:

### 7

Se creó el archivo .sh:

```
nano parcial.sh
```

Dentro de este se puso:

```
#!/bin/bash
#SBATCH -p normal
#SBATCH -N 1
#SBATCH -n 10
#SBATCH -t 0-00:20
#SBATCH -o salida.out
#SBATCH -e error.err
#SBATCH --mail-user=davidl.prieto@urosario.edu.co
#SBATCH --mail-type= ALL

muscle -in pun2.fasta -out alineamiento.fasta
```

Y luego se corrió el código:

```
sbatch parcial.sh
```

### 8

El código usado para generar el árbol fue:

```
./FASconCAT-G_v1.05.1.pl -n -p -s
module load iqtree/1.6.12
iqtree -s FcC_supermatrix.fas -m GTR+I+G -bb 1000 -pre ballenas
```

### 9

Las especies son:
>AB201258.1_Balaenoptera_edeni
>AP006469.1_Balaenoptera_brydei
>AP006470.1_Balaenoptera_borealis
>AB201256.1_Balaenoptera_omurai
>X72204.1_Balaenoptera_musculus
>AJ554054.1_Balaenoptera_acutorostrata
>AJ554052.1_Caperea_marginata
>AP006475.1_Caperea_marginata
>AP006472.1_Balaena_mysticetus
>seq1 (que anteriormente había sido identificada como AP006472.1_Balaena_mysticetus)

![Caos](https://github.com/remagrepaer/parcial_bioinformatica/blob/main/Ryjgf6cfdXKJOMm2KaqkEA.png)

Se puede observar que los individuos la especie _Caperea marginata_ forman un grupo monofiletico, lo que lo hace un grupo biológico real.

Sin embargo, la seq1 parece estar más emparentada con la especie _Balaenoptera edeni_, la cuál está incluso más cerca a la especie _Baleenoptera brydei_.

La especie más alejada en termnos de relaciones filogenéticas es la especie _Balaenoptera omurai_.
