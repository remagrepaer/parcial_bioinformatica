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
