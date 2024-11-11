## D.1. ANÁLISIS DE DATOS ONT: BLAST

En esta última sección, demostraremos aplicaciones reales de los datos ONT en genómica: _A_) consultar secuencias (lecturas ONT) contra una referencia y _B_) ensamblar genomas preliminares.

Quizás una de las aplicaciones más utilizadas de los datos ONT es la identificación de organismos mediante métodos de alineación, por ejemplo, usando [BLAST](https://www.ncbi.nlm.nih.gov/books/NBK279690/). BLAST toma como entrada secuencias de nucleótidos o aminoácidos y las alinea contra una base de datos de secuencias. Esta base de datos debe crearse mediante una de las aplicaciones de BLAST, `makeblastdb`. Luego, el alineamiento (búsqueda) se realiza (dependiendo de los datos de entrada) con `blastn`, `blastp` o `magicblast`.

### ACTIVIDAD:
Para averiguar qué proporción de los datos ONT corresponde a ADN de _Artocarpus_, en el siguiente ejercicio, consultaremos nuestras lecturas ONT contra el genoma nuclear de referencia de _Artocarpus heterophyllus_. **NB: Los archivos necesarios para ejecutar esta sección del taller están disponibles en la carpeta [NGSdat](https://github.com/siriusb-nox/Taller-Oxford-Nanopore-Dec-2022/tree/main/NGSdat) de este repositorio.**

Para ello, debes:

1. Descargar el genoma de referencia de _A. heterophyllus_ desde la página web de NCBI (disponible [aquí:](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_025403435.1/)).
2. Construir localmente una base de datos BLAST a partir de este genoma de referencia. El comando básico es:

```bash
makeblastdb -in refgenomes.fasta -out refDB.out -parse_seqids -dbtype nucl
```

donde:

```bash
-in # tu genoma de referencia de entrada (formato fasta)
-out # el nombre de tu base de datos de salida
-parse_seqids # conservar los IDs originales de las secuencias en el genoma de referencia
-dbtype # especificar el tipo de base de datos a construir (nucleótido, proteína)
```

3.a. Alinear las lecturas ONT contra la base de datos BLAST local creada en el paso 2, usando `magicblast` (a diferencia de `blastn`, este programa puede usar archivos de entrada en formato fastq). Un ejemplo de comando es el siguiente:

```bash
magicblast -query input_ONT.fastq -db refDB.out -out blast.search.out -outfmt tabular -no_unaligned -infmt fastq
```

donde:

```bash
-query # tus lecturas ONT de entrada (formato fastq)
-db # tu base de datos BLAST (creada en el paso 2)
-out # nombre de tu archivo de salida
-outfmt # formato de salida (tabla, archivo SAM)
-infmt # tipo de entrada (fastq, fasta)
```

La salida debe ser un archivo delimitado por tabulaciones y se verá así:

```bash
# MAGICBLAST 1.7.2
# magicblast -query FAT98192_pass_deec7cb2_ec30cd82_9.fastq -db ../../../blastDB/Art_altilis_CP_NC_059002.1.blastdb -out /Users/o.perez-escobar/Documents/JORMUNGANDR/Projects/ONT_DarwinBogor_2023/GitHub/ONT_JAVA_2023/ONT-workshop-Oct-2023/NGSdat/magicblast_out/FAT98192_pass_deec7cb2_ec30cd82_9.out -outfmt tabular -no_unaligned -infmt fastq
# Fields: query acc.    reference acc.  % identity      not used        not used        not used        query start     query end       reference start reference end   not used        not used        score   query strand reference strand        query length    BTOP    num placements  not used        compartment     left overhang   right overhang  mate reference  mate ref. start composite score
68dc22d3-44d6-593f-9dd1-e55f86169c77    NC_059002.1     99.2769 0       0       0       35      1414    118851  120231  0       99      1333    plus    plus    1415    82-T-A339A-711AG2AGTC3-T64TAC-168CT4    1            -       1:0     GCAATACGTAACTGAACCAAGTACAGGCAA  T       -       -       1333
8e298d68-42ba-4ee1-9641-c39f4bc490a1    NC_059002.1     74.7208 0       0       0       28      1004    55340   54373   0       99      572     plus    minus   13962   61G-26GA1TG2-A2GA27C-G-1TA2CA26GC3C-35A-21-A16-T-A8T-T-G-1CA18AG2CAAG1-A7AT1T-18_216_%210%106CT62-G87-A31CAAG126C-T-19-T26   1       -       1:1     AGCAATACGTAACTGAACGAAGCCACA     CCTCGTGTCCAAAGTATGAAGATTTCCCTA  -       -       572
```

**Intenta ahora programar un bucle _for_ para realizar BLAST en todos los archivos en una sola línea de código. Luego, usando la salida de `magicblast`, intenta averiguar cuántas secuencias fastq coinciden con el genoma de _A. heterophyllus_!** Podrías usar `awk`, `sort`, `cut`, `sed` y/o `wc` para esta tarea.

3.b. Alinear las lecturas ONT utilizando los servicios remotos de NCBI. Para esto, necesitamos depender de la herramienta BLAST de NCBI, ya que es la única capaz de buscar en múltiples bases de datos. Las lecturas en formato fastq deben convertirse primero a formato fasta. Para hacerlo, prueba el siguiente comando:

```bash
cat input.fastq | paste - - - - | cut -f 1,2 | sed 's/@/>/g' | tr -s "/t" "/n" > output.fasta
```

>[!NOTE]
>**En este taller, trabajaremos con un subconjunto de lecturas fasta derivadas de nuestros datos de secuenciación del experimento de secuenciación WGS17 de _A. altilis_.** El archivo se llama `A_altilis_CP.unitigs.fasta` y esta disponible en la carpeta `/home/oscarp/ONT_IBUNAM_2024/NGSdat`.

Intentaremos averiguar cuántos datos de lectura ONT coinciden con la búsqueda remota de NCBI contra la base de datos "nu". Aquí, es importante tener acceso a una cuenta de NCBI y una clave API. Usar una clave API permitirá un mayor número de búsquedas de secuencias por segundo en las bases de datos en línea. Para introducir tu clave API en PATH, usa:

```bash
export NCBI_API_KEY=09ddca88b438c887b83f8d58fcc890c321XX
```

Para ejecutar BLAST en modo remoto, utiliza:

```bash
blastn -query FAT98192_pass_deec7cb2_ec30cd82_9.fasta -db nt -remote -task blastn-short -evalue 0.01 -entrez_query "Artocarpus [organism]" -outfmt 6 -out blast_result_misteriousplant.table -max_target_seqs 10 -max_hsps 5
```

donde:

```bash
-query # tus lecturas ONT de entrada (formato fasta)
-remote # realizar una búsqueda en línea de forma remota
-task # optimizar para un análisis particular (aquí datos de lecturas cortas, es más rápido)
-evalue # valor para excluir coincidencias de secuencias pobres
-db # tu base de datos BLAST
-out # nombre de tu archivo de salida
-outfmt # formato de salida (tabla)
-max_target_seqs # número de secuencias alineadas a mantener.
-max_hsps # número máximo de HSPs (alineamientos) a mantener por cada par consulta-sujeto.
```

## D.1. ANÁLISIS DE DATOS ONT: ANOTACIÓN DEL GENOMA DEL PLÁSTIDO
La anotación del genoma es el proceso de obtener funciones y estructuras de un genoma. Es un paso esencial para completar un ensamblaje de genoma, ya que los ensamblajes no contienen dicha información.

Una de las herramientas más populares para la anotación de genomas de orgánulos es `GetSeq` del [kit de herramientas CHLOROBOX](https://chlorobox.mpimp-golm.mpg.de). La anotación se realiza comparando un ensamblaje de genoma enviado con aquellos disponibles en la base de datos de NCBI y curados por `GetSeq`. En esta última sección del taller, aprenderemos cómo anotar nuestro propio ensamblaje del genoma del plástido utilizando `GetSeq`.

### ACTIVIDAD
1. Observa detenidamente los andamios ensamblados producidos por `canu` (debería ser un archivo llamado `prefix.unitigs.fasta`, por ejemplo, `A_altilis_CP.unitigs.fasta`). **Averigua cuántos andamios hay.** ¿Hay algún andamio que coincida con el tamaño conocido de un plastoma? (pista: usa `grep` para esta tarea).
2. Intenta extraer de este archivo la secuencia que coincida en tamaño con el tamaño de un plastoma regular (~160,000 pb). Para ello, primero querrás convertir tu archivo fasta de formato intercalado (secuencia de ADN en múltiples líneas) a formato secuencial (secuencia de ADN en una sola línea). Utiliza el siguiente comando para hacerlo.

```bash
awk '{if(NR==1) {print $0} else {if($0 ~ /^>/) {print "\n"$0} else {printf $0}}}' input.interleaved.fasta > output.singleline.fasta
```

3. Esta secuencia está lista para ser cargada en la página web de [GetSeq](https://chlorobox.mpimp-golm.mpg.de/geseq.html).

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/getseq_chlorobox_screenshot.png" alt="Captura de pantalla de la página web de GetSeq"/>
</p>
