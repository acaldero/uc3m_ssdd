## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## HPC en Sistemas Distribuidos

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


### 3. Hola mundo en MPI

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
mpicc -g -Wall -c hello_world.c -o hello_world.o
mpicc -g -Wall -o hello_world hello_world.o
```

* Ha de crear un archivo machines con la lista de máquinas (una por línea) que van a ser usadas para ejecutar:
``` bash
cat <<EOF > machines
localhost
localhost
EOF
```

* Para ejecutar en unas máquinas accesibles por ssh se puede usar mpirun:
``` bash
mpirun -np 2 -machinefile machines ./hello_world
```

#### Bibliografía de ejemplos de MPI

* [mpi_hello_world.c](https://github.com/mpitutorial/mpitutorial/blob/gh-pages/tutorials/mpi-hello-world/code/mpi_hello_world.c)
* [llamadas colectivas](https://github.com/mpitutorial/mpitutorial/tree/gh-pages/tutorials/mpi-broadcast-and-collective-communication)


