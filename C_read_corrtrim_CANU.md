## C.1. CORRECCIÓN Y FILTRADO DE DATOS ONT

Existen varios programas que pueden operar con lecturas de ONT. En este taller, utilizaremos `canu`, que es un programa diseñado para realizar corrección de lecturas ONT, filtrado (eliminación de bases o fragmentos de secuencias de baja calidad) y ensamblaje de andamios. El programa **requiere muchos recursos para su ejecución** (esto depende del tamaño de los genomas, su complejidad y la cantidad inicial de datos utilizada para su ejecución). **El programa recomienda comenzar la ejecución con una cantidad de datos que represente entre 20x-60x+ de cobertura teórica (por ejemplo, Sun et al., 2021)**. El programa puede operar con menores cantidades de datos de entrada, pero a expensas de tasas de error más altas (mayor número de bases mal llamadas en la lectura después del análisis de corrección).

*Extracto de Sun et al., 2021*  
>Los siguientes ensambladores se usaron para la evaluación comparativa, incluidos los ensambladores solo de lecturas largas (Canu [12], Flye [13], Wtdbg2 [14], Miniasm [15], NextDenovo (https://github.com/Nextomics/NextDenovo), NECAT [6], Raven [16] y Shasta [7]) y ensambladores híbridos (MaSuRCA [17] y QuickMerge [18]). **Canu no se probó en el genoma de M. coruscus debido al tiempo de cómputo extremadamente intensivo requerido para este genoma grande**. Análisis previos han sugerido que el uso de lecturas ONT corregidas podría mejorar el ensamblaje del genoma [19]. Para verificar el efecto que esto tiene en los ensamblajes, se usaron lecturas ONT que fueron corregidas y/o recortadas por Canu.

`canu` es un programa modular, lo que significa que cada modo (_correction_, _trim_, _assembly_) se puede ejecutar por separado. Esto es recomendable ya que en algunas instancias el programa puede fallar debido a la falta de recursos, especialmente al ensamblar genomas complejos y al usar grandes cantidades de datos de entrada (por ejemplo, genomas nucleares de más de 1 Gb). En este caso, se recomienda usar `canu` para correcciones y filtrado, y otros programas para ensamblar datos corregidos y filtrados (por ejemplo, SMARTdenovo).

En esta sección del taller, corregiremos, recortaremos y ensamblaremos un pequeño subconjunto del conjunto de datos de _Artocarpus altilis_ secuenciado a partir de una muestra recolectada en un ambiente seco (WGS17). ¡Nuestro objetivo será ensamblar el genoma del plástido de _A. altilis_!

>[!IMPORTANT]
>Los datos necesarios para ejecutar esta sección estan disponibles en el directorio `/home/oscarp/ONT_IBUNAM_2024/NGSdat/canu_fastq`. El archivo se llama `FAT98192_pass_deec7cb2_ec30cd82_0_40.dir.tar.gz` y debe estar disponible en su directorio local.

Para ejecutar el programa en modo de corrección, utiliza:

```bash
canu -correct genomeSize=160k -nanopore-raw *.fastq -p canu_corr -d ../canu_corr/
```

Donde:
```bash
genomeSize # indica el tamaño del genoma del organismo a analizar (en Mb, Gb o Kb)
-nanopore-raw # indica qué tipo de entradas se están utilizando
-p # prefijo de los archivos de salida
-d # directorio de salida
```

Usaremos aquí un tamaño de genoma de 160,000 pb porque es el tamaño estándar de un genoma de plástido (además, un genoma de plástido de referencia de A. altilis, disponible en [GenBank](https://www.ncbi.nlm.nih.gov/nucleotide/NC_059002.1), indica que tiene 160,184 pb).

Este comando generará lecturas que han sido corregidas. La corrección ocurre construyendo gráficos de superposición (ver Fig. 2 de este [artículo](https://genome.cshlp.org/content/27/5/722/F2.expansion.html)). Este comando también emitirá un informe que se ve así:

```bash
[CORRECTION/LAYOUT]
--                             original      original
--                            raw reads     raw reads
--   category                w/overlaps  w/o/overlaps
--   -------------------- ------------- -------------
--   Number of Reads               6030           544
--   Number of Bases           33156409       1831339
--   Coverage                   207.228        11.446
--   Median                        4152          2563
--   Mean                          5498          3366
--   N50                           6467          4381
--   Minimum                       1001             0
--   Maximum                      62276         41616
--   
--                                        --------corrected---------  ----------rescued----------
--                             evidence                     expected                     expected
--   category                     reads            raw     corrected            raw     corrected
--   -------------------- -------------  ------------- -------------  ------------- -------------
--   Number of Reads               6026            491           491            133           133
--   Number of Bases           31502614        6839053       6400196        1090108        291738
--   Coverage                   196.891         42.744        40.001          6.813         1.823
--   Median                        4036          11603         11237           5348          1914
--   Mean                          5227          13928         13035           8196          2193
--   N50                           6056          13582         12560          12933          2395
--   Minimum                       1001           8831          8794           1523          1036
--   Maximum                      56138          56138         38973          33858          6024
--   
--                        --------uncorrected--------
--                                           expected
--   category                       raw     corrected
--   -------------------- ------------- -------------
--   Number of Reads               5950          5950
--   Number of Bases           27058587      14425376
--   Coverage                   169.116        90.159
--   Median                        3787          1928
--   Mean                          4547          2424
--   N50                           5166          4992
--   Minimum                          0             0
--   Maximum                      62276         48522
--   
--   Maximum Memory          1083529106
```

Para ejecutar canu en modo de filtrado (_trim_), utiliza:

```bash
canu -trim genomeSize=160k -nanopore-corrected canu_corr.correctedReads.fasta.gz -p canu_trim -d ../canu_trim/
```

Donde:
```bash
-nanopore-corrected # indica qué tipo de entradas se están utilizando
-p # prefijo de los archivos de salida
-d # directorio de salida
```

Este comando generará lecturas que han sido recortadas.

## C.2. ANÁLISIS DE DATOS ONT: ENSAMBLAJE DEL GENOMA COMPLETO (WGA)

Finalmente, ensamblar genomas con datos ONT corregidos puede ser más o menos sencillo dependiendo de la complejidad del genoma (niveles de ploidía, proporción de elementos repetitivos).

Para ensamblar el genoma del plástido usando `canu` y las lecturas recortadas, intenta el siguiente comando:

```bash
canu -assemble -p A_altilis_CP genomeSize=160k correctedErrorRate=0.14 -nanopore-corrected canu_trim.trimmedReads.fasta.gz -d ../canu_ass/
```

Donde:
```bash
-nanopore-corrected # indica qué tipo de entradas se están utilizando
correctedErrorRate=0.14 # indica la tasa de error de superposición. El valor aquí es adecuado para lecturas de nanopore corregidas.
-d # directorio de salida
```

En algunas instancias, usar `canu` para ensamblaje de genomas podría ser prohibitivo cuando se utilizan grandes cantidades de datos de entrada o se ensamblan genomas complejos y grandes. En tal caso, una buena alternativa para realizar el ensamblaje del genoma es `SMARTdenovo`. Este programa funciona considerablemente más rápido que `canu`, con un [rendimiento comparable](https://gigabytejournal.com/articles/15). Requiere como datos de entrada lecturas ONT sin procesar o corregidas.

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/Liu_al_2023_Gigabyte_SMARTdenovo.png" alt="Rendimiento de SMARTdenovo"/>
</p>

Para ejecutar `SMARTdenovo`, utiliza el siguiente comando.

```bash
smartdenovo.pl -c 1 -t 64 -p prefix_output *fastq.fastq > prefix_output.mak

make -f *.mak
```

Donde:
```bash
-p # prefijo de salida 
-t # número de hilos (por defecto es 8)
-k # longitud de k-mer para la superposición (por defecto es 16)
-J # longitud mínima de lectura (por defecto es 5000)
-c # generar secuencia de consenso
```

### ACTIVIDAD
1. Intenta ensamblar un borrador del genoma a partir de las lecturas recortadas de `canu`, usando SMARTdenovo. ¿Qué tan diferente es el ensamblaje resultante del genoma en comparación con el producido por `canu`?

## C.3. ANÁLISIS DE DATOS ONT: PULIDO DEL GENOMA
Cuando se ensamblan genomas con datos ONT, es extremadamente importante garantizar que los andamios sean lo más precisos posible. Aquí, las bases secuenciadas erróneamente a partir de datos ONT podrían haber permanecido y podrían impedir una mejor contigüedad en el ensamblaje. Una forma de mejorar la precisión es mediante el "pulido" del genoma, utilizando datos de secuenciación de lectura corta producidos del **mismo individuo exacto**. Esto se realiza en dos pasos.

1. Mapeo de los datos de secuenciación de Illumina contra el ensamblaje del genoma, utilizando alineadores de lectura, como `minimap2`. Este programa genera un archivo de índice con coordenadas de las lecturas mapeadas contra el ensamblaje del genoma. Aquí, cada lectura tiene una puntuación de calidad de mapeo e información sobre cuándo las bases de las lecturas cortas no coinciden con la referencia. La salida de este comando es un archivo _SAM_ (lee más sobre archivos SAM [aquí](https://en.wikipedia.org/wiki/SAM_(file_format))). **En este taller, no ejecutaremos estos comandos porque carecemos de los datos de secuenciación de Illumina**. Para ejecutar `minimap2`, utiliza:

```bash
minimap2 -ax sr -t 64 ../genome.assembly.fasta input_illumina_read_files_R1_001.fastq input_illumina_read_files_R2_002.fastq > output_minimap.sam
```

Donde:
```bash
-ax # mapeo de lecturas cortas.
-t # número de hilos
```

2. Corrige los andamios utilizando el archivo SAM producido por `minimap2` y las lecturas filtradas y corregidas de ONT (producidas por `canu`). Utilizamos `racon` para este propósito.

```bash
racon -t 64 corrected.trimmed.canu.reads.fasta output_minimap.sam genome.assembly.smartdenovo.fasta
```

Donde:
```bash
-t # número de hilos
```

# Literatura recomendada
1. Koren S, Walenz BP, Berlin K, Miller J, Bergman NH, Phillippy A. (2017) Canu: scalable and accurate long-read assembly via adaptive k-mer weighting and repeat separation. Genome Res. 27: 722-736
2. Sun J, Li R, Chen C, Sigwart JD, Kocot KM. (2021) Benchmarking ONT read assemblers for high-quality molluscan genomes. Proc. B Phil Trans. https://doi.org/10.1098/rstb.2020.0160
3. Liu H, Wu S, Li A, Ruan J. (2021). SMARTdenovo: a de-novo assembler using long noisy reads. Gigabyte. https://gigabytejournal.com/articles/15
4. Vaser R, Sovic I, Nagarajan N, Sikic M. (2017). Fast and accurate de novo genome assembly from long uncorrected reads. Genome Res. https://genome.cshlp.org/content/early/2017/01/18/gr.214270.116
5. Li H. (2018). Minimap2: pairwise alignment for nucleotide sequences. Bioinformatics, 34:3094-3100. https://academic.oup.com/bioinformatics/article/34/18/3094/4994778?login=true

