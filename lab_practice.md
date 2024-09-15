Curso

### 1.- abrir cloud shell

#### 1.1.- Validar cuenta activa
Ingresar el siguiente comando

```
gcloud auth list
```

##### Resultado: 
```
Credentialed Accounts

ACTIVE: *
ACCOUNT: student-04-7b33df48a056@qwiklabs.net

To set the active account, run:
    $ gcloud config set account `ACCOUNT`
```

### 1.2- Solicitar el ID del proyecto

#### Escribir el codigo: 

```
gcloud config list project
```

##### Resultado:

```
student_04_7b33df48a056@cloudshell:~ (qwiklabs-gcp-03-1b80db7af537)$ gcloud config list project
[core]
project = qwiklabs-gcp-03-1b80db7af537

Your active configuration is: [cloudshell-17100]
```

### 2.- Validar Dataflow

#### 2.1.- Deshabilitar Dataflow API

Se debe deshabilitar buscando Dataflow API, luego seleccionar *Disable API*

#### 2.2.- Habilitar Dataflow API

Se debe volver habilitar la API Dataflow.

Se valida la tarea de progreso ✅

### 3.- Descarga el código de partida

#### 3.1.- Ejecuta el siguiente comando en Cloud Shell para obtener ejemplos de Dataflow para Python del GitHub de servicios profesionales de Google Cloud (https://github.com/GoogleCloudPlatform/professional-services/blob/master/examples/dataflow-python-examples/README.md):

```
Ingresar el siguiente codigo en Google Shell
gsutil -m cp -R gs://spls/gsp290/dataflow-python-examples .
```
#### 3.2.- Establecer una variable igual a nuestra ID del proyecto

```
export PROJECT=qwiklabs-gcp-03-1b80db7af537
```

#### 3.3.- Modificamos finalmente el nombre

```
gcloud config set project $PROJECT
```

##### Resultado

```
student_04_7b33df48a056@cloudshell:~ (qwiklabs-gcp-03-1b80db7af537)$ gcloud config set project $PROJECT
Updated property [core/project].
```

### 4.- Crear un bucket de Cloud Storage

**Usa el comando correspondiente en Cloud Shell para crear un nuevo bucket regional en la región us-central1 dentro de tu proyecto:**

#### Resultado:

```
student_04_7b33df48a056@cloudshell:~ (qwiklabs-gcp-03-1b80db7af537)$ gsutil mb -c regional -l us-central1  gs://$PROJECT
Creating gs://qwiklabs-gcp-03-1b80db7af537/...
```

Se valida la tarea de progreso ✅

### 5.- Copia archivos al bucket

**Usa el comando gsutil en Cloud Shell para copiar archivos al bucket de Cloud Storage que acabas de crear:**

```
gsutil cp gs://spls/gsp290/data_files/usa_names.csv gs://$PROJECT/data_files/
gsutil cp gs://spls/gsp290/data_files/head_usa_names.csv gs://$PROJECT/data_files/
```

#### Resultado: 

```
student_04_7b33df48a056@cloudshell:~ (qwiklabs-gcp-03-1b80db7af537)$ gsutil cp gs://spls/gsp290/data_files/usa_names.csv gs://$PROJECT/data_files/
gsutil cp gs://spls/gsp290/data_files/head_usa_names.csv gs://$PROJECT/data_files/
Copying gs://spls/gsp290/data_files/usa_names.csv [Content-Type=text/csv]...
- [1 files][162.0 MiB/162.0 MiB]                                                
Operation completed over 1 objects/162.0 MiB.                                    
Copying gs://spls/gsp290/data_files/head_usa_names.csv [Content-Type=text/csv]...
/ [1 files][  307.0 B/  307.0 B]                                                
Operation completed over 1 objects/307.0 B.            
```

Se valida la tarea de progreso ✅

### 6.- Crea un conjunto de datos de Bigquery

**En Cloud Shell, crea un conjunto de datos en BigQuery llamado** *lake.* **Todas tus tablas se cargarán en BigQuery aquí:**

```
bq mk lake
```

#### Resultado:

```
student_04_7b33df48a056@cloudshell:~ (qwiklabs-gcp-03-1b80db7af537)$ bq mk lake
Dataset 'qwiklabs-gcp-03-1b80db7af537:lake' successfully created.
```

Se valida la tarea de progreso ✅

### 7.- Crea una canalización de Dataflow

**En esta sección, crearás un Dataflow que solo permite anexar y que transferirá datos a la tabla de BigQuery. Puedes usar el Editor de código incorporado que te permitirá ver y editar el código en la consola de Google Cloud.**

#### 7.1.- Abrir el editor de Cloud Shell

#### 7.2.- Transfiere datos con una canalización de DataFlow

**Ahora, crearás una canalización de Dataflow con una fuente de TextIO y un destino de BigQueryIO para transferir datos a BigQuery. Específicamente, la canalización hará lo siguiente:**

• Transferir los archivos desde Cloud Storage
• Filtrar la fila del encabezado en los archivos
• Convertir las líneas leídas en objetos del diccionario
• Enviar las filas a BigQuery 

#### 7.3.- Revisa el código de Python de la canalización

**En el Editor de código, navega a dataflow-python-examples > dataflow_python_examples y abre el archivo data_ingestion.py. Lee los comentarios en el archivo que explican lo que hace el código. Este código propagará el conjunto de datos lake con una tabla en BigQuery.**

https://cdn.qwiklabs.com/wRFYQrAqdsaCRDuCheahYj6OJeCti0a1%2BWAwuqYtZRE%3D

