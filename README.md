## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> License <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">GNU LGPL 2.1</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## HPC en Sistemas Distribuidos

### MPI: ejemplos

* [mpi_hello_world.c](https://github.com/mpitutorial/mpitutorial/blob/gh-pages/tutorials/mpi-hello-world/code/mpi_hello_world.c)
* [llamadas colectivas](https://github.com/mpitutorial/mpitutorial/tree/gh-pages/tutorials/mpi-broadcast-and-collective-communication)

### Entornos

* [Cluster virtual]()


## Alta Escalabilidad en Sistemas Distribuidos

### Spark: ejemplos

#### Ejemplo de uso de CPU para reparto de trabajo con Spark (cálculo del número PI):

* calculo-pi.ipynb
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
* pg2000.ipynb
``` python
from pyspark import SparkContext
sc = SparkContext.getOrCreate()
myRDD = sc.textFile("c:\\\\temp\\\\pg2000.txt")
words = myRDD.flatMap(lambda line: line.split(" ")) \
             .map(lambda word: (word, 1)) \
             .reduceByKey(lambda a, b: a + b)
words.saveAsTextFile("c:\\\\temp\\\\pg2000-w.txt")
```

#### Bibliografía de ejemplos de Spark

* [Ejemplos iniciales de uso de Spark](https://spark.apache.org/examples.html)


### Entornos

* [Cluster virtual]()
