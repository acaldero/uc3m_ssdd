## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## Alta Escalabilidad en Sistemas Distribuidos

### 1. Instalación de software (si no está ya instalado)

#### A. Instalación Apache Spark

Instalar dependencias:
```
sudo apt-get install ssh rsync
sudo apt-get install default-jdk
```

Instalar Apache Spark:
```
wget https://www.apache.org/dyn/closer.lua/spark/spark-3.3.1/spark-3.3.1-bin-hadoop3.tgz
tar zxf spark-3.3.1-bin-hadoop3.tgz
ln -s spark-3.3.1-bin-hadoop3 spark
```

Prueba básica:
```
./spark/bin/run-example SparkPi 5
```

#### B. Instalación Anaconda

Instalar Anaconda:
```
wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh
chmod a+x Anaconda3-2022.10-Linux-x86_64.sh
./Anaconda3-2022.10-Linux-x86_64.sh -b
source ~/.profile
conda update --all
conda clean  --all
```

#### C. Instalación Jupyter notebook

Instalar jupyter:
```
apt-get install jupyter-notebook
```

Instalar jupyter con conda:
```
conda install jupyter
```

Prueba básica:
```
jupyter notebook
```


### 2. Spark: ejemplos

#### Ejemplo de uso de CPU para reparto de trabajo con Spark (cálculo del número PI):

* calculo-pi.py
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

#### Ejemplo de procesar datos (contar ocurrencia de palabras en fichero):

* [pg2000.txt](https://www.gutenberg.org/files/2000/2000-0.txt)
* pg2000.py
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

#### Bibliografía de ejemplos de Spark

* [Ejemplos iniciales de uso de Spark](https://spark.apache.org/examples.html)
* [Ejemplos en python](https://github.com/apache/spark/tree/master/examples/src/main/python)

