## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## Alta Escalabilidad en Sistemas Distribuidos

1 Máquinas de trabajo:
  * [1.1 Pre-requisitos para trabajar en ssddX.cloud.lab.inf.uc3m.es](#11-pre-requisitos-para-trabajar-en-ssddx.cloud.lab.inf.uc3m.es)
  * [1.2 Conexión SSH con las máquinas de trabajo](#11-conexion-ssh-con-las-maquinas-de-trabajo)

2 Software necesario:
  * [2.1 Instalación de Python](#21-instalacion-de-python)
  * [2.2 Instalación de Apache Spark](#22-instalacion-de-apache-spark)

3 Ejemplos para aprender:
  * [3.1 Apache Spark en nodo autónomo y shell interactivo](#31-apache-spark-en-nodo-autonomo-y-shell-interactivo)
  * [3.2 Ejemplo: cálculo de pi en nodo autónomo y shell interactivo](#32-ejemplo-calculo-de-pi-en-nodo-autonomo-y-shell-interactivo)
  * [3.3 Ejemplo: contar ocurrencias de palabras en fichero autónomo y shell interactivo](#33-ejemplo-contar-ocurrencias-de-palabras-en-nodo-autonomo-y-shell-interactivo)

[Agradecimientos](#agradecimientos)


## Máquinas de trabajo

### 1. Pre-requisitos para trabajar en ssddX.cloud.lab.inf.uc3m.es

Ha de disponer de:
1. Cuenta en el Laboratorio del Departamento de Informática.
2. El software MobaXterm (o similar).
3. Estar conectado dentro de la red de la Universidad, ya sea con VPN o con conexión a través de guernika.lab.inf.uc3m.es.

Como ayuda:
1. Para solicitar la apertura de cuenta siga los pasos indicados en: https://www.lab.inf.uc3m.es/servicios/apertura-de-cuenta/
2. El Laboratorio del Departamento de Informática dispone de un manual de MobaXterm en: https://www.lab.inf.uc3m.es/wp-content/docs/Manual_ConexionSSH.pdf
3. La información de VPN para la Universidad está en: https://www.uc3m.es/sdic/servicios/vpn


### 2. Conexión SSH con las máquinas de trabajo

* Estando dentro de la Universidad para iniciar la sesión de trabajo ha de conectarse a ssddX.cloud.lab.inf.uc3m.es, siendo X = {0, 1, ... 8}:
```
ssh  lab@ssdd0.cloud.lab.inf.uc3m.es
lab@ssdd0.lab.inf.uc3m.es's password: <clave que no se mostrará cuando escriba>
Linux ssdd0...
...
```

* Una vez que finalice su sesión de trabajo, ha de cerrar cada conexión abierta usando el mandato exit:
```
exit
```


## Software necesario

### 3. Instalación de Python


<html>
<table>

<tr>
<td></td>
<td>Uso de PIP</td>
<td>Uso de Anaconda</td>
</tr>

<tr>
<td>Instalar Python3</td>
<td>
<small><pre class="m-0">
sudo apt-get install \
     python3-minimal python3-pip
</pre></small>
</td>
<td>
<small><pre class="m-0">
wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh
chmod a+x Anaconda3-2022.10-Linux-x86_64.sh
./Anaconda3-2022.10-Linux-x86_64.sh -b
source ~/.profile
conda update --all
conda clean  --all
</pre></small>
</td>
</tr>

<tr>
<td>Instalar jupyter</td>
<td><pre>sudo apt-get install jupyter-notebook</pre></td>
<td><pre>conda install jupyter</pre></td>
</tr>

<tr>
<td>Prueba básica</td>
<td colspan="1"><pre>jupyter notebook</pre></td>
<td colspan="1"><pre>jupyter notebook</pre></td>
</tr>

</table>
</html>


### 4. Instalación de Apache Spark

Para instalar las dependencias puede ejecutar:
```
sudo apt-get install ssh rsync default-jdk
```

Para instalar Apache Spark en su cuenta personal puede ejecutar:
```
cd $HOME
wget https://www.apache.org/dyn/closer.lua/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz
tar zxf spark-3.3.1-bin-hadoop3.tgz
ln   -s spark-3.3.1-bin-hadoop3  spark
```

La prueba básica de que funciona la instalación es:
```
./spark/bin/run-example SparkPi 5
```
Debería de ver una salida como la siguiente:
```
...
22/11/24 19:20:04 INFO SparkContext: Running Spark version 3.3.0
...
22/11/24 19:20:07 INFO DAGScheduler: Job 0 finished: reduce at SparkPi.scala:38, took 0.851307 s
Pi is roughly 3.14246228492457
22/11/24 19:20:07 INFO SparkUI: Stopped Spark web UI at http://master:4040
22/11/24 19:20:07 INFO MapOutputTrackerMasterEndpoint: MapOutputTrackerMasterEndpoint stopped!
22/11/24 19:20:07 INFO MemoryStore: MemoryStore cleared
22/11/24 19:20:07 INFO BlockManager: BlockManager stopped
22/11/24 19:20:07 INFO BlockManagerMaster: BlockManagerMaster stopped
22/11/24 19:20:07 INFO OutputCommitCoordinator$OutputCommitCoordinatorEndpoint: OutputCommitCoordinator stopped!
22/11/24 19:20:07 INFO SparkContext: Successfully stopped SparkContext
22/11/24 19:20:08 INFO ShutdownHookManager: Shutdown hook called
22/11/24 19:20:08 INFO ShutdownHookManager: Deleting directory /tmp/spark-5d7bb31d-80e7-4268-ae61-c9e5f30a5344
22/11/24 19:20:08 INFO ShutdownHookManager: Deleting directory /tmp/spark-4952b716-43c1-4d45-8e6b-7fa454fec4a2
```


## Ejemplos para aprender

### 5. Apache Spark en nodo autónomo y shell interactivo

Para trabajar con un shell interactivo en un nodo autónomo hay que ejecutar:
```
./spark/bin/pyspark  --master local[2]
```

Donde el parámetro de "--master" puede ser:
* local	    -> 1 hilo
* local[N]  -> N hilos
* local[\*] -> tantos hilos como cores haya en el sistema (nproc --all)


Tras ejecutar pyspark como se ha indicado anteriormente, la salida debería ser parecida a:
```
Python 3.10.6 (main, Nov  2 2022, 18:53:38) [GCC 11.3.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
...
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 3.3.0
      /_/

Using Python version 3.10.6 (main, Nov  2 2022 18:53:38)
Spark context Web UI available at http://master:4040
Spark context available as 'sc' (master = local[*], app id = local-1669315623573).
SparkSession available as 'spark'.
>>>
```

Cuando tengamos que terminar la sesión de trabajo de Apache Spark deberemos ejecutar:
```
quit()
```
También final de fichero (con las teclas control y D) debería de permitir finalizar la sesión con Apache Spark.


### 6. Ejemplo: cálculo de pi en nodo autónomo y shell interactivo

Para trabajar con un shell interactivo en un nodo autónomo hay que ejecutar:
```
./spark/bin/pyspark  --master local[*]
```

Spark imprime ">>>" para indicarnos que está a la espera de código Python para ejecutar.
Vamos a copiar el siguiente código en la terminal:
``` python
import sys
from random import random
from operator import add
from pyspark.sql import SparkSession

partitions = 2
n = 100000 * partitions
def f(_):
   x = random() * 2 - 1
   y = random() * 2 - 1
   return 1 if x ** 2 + y ** 2 < 1 else 0

spark = SparkSession.builder.appName("PythonPi").getOrCreate()
count = spark.sparkContext.parallelize(range(1, n + 1), partitions).map(f).reduce(add)
print("Pi is roughly %f" % (4.0 * count / n))
spark.stop()
```

Tras introducir el código y dar enter se ejecutará, y la salida debería ser parecida a:
```
>>> import sys
>>> from random import random
>>> from operator import add
>>> from pyspark.sql import SparkSession
>>> 
>>> partitions = 2
>>> n = 100000 * partitions
>>> def f(_):
...    x = random() * 2 - 1
...    y = random() * 2 - 1
...    return 1 if x ** 2 + y ** 2 < 1 else 0
... 
>>> spark = SparkSession.builder.appName("PythonPi").getOrCreate()

22/11/26 20:42:12 WARN SparkSession: Using an existing Spark session; only runtime SQL configurations will take effect.
>>> count = spark.sparkContext.parallelize(range(1, n + 1), partitions).map(f).reduce(add)
>>> print("Pi is roughly %f" % (4.0 * count / n))                               
Pi is roughly 3.138680
>>> spark.stop()
>>> 
```

Para terminar la sesión ejecutaremos:
```
quit()
```


### 7. Ejemplo: contar ocurrencias de palabras en fichero en nodo autónomo y shell interactivo

Usaremos el quijote en texto plano para trabajar ([pg2000.txt](https://www.gutenberg.org/files/2000/2000-0.txt)) para lo que usaremos:
```
wget -m https://www.gutenberg.org/files/2000/2000-0.txt
```

Deberemos borrar cualquier resultado anterior de trabajo:
```
rm -fr /home/lab/lab_spark/pg2000-w
```

Para trabajar con un shell interactivo en un nodo autónomo hay que ejecutar:
```
./spark/bin/pyspark  --master local[*]
```

Spark imprime ">>>" para indicarnos que está a la espera de código Python para ejecutar.
Vamos a copiar el siguiente código en la terminal:
``` python
import sys
from operator import add
from pyspark.sql import SparkSession

sc = SparkSession.builder.appName("pywc").getOrCreate()
lines = sc.read.text("/home/lab/lab_spark/2000-0.txt").rdd.map(lambda r: r[0])
counts = lines.flatMap(lambda x: x.split(' ')) \
              .map(lambda x: (x, 1)) \
              .reduceByKey(add)
output = counts.collect()
counts.saveAsTextFile("/home/lab/lab_spark/pg2000-w")
sc.stop()
```

Tras introducir el código y dar enter se ejecutará, y la salida debería ser parecida a:
```
>>> 
>>> import sys
>>> from operator import add
>>> from pyspark.sql import SparkSession
>>> 
>>> sc = SparkSession.builder.appName("pywc").getOrCreate()

22/11/26 20:44:30 WARN SparkSession: Using an existing Spark session; only runtime SQL configurations will take effect.
>>> lines = sc.read.text("/home/lab/lab_spark/2000-0.txt").rdd.map(lambda r: r[0])
>>> counts = lines.flatMap(lambda x: x.split(' ')) \
...               .map(lambda x: (x, 1)) \
...               .reduceByKey(add)
>>> output = counts.collect()
>>> counts.saveAsTextFile("/home/lab/lab_spark/pg2000-w")                       
>>> sc.stop()
>>>
```

Para terminar la sesión ejecutaremos:
```
quit()
```

Para ver el resultado ejecutaremos:
```
cat /home/lab/lab_spark/pg2000-w/part-00000
```


## Bibliografía de ejemplos de Spark

* [Ejemplos iniciales de uso de Spark](https://spark.apache.org/examples.html)
* [Ejemplos en python](https://github.com/apache/spark/tree/master/examples/src/main/python)


## Agradecimientos

Por último pero no por ello menos importante, agradecer al personal del Laboratorio del Departamento de Informática toda la ayuda prestada para que este laboratorio sea posible.

