## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## HPC en Sistemas Distribuidos


### 1. Conexión SSH con las máquinas de trabajo

* Estando dentro de la Universidad ha de conectarse a la máquina ssddX.cloud.lab.inf.uc3m.es, siendo X = {0, 1, ... 8}:
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


### 2. Ciclo de trabajo habitual

* Se parte de un archivo fuente vacío y el ciclo de trabajo típico es un bucle con los siguientes pasos en cada iteración:
  1 Editar el archivo fuente para añadir funcionalidad.
    * Anotar como comentario lo que se quiere hacer
    * Después de cada comentario añadir el código fuente asociado.
  2 Compilar el archivo fuente a binario.
    * Eliminar errores de compilación.
  3 Ejecutar el archivo binario.
    * Eliminar errores de ejecución.


### 3. Hola mundo en MPI

* Hay que editar un archivo hola.c con un contenido similar a:
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

   // Obtener el número de procesos e identificador del proceso actual (rank)
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
mpicc -g -Wall -c hola.c -o hola.o
mpicc -g -Wall -o hola hola.o
```

* Para ejecutar en la máquina local hay que hacer dos pasos:
  * Ha de crearse un archivo machines con la lista de máquinas (una por línea) que van a ser usadas para ejecutar:
``` bash
cat <<EOF > machines
localhost
localhost
EOF
```
  * Ha de lanzarse la ejecución en las máquinas deseadas usando mpirun:
``` bash
mpirun -np 2 -machinefile machines ./hola
```


* Para ejecutar en dos nodos hay que hacer tres pasos:
  * Ha de crearse un archivo machines con la lista de máquinas (una por línea) que van a ser usadas para ejecutar:
``` bash
cat <<EOF > machines
nodo1
nodo2
EOF
```
  * Ha de tener el ejecutable en todos los nodos
``` bash
scp hola nodo1:~/hola
scp hola nodo2:~/hola
```
  * Ha de lanzarse la ejecución en las máquinas deseadas usando mpirun:
``` bash
mpirun -np 2 -machinefile machines ./hola
    La salida será:
```
``` bash
Hola mundo desde 'nodo1' (rank 0 de 2)
Hola mundo desde 'nodo2' (rank 1 de 2)
```

#### Bibliografía de ejemplos de MPI

* [mpi_hola.c](https://github.com/mpitutorial/mpitutorial/blob/gh-pages/tutorials/mpi-hello-world/code/mpi_hola.c)
* [llamadas colectivas](https://github.com/mpitutorial/mpitutorial/tree/gh-pages/tutorials/mpi-broadcast-and-collective-communication)


