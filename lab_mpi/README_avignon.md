## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> License <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">GNU LGPL 2.1</a> </li>
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
<usuario+a en lab>@avignon.lab.inf.uc3m.es's password: <clave de la cuenta que no se mostrará cuando escriba>
Linux avignon-frontend 5.10.0-18-amd64 #1 SMP Debian 5.10.140-1 (2022-09-02) x86_64
...
```

* A continuación conectarse a la máquina avignon.lab.inf.uc3m.es:
```
ssh -l <usuario+a cuenta en laboratorio> avignon.lab.inf.uc3m.es
<usuario+a en lab>@avignon.lab.inf.uc3m.es's password: <clave de la cuenta que no se mostrará cuando escriba>
Linux avignon-frontend 5.10.0-18-amd64 #1 SMP Debian 5.10.140-1 (2022-09-02) x86_64
...
```

* Una vez que finalice su sesión de trabajo, ha de cerrar cada conexión abierta (avignon, guernika, etc.) usando el mandato exit:
```
exit
```

### 3. Uso del sistema de colas de avignon.lab.inf.uc3m.es

* Hay ciertos casos en los que varios usuarios+as han de ejecutar sus programas de forma que los ordenadores que usen (llamados nodos) se asignen en exclusividad para esa persona (en un mismo nodo no se ejecuten los trabajos de dos o más usuarios+as a la vez).
* Uno de estos casos es la ejecución de una aplicación en el menor tiempo posible, no se quiere tener interferencias de la ejecución de otro programa a la vez.
* Para estos casos se usa un sistema de trabajos en cola, que se encarga de asignar nodos durante un tiempo limitado a las personas que lo usen para que puedan trabajar en exclusividad si lo precisan.

#### 3.1 Lanzar un trabajo en la cola
  a. Compruebe primero que está conectado a avignon.lab.inf.uc3m.es (que es el nodo front-end o nodo de cabecera)
  
  b. Ha de crear un script con todo lo que quiera ejecutar en ese trabajo encolado.
     Por ejemplo, el script do-work.sh tendrá el siguiente contenido:
```
#!/bin/sh
set -x
hostname
```

  c. A continuación ha de usar sbatch para pedir ejecutar el script como trabajo en cola:
```
sbatch do-work.sh
``` 

  d. Puede que tarde en ejecutarse, para ver el estado en la cola puede ejecutar:
```
squeue
```
Y la salida será:
```
   JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
     555       all  do-work acaldero  R       0:02      1 c54
```
Si no encuentra su trabajo en la lista es que posiblemente haya finalizado ya su ejecución.

  e. Tras su ejecución se genera un archivo slurm-XXX.out (siendo XXX un número que identifica a la petición de ejecución, es decir, el JOBID) por lo que el resultado de su ejecución se puede ver con:
```
cat slurm-XXX.out
```

  x. Si por cualquier razón el trabajo entra en un bucle infinito y no termina, tenemos la posibilidad de cancelarlo con scancel y el identificador de trabajo (JOBID):
```
scancel 555
```

#### 3.2 Pedir un trabajo interactivo
  a. Compruebe primero que está conectado a avignon.lab.inf.uc3m.es (que es el nodo front-end o nodo de cabecera)
  
  b. Ha de solicitar una sesión interactiva (es útil para depurar o trabajos cortos interactivos):
```
srun --pty /bin/bash
```

  c. A continuación puede ejecutar los mandatos interactivos que precise:
```
hostname
```

  d. Por último ha de finalizar la sesión interactiva:
```
exit
```


### 4. Ejecución de aplicación paralela en el sistema de colas de avignon.lab.inf.uc3m.es

* Es posible reservar varios nodos para ejecutar un proceso MPI en cada uno de los nodos.
* Todos los procesos ejecutando a la vez forman una aplicación MPI paralela.

#### 3.1 Lanzar un trabajo paralelo en la cola
  a. Compruebe primero que está conectado a avignon.lab.inf.uc3m.es (que es el nodo front-end o nodo de cabecera)
  
  b. Ha de crear un script con la orden de necesaria para ejecutar el trabajo paralelo.
     Por ejemplo, el script do-work.sh tendrá el siguiente contenido:
```
#!/bin/sh
set -x

#SBATCH --job-name=SSDD
#SBATCH --nodes=4
#SBATCH --ntasks-per-node=1
#SBATCH --time=00:20:00

# From https://stackoverflow.com/questions/65439640/hostfile-with-mpirun-on-multinode-with-slurm
scontrol show hostname $SLURM_JOB_NODELIST | perl -ne 'chomb; print "$_"x1' > hostname.txt

mpirun -np 4 -hostfile hostname.txt uname -a
```

  c. A continuación ha de usar sbatch para pedir ejecutar el script como trabajo en cola indicando el número de nodos (4 en este ejemplo):
```
sbatch -N4 do-work.sh
``` 

  d. Puede que tarde en ejecutarse, para ver el estado en la cola puede ejecutar:
```
squeue
```
Y la salida será:
```
   JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
     555       all  do-work acaldero  R       0:02      1 c54
```
Si no encuentra su trabajo en la lista es que posiblemente haya finalizado ya su ejecución.

  e. Tras su ejecución se genera un archivo slurm-XXX.out (siendo XXX un número que identifica a la petición de ejecución, es decir, el JOBID) por lo que el resultado de su ejecución se puede ver con:
```
cat slurm-XXX.out
```

  x. Si por cualquier razón el trabajo entra en un bucle infinito y no termina, tenemos la posibilidad de cancelarlo con scancel y el identificador de trabajo (JOBID):
```
scancel 555
```


