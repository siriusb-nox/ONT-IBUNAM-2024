<p align="center">
  <img src="https://github.com/siriusb-nox/ONT-workshop-Oct-2023/blob/main/IMG/bash_logo_bashlogo.com.png" alt="logo de bash de la página web de bash"/>
</p>

## Tutorial básico para navegar un terminal en entornos UNIX

#### 1. Introducción
UNIX es una familia de sistemas operativos (SO) desarrollada en 1960. Quizás los SO más famosos son [Ubuntu](https://ubuntu.com/), [Linux](https://www.linux.org/) y [MacOS](https://www.apple.com/uk/macos/ventura/). En bioinformática, _bash_ (es decir, "bourne Again Shell") o el _shell_ (es decir, terminal, o el traductor entre las operaciones que realiza la computadora y las instrucciones en "lenguaje humano") de UNIX es un lenguaje ampliamente utilizado, ya que contiene una serie de programas básicos pero poderosos para manejar archivos de texto (por ejemplo, secuencias fasta, fastq, genomas, etc.). Además, muchos programas de bioinformática están más disponibles para entornos UNIX que para otras plataformas (por ejemplo, el DOS de Microsoft... ¡uf!).

#### 2. Sintaxis
La sintaxis de UNIX en la terminal es muy sencilla. Cada vez que la terminal muestra "$" o "%", está lista para recibir instrucciones. Básicamente:

`$programa opción1 opción2 entrada > salida`.

Las opciones en UNIX son generalmente parámetros (_flags_) que permiten modificar el comportamiento habitual/predeterminado de un programa. Por lo general, se especifican con un "-", por ejemplo:

`ls` vs `ls -lrt` - ¿Cuál es la diferencia entre estos dos comandos?

#### 3. Programas esenciales para navegar por el terminal de UNIX e interactuar/mostrar archivos
Aquí hay una lista breve de programas básicos de UNIX que usaremos con frecuencia:

#### 3.1 `cd`

"Change directory" se utiliza para cambiar de una carpeta/directorio a otro. Por ejemplo:

```bash
cd
```
Para subir un nivel de carpeta, use "cd ..". Además, _cd_ también permite cambiar a una dirección completa de directorio, por ejemplo, **/home/usr/etc**:

```bash
cd ..
cd /home/usr/etc
```

### 3.2 `mkdir`
Crea nuevas carpetas.

```bash
mkdir carpeta_nueva
```
Se pueden crear varias carpetas al mismo tiempo, por ejemplo:

```bash
mkdir carpeta1 carpeta2 carpeta3 carpeta3
```
Se pueden crear directorios completos con la opción _-p_, así:

```bash 
mkdir -p /home/usr/usr/foo/bla/
```
Las carpetas también se pueden crear en subdirectorios existentes, utilizando una dirección existente. Por ejemplo, crear 'bioinf' en '/home/usr/foo/bla/':

```bash 
mkdir /home/usr/usr/foo/bla/bioinf
```

### 3.3 `pwd`
Muestra el directorio en el que se encuentra

```bash
pwd
```

### 3.4 `ls`
Lista los archivos o carpetas presentes en un directorio. `ls` tiene muchas opciones... Por ejemplo, la opción `-l` presenta una lista detallada de archivos/carpetas, mostrando información como tamaño exacto del archivo, propietario, acceso y fecha de modificación.

```bash
ls
ls -l
```

### 3.5 `cat`
Se utiliza para ver archivos "no binarios", por ejemplo:
* Ver archivos de texto en la terminal
* Combinar archivos de texto

```bash
cat archivo.txt
cat archivo1.txt archivo2.txt
cat archivo1.txt archivo2.txt > archivo_combinado.txt
cat < archivo1.txt > archivo2.txt
```

### 3.6 `less`
Se utiliza para explorar archivos ``no binarios``, por ejemplo:

```bash
less entrada.txt
```
Por ejemplo, ``less`` se puede usar para echar un vistazo rápido a un archivo fastq, de esta manera (archivo disponible en el tutorial, primero descomprima usando `gunzip`):

```bash
cat /directorio/personal/Taller-Oxford-Nanopore-Dic-2022/NGSdat/Cinchona_PAD61320_sizeSelect_1Kseq_99.fastq
```

### 3.7 `grep`
Este programa busca patrones de texto, incluidas [_expresiones regulares_](https://sospedia.net/el-shell-bash-de-gnulinux-4-expresiones-regulares/).

```bash
grep opción1 opción2 entrada
```

Por ejemplo, podemos buscar un nombre de secuencia específico ("@7318713f-55b4-4e19-8fb1-4bd89b35568e") en un archivo fastq así:

```bash
grep "^@7318713f-55b4-4e19-8fb1-4bd89b35568e" /directorio/personal/Taller-Oxford-Nanopore-Dic-2022/NGSdat/NGSdat/Cinchona_PAD61320_sizeSelect_1Kseq_99.fastq
```

... hay muchos otros programas que permiten visualizar datos y que recomendamos aprender. Algunos ejemplos están disponibles [aquí](https://www.biostars.org/p/17680/).

### 3.8 `man`
Este programa proporciona información detallada sobre programas básicos de UNIX.

```bash
man programa
man ls
```

### ACTIVIDAD: Ejercicios de Unix y Bioinformática

### Ejercicio 1: Navegación Básica y Manipulación de Archivos

**Objetivo:** Aprender a navegar por directorios, crear archivos y moverlos. Para estos ejercicios, usaremos un set de secuencias sinteticas disponibles en el archivo `sequences.fasta` disponible en el directorio `/home/oscarp/ONT_IBUNAM_2024/NGSdat`

**Tareas:**

- Usa `pwd` para imprimir el directorio actual.
- Usa `ls` para listar los archivos en un directorio.
- Crea un directorio llamado `bioinformatics_intro` usando `mkdir`.
- Navega al directorio `bioinformatics_intro` usando `cd`.
- Crea un nuevo archivo llamado `sequences.fasta` usando el comando `touch`.
- Mueve `sequences.fasta` a otro directorio (por ejemplo, `mv sequences.fasta /tmp/`).

### Ejercicio 2: Visualización y Edición de Archivos

**Objetivo:** Aprender a visualizar y editar archivos usando comandos de Unix.

**Tareas:**

- Usa `cat` para imprimir el contenido del archivo `sequences.fasta` en la terminal.
- Usa `head` para mostrar las primeras 10 líneas del archivo.
- Usa `tail` para mostrar las últimas 5 líneas del archivo.
- Usa `nano` (o `vim`) para abrir el archivo `sequences.fasta` y agregar una nueva secuencia:

Guarda y sal del editor.

### Ejercicio 3: Contar el Número de Secuencias en un Archivo FASTA

**Objetivo:** Aprender a manipular archivos de texto y contar elementos específicos.

**Tareas:**

Usa `grep` para contar el número de secuencias en un archivo FASTA buscando líneas que empiecen con `>`.

**Comando:**

```bash
grep -c "^>" sequences.fasta
```

**Explicación:** `grep -c "^>"` cuenta las líneas que comienzan con `>`, que suelen ser los encabezados de las secuencias en archivos FASTA.

### Ejercicio 4: Contar el Número de Nucleótidos en Cada Secuencia

**Objetivo:** Aprender a procesar el contenido de un archivo FASTA para calcular la longitud de las secuencias.

**Tareas:**

- Usa `grep -v "^>" sequences.fasta` para imprimir solo las secuencias (sin encabezados).
- Usa `wc -c` para contar el número de caracteres (nucleótidos) en cada secuencia:

```bash
grep -v "^>" sequences.fasta | wc -c
```

- Usa `awk` para imprimir la longitud de cada secuencia:

**Comando:**

```bash
awk '/^>/ {if (seqlen) print seqlen; seqlen=0; next} {seqlen += length($0)} END {print seqlen}' sequences.fasta
```

### Ejercicio 5: Contar el Número de Especies en el Archivo FASTA

**Objetivo:** Aprender a extraer nombres de especies únicas de un archivo FASTA.

**Tareas:**

Usa `cut` y `sort` para extraer nombres de especies únicas:

**Comando:**

```bash
grep "^>" sequences.fasta | cut -d"_" -f1 | sort | uniq
```

**Explicación:** Este comando busca encabezados, corta la primera parte del encabezado (asumiendo que los nombres de especies están antes del primer `_`), los ordena y extrae las especies únicas.

### Ejercicio 6: Filtrar Secuencias por Longitud

**Objetivo:** Aprender a filtrar secuencias que sean más cortas o más largas que una cierta longitud.

**Tareas:**

Usa `awk` para filtrar secuencias menores de 50 nucleótidos:

**Comando:**

```bash
awk '/^>/ {if (seqlen >= 50) print header"\n"seq; header=$0; seq=""; next} {seq=seq""$0} END {if (seqlen >= 50) print header"\n"seq}' sequences.fasta
```

**Explicación:** Este comando verifica la longitud de cada secuencia y solo imprime aquellas con 50 o más nucleótidos.

### Ejercicio 7: Crear Directorios y Organizar Archivos

**Objetivo:** Aprender a crear directorios y organizar archivos de manera programática.

**Tareas:**

Crea un directorio llamado `filtered_sequences`.

Escribe un comando para mover todos los archivos FASTA al nuevo directorio:

**Comandos:**

```bash
mkdir filtered_sequences

mv *.fasta filtered_sequences/
```

### Ejercicio 8: Crear un Informe Resumido

**Objetivo:** Aprender a generar informes resumidos con conteos y estadísticas sobre las secuencias.

**Tareas:**

Crea un informe resumido con:

- Número de secuencias en el archivo FASTA.
- Número de nucleótidos en cada secuencia.
- Número de especies únicas.

Guarda la salida en un archivo llamado `summary_report.txt`:

**Comandos:**

```bash
echo "Número de secuencias: $(grep -c "^>" sequences.fasta)" > summary_report.txt

echo "Número de especies únicas: $(grep "^>" sequences.fasta | cut -d"_" -f1 | sort | uniq | wc -l)" >> summary_report.txt

grep -v "^>" sequences.fasta | awk '{print length($0)}' >> summary_report.txt
```

### Ejercicio 9: Escribir un Script para Automatizar el Conteo de Secuencias

**Objetivo:** Aprender a escribir un script simple en bash para automatizar el conteo de secuencias en todos los archivos FASTA en un directorio.

**Tareas:**

Escribe un script bash (`count_sequences.sh`) para contar las secuencias en todos los archivos `.fasta` en el directorio actual e imprime el nombre del archivo y el conteo de secuencias para cada archivo.

**Comando:**

```bash
#!/bin/bash

for fasta_file in *.fasta; do
    seq_count=$(grep -c "^>" "$fasta_file")
    echo "$fasta_file: $seq_count sequences"
done
```

Haz que el script sea ejecutable:

**Comando:**

```bash
chmod +x count_sequences.sh
```

Ejecuta el script:

**Comando:**

```bash
./count_sequences.sh
```

### Ejercicio 10: Práctica Adicional con Bucles y Condiciones

**Objetivo:** Aprender más sobre bucles y condiciones.

**Tareas:**

Crea un bucle para verificar secuencias que faltan en algunos archivos FASTA (por ejemplo, especies que no están representadas en todos los archivos).

Usa `grep` para buscar un patrón de secuencia particular en todos los archivos:

**Comando:**

```bash
for fasta_file in *.fasta; do
    grep "ATGCG" "$fasta_file" && echo "$fasta_file contiene ATGCG";
done

