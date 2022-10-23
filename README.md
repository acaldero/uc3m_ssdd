## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## HPC en Sistemas Distribuidos

### Hola mundo en MPI

* Hay que editar un archivo hello_world.c con un contenido similar a:
``` C
#include <stdio.h>
#include <mpi.h>


int main(int argc, char** argv)
{
   int world_size;
   int world_rank;
   char processor_name[MPI_MAX_PROCESSOR_NAME];
   int name_len;

   MPI_Init(&argc, &argv);

   // Obtener el número de procesos e identificador del actual proceso (rank)
   MPI_Comm_size(MPI_COMM_WORLD, &world_size);
   MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

   // Obtener el nombre de la máquina
   MPI_Get_processor_name(processor_name, &name_len);

   // Imprimir hola mundo...
   printf("Hola mundo desde '%s' (rank %d de %d)\n",
          processor_name, world_rank, world_size);

   MPI_Finalize();
}
```

* Para compilar hay que usar mpicc:
``` bash
mpicc -g -Wall -c hello_world.c -o hello_world.c
mpicc -g -Wall -o hello_world
```

* Para ejecutar en unas máquinas accesibles por ssh se puede usar mpirun:
``` bash
cat <<EOF > machines
#!/bin/bash
localhost
localhost
EOF

mpirun -np 2  hello_world
```

#### Bibliografía de ejemplos de MPI

* [mpi_hello_world.c](https://github.com/mpitutorial/mpitutorial/blob/gh-pages/tutorials/mpi-hello-world/code/mpi_hello_world.c)
* [llamadas colectivas](https://github.com/mpitutorial/mpitutorial/tree/gh-pages/tutorials/mpi-broadcast-and-collective-communication)



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


## Entornos

* [Cluster virtual]()
