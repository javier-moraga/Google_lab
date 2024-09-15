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


