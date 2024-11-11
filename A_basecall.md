## A. Basecalling desde archivos fast5 usando `guppy` (obsoleto)

`guppy` es uno de los _programas oficiales_ diseñados por Oxford Nanopore para traducir las señales eléctricas contenidas en los archivos FAST5 en nucleótidos (archivos FASTQ). Este programa solo está accesible si tienes una cuenta de cliente con ONT. Este programa ha sido reemplazado por `dorado` (ver sección a continuación).
El basecalling se realiza directamente al secuenciar una biblioteca en el minION (es decir, _basecalling en vivo_), pero este proceso es intensivo en cálculo y tiene algunas ventajas realizar el basecalling después de la secuenciación (por ejemplo, ejecutar modelos de basecalling de alta precisión usando una computadora potente).

Los archivos del instalador de `guppy` vienen en dos versiones: **CPU** (usa los recursos del procesador) y **GPU** (usa los recursos de la tarjeta gráfica - disponible para un tipo limitado de tarjetas, por ejemplo, NVIDIA). La versión GPU es mucho más eficiente que la CPU.

Para ejecutar guppy, puedes ejecutar el siguiente comando:

```bash
ls *.fast5 | guppy_basecaller -s . -c dna_r9.4.1_e8.1_hac.cfg --compress_fastq --trim_adapters -x auto --min_qscore 10
```

Donde:

```bash
-s # especifica el directorio para almacenar los resultados del análisis
-c # indica el modelo para hacer el basecalling (dna_r9.4.1_e8.1_hac.cfg es uno de los modelos más precisos)
-x # habilita la opción para detectar automáticamente qué dispositivo GPU está disponible en la computadora
--min_qscore 10 # excluirá cualquier secuencia de ADN con una puntuación de calidad inferior a 10
```

Una idea de cuántos recursos de GPU/CPU consumen los diferentes modelos de basecalling está disponible [aquí](https://esr-nz.github.io/gpu_basecalling_testing/gpu_benchmarking.html#cfg_files).

Cuando `guppy` ha completado el basecalling con éxito, el programa imprime un mensaje como el siguiente:

```bash
usuario@maquina:/home/selamatpagi/Downloads/software/guppy/ont-guppy/bin/guppy_basecaller -s . -c dna_r9.4.1_e8.1_hac.cfg --compress_fastq --trim_adapters -x auto --min_qscore 10
ONT Guppy basecalling software version 3.1.5+781ed57
config file:        /home/selamatpagi/Downloads/software/guppy/ont-guppy/data/dna_r9.4.1_450bps_hac.cfg
model file:         /home/selamatpagi/Downloads/software/guppy/ont-guppy/data/template_r9.4.1_450bps_hac.jsn
input path:         raw_data/
save path:          MyGenome_basecalled_20190621_hac/
chunk size:         1000
chunks per runner:  1000
records per file:   4000
fastq compression:  ON
num basecallers:    4
gpu device:         cuda:0
kernel path:
runners per device: 2

Found 1215050 fast5 files to process.
Init time: 12755 ms

0%   10   20   30   40   50   60   70   80   90   100%
|----|----|----|----|----|----|----|----|----|----|
***************************************************
Caller time: 12227895 ms, Samples called: 117512178322, samples/s: 9.61017e+06
Finishing up any open output files.
Basecalling completed successfully.
```

Además de los archivos fastq basecalls, `guppy` también produce varios informes, incluida una recopilación de estadísticas de secuenciación en formato html. Un ejemplo se puede ver [aquí:](http://htmlpreview.github.io/?https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/guppy/report_FAV15499_20231010_1636_deec7cb2.html)

*aquí hay una captura de pantalla del informe (tomada del experimento de secuenciación Art_altilis_DRY_WGS17):*
<p align="center">
 <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/guppy_report_example_Art_altilis.png" alt="Una sección de un informe de guppy sobre un experimento de secuenciación"/>
</p>

## Basecalling usando `dorado` (software de acceso abierto)
Este software está reemplazando a `guppy`. `dorado` (disponible [aquí](https://github.com/nanoporetech/dorado)) ofrece una amplia gama de análisis, incluido el basecalling (simple y dúplice), basecalling modificado (detección de bases modificadas) y clasificación de codificación simple (útil al secuenciar múltiples muestras por experimento de secuenciación). Un punto importante es que, a diferencia de guppy, este software es de acceso abierto.

Un requisito de dorado es el uso de archivos de entrada **POD5** (producidos ahora por las más recientes máquinas de secuenciación y químicas, que eventualmente reemplazarán a los archivos *.fast5).

**En este tutorial, trabajaremos con el archivo de texto `FAT98192_pass_deec7cb2_ec30cd82_101.pod5`. Este archivo esta disponible en `/home/oscarp/ONT_IBUNAM_2024/NGSdat/basecall` y debe ser copiado en su directorio local.**

```bash
# ver lista de modelos disponibles
dorado download --list

# descargar el modelo de basecalling elegido
dorado download --model dna_r10.4.1_e8.2_400bps_hac@v4.2.0

# basecall desde pod5
dorado basecaller --emit-fastq dna_r10.4.1_e8.2_400bps_hac@v4.2.0 FAT98192_pass_deec7cb2_ec30cd82_101.pod5 > FAT98192_pass_deec7cb2_ec30cd82_101.fastq
```

Donde:

```bash
--emit-fastq # produce archivos *.fastq como salida
dna_r10.4.1_e8.2_400bps_hac@v4.2.0 # indica el modelo para hacer el basecalling (este es el modelo más preciso)
FAT98192_pass_deec7cb2_ec30cd82_101.pod5 # archivo de entrada
```

**Nota: El comando por defecto no utiliza ningún umbral de filtrado de calidad (es decir, `--min-qscore 0`)**

El programa repetirá el siguiente mensaje:
```bash
[2023-10-13 16:27:08.374] [info] > Creating basecall pipeline
[2023-10-13 16:27:08.457] [info]  - set batch size to 1824
[2023-10-13 16:28:16.576] [info] > Simplex reads basecalled: 4100
[2023-10-13 16:28:16.576] [info] > Simplex reads filtered: 1
[2023-10-13 16:28:16.576] [info] > Basecalled @ Samples/s: 1.783569e+06
[2023-10-13 16:28:16.681] [info] > Finished
