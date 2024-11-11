## B. ANÁLISIS DE CALIDAD DE DATOS CON `NanoPlot`
NanoPlot es un programa diseñado para producir gráficas y resúmenes de los resultados de experimentos de secuenciación producidos por ONT. El programa utiliza como entrada una serie de archivos fastq (usando la opción `--fastq`), o un archivo de texto producido por ONT (por defecto llamado "sequencing_summary.txt"). Al analizar archivos fastq directamente, se pueden producir gráficas con información más detallada.

>[!IMPORTANT]
>En este tutorial, trabajaremos con el archivo de texto `sequencing_summary_FAT98192_deec7cb2_ec30cd82.txt.gz`. Este archivo se encuentra disponible en `/home/oscarp/ONT_IBUNAM_2024/NGSdat/NanoPlot` y debe ser copiado en su directorio local.

```bash
NanoPlot --summary sequencing_summary.txt --loglength --o summary-plots-log-transformed
```

Como resultado, el programa creará un gran número de gráficas (archivos .png) y una compilación en formato html. Estos archivos se colocarán en una carpeta llamada "summary-plots-log-transformed" (disponible [aquí](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/NanoPlot/summary-plots-log-transformed.zip), carpeta comprimida).

Un ejemplo de la salida es la siguiente gráfica, obtenida de un experimento de secuenciación previo, usando un flowcell+química R9 (obsoleto):

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/LengthvsQualityScatterPlot_dot.png" alt="Longitud vs Calidad de secuencia"/>
</p>

Este gráfico indica cómo la longitud de la secuencia de ADN producida por ONT se relaciona con su calidad (expresada en valores similares a [Phred](https://en.wikipedia.org/wiki/Phred_quality_score)). Curiosamente, muchas secuencias tienen valores por debajo de Phred 10 (es decir, probabilidad de 1 en 10 nucleótidos de ser llamados incorrectamente). Pero, ¿cómo se compara esto con los kits v14 más nuevos y los flowcells R10? (Lee más sobre la precisión de ONT [aquí](https://nanoporetech.com/accuracy)). Estudios recientes sugieren que los flowcells R10 en combinación con los kits de secuenciación v14 superan a las versiones más antiguas de flowcells y kits.

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/Ni_al_2023_CompStrBioJ.png" alt="Longitud vs Calidad de secuencia"/>
</p>

(tomado de Ni et al 2023, Comp. Str. Biotech. J.)
>**Fig. 1.** La calidad de lectura de nanopore a partir de la secuenciación WGS y WGA de célula única usando los flowcells R9.4.1 y R10.4. Las lecturas R10.4 de las bibliotecas de secuenciación WGS y scWGA superaron a las lecturas R9.4.1 en cuanto a la precisión estimada por el basecaller original de Guppy (gris) y la observada por mapeo de lectura (blanco) con medias y cuartiles. La dispersión del diagrama de caja también muestra que la precisión de lectura observada es mayor que la estimada. B El gráfico de distribución de densidad indica que las lecturas R10.4 tenían una mayor precisión modal de lectura que las R9.4.1 tanto en las estimadas (superior) como en las observadas (inferior). C El R10.4 tuvo una mayor tasa de detección de precisión promedio en homopolímeros que iban de 4 a 9 bp que el R9.4.1 tanto para las bibliotecas WGS como scWGA. También puede identificar la preferencia por adenina (A) y timina (T) sobre citosina (C) y guanina (G) en la detección de homopolímeros de lecturas de nanopore.

Aquí hay un gráfico de longitud vs calidad de secuencia derivado del experimento de secuenciación "Art_altilis_DRY_WGS17".

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/LengthvsQualityScatterPlot_dot_Art_altilis_DRY_WGS17.png" alt="Longitud vs Calidad de secuencia"/>
</p>

Las secuencias de baja calidad siempre ocurrirán, independientemente de la tecnología de secuenciación. Por cierto, hay ciertas regiones del genoma que son más difíciles de secuenciar ([regiones de baja complejidad](https://academic.oup.com/nar/article/32/suppl_2/W628/1040725)), y ONT tiende a producir datos de baja calidad de secuencia en estas regiones (es decir, mayor probabilidad de bases mal llamadas, ver sección de **literatura recomendada**) si tales regiones están asociadas con un alto contenido de GC:

<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/GC_qual_bias_ONT_Delahaye_Nicolas_2021_PLoSONE.png" alt="Contenido de GC y sesgo de calidad en datos de ONT"/>
</p>

(tomado de Delahaye & Nicolas 2021, PLoSONE)
>**Figura 4**. Tasas de error globales de las lecturas según su contenido de GC. Los contenidos de GC se agruparon por valor entero. Solo se representaron los valores respaldados por al menos *n* lecturas. Para cada especie se calcula la tasa de error media y se muestra la mediana de todos estos valores. A: Resultados para datos bacterianos, n = 100. Las especies con bajo contenido de GC tienen tasas de error globales más bajas que las de alto GC.

Además de la salida gráfica, `NanoPlot` genera un informe conciso sobre la secuencia general, donde se proporcionan valores como las longitudes de secuencia promedio y mediana, calidad, número de lecturas, etc. Este archivo generalmente se llama ``NanoStats.txt`` y se ve así:

```
Resumen general:         
Canales activos:                    488.0
Longitud media de lectura:         1,363.1
Calidad media de lectura:            13.0
Longitud mediana de lectura:         783.0
Calidad mediana de lectura:          13.8
Número de lecturas:              4,488,678.0
Longitud de lectura N50:           2,330.0
STDEV longitud de lectura:         1,727.3
Bases totales:              6,118,383,425.0
Número, porcentaje y megabases de lecturas por encima de los umbrales de calidad
>Q5:  4196018 (93.5%) 5827.1Mb
>Q7:  3944100 (87.9%) 5526.7Mb
>Q10: 3515763 (78.3%) 5019.4Mb
>Q12: 2986773 (66.5%) 4403.6Mb
>Q15: 1635361 (36.4%) 2634.
