## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2022-2023</li>
</ul>
</html>


## Laboratorios complementarios: pre-requisitos generales

Ha de disponer de un ordenador propio o acceso remoto a un ordenador.
Como ordenador vale una máquina real, una máquina virtual o un contenedor.
Se recomienda tener acceso a tres ordenadores interconectados entre ellos con SSH.

Para seguir los laboratorios se precisa que, en cualquier caso, los ordenadores tengan:
* Instalada una distribución Linux Ubuntu 22.04 LTS o compatible.
* Al menos 2 GiB de RAM y 20 GiB de disco duro.
* Acceso por SSH.


## Pre-requisitos para trabajar en ssddX.cloud.lab.inf.uc3m.es

Ha de disponer de:
1. Cuenta en el Laboratorio del Departamento de Informática.
2. El software MobaXterm (o similar).
3. Estar conectado dentro de la red de la Universidad, ya sea con VPN o con conexión a través de guernika.lab.inf.uc3m.es.

Como ayuda:
1. Para solicitar la apertura de cuenta siga los pasos indicados en: https://www.lab.inf.uc3m.es/servicios/apertura-de-cuenta/
2. El Laboratorio del Departamento de Informática dispone de un manual de MobaXterm en: https://www.lab.inf.uc3m.es/wp-content/docs/Manual_ConexionSSH.pdf
3. La información de VPN para la Universidad está en: https://www.uc3m.es/sdic/servicios/vpn


## Conexión SSH con las máquinas de trabajo

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


### Laboratorios complementarios propuestos:

* [HPC en Sistemas Distribuidos](https://github.com/acaldero/uc3m_ssdd/blob/main/lab_mpi/README_cloud.md)
* [Alta Escalabilidad en Sistemas Distribuidos](https://github.com/acaldero/uc3m_ssdd/blob/main/lab_spark/README_cloud.md)

