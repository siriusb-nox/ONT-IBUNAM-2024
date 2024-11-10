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
