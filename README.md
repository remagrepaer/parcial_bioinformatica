# parcial_bioinformatica

# Punto 1

## Procedimiento:

Fui a la página del **NCBI**, seleccioné **Nucleotid** y pegué cada uno de los números de accesos que aparecían en la tabla.

Usé el código:

``` cp /mnt/c/Users/Admin/Downloads/seqpar*```

para pasar los archivos de mi PC local a Ubuntu.

Luego de esto los concatené de la siguiente forma:

```cat *.fasta > sequences.fasta```

y los moví al cluster de la siguiente manera:

```scp -i bio.pt.pem -P 53841 *.fasta bio.pt@loginpub-hpc.urosario.ed
u.co:/home/bio.pt/data/Parcial1/parcial1_DavidLeonardoPrietoNino/```

(Esto lo hice para pasar no sólo el archivo concatenado, sino los otros archivos fasta que había descargado del NCBI por si los necesitaba)
