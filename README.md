# Auditoría y Ejecución Distribuida en Apache Hadoop (HDFS + YARN)

Repositorio correspondiente a la práctica de laboratorio sobre arquitectura, almacenamiento distribuido y procesamiento MapReduce en un clúster de Apache Hadoop desplegado sobre contenedores Docker.

## 1. Arquitectura y Configuración del Clúster

El entorno se ha desplegado mediante Docker Compose con los siguientes servicios:

***Almacenamiento (HDFS):***

- **namenode**: Nodo maestro encargado de gestionar los metadatos y el árbol de directorios (http://localhost:9870).

- **datanode1, datanode2, datanode3**: Nodos trabajadores responsables del almacenamiento físico de bloques.

***Cómputo y Recursos (YARN / MapReduce):***

- **resourcemanager**: Planificador y gestor global de recursos (http://localhost:8088).

- **nodemanager: Gestor de contenedores de ejecución en los nodos trabajadores.

***Parámetros clave del entorno:***

- **Tamaño de bloque HDFS (dfs.blocksize)**: 64 MiB (67108864 bytes)

- **Factor de replicación (dfs.replication)**: 2

- **Memoria total por NodeManager**: 2048 MB

## 2. Estructura del Repositorio

.

├── README.md          # Presentación y guía rápida del repositorio

├── INFORME.pdf         # Memoria técnica completa con resolución de cuestiones y evidencias

├── img/               # Capturas de pantalla de la consola e interfaces web (:9870 y :8088)

└── docker/            # Archivos de configuración del clúster (si aplica)


## 3. Comandos Principales Ejecutados

### 3.1. Auditoría de bloques y replicación en HDFS

#### Comprobar integridad, división en bloques y ubicación física de réplicas
<code>hdfs fsck /datos/auditoria/telemetria_bruta.dat -files -blocks -locations</code>

#### Consultar estado de los DataNodes y capacidad del clúster
<code>hdfs dfsadmin -report</code>

--------------------------

### 3.2. Simulación de caída de nodo y tolerancia a fallos

#### Detener un DataNode desde el equipo anfitrión
<code>docker stop datanode2</code>

#### Verificar integridad y tamaño total tras la caída de un nodo
<code>hdfs dfs -cat /datos/auditoria/telemetria_bruta.dat | wc -c</code>

--------------------------

### 3.3. Ejecución de MapReduce (WordCount)

#### Lanzar trabajo WordCount ajustando memoria de contenedores en YARN
<code>hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-*.jar wordcount \
  -D yarn.app.mapreduce.am.resource.mb=512 \
  -D mapreduce.map.memory.mb=512 \
  -D mapreduce.reduce.memory.mb=512 \
  /datos/wordcount/input /datos/wordcount/output</code>

#### Verificar archivos de salida (_SUCCESS y part-r-00000)
<code>hdfs dfs -ls /datos/wordcount/output</code>


## 4. Autoría

Alumno/a: Leandro Ligero Picón

Asignatura: Big Data Aplicado (BDA)

Fecha: Noviembre 2026