# Sesión II - Gestión avanzada y descarga de datos
Herramientas computacionales para bioinformática: UNIX, expresiones regulares y shell script

Edita esta plantilla en formato markdown [Guía aquí](https://guides.github.com/features/mastering-markdown/) como se pide en el guión. 
Cuando hayas acabado, haz un commit de tus cambios y súbelos al repositorio antes de la fecha de entrega señalada. 
Puedes usar el cliente 

======================================
Antes de comenzar, crea


## Ejercicio 1
Crea una copia de la carpeta gtfs. Luego crea enlaces duros y blandos a los ficheros .gtf y responde:

1. ¿Qué ocurre cuando se borra el origen y se intenta acceder al destino?
2. ¿Qué ocurre cuando se borra el destino y se intenta acceder al origen?
3. ¿Qué ocurre con la otra parte cuando se edita el destino o el origen del enlace?
4. ¿Qué ocurre cuando copiamos un enlace?

## Respuesta ejercicio 1

La orden para crear la copia : `cp -r gtfs/ gtfs-1`

Nos aseguramos de tener la copia : `ls -l`

**drwxr-xr-x 2 mcuadrado mcuadrado  4096 oct 24 16:35 gtfs**

**drwxr-xr-x 2 mcuadrado mcuadrado  4096 oct 24 16:36 gtfs-1**

(2 entradas en cada uno de ellos)

I- ***CREAMOS UN ENLACE DURO***

Como en el guión de las prácticas, primero creamos un directorio: `mkdir enlduro`

Ahora hacemos el enlace con la orden: `ln Homo_sapiens.GRCh38.102.gtf.gz enlduro/`

Comprobamos que lo hemos hecho correctamente: `ls -li enlduro/`
```
total 47164
229900453 -rwxr-xr-x 2 mcuadrado mcuadrado 48295448 oct 24 16:36 Homo_sapiens.GRCh38.102.gtf.gz**
```

1. ¿Qué ocurre cuando se borra el origen y se intenta acceder al destino?


Borramos el fichero origen : `rm Homo_sapiens.GRCh38.102.gtf.gz`

Comprobamos que se ha borrado: `ls -lh -i`
```
total 181M
229900454 -rwxr-xr-x 1 mcuadrado mcuadrado 135M oct 24 16:36 Drosophila_melanogaster.BDGP6.28.102.gtf
229900455 drwxr-xr-x 2 mcuadrado mcuadrado 4,0K oct 24 16:56 enlduro
```
Accedemos al destino: `cd enlduro/`
                      `ls -lh -i`
                      
**Respuesta_1: Se han mantenido los datos y podemos acceder a ellos:**

**229900453 -rwxr-xr-x 1 mcuadrado mcuadrado 47M oct 24 16:36 Homo_sapiens.GRCh38.102.gtf.gz**


2. ¿Qué ocurre cuando se borra el destino y se intenta acceder al origen? 

**Respuesta_2: el fichero origen está impecable**

**229900453 -rwxr-xr-x 1 mcuadrado mcuadrado  47M oct 24 17:28 Homo_sapiens.GRCh38.102.gtf.gz**

3. ¿Qué ocurre con la otra parte cuando se edita el destino o el origen del enlace?

**Respuesta_3: No sé como editar datos con .gtf. Imagino que se mantienen los cambios que hemos editado en el fichero origen o destino (ambos apuntan a los mismos datos)**

4. ¿Qué ocurre cuando copiamos un enlace?

**Respuesta_4: Copia los datos origen al nuevo destino que le hemos dicho pero _IMPORT._ NO aumenta el número de enlaces a los datos, se mantiene en 2 y NO aumenta el total 181M** (el inodo va en aumento con los enlaces que ido haciendo)

`cp -r enlduro/ enlduro-1`

```
total 181M
229900454 -rwxr-xr-x 1 mcuadrado mcuadrado 135M oct 24 17:28 Drosophila_melanogaster.BDGP6.28.102.gtf
229900455 drwxr-xr-x 2 mcuadrado mcuadrado 4,0K oct 24 17:38 enlduro
229900456 drwxr-xr-x 2 mcuadrado mcuadrado 4,0K oct 24 17:38 enlduro-1
229900453 -rwxr-xr-x 2 mcuadrado mcuadrado  47M oct 24 17:28 Homo_sapiens.GRCh38.102.gtf.gz
```
 ----------------------------------------------------------------------------------------------------
 
 II- ***CREAMOS UN ENLACE BLANDO***

Creamos un nuevo directorio: `mkdir enlsoft`

Ahora hacemos el enlace con la orden: `ln -s Drosophila_melanogaster.BDGP6.28.102.gtf enlsoft/`

Comprobamos que lo hemos hecho correctamente: `ls -lh -li`

```
total 181M
229900454 -rwxr-xr-x 1 mcuadrado mcuadrado 135M oct 24 18:15 Drosophila_melanogaster.BDGP6.28.102.gtf
229900455 drwxr-xr-x 2 mcuadrado mcuadrado 4,0K oct 24 18:21 enlsoft
229900453 -rwxr-xr-x 1 mcuadrado mcuadrado  47M oct 24 18:15 Homo_sapiens.GRCh38.102.gtf.gz
```

enlsoft tiene 2 entradas pero el fichero origen 1 (esto es diferente al enlace duro, en el que ambos tienen 2 entradas)

1. ¿Qué ocurre cuando se borra el origen y se intenta acceder al destino? 

**Respuesta_1: Mantiene el nombre del fichero origen, pero debe estar vacio. El total ha bajado total 47M y he perdido los 135M del archivo de Drosophila**

```
mcuadrado@cpg3:~/2-gestion-y-descarga-mcuadrado-smuntion/gtfs-1$ lista
total 47M
229900455 drwxr-xr-x 2 mcuadrado mcuadrado 4,0K oct 24 18:21 enlsoft
229900453 -rwxr-xr-x 1 mcuadrado mcuadrado  47M oct 24 18:15 Homo_sapiens.GRCh38.102.gtf.gz
```

Cuando le doy la orden: `ls -lh -li enlsoft/`

**El enlace en sí se mantiene, pero como he borrado los datos no podré acceder a ellos.**

```
total 0
229900460 lrwxrwxrwx 1 mcuadrado mcuadrado 40 oct 24 17:55 
Drosophila_melanogaster.BDGP6.28.102.gtf -> Drosophila_melanogaster.BDGP6.28.102.gtf
```


2. ¿Qué ocurre cuando se borra el destino y se intenta acceder al origen?

**Respuesta_2: el fichero origen está impecable**

```
total 181M
229900454 -rwxr-xr-x 1 mcuadrado mcuadrado 135M oct 24 18:15 Drosophila_melanogaster.BDGP6.28.102.gtf
```

3. ¿Qué ocurre con la otra parte cuando se edita el destino o el origen del enlace?

**Respuesta_3: No sé editar el fichero .gtf. Imagino que si modifico el archivo de datos tanto en el origen como en el destino se me mantendrán los datos***



4. ¿Qué ocurre cuando copiamos el enlace?

**Respuesta_4: lo copia al nuevo directorio que crea. El archivo origen se mantiene con 1 entrada.**
**El archivo destino y la copia con 2 entradas. No aumenta el total 181M**

```
total 181M
229900454 -rwxr-xr-x 1 mcuadrado mcuadrado 135M oct 24 18:42 Drosophila_melanogaster.BDGP6.28.102.gtf
229900455 drwxr-xr-x 2 mcuadrado mcuadrado 4,0K oct 24 18:43 enlsoft
229900458 drwxr-xr-x 2 mcuadrado mcuadrado 4,0K oct 24 18:44 enlsoft-2
229900453 -rwxr-xr-x 1 mcuadrado mcuadrado  47M oct 24 18:42 Homo_sapiens.GRCh38.102.gtf.gz
```

```
mcuadrado@cpg3:~/2-gestion-y-descarga-mcuadrado-smuntion/gtfs-1$ lista enlsoft-2
total 0
229900459 lrwxrwxrwx 1 mcuadrado mcuadrado 40 oct 24 18:44 Drosophila_melanogaster.BDGP6.28.102.gtf -> Drosophila_melanogaster.BDGP6.28.102.gtf
```
-----------------------------------------------------------------------------------------------------------
 
 
 
 


## Ejercicio 2
Usa la documentación de `find` para encontrar todos los notebook Jupyter con fecha de última modificación 30 de Noviembre de 2020 que haya en tu directorio HOME. Excluye todos aquellos que se encuentren dentro de directorios ocultos (aquellos que comienzan por un punto `.`). 

### Respuesta ejercicio 2
Ponemos find /home/alejandro -name "*.ipynb" y a continuacion nos muestran todos estos archivos

```
/home/alejandro/P1-matriz-old.ipynb
/home/alejandro/un_cuaderno.ipynb
/home/alejandro/P6-enriquecimiento.ipynb
/home/alejandro/P3-expresion.ipynb
/home/alejandro/P4-expresionDiferencial.ipynb
/home/alejandro/P1-matriz.ipynb
/home/alejandro/.ipynb_checkpoints/un_cuaderno-checkpoint.ipynb
/home/alejandro/P8-edgeR.ipynb
/home/alejandro/.local/share/jupyter/nbgrader_cache/python-bio/alejandro+ps1+2020-10-09 08:27:41.013624 UTC/bloque1.ipynb
/home/alejandro/.local/share/jupyter/nbgrader_cache/python-bio/alejandro+ps1+2020-10-08 18:00:41.993364 UTC/bloque1.ipynb
/home/alejandro/.local/share/jupyter/nbgrader_cache/python-bio/alejandro+ps1+2020-10-08 18:02:23.551532 UTC/bloque1.ipynb
/home/alejandro/ps1/.ipynb_checkpoints/bloque1-checkpoint.ipynb
/home/alejandro/ps1/bloque1.ipynb
/home/alejandro/P5-coexpresion.ipynb 

```


## Ejercicio 3
Descarga, empleando la orden oportuna, todos los ficheros [de esta URL](ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/). 
- Inspecciona el fichero CHECKSUMS y genera los checksums adecuados para asegurarte de que los datos son íntegros. 
- Genera un fichero SHA-1 para todos los ficheros .gz descargados y documenta en esta memoria el comando con el que te descargaste los datos, adjuntando aquí los checksums. 


### Respuesta ejercicio 3
Escribiendo en el terminal wget -r Fttt.....
Finalemnte nos ha descargado la secuencia
wget -r ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/
--2021-10-27 15:26:04--  ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/
           => «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/.listing»
Resolviendo ftp.ensembl.org (ftp.ensembl.org)... 193.62.193.139
Conectando con ftp.ensembl.org (ftp.ensembl.org)[193.62.193.139]:21... conectado.
Identificándose como anonymous ... ¡Dentro!
==> SYST ... hecho.   ==> PWD ... hecho.
==> TYPE I ... hecho.  ==> CWD (1) /pub/release-102/gtf/accipiter_nisus ... hecho.
==> PASV ... hecho.   ==> LIST ... hecho.

ftp.ensembl.org/pub     [ <=>                ]     473  --.-KB/s    en 0,005s  

2021-10-27 15:26:05 (91,4 KB/s) - «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/.listing» guardado [473]

«ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/.listing» eliminado.
--2021-10-27 15:26:05--  ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/Accipiter_nisus.Accipiter_nisus_ver1.0.102.abinitio.gtf.gz
           => «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/Accipiter_nisus.Accipiter_nisus_ver1.0.102.abinitio.gtf.gz»
==> no se requiere CWD.
==> PASV ... hecho.   ==> RETR Accipiter_nisus.Accipiter_nisus_ver1.0.102.abinitio.gtf.gz ... hecho.
Longitud: 3468742 (3,3M)

ftp.ensembl.org/pub 100%[===================>]   3,31M  1,19MB/s    en 2,8s    

2021-10-27 15:26:09 (1,19 MB/s) - «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/Accipiter_nisus.Accipiter_nisus_ver1.0.102.abinitio.gtf.gz» guardado [3468742]

--2021-10-27 15:26:09--  ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/Accipiter_nisus.Accipiter_nisus_ver1.0.102.gtf.gz
           => «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/Accipiter_nisus.Accipiter_nisus_ver1.0.102.gtf.gz»
==> no se requiere CWD.
==> PASV ... hecho.   ==> RETR Accipiter_nisus.Accipiter_nisus_ver1.0.102.gtf.gz ... hecho.
Longitud: 9593838 (9,1M)

ftp.ensembl.org/pub 100%[===================>]   9,15M   651KB/s    en 15s     

2021-10-27 15:26:24 (622 KB/s) - «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/Accipiter_nisus.Accipiter_nisus_ver1.0.102.gtf.gz» guardado [9593838]

--2021-10-27 15:26:24--  ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/CHECKSUMS
           => «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/CHECKSUMS»
==> no se requiere CWD.
==> PASV ... hecho.   ==> RETR CHECKSUMS ... hecho.
Longitud: 152

ftp.ensembl.org/pub 100%[===================>]     152  --.-KB/s    en 0,01s   

2021-10-27 15:26:24 (10,5 KB/s) - «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/CHECKSUMS» guardado [152]

--2021-10-27 15:26:24--  ftp://ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/README
           => «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/README»
==> no se requiere CWD.
==> PASV ... hecho.   ==> RETR README ... hecho.
Longitud: 9403 (9,2K)

ftp.ensembl.org/pub 100%[===================>]   9,18K  --.-KB/s    en 0,02s   

2021-10-27 15:26:24 (513 KB/s) - «ftp.ensembl.org/pub/release-102/gtf/accipiter_nisus/README» guardado [9403]

ACABADO --2021-10-27 15:26:24--
Tiempo total de reloj: 20s
Descargados: 4 ficheros, 12M en 18s (714 KB/s)

`shasum index.html`

***aa802a538fe7f91e0585d158f11731d50ec95624  index.html***

```
muntion@cpg3:~/ejercicio3$ shasum ./*html > html_checksums.sha
smuntion@cpg3:~/ejercicio3$ cat html_checksums.sha
aa802a538fe7f91e0585d158f11731d50ec95624  ./index.html
shasum -c html_checksums.sha
```

***./index.html: OK***

Y los Checksums estan OK.



## Correciones

En general, poner atención en el formato del documento markdown cuando estamos compartiendo el código para que sea más sencillo para los demás de seguir. La recomendación es copiar y pegar el código de la consola que incluye la línea del usuario con la localización actual y la linea de ordenes y a continuación el resultado. Y eso lo colocamos entre los ```.
De esta manera es más fácil reproducir el trabajo que se viene haciendo. Se puede consultar la correción del professor para ver el formato.

**Ejercicio 1**

El enunciado pide hacer enlaces a archivos .gft, estos archivos son archivos de texto que se pueden abrir con el nano. El archivo gft.gz es un archivo comprimido.
Se crearon los enlaces y se hicieron algunas comprobaciones, pero no todas las que se podrían y algunas de las cosas que se suponen podrían ser comprobadas abriendo los ficheros de texto como pedía el enunciado.

 Puntuación: 2

**Ejercicio 2**

La formatación hace que sea difícil de leer, he formatado el resultado de la línea de comandos como ejemplos. Faltó la parte de la fecha, se podría haber usar el flag de find  -newermt ‘date’ y -not -newermt ‘date’

Puntuación: 1,75


**Ejercicio 3**

Se realizó la comprobación checksum, pero de un archivo incorrecto

Pùntuación: 3

Total: 6,75


