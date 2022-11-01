## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## Alta Escalabilidad en Sistemas Distribuidos

### 1. Pre-requisitos

Ha de disponer de:
1. Cuenta en el Laboratorio del Departamento de Informática:
   * Para solicitar la apertura de cuenta siga los pasos indicados en:
     * https://www.lab.inf.uc3m.es/servicios/apertura-de-cuenta/
1. El software MobaXterm (o similar):
   * El Laboratorio del Departamento de Informática dispone de un manual muy recomendable en:
     * https://www.lab.inf.uc3m.es/wp-content/docs/Manual_ConexionSSH.pdf


### 2. Conexión SSH con las máquinas de trabajo

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


### 3. Spark: ejemplos

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


## Entornos

* [Cluster virtual]()
