## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## Alta Escalabilidad en Sistemas Distribuidos


### 1. Conexión SSH con las máquinas de trabajo

* Si está fuera de la Universidad entonces una opción es primero conectarse a la máquina guernika.lab.inf.uc3m.es:
```
ssh -l <usuario+a cuenta en laboratorio> guernika.lab.inf.uc3m.es
<usuario+a en lab>@avignon.lab.inf.uc3m.es's password: <clave que no se mostrará cuando escriba>
Linux guernika...
...
```

* Si está dentro de la Universidad (ya sea con VPN o con conexión a guernika), a continuación ha de conectarse a la máquina avignon.lab.inf.uc3m.es:
```
ssh -l <usuario+a cuenta en laboratorio> avignon.lab.inf.uc3m.es
<usuario+a en lab>@avignon.lab.inf.uc3m.es's password: <clave que no se mostrará cuando escriba>
Linux avignon-frontend...
...
```

* A continuación conectarse a la máquina ssdd0.cloud.lab.inf.uc3m.es:
```
ssh  lab@avignon.lab.inf.uc3m.es
lab@ssdd0.lab.inf.uc3m.es's password: <clave que no se mostrará cuando escriba>
Linux ssdd0...
...
```

* Una vez que finalice su sesión de trabajo, ha de cerrar cada conexión abierta (ssdd0, avignon, guernika, etc.) usando el mandato exit:
```
exit
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

