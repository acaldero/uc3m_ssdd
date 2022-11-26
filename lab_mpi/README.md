## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## Laboratorio de HPC en Sistemas Distribuidos

* Máquinas de trabajo
1. [Pre-requisitos para trabajar en ssddX.cloud.lab.inf.uc3m.es](#pre-requisitos-para-trabajar-en-ssddx.cloud.lab.inf.uc3m.es)
2. [Conexión SSH con las máquinas de trabajo](#conexion-ssh-con-las-maquinas-de-trabajo)

* Software necesario
1. [Instalación del paquete de desarrollo](#instalacion-del-paquete-de-desarrollo)
2. [Instalación de MPI](#instalacion-de-mpi)

* Ejemplos para aprender
1. [Hola mundo en MPI](#hola-mundo-en-mpi)

* [Agradecimientos](#agradecimientos)




## Máquinas de trabajo

### Pre-requisitos para trabajar en ssddX.cloud.lab.inf.uc3m.es

Ha de disponer de:
1. Cuenta en el Laboratorio del Departamento de Informática.
2. El software MobaXterm (o similar).
3. Estar conectado dentro de la red de la Universidad, ya sea con VPN o con conexión a través de guernika.lab.inf.uc3m.es.

Como ayuda:
1. Para solicitar la apertura de cuenta siga los pasos indicados en: https://www.lab.inf.uc3m.es/servicios/apertura-de-cuenta/
2. El Laboratorio del Departamento de Informática dispone de un manual de MobaXterm en: https://www.lab.inf.uc3m.es/wp-content/docs/Manual_ConexionSSH.pdf
3. La información de VPN para la Universidad está en: https://www.uc3m.es/sdic/servicios/vpn

### Conexión SSH con las máquinas de trabajo

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

### Instalación del paquete de desarrollo

Instalar compilador y depurador:
```
apt-get install build-essential
```

Instalar software adicional de apoyo:
```
apt-get install gdb ddd valgrind cgdb ddd-doc
```

### Instalación de MPI

Instalar una implementación de MPI:
```
apt-get install mpi-default-bin mpi-default-dev
```

Instalar software adicional de apoyo:
```
apt-get install valgrind-mpi xmpi
```


## Ejemplos para aprender

Se parte de un archivo fuente vacío y el ciclo de trabajo típico es un bucle con los siguientes pasos en cada iteración:

 1 Editar el archivo fuente para añadir funcionalidad.
   * Anotar como comentario lo que se quiere hacer
   * Después de cada comentario añadir el código fuente asociado.

 2 Compilar el archivo fuente a binario.
   * Eliminar errores de compilación.

 3 Ejecutar el archivo binario.
   * Eliminar errores de ejecución.


### Hola mundo en MPI

Ejemplo de "hola mundo" en MPI:
1. [Editar](#1-editar)
2. [Compilar](#2-compilar)
3. [Ejecutar (local)](#3-ejecutar-local)
3. [Ejecutar (remoto)](#4-ejecutar-remoto)

#### 1. Editar

Hay que editar un archivo [hola.c](hola.c) con un contenido similar a:
``` C
#include <stdio.h>
#include <mpi.h>

int main(int argc, char** argv)
{
   int world_size;
   int world_rank;
   char processor_name[MPI_MAX_PROCESSOR_NAME];
   int name_len;

   // Primera llamada MPI: inicializar la implementación
   MPI_Init(&argc, &argv);

   // Obtener el número de procesos e identificador del proceso actual (rank)
   MPI_Comm_size(MPI_COMM_WORLD, &world_size);
   MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);

   // Obtener el nombre de la máquina
   MPI_Get_processor_name(processor_name, &name_len);

   // Imprimir hola mundo...
   printf("Hola mundo desde '%s' (rank %d de %d)\n",
          processor_name, world_rank, world_size);

   // Última llamada MPI a usar en el programa
   MPI_Finalize();
}
```


#### 2. Compilar

Para compilar hay que usar mpicc:
``` bash
mpicc -g -Wall -c hola.c -o hola.o
mpicc -g -Wall -o hola hola.o
```


#### 3. Ejecutar (local)

Para ejecutar en la máquina local hay que hacer dos pasos:
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


#### 4. Ejecutar (remoto)

Para ejecutar en dos nodos hay que hacer tres pasos:
  * Ha de crearse un archivo machines con la lista de máquinas (una por línea) que van a ser usadas para ejecutar:
``` bash
cat <<EOF > machines
nodo1
nodo2
EOF
```
  * Ha de tener el ejecutable en todos los nodos:
``` bash
scp hola nodo1:~/hola
scp hola nodo2:~/hola
```
  * Ha de lanzarse la ejecución en las máquinas deseadas usando mpirun:
``` bash
mpirun -np 2 -machinefile machines ./hola
```
La salida será:
``` bash
Hola mundo desde 'nodo1' (rank 0 de 2)
Hola mundo desde 'nodo2' (rank 1 de 2)
```


## Bibliografía de ejemplos de MPI

* [mpi_hola.c](https://github.com/mpitutorial/mpitutorial/tree/gh-pages/tutorials/mpi-hello-world/code)
* [llamadas colectivas](https://github.com/mpitutorial/mpitutorial/tree/gh-pages/tutorials/mpi-broadcast-and-collective-communication)


## Agradecimientos

Por último pero no por ello menos importante, agradecer al personal del Laboratorio del Departamento de Informática toda la ayuda prestada para que este laboratorio sea posible.

