Curso

### 1.- abrir cloud shell

#### 1.1.- Validar cuenta activa
Ingresar el siguiente comando

```
gcloud auth list
```

##### Resultado: 
```
student_04_7b33df48a056@cloudshell:~ (qwiklabs-gcp-02-6f263c0fe357)$ gcloud auth list
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
student_04_7b33df48a056@cloudshell:~ (qwiklabs-gcp-02-6f263c0fe357)$ gcloud config list project
[core]
project = qwiklabs-gcp-02-6f263c0fe357

Your active configuration is: [cloudshell-11781]
```



