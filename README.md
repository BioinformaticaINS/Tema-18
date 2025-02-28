# Tema 18: Obtención, calidad y limpieza de lecturas

## Estructura de la práctica:

1. Instalación de programas
2. Análisis de calidad del secuenciamiento Illumina
3. Limpieza de los archivos FASTQ de Illumina
4. Basecalling de los archivos FAST5
5. Basecalling de los archivos POD5
6. Análisis de calidad del secuenciamiento Nanopore 
7. Limpieza de los archivos FASTQ de Nanopore 

## Programas bioinformáticos:

- dorado : [Pagina web](https://github.com/nanoporetech/dorado) 
- fastqc : [Pagina web](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)
- guppy : [Pagina web](https://community.nanoporetech.com/downloads/)
- nanofilt : [Pagina web](https://github.com/wdecoster/nanofilt)
- nanoplot : [Pagina web](https://github.com/wdecoster/NanoPlot)
- multiqc : [Pagina web](https://github.com/ewels/MultiQC)
- porechop : [Pagina web](https://github.com/rrwick/Porechop)
- trimgalore : [Pagina web](https://github.com/FelixKrueger/TrimGalore)
- trimmomatic : [Pagina web](http://www.usadellab.org/cms/?page=trimmomatic)

## Metodología:

## 1. Instalación de programas

### Instalación de guppy

Guppy es un programa de basecalling y análisis de datos de secuenciación de nanoporos desarrollado por Oxford Nanopore Technologies.

```bash
cd

mkdir genomics

cd genomics

mkdir software

cd software

pip install gdown

gdown https://drive.google.com/uc?id=1eGGqEJUsHj5rzc5S4LGDvhj0ov6lsr-3

sudo dpkg -i ont_guppy_cpu_6.5.7-1~focal_amd64.deb 

guppy_basecaller -h
```

### Instalacion de dorado

Dorado es un programa de basecalling y análisis de datos de secuenciación de nanoporos desarrollado por Oxford Nanopore Technologies.

```bash

wget https://cdn.oxfordnanoportal.com/software/analysis/dorado-0.9.1-linux-x64.tar.gz

tar xvfz dorado-0.9.1-linux-x64.tar.gz

cd dorado-0.9.1-linux-x64

cd bin

sudo ln -s /home/ins_user/software/dorado-0.9.1-linux-x64/bin/dorado /usr/local/bin/dorado

dorado --help
```

### Instalación de POD5

Permite convertir fast5 a pod5 antes del basecalling

> **Comentario:** POD5 es un formato de archivo desarrollado por Oxford Nanopore Technologies para almacenar datos de secuenciación de nanoporos. Es un formato más eficiente y comprimido que FAST5

```bash
pip install pod5 
```

### Instalar los siguientes programas en un ambiente de conda

```bash
conda create -n quality -c bioconda fastqc nanofilt nanoplot multiqc porechop trim-galore trimmomatic seqkit samtools bedtools
```

> **Comentario:** 
> - `conda create`: Este es el comando base para crear un nuevo entorno virtual con conda.
> - `-n quality`: Esta opción especifica el nombre del nuevo entorno virtual, que será "quality". Los entornos virtuales permiten tener diferentes versiones de software instaladas sin que interfieran entre sí.
> - `-c bioconda`: Esta opción le dice a conda que busque los paquetes a instalar en el canal "bioconda". Bioconda es un canal de conda especializado en software bioinformático, lo que garantiza que las herramientas estén optimizadas para análisis de datos biológicos.
> - `fastqc`: Instala FastQC, una herramienta para el control de calidad de datos de secuenciación de alto rendimiento (NGS).
> - `nanofilt`: Instala NanoFilt, una herramienta para filtrar datos de secuenciación de Oxford Nanopore Technologies (ONT) basándose en la longitud y la calidad de las lecturas.
> - `nanoplot`: Instala NanoPlot, una herramienta para visualizar datos de secuenciación de ONT.
> - `multiqc`: Instala MultiQC, una herramienta que agrega los resultados de múltiples herramientas de control de calidad en un único informe HTML.
> - `porechop`: Instala Porechop, una herramienta para recortar adaptadores y detectar lecturas de concatenación en datos de secuenciación de ONT.
> - `trim-galore`: Instala Trim Galore!, un envoltorio (wrapper) que combina Cutadapt y FastQC para recortar adaptadores y realizar control de calidad en datos de secuenciación de alto rendimiento (NGS).
> - `trimmomatic`: Instala Trimmomatic, otra herramienta popular para recortar adaptadores y realizar control de calidad en datos de secuenciación de alto rendimiento (NGS).
> - `seqkit`: Instala seqkit, una caja de herramientas rápida y multiplataforma para manipular archivos de secuencia FASTA/Q.
> - `samtools`: Instala samtools, una suite de herramientas para manipular archivos SAM/BAM (formatos para almacenar datos de alineamiento de secuencias).
> - `bedtools`: Instala bedtools, una suite de utilidades para realizar operaciones con archivos BED (formatos para definir regiones genómicas).

## 2. Obtención de los datos de secuenciación 

```bash
cd ~/genomics

mkdir raw_data

cd raw_data

### Datos de Illumina

gdown https://drive.google.com/uc?id=1hpoWBRzA2OzWbtxtvwqM-3qnauhM1ulk

gdown https://drive.google.com/uc?id=1GUHeRwpfelPAmU5b6dPoiT79iQ-QQYMP

### Datos de Nanopore

gdown https://drive.google.com/uc?id=1FyGJAS33tB-DkbAiL2195ACaWf_qIi3Z

gdown https://drive.google.com/uc?id=1FP6zi0jJmPf6n1ic0Fd3Cdytcf8gY3bk

unzip pod5.zip

unzip fast5.zip 
```

## 3. Analisis de calidad de datos de secuenciación Illumina

```bash
cd ~/genomics

mkdir quality

cd quality

mkdir illumina

cd illumina

conda activate quality

fastqc -t 2 /data/2024_2/genome/illumina/*.fastq.gz -o .
```
> **Comentario:** 
> - `-t 2`: Esta opción especifica el número de hilos (threads) que FastQC debe utilizar. Al usar múltiples hilos, el programa puede procesar los datos más rápidamente. En este caso, se están usando 2 hilos.
> - `/data/2024_2/genome/illumina/*.fastq.gz`: Esta parte del comando indica la ubicación de los archivos que FastQC debe analizar.
> - `*.fastq.gz`: Es un comodín que selecciona todos los archivos en ese directorio que terminan con ".fastq.gz". Esto significa que FastQC analizará todos los archivos FASTQ comprimidos en ese directorio. Los archivos fastq.gz son el formato de archivos donde se guardan las lecturas de las secuencias de ADN.
> - `-o .`: Esta opción define el directorio de salida. El punto "." representa el directorio actual. Esto significa que los informes HTML generados por FastQC se guardarán en el mismo directorio donde se ejecuta el comando.

```bash
multiqc -o raw_illumina .
```
> **Comentario:**
> - `-o raw_illumina`: Esta opción especifica el directorio de salida donde se guardará el informe HTML generado por MultiQC.
> - `.`: Representa el directorio actual. Esto le dice a MultiQC que busque archivos de resultados (los que ha generado FastQC por ejemplo) dentro del directorio en el que estás ejecutando el comando. MultiQC buscará automáticamente archivos de salida de las herramientas de control de calidad compatibles que se encuentren en el directorio actual y sus subdirectorios.

### Limpieza con trim galore

```bash
cd ~/genomics

mkdir trimming

cd trimming

mkdir trim_galore

cd trim_galore

trim_galore --quality 30 --length 50 --phred33 --cores 2 --fastqc --paired /home/ins_user/genomics/raw_data/T4_S1_L001_R1_001.fastq.gz /home/ins_user/genomics/raw_data/T4_S1_L001_R2_001.fastq.gz

multiqc -o trimming_trim_galore .
```

### Limpieza con trimmomatic

```bash
cd ~/genomics/trimming

mkdir trimmomatic

cd trimmomatic

> **Comentario:** Crear el archivo NexteraPE.fa

nano NexteraPE.fa

>PrefixNX/1
AGATGTGTATAAGAGACAG
>PrefixNX/2
AGATGTGTATAAGAGACAG
>Trans1
TCGTCGGCAGCGTCAGATGTGTATAAGAGACAG
>Trans1_rc
CTGTCTCTTATACACATCTGACGCTGCCGACGA
>Trans2
GTCTCGTGGGCTCGGAGATGTGTATAAGAGACAG
>Trans2_rc
CTGTCTCTTATACACATCTCCGAGCCCACGAGAC

trimmomatic PE /home/ins_user/genomics/raw_data/T4_S1_L001_R1_001.fastq.gz /home/ins_user/genomics/raw_data/T4_S1_L001_R2_001.fastq.gz T4_R1.trim.fastq.gz T4_R1.unpaired.fastq.gz T4_R2.trim.fastq.gz T4_R2.unpaired.fastq.gz ILLUMINACLIP:NexteraPE.fa:2:30:10 SLIDINGWINDOW:4:30 MINLEN:50 -threads 2

fastqc -t 2 *.trim.fastq.gz -o .

multiqc -o trimming_trimmomatic .
```

## 4. Basecalling de datos de secuenciación Nanopore

### Basecalling de FAST5

```bash
cd ~/genomics

mkdir basecalling

cd basecalling

guppy_basecaller -i /home/ins_user/genomics/raw_data/fast5 -s fast5 -c dna_r10.4_e8.1_fast.cfg
```

### Basecalling de POD5

#### Obtención de archivos POD5
```bash
cd ~/genomics/basecalling

mkdir pod5

cd pod5

conda deactivate

pod5 convert fast5 /home/ins_user/genomics/raw_data/fast5/*.fast5 --output converted.pod5
```

> **Comentario:** Esta línea de código utiliza `pod5` para convertir todos los archivos FAST5 en el directorio `fast5` a un único archivo POD5 llamado `converted.pod5`

#### Realizamos el llamado de las bases

```bash
dorado basecaller fast --kit-name SQK-NBD114-24 --min-qscore 8 --barcode-both-ends converted.pod5 > calls.bam

dorado demux --kit-name SQK-NBD114-24 --output-dir barcodes --emit-summary calls.bam

cd barcodes
```

> **Comentario:** 
> - `--kit-name SQK-16S024`: Especifica el nombre del kit de secuenciación utilizado.
> - `--min-qscore 8`: Establece un umbral de calidad mínimo de 8 para las lecturas.
> - `--barcode-both-ends`: Indica que los códigos de barras están presentes en ambos extremos de las lecturas.
> - `> calls.bam`: Redirige la salida a un archivo BAM llamado `calls.bam`.
> - `--output-dir barcodes`: Especifica el directorio de salida para los archivos demultiplexados.
> - `--emit-summary`: Genera un resumen de la ejecución de secuenciación.

```
cat barcoding_summary.txt | head

filename	read_id	barcode
converted.pod5	79575555-aded-46c6-9f18-e9556fbd576d	SQK-NBD114-24_barcode15
converted.pod5	00870608-57ee-497a-a738-b31d4876c161	unclassified
converted.pod5	0054a804-cf1f-4218-9877-f490288b5675	unclassified
converted.pod5	0046d92e-d735-4f25-9fb3-6b80ef88a3fd	unclassified
converted.pod5	032006e4-6d5a-4e8e-a1f7-e6f695d732ad	unclassified
converted.pod5	03d484a9-6644-4856-bf4e-ddf071e2a52e	unclassified
converted.pod5	013562b4-3582-4bf5-852d-11297ed2b35f	unclassified
converted.pod5	037de90d-c40a-4ae4-8b7b-887350c6c219	unclassified
converted.pod5	043aa25b-75b0-4363-ab12-3c3fe75dcf89	unclassified
```

#### Resumen de la secuenciación 

```bash
for file in *.bam; do prefix="${file%.bam}"; dorado summary "$file" > "${prefix}_summary.tsv"; done
```

> **Comentario:** Genera un resumen para cada archivo BAM en el directorio y lo guarda en un archivo TSV.

```
cat bcf4b7732185c1a3353d1b4fe80266cd3ac60162_SQK-NBD114-24_barcode13_summary.tsv | head

filename	read_id	run_id	channel	mux	start_time	duration	template_start	template_duration	sequence_length_template	mean_qscore_template	barcode
converted.pod5	05886056-e7c4-4c7f-9f61-a1f025b940a4	bcf4b7732185c1a3353d1b4fe80266cd3ac60162	31	1	9069.31	1.7564	9069.31	1.7564	678	10.5307	SQK-NBD114-24_barcode13
converted.pod5	a87c7559-6f22-463c-b288-a281fa7b0b41	bcf4b7732185c1a3353d1b4fe80266cd3ac60162	66	1	9083.14	0.9576	9083.14	0.9576	219	11.1093	SQK-NBD114-24_barcode13
converted.pod5	44432ea9-99d6-4f46-aba7-82a288b6c9ff	bcf4b7732185c1a3353d1b4fe80266cd3ac60162	132	3	8941.51	1.6908	8941.51	1.6908	621	9.64351	SQK-NBD114-24_barcode13
converted.pod5	af970a9d-239a-4623-80a3-7e26485f66ae	bcf4b7732185c1a3353d1b4fe80266cd3ac60162	328	3	9135.62	3.1966	9135.62	3.1966	1267	12.403	SQK-NBD114-24_barcode13
converted.pod5	9a105f99-ef53-43bb-9e26-321139cd8bad	bcf4b7732185c1a3353d1b4fe80266cd3ac60162	434	4	9016.32	1.05	9016.32	1.05	219	8.39137	SQK-NBD114-24_barcode13
```

### Convertimos de bam a fastq

```bash
for file in *.bam; do prefix="${file%.bam}"; samtools sort -n "$file" -o "${prefix}_sorted.bam"; done
for file in *_sorted.bam; do prefix="${file%.bam}"; bedtools bamtofastq -i "${prefix}.bam" -fq "${prefix}.fastq"; done
seqkit stats -a -j 4 *_sorted.fastq > stats_sorted_fastq.txt
```

> **Comentario:** 
> - `samtools sort -n`: Ordena los archivos BAM por nombre de lectura.
> - `bedtools bamtofastq`: Convierte los archivos BAM ordenados a formato FASTQ.

```bash
file                               format  type  num_seqs      sum_len  min_len  avg_len  max_len       Q1       Q2     Q3  sum_gap    N50  Q20(%)  Q30(%)
SQK-16S024_barcode01_sorted.fastq  FASTQ   DNA      2,232    2,812,246        2    1,260    3,808    1,230    1,405  1,455        0  1,421   50.76    8.48
SQK-16S024_barcode02_sorted.fastq  FASTQ   DNA      1,626    1,903,444        4  1,170.6    1,953      980    1,387  1,452        0  1,413   50.53    8.58
SQK-16S024_barcode03_sorted.fastq  FASTQ   DNA     12,802   15,342,892        7  1,198.5    2,877    1,077    1,394  1,445        0  1,408   50.64    7.79
SQK-16S024_barcode04_sorted.fastq  FASTQ   DNA      8,550    9,958,508        9  1,164.7    2,091    1,029    1,387  1,436        0  1,402    50.3     7.5
SQK-16S024_barcode05_sorted.fastq  FASTQ   DNA        678      871,012       59  1,284.7    1,937    1,333    1,404  1,455        0  1,419   49.97    7.33
SQK-16S024_barcode06_sorted.fastq  FASTQ   DNA      3,192    4,003,420        3  1,254.2    2,250    1,306    1,407  1,452        0  1,421   51.25    7.43
SQK-16S024_barcode07_sorted.fastq  FASTQ   DNA     12,476   14,802,626        4  1,186.5    2,838    1,053    1,395  1,448        0  1,409   50.54    7.45
SQK-16S024_barcode08_sorted.fastq  FASTQ   DNA        312      437,058       28  1,400.8    1,861  1,389.5    1,421  1,454        0  1,424   50.44    8.41
SQK-16S024_barcode09_sorted.fastq  FASTQ   DNA     22,938   18,677,366        2    814.3    1,993      346      873  1,335        0  1,254   51.16    7.87
SQK-16S024_barcode10_sorted.fastq  FASTQ   DNA     17,742   19,764,174        8    1,114    2,654      717    1,308  1,409        0  1,391   50.81    8.19
SQK-16S024_barcode11_sorted.fastq  FASTQ   DNA     13,662   14,534,452        1  1,063.9    2,029      838    1,192  1,384        0  1,337   50.87    7.85
SQK-16S024_barcode12_sorted.fastq  FASTQ   DNA     11,866   13,408,054       10    1,130    2,012      944    1,335  1,399        0  1,377   50.37     7.7
SQK-16S024_barcode13_sorted.fastq  FASTQ   DNA     13,526   15,222,272        1  1,125.4    2,696      914    1,326  1,398        0  1,374   50.65    7.65
SQK-16S024_barcode14_sorted.fastq  FASTQ   DNA     16,278   15,979,112        2    981.6    2,320      504    1,126  1,392        0  1,359   50.59    8.19
SQK-16S024_barcode15_sorted.fastq  FASTQ   DNA     14,378   16,015,288        2  1,113.9    2,440      831    1,338  1,402        0  1,388   50.57    7.33
SQK-16S024_barcode16_sorted.fastq  FASTQ   DNA     14,146   15,581,836        2  1,101.5    2,109      814    1,300  1,400        0  1,379   50.65     7.9
SQK-16S024_barcode17_sorted.fastq  FASTQ   DNA     17,988   18,836,740        2  1,047.2    3,057      624  1,304.5  1,399        0  1,380   50.24    7.07
SQK-16S024_barcode18_sorted.fastq  FASTQ   DNA     16,030   16,605,426        2  1,035.9    2,088      537    1,287  1,399        0  1,386   50.48    7.85
unclassified_sorted.fastq          FASTQ   DNA    474,168  648,009,092        7  1,366.6    4,216    1,393    1,405  1,421        0  1,406   53.45    8.42
```

## 4. Análisis de calidad del secuenciamiento Nanopore (30 minutos)

```bash
$ cd ~/b00_genome/
$ mkdir raw_data
$ cd raw_data
$ cat /data/2024_2/genome/fastq/barcode00/pass/*.fastq > b00_sup.fastq
```

```bash
$ cd ~/b00_genome/
$ mkdir quality
$ cd quality
$ mkdir pycoqc
$ cd pycoqc
$ conda activate nanopore_00
$ pycoQC -f /data/2024_2/genome/fastq/barcode00/sequencing_summary.txt --html_outfile b00_sup_pycoqc.html
```

```bash
$ cd ~/b00_genome/quality/
$ mkdir fastqc
$ cd fastqc
$ conda activate nanopore_01
$ fastqc -t 20 /home/alumnoZZ/b00_genome/raw_data/b00_sup.fastq -o .
```

```bash
$ cd ~/b00_genome/quality/
$ mkdir nanoplot
$ cd nanoplot
$ NanoPlot -t 20 --fastq /home/alumnoZZ/b00_genome/raw_data/b00_sup.fastq -p b00_sup_raw_ -o b00_sup_raw --maxlength 100000000
```

## 5. Limpieza de los archivos FASTQ de Nanopore (80 minutos)

```bash
$ cd ~/b00_genome/
$ mkdir trim
$ cd trim
$ mkdir porechop
$ cd porechop
$ porechop -t 20 -i /home/alumnoZZ/b00_genome/raw_data/b00_sup.fastq -o b00_sup_porechop.fastq.gz
```

```bash
$ cd ~/b00_genome/quality/nanoplot/
$ NanoPlot -t 20 --fastq /home/alumnoZZ/b00_genome/trim/porechop/b00_sup_porechop.fastq.gz -p b00_sup_porechop_ -o b00_sup_porechop --maxlength 100000000
```

```bash
$ cd ~/b00_genome/trim/
$ mkdir nanofilt
$ cd nanofilt
$ gunzip -c /home/alumnoZZ/b00_genome/trim/porechop/b00_sup_porechop.fastq.gz | NanoFilt -q 10 --length 1000 | gzip > b00_sup_nanofilt.fastq.gz
```

```bash
$ cd ~/b00_genome/quality/nanoplot/
$ NanoPlot -t 20 --fastq /home/alumnoZZ/b00_genome/trim/nanofilt/b00_sup_nanofilt.fastq.gz -p b00_sup_nanofilt_ -o b00_sup_nanofilt --maxlength 100000000
```

