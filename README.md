
# Taller "Principios sobre el análisis de conjuntos de datos de secuenciación de Oxford Nanopore" - 11 de Noviembre de 2024
## Instructor: [Oscar A. Pérez Escobar](https://operez-escobar.wixsite.com/orchidevo) [(Royal Botanic Gardens, Kew)](https://scholar.google.co.uk/citations?user=tSzyp6QAAAAJ&hl=en)
### Organizadores: [Carolina Granados Mendonza](https://scholar.google.com/citations?user=yswIvBwAAAAJ&hl=es) [(Instituto de Biologia, UNAM)](https://www.ib.unam.mx/ib/directorio-del-personal-academico/perfil/index.php?crypt=ZmZLRTZQNHRXK2tGREFHeXUxODFHdz09), [Ulises Rosas](https://scholar.google.com/citations?user=a22UgrAAAAAJ&hl=en) [(Instituto de Biologia, UNAM)](https://www.ib.unam.mx/ib/directorio-del-personal-academico/perfil/index.php?crypt=UmRsNnd2eFFzUlBpYVBzUDFkcWVHZz09), [Lazaro Guevara](https://scholar.google.es/citations?user=G4TO4w0AAAAJ&hl=es) [(Instituto de Biologia, UNAM)](https://www.ib.unam.mx/ib/directorio-del-personal-academico/perfil/index.php?crypt=UTB0dHFEU3BGWTRMY2VqZkw5cHJ4Zz09), Oscar A. Pérez Escobar.
### Asistentes: [Rubi Meza](https://www.fciencias.unam.mx/directorio/56530), [Cristian Cervantes](https://www.ib.unam.mx/ib/directorio-del-personal-academico/perfil/index.php?crypt=Yi9BdHNya3JpSkFJNDh4dlh1b3kydz09)
##### Este taller está financiado y apoyado por: [Insituto de Biologia de la Universidad Nacional Autonoma de Mexico](https://www.darwininitiative.org.uk) - [Royal Botanic Gardens, Kew (RBG Kew)](https://www.kew.org)

## 1. Introducción
Este repositorio contiene una guía tutorial para el análisis de datos brutos derivados de Oxford Nanopore Technologies (ONT) y los pasos iniciales para realizar un ensamblaje del genoma. Además, incluye un ejemplo real de la aplicación/usabilidad de datos ONT, por ejemplo, realizar búsquedas de secuencias en una base de datos predeterminada utilizando ncbi blast para determinar la identidad de un organismo secuenciado con ONT.

Este tutorial está dirigido a usuarios con conocimientos básicos en programación y está diseñado para ejecutarse en entornos UNIX. El participante idealmente debería tener experiencia usando shell y manipulando archivos de texto (por ejemplo, usando **awk, sed, grep, entre otros**). El taller se llevará a cabo en el servidor *Beagle* del Instituto de Biologia de la UNAM. _Una introducción básica al entorno UNIX con algunos comandos útiles está disponible [aquí](https://github.com/siriusb-nox/ONT-IBUNAM-2024/blob/main/bash_tutorial.md)_.

Este tutorial requiere los siguientes programas/dependencias (se recomienda encarecidamente tenerlos instalados antes de comenzar el tutorial). **Asegúrese de que también estén disponibles las dependencias de estos programas**:

1. [**NCBI blast:**](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download) Este programa construye bases de datos de blast, las cuales son necesarias para realizar búsquedas de secuencias de ADN/AA en bases de datos blast.
2. [**NCBI magicblast:**](https://ncbi.github.io/magicblast/doc/download.html) Este programa realiza búsquedas de secuencias de ADN/AA derivadas de experimentos de secuenciación de illumina/Nanopore (en formato fasta o fastq) contra bases de datos blast. **IMPORTANTE: por favor, cree una cuenta gratuita de NCBI para luego acceder libremente a una clave API de NCBI [aquí](https://account.ncbi.nlm.nih.gov/?back_url=https%3A%2F%2Fwww.ncbi.nlm.nih.gov%2F). Esto es necesario para realizar consultas remotas (en línea) de secuencias.**
3. [**CANU:**](https://github.com/marbl/canu) este programa permite la corrección y filtrado de secuencias ONT/PacBio.
4. [**SMARTdenovo:**](https://github.com/ruanjue/smartdenovo) este programa ensambla secuencias ONT/PacBio corregidas y recortadas de novo.
5. [**minimap2:**](https://github.com/lh3/minimap2) este programa realiza alineamientos de genomas por pares o mapeo de lecturas cortas-largas, cortas-cortas. Esto es necesario para el pulido del genoma.
6. [**racon:**](https://github.com/isovic/racon) este programa corrige lecturas largas o scaffolds a partir de lecturas mapeadas de lecturas cortas contra dichos scaffolds/genoma. Esto es necesario para el pulido del genoma.
7. [**NanoPlot:**](https://github.com/wdecoster/NanoPlot) Una versión ejecutable en línea está disponible [aquí](https://nanoplot.bioinf.be/); este programa produce gráficos con información asociada a experimentos de secuenciación realizados con tecnologías ONT.
8. [**Guppy:**](https://nanoporetech.com/nanopore-sequencing-data-analysis) Este programa (ahora en desuso) llama a bases a partir de archivos FAST5 generados por ONT. Solo está disponible para usuarios de ONT (esta parte del tutorial, aunque explicada, no se ejecutará).
9. [**dorado:**]() Este programa (ahora el basecaller oficial de ONT) llama a bases a partir de archivos POD5 generados por ONT. Es de acceso libre y puede realizar una amplia gama de funciones.

## 2. Estructura del taller
Este tutorial se divide en cuatro pasos principales:

A. [**Llamada de bases**](https://github.com/siriusb-nox/ONT-IBUNAM-2024/blob/main/A_basecall.md)

B. [**Análisis de calidad de datos**](https://github.com/siriusb-nox/ONT-IBUNAM-2024/blob/main/B_NanoPlot.md)

C. [**Recorte, corrección y ensamblaje del genoma**](https://github.com/siriusb-nox/ONT-IBUNAM-2024/blob/main/C_read_corrtrim_CANU.md)

D. [**Búsqueda y/o operaciones de anotación del genoma**](https://github.com/siriusb-nox/ONT-IBUNAM-2024/blob/main/D_DataAnalysis_WGS_BLAST.md)

![Figura 1](https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/pipeline_overview_v0_OP_16102023.png?raw=true)
**Figura 1**: Vista simplificada del tutorial/pipeline

>[!IMPORTANT]
>**Los datos necesarios para ejecutar este taller se encuentran en el siguiente directorio (/home/oscarp/ONT_IBUNAM_2024/). Idealmente, favor copiar esta carpeta en su directorio local**.

## 2.1. Configuración del pipeline
En cualquier pipeline bioinformático, es esencial relacionar los programas de los que depende el pipeline y saber dónde se encuentran los archivos de entrada, etc. Para ejecutar este tutorial, debe cargar el ambiente (_environment_) en la sesion, usando:

```bash
conda activate ontasia
```

**Este comando cargara todas las dependencias necesarias para ejectuar el taller**

