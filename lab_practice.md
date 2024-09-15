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
