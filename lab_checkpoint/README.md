## Materiales para Sistemas Distribuidos

<html>
<ul>
<li> <a href="https://github.com/acaldero/uc3m_ssdd/blob/main/LICENSE">License</a> </li>
<li> Curso 2023-2024</li>
</ul>
</html>


## Alta Escalabilidad en Sistemas Distribuidos

1 Máquinas de trabajo:
  * [1.1 Pre-requisitos para trabajar en ssddX.cloud.lab.inf.uc3m.es](#11-pre-requisitos-para-trabajar-en-ssddx.cloud.lab.inf.uc3m.es)
  * [1.2 Conexión SSH con las máquinas de trabajo](#11-conexion-ssh-con-las-maquinas-de-trabajo)

2 Software necesario:
  * [2.1 Instalación de Python](#21-instalacion-de-python)

3 Ejemplos para aprender:
  * [3.1 Checkpointing](#31-checkpointing)

[Agradecimientos](#agradecimientos)


## Máquinas de trabajo

### 1. Pre-requisitos para trabajar en ssddX.cloud.lab.inf.uc3m.es

Ha de disponer de:
1. Cuenta en el Laboratorio del Departamento de Informática.
2. El software MobaXterm (o similar).
3. Estar conectado dentro de la red de la Universidad, ya sea con VPN o con conexión a través de guernika.lab.inf.uc3m.es.

Como ayuda:
1. Para solicitar la apertura de cuenta siga los pasos indicados en: https://www.lab.inf.uc3m.es/servicios/apertura-de-cuenta/
2. El Laboratorio del Departamento de Informática dispone de un manual de MobaXterm en: https://www.lab.inf.uc3m.es/wp-content/docs/Manual_ConexionSSH.pdf
3. La información de VPN para la Universidad está en: https://www.uc3m.es/sdic/servicios/vpn


### 2. Conexión SSH con las máquinas de trabajo

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

### 3. Instalación de Python


<html>
<table>

<tr>
<td></td>
<td>Uso de PIP</td>
<td>Uso de Anaconda</td>
</tr>

<tr>
<td>Instalar Python3</td>
<td>
<small><pre>
sudo apt-get install \
     python3-minimal python3-pip
</pre></small>
</td>
<td>
<pre>
wget <sub>https://repo.anaconda.com/archive/Anaconda3-2023.09-0-Linux-x86_64.sh</sub>
chmod a+x Anaconda3-2023.09-0-Linux-x86_64.sh
./Anaconda3-2023.09-0-Linux-x86_64.sh -b
source ~/.profile
conda update --all
conda clean  --all
</pre>
</td>
</tr>

</table>
</html>


No obstante para los ejemplos se usará PIP.


## Ejemplos para aprender

### 3.1 Checkpointing

#### Preparación

Hay que introducir:
```
cd lab_checkpoint
chmod a+x *.sh
./run.sh
```


#### Ejecutar

<html>
<table>
<tr><th>Paso</th><th>Cliente</th></tr>

<tr>
<td>1</td>
<td>

```
./run.sh
+ python3 app.py
iter:  0
iter:  1
iter:  2
iter:  3
iter:  4
🧨 🧨
```

</td>
</tr>

<tr>
<td>2</td>
<td>

```
./run.sh
+ python3 app.py
iter:  4
iter:  5
iter:  6
iter:  7
iter:  8
iter:  9
🧨 🧨
```

</td>
</tr>

</table>
</html>


## Agradecimientos

Por último pero no por ello menos importante, agradecer al personal del Laboratorio del Departamento de Informática toda la ayuda prestada para que este laboratorio sea posible.

