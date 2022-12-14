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
  * [3.4 Ejemplo: uso de jupyter notebook](#34-ejemplo-uso-de-jupyter-notebook)
  * [3.5 Ejemplo: uso de jupyter notebook con cluster](#34-ejemplo-uso-de-jupyter-notebook-con-cluster)

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
<small><pre>
sudo apt-get install \
     python3-minimal python3-pip
</pre></small>
</td>
<td>
<pre>
wget <sub>https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh</sub>
chmod a+x Anaconda3-2022.10-Linux-x86_64.sh
./Anaconda3-2022.10-Linux-x86_64.sh -b
source ~/.profile
conda update --all
conda clean  --all
</pre>
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


Alternativamente, es posible usar docker para la instalación, configuración y ejecución en un entorno simple:
```
docker run -it --rm -p 8888:8888 jupyter/pyspark-notebook
```
No obstante para estos ejemplos se usará PIP.


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

### 3.1 Apache Spark en nodo autónomo y shell interactivo

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


### 3.2 Ejemplo: cálculo de pi en nodo autónomo y shell interactivo

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


### 3.3 Ejemplo: contar ocurrencias de palabras en fichero en nodo autónomo y shell interactivo

Usaremos el quijote en texto plano para trabajar ([pg2000.txt](https://www.gutenberg.org/files/2000/2000-0.txt)) para lo que usaremos:
```
wget -m https://www.gutenberg.org/files/2000/2000-0.txt
```

Deberemos borrar cualquier resultado anterior de trabajo:
```
rm -fr   /home/lab/lab_spark/pg2000-w
mkdir -p /home/lab/lab_spark/pg2000-w
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


### 3.4 Ejemplo: uso de jupyter notebook

Debemos configurar pyspark para que use jupyter y notebook:
```
export PATH=$HOME/spark/bin:$PATH
export PYSPARK_DRIVER_PYTHON=jupyter
export PYSPARK_DRIVER_PYTHON_OPTS='notebook --ip=0.0.0.0 --no-browser'
```

Si ejecutamos pyspark:
```
pyspark
```

En la salida nos indica una URL en la que conectarnos:
```
[I 10:01:40.340 NotebookApp] Serving notebooks from local directory: /home/lab
[I 10:01:40.340 NotebookApp] Jupyter Notebook 6.5.2 is running at:
[I 10:01:40.340 NotebookApp] http://master:8888/?token=33f689de85205df46d687e568065e2ebfaebfcadddb65abb
[I 10:01:40.340 NotebookApp]  or http://127.0.0.1:8888/?token=33f689de85205df46d687e568065e2ebfaebfcadddb65abb
[I 10:01:40.340 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:01:40.344 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///home/lab/.local/share/jupyter/runtime/nbserver-13354-open.html
    Or copy and paste one of these URLs:
        http://master:8888/?token=33f689de85205df46d687e568065e2ebfaebfcadddb65abb
     or http://127.0.0.1:8888/?token=33f689de85205df46d687e568065e2ebfaebfcadddb65abb
```

Por lo que podemos usar con esta salida la URL:
```
http://ssdd0.cloud.lab.inf.uc3m.es:8888/?token=33f689de85205df46d687e568065e2ebfaebfcadddb65abb
```
Para conectarnos (estando en la misma red donde master es visible).

Una vez conectados+as estaremos en la página inicial: ![image](jn01-intro.png)
En la página inicial procederemos a crear un nuevo notebook: ![image](jn02-new-notebook.png)
En el nuevo libro tendremos una nueva celda: ![image](jn03-new-cell.png)
En la celda podemos copiar el ejemplo de pi: ![image](jn04-pi.png)
En una nueva celda podemos copiar el número de palabras en local: ![image](jn05-pywc-local.png)
En una nueva celda podemos copiar el número de palabras en remoto: ![image](jn06-pywc-remote.png)


### 3.5 Ejemplo: uso de jupyter notebook con cluster

Se configura los workers de Spark:
```
echo "nodo1" >> spark/conf/workers
echo "nodo2" >> spark/conf/workers
```

Todos los nodos han de poder comunicarse con SSH sin precisar clave, se recomienda este [tutorial de IBM](https://www.ibm.com/support/pages/configuring-ssh-login-without-password):
```
ssh-keygen -t rsa -P ""
ssh-copy-id -i .ssh/id_rsa.pub lab@nodo1
ssh-copy-id -i .ssh/id_rsa.pub lab@nodo2
```

Si no hay una cuenta compartida en todos los nodos entonces se copia el mismo Spark a todos los nodos:
```
scp –r spark acaldero@nodo[1-2]:~/
```

En el nodo master se arranca Spark:
```
./spark/sbin/start-all.sh
```

Para trabajar en una sesión de trabajo, podemos usar pyspark y el siguiente fragmento de código:
```
val spark = SparkSession.builder()
            .appName("EjemploSpark")
            .master("spark://master:7077")
            .getOrCreate()
```
El [tutorial](https://towardsdatascience.com/how-to-connect-jupyter-notebook-to-remote-spark-clusters-and-run-spark-jobs-every-day-2c5a0c1b61df) muestra un entorno parecido.

El programa cambia solo añadiendo ".master("spark://master:7077")" en nuestro ejemplo:
```
import sys
from operator import add
from pyspark.sql import SparkSession

sc = SparkSession.builder.appName("pywc").master("spark://master:7077").getOrCreate()
lines = sc.read.text("/home/lab/lab_spark/2000-0.txt").rdd.map(lambda r: r[0])
counts = lines.flatMap(lambda x: x.split(' ')) \
              .map(lambda x: (x, 1)) \
              .reduceByKey(add)
output = counts.collect()
counts.saveAsTextFile("/home/lab/lab_spark/pg2000-w")
sc.stop()
```

En el nodo master cuando se termine la sesión de trabajo se para Spark usando:
```
./spark/sbin/stop-all.sh
```



## Bibliografía de ejemplos de Spark

* [Ejemplos iniciales de uso de Spark](https://spark.apache.org/examples.html)
* [Ejemplos en python](https://github.com/apache/spark/tree/master/examples/src/main/python)


## Agradecimientos

Por último pero no por ello menos importante, agradecer al personal del Laboratorio del Departamento de Informática toda la ayuda prestada para que este laboratorio sea posible.

