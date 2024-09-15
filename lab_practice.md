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

### 8.- Ejecuta la canalización de Apache Beam

**8.1 Vuelve a tu sesión de Cloud Shell para realizar el siguiente paso. Ahora, configurarás las bibliotecas de Python requeridas.**

El trabajo de Dataflow en este lab requerirá Python3.8. Para garantizar que estás utilizando la versión correcta, ejecutarás los procesos de Dataflow en un contenedor de Docker Python 3.8.

**8.2 Ejecuta lo siguiente en Cloud Shell para iniciar un contenedor de Python:**

```
docker run -it -e PROJECT=$PROJECT -v $(pwd)/dataflow-python-examples:/dataflow python:3.8 /bin/bash
```

Con este comando, se obtendrá un contenedor de Docker con la versión estable más reciente de Python 3.8 y se ejecutará una shell de comando para ejecutar los siguientes comandos en el contenedor. La marca -v proporciona el código fuente como un volumen para el contenedor, de manera que podamos utilizar el editor de Cloud Shell y, aun así, acceder a él dentro del contenedor en ejecución.

**8.3 Una vez que el contenedor termine la extracción y comience a ejecutarse en Cloud Shell, ejecuta lo siguiente para instalar apache-beam en ese contenedor en ejecución:**

```
pip install apache-beam[gcp]==2.24.0
```

**8.4 Luego, en el contenedor en ejecución en Cloud Shell, cambia al directorio en el que vinculaste el código fuente:**

```
cd dataflow/
```

**8.5 Ejecuta la canalización de DataFlow de transferencia en la nube**

Lo que se muestra a continuación iniciará los trabajadores requeridos y los cerrará cuando el proceso haya finalizado:

```
python dataflow_python_examples/data_ingestion.py \
  --project=$PROJECT --region=us-central1 \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --input gs://$PROJECT/data_files/head_usa_names.csv \
  --save_main_session
```

**8.6 Vuelve a la consola de Cloud y abre el menú de navegación > Dataflow para ver el estado de tu trabajo.**

**8.7 Haz clic en el nombre del trabajo para ver el progreso. Una vez que el Estado del trabajo sea Sin errores, puedes continuar con el siguiente paso. Esta canalización de Dataflow tardará aproximadamente cinco minutos en comenzar, completar el trabajo y, luego, apagarse.**

**8.8 Navega a BigQuery (menú de navegación > BigQuery) para verificar si tus datos se propagaron.**

**8.9 Haz clic en el nombre de tu proyecto para ver la tabla usa_names en el conjunto de datos lake.**

**8.10 Haz clic en el nombre de tu proyecto para ver la tabla usa_names en el conjunto de datos lake.**

Se valida la tarea de progreso ✅

### 9.- Transforma los datos

**Ahora, crearás una canalización de Dataflow con una fuente de TextIO y un destino de BigQueryIO para transferir datos a BigQuery. En específico, harás lo siguiente:**

• Transferir los archivos desde Cloud Storage
• Convertir las líneas leídas en objetos del diccionario
• Transformar los datos que contengan el año a un formato que BigQuery entienda como una fecha
• Enviar las filas a BigQuery

**Codigo**

```
# Copyright 2017 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

""" data_transformation.py is a Dataflow pipeline which reads a file and writes
its contents to a BigQuery table.

This example reads a json schema of the intended output into BigQuery,
and transforms the date data to match the format BigQuery expects.
"""


import argparse
import csv
import logging
import os

import apache_beam as beam
from apache_beam.options.pipeline_options import PipelineOptions
from apache_beam.io.gcp.bigquery_tools import parse_table_schema_from_json


class DataTransformation:
    """A helper class which contains the logic to translate the file into a
  format BigQuery will accept."""

    def __init__(self):
        dir_path = os.path.dirname(os.path.realpath(__file__))
        self.schema_str = ''
        # Here we read the output schema from a json file.  This is used to specify the types
        # of data we are writing to BigQuery.
        schema_file = os.path.join(dir_path, 'resources', 'usa_names_year_as_date.json')
        with open(schema_file) \
                as f:
            data = f.read()
            # Wrapping the schema in fields is required for the BigQuery API.
            self.schema_str = '{"fields": ' + data + '}'

    def parse_method(self, string_input):
        """This method translates a single line of comma separated values to a
    dictionary which can be loaded into BigQuery.

        Args:
            string_input: A comma separated list of values in the form of
            state_abbreviation,gender,year,name,count_of_babies,dataset_created_date
                example string_input: KS,F,1923,Dorothy,654,11/28/2016

        Returns:
            A dict mapping BigQuery column names as keys to the corresponding value
            parsed from string_input.  In this example, the data is not transformed, and
            remains in the same format as the CSV.  There are no date format transformations.

                example output:
                      {'state': 'KS',
                       'gender': 'F',
                       'year': '1923-01-01', <- This is the BigQuery date format.
                       'name': 'Dorothy',
                       'number': '654',
                       'created_date': '11/28/2016'
                       }
        """
        # Strip out return characters and quote characters.
        schema = parse_table_schema_from_json(self.schema_str)

        field_map = [f for f in schema.fields]

        # Use a CSV Reader which can handle quoted strings etc.
        reader = csv.reader(string_input.split('\n'))
        for csv_row in reader:
            # Our source data only contains year, so default January 1st as the
            # month and day.
            month = '01'
            day = '01'
            # The year comes from our source data.
            year = csv_row[2]

            row = {}
            i = 0
            # Iterate over the values from our csv file, applying any transformation logic.
            for value in csv_row:
                # If the schema indicates this field is a date format, we must
                # transform the date from the source data into a format that
                # BigQuery can understand.
                if field_map[i].type == 'DATE':
                    # Format the date to YYYY-MM-DD format which BigQuery
                    # accepts.
                    value = '-'.join((year, month, day))

                row[field_map[i].name] = value
                i += 1

            return row


def run(argv=None):
    """The main function which creates the pipeline and runs it."""
    parser = argparse.ArgumentParser()
    # Here we add some specific command line arguments we expect.   Specifically
    # we have the input file to load and the output table to write to.
    parser.add_argument(
        '--input', dest='input', required=False,
        help='Input file to read.  This can be a local file or '
             'a file in a Google Storage Bucket.',
        # This example file contains a total of only 10 lines.
        # It is useful for developing on a small set of data
        default='gs://spls/gsp290/data_files/head_usa_names.csv')
    # This defaults to the temp dataset in your BigQuery project.  You'll have
    # to create the temp dataset yourself using bq mk temp
    parser.add_argument('--output', dest='output', required=False,
                        help='Output BQ table to write results to.',
                        default='lake.usa_names_transformed')

    # Parse arguments from the command line.
    known_args, pipeline_args = parser.parse_known_args(argv)
    # DataTransformation is a class we built in this script to hold the logic for
    # transforming the file into a BigQuery table.
    data_ingestion = DataTransformation()

    # Initiate the pipeline using the pipeline arguments passed in from the
    # command line.  This includes information like where Dataflow should
    # store temp files, and what the project id is.
    p = beam.Pipeline(options=PipelineOptions(pipeline_args))
    schema = parse_table_schema_from_json(data_ingestion.schema_str)

    (p
     # Read the file.  This is the source of the pipeline.  All further
     # processing starts with lines read from the file.  We use the input
     # argument from the command line.  We also skip the first line which is a
     # header row.
     | 'Read From Text' >> beam.io.ReadFromText(known_args.input,
                                                skip_header_lines=1)
     # This stage of the pipeline translates from a CSV file single row
     # input as a string, to a dictionary object consumable by BigQuery.
     # It refers to a function we have written.  This function will
     # be run in parallel on different workers using input from the
     # previous stage of the pipeline.
     | 'String to BigQuery Row' >> beam.Map(lambda s:
                                            data_ingestion.parse_method(s))
     | 'Write to BigQuery' >> beam.io.Write(
        beam.io.BigQuerySink(
            # The table name is a required argument for the BigQuery sink.
            # In this case we use the value passed in from the command line.
            known_args.output,
            # Here we use the JSON schema read in from a JSON file.
            # Specifying the schema allows the API to create the table correctly if it does not yet exist.
            schema=schema,
            # Creates the table in BigQuery if it does not yet exist.
            create_disposition=beam.io.BigQueryDisposition.CREATE_IF_NEEDED,
            # Deletes all data in the BigQuery table before writing.
            write_disposition=beam.io.BigQueryDisposition.WRITE_TRUNCATE)))
    p.run().wait_until_finish()


if __name__ == '__main__':
    logging.getLogger().setLevel(logging.INFO)
    run()
```


### 10. Ejecuta la canalización de transformación de Dataflow

Ejecutarás la canalización de Dataflow en la nube. Esto iniciará los trabajadores requeridos y los cerrará cuando el proceso haya finalizado:

#### 10.1 Para hacerlo, ejecuta los siguientes comandos:

```
python dataflow_python_examples/data_transformation.py \
  --project=$PROJECT \
  --region=us-central1 \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --input gs://$PROJECT/data_files/head_usa_names.csv \
  --save_main_session
```

#### Navega al menú de navegación > Dataflow y haz clic en el nombre del trabajo para ver su estado. Esta canalización de Dataflow tardará aproximadamente cinco minutos en comenzar, completar el trabajo y, luego, apagarse.

#### Una vez que el Estado del trabajo sea Sin errores en la pantalla de estado del trabajo de Dataflow, navega a BigQuery para verificar que se hayan propagado tus datos.

#### Deberías ver la tabla usa_names_transformed debajo del conjunto de datos lake.

#### Haz clic en la tabla y navega hasta la pestaña Vista previa para ver ejemplos de los datos de usa_names_transformed.

Se valida la tarea de progreso ✅

### 11. Enriquece los datos

Ahora, crearás una canalización de Dataflow con una fuente de TextIO y un destino de BigQueryIO para transferir datos a BigQuery. En específico, harás lo siguiente:

• Transferir los archivos desde Cloud Storage
• Filtrar la fila del encabezado en los archivos
• Convertir las líneas leídas en objetos del diccionario
• Enviar las filas a BigQuery

### 12. Revisa el código de Python de la canalización de enriquecimiento de datos

#### En el Editor de código, abre el archivo data_enrichment.py.

```
# Copyright 2017 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

""" data_enrichment.py demonstrates a Dataflow pipeline which reads a file and
 writes its contents to a BigQuery table.  Along the way, data from BigQuery
 is read in as a side input and joined in with the primary data from the file.

"""


import argparse
import csv
import logging
import os
import sys

import apache_beam as beam
from apache_beam.io.gcp import bigquery
from apache_beam.io.gcp.bigquery import parse_table_schema_from_json
from apache_beam.options.pipeline_options import PipelineOptions
from apache_beam.pvalue import AsDict


class DataIngestion(object):
    """A helper class which contains the logic to translate the file into a
  format BigQuery will accept."""

    def __init__(self):
        dir_path = os.path.dirname(os.path.realpath(__file__))
        self.schema_str = ''
        # This is the schema of the destination table in BigQuery.
        schema_file = os.path.join(dir_path, 'resources', 'usa_names_with_full_state_name.json')
        with open(schema_file) \
                as f:
            data = f.read()
            # Wrapping the schema in fields is required for the BigQuery API.
            self.schema_str = '{"fields": ' + data + '}'

    def parse_method(self, string_input):
        """This method translates a single line of comma separated values to a
    dictionary which can be loaded into BigQuery.

        Args:
            string_input: A comma separated list of values in the form of
            state_abbreviation,gender,year,name,count_of_babies,dataset_created_date
                example string_input: KS,F,1923,Dorothy,654,11/28/2016

        Returns:
            A dict mapping BigQuery column names as keys to the corresponding value
            parsed from string_input.  In this example, the data is not transformed, and
            remains in the same format as the CSV.  There are no date format transformations.

                example output:
                      {'state': 'KS',
                       'gender': 'F',
                       'year': '1923-01-01', <- This is the BigQuery date format.
                       'name': 'Dorothy',
                       'number': '654',
                       'created_date': '11/28/2016'
                       }

     """
        # Strip out return characters and quote characters.
        schema = bigquery.parse_table_schema_from_json(self.schema_str)

        field_map = [f for f in schema.fields]

        # Use a CSV Reader which can handle quoted strings etc.
        reader = csv.reader(string_input.split('\n'))
        for csv_row in reader:
            if (sys.version_info.major < 3.0):
                values = [x.decode('utf8') for x in csv_row]
            else:
                values = csv_row
            # Our source data only contains year, so default January 1st as the
            # month and day.
            month = '01'
            day = '01'
            # The year comes from our source data.
            year = values[2]
            row = {}
            i = 0
            # Iterate over the values from our csv file, applying any transformation logic.
            for value in values:
                # If the schema indicates this field is a date format, we must
                # transform the date from the source data into a format that
                # BigQuery can understand.
                if field_map[i].type == 'DATE':
                    # Format the date to YYYY-MM-DD format which BigQuery
                    # accepts.
                    value = '-'.join((year, month, day))

                row[field_map[i].name] = value
                i += 1

            return row


def run(argv=None):
    """The main function which creates the pipeline and runs it."""
    parser = argparse.ArgumentParser()
    # Here we add some specific command line arguments we expect.   Specifically
    # we have the input file to load and the output table to write to.
    parser.add_argument(
        '--input', dest='input', required=False,
        help='Input file to read.  This can be a local file or '
             'a file in a Google Storage Bucket.',
        # This example file contains a total of only 10 lines.
        # Useful for quickly debugging on a small set of data
        default='gs://spls/gsp290/data_files/head_usa_names.csv')
    # The output defaults to the lake dataset in your BigQuery project.  You'll have
    # to create the lake dataset yourself using this command:
    # bq mk lake
    parser.add_argument('--output', dest='output', required=False,
                        help='Output BQ table to write results to.',
                        default='lake.usa_names_enriched')

    # Parse arguments from the command line.
    known_args, pipeline_args = parser.parse_known_args(argv)

    # DataIngestion is a class we built in this script to hold the logic for
    # transforming the file into a BigQuery table.
    data_ingestion = DataIngestion()

    # Initiate the pipeline using the pipeline arguments passed in from the
    # command line.  This includes information like where Dataflow should store
    #  temp files, and what the project id is
    p = beam.Pipeline(options=PipelineOptions(pipeline_args))
    schema = parse_table_schema_from_json(data_ingestion.schema_str)

    # This function adds in a full state name by looking up the
    # full name in the short_to_long_name_map.  The short_to_long_name_map
    # comes from a read from BigQuery in the next few lines
    def add_full_state_name(row, short_to_long_name_map):
        row['state_full_name'] = short_to_long_name_map[row['state']]
        return row

    # This is a second source of data.  The source is from BigQuery.
    # This will come into our pipeline a side input.

    read_query = """
    SELECT
    name as state_name,
    abbreviation as state_abbreviation
    FROM
    `qwiklabs-resources.python_dataflow_example.state_abbreviations`"""

    state_abbreviations = (
        p
        | 'Read from BigQuery' >> beam.io.Read(
            beam.io.BigQuerySource(query=read_query, use_standard_sql=True))
        # We must create a python tuple of key to value pairs here in order to
        # use the data as a side input.  Dataflow will use the keys to distribute the
        # work to the correct worker.
        | 'Abbreviation to Full Name' >> beam.Map(
            lambda row: (row['state_abbreviation'], row['state_name'])))

    (p
     # Read the file.  This is the source of the pipeline.  All further
     # processing starts with lines read from the file.  We use the input
     # argument from the command line.  We also skip the first line which is
     # a header row.
     | 'Read From Text' >> beam.io.ReadFromText(known_args.input,
                                                skip_header_lines=1)
     # Translates from the raw string data in the CSV to a dictionary.
     # The dictionary is a keyed by column names with the values being the values
     # we want to store in BigQuery.
     | 'String to BigQuery Row' >> beam.Map(lambda s:
                                            data_ingestion.parse_method(s))
     # Here we pass in a side input, which is data that comes from outside our
     # CSV source.  The side input contains a map of states to their full name.
     | 'Join Data' >> beam.Map(add_full_state_name, AsDict(
        state_abbreviations))
     # This is the final stage of the pipeline, where we define the destination
     #  of the data.  In this case we are writing to BigQuery.
     | 'Write to BigQuery' >> beam.io.Write(
        beam.io.BigQuerySink(
            # The table name is a required argument for the BigQuery sink.
            # In this case we use the value passed in from the command line.
            known_args.output,
            # Here we use the JSON schema read in from a JSON file.
            # Specifying the schema allows the API to create the table correctly if it does not yet exist.
            schema=schema,
            # Creates the table in BigQuery if it does not yet exist.
            create_disposition=beam.io.BigQueryDisposition.CREATE_IF_NEEDED,
            # Deletes all data in the BigQuery table before writing.
            write_disposition=beam.io.BigQueryDisposition.WRITE_TRUNCATE)))
    p.run().wait_until_finish()


if __name__ == '__main__':
    logging.getLogger().setLevel(logging.INFO)
    run()
```

#### Se edita la linea 83 del código

```
values = [x.decode('utf8') for x in csv_row]
```

#### Se reemplaza por la siguiente

```
values = [x for x in csv_row]
```

### 12. Ejecuta la canalización de Dataflow de enriquecimiento de datos

Aquí ejecutarás la canalización de Dataflow en la nube.

#### 12.1.- Ejecuta lo siguiente en Cloud Shell para iniciar los trabajadores necesarios y apagarlos cuando se complete la tarea:

```
python dataflow_python_examples/data_enrichment.py \
  --project=$PROJECT \
  --region=us-central1 \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --input gs://$PROJECT/data_files/head_usa_names.csv \
  --save_main_session
```

#### 12.2 Ve al menú de navegación > Dataflow para ver el estado del trabajo. Esta canalización de Dataflow tardará aproximadamente cinco minutos en comenzar, completar el trabajo y, luego, apagarse.

#### 12.3 Una vez que el Estado del trabajo sea Sin errores en la pantalla de estado del trabajo de Dataflow, navega a BigQuery para verificar que se hayan propagado tus datos.

#### 12.4 Deberías ver la tabla usa_names_enriched debajo del conjunto de datos lake.

#### 12.5 Haz clic en la tabla y navega hasta la pestaña Vista previa para ver ejemplos de los datos de usa_names_enriched.

Se valida la tarea de progreso ✅

### 13. Data lake a data mart y revisión del código de Python de la canalización

Ahora, crea una canalización de Dataflow que lee los datos de dos fuentes de datos de BigQuery y, luego, une las fuentes de datos. Específicamente, harás lo siguiente:

• Transferir archivos desde dos fuentes de BigQuery
• Unir las dos fuentes de datos
• Filtrar la fila del encabezado en los archivos
• Convertir las líneas leídas en objetos del diccionario
• Enviar las filas a BigQuery

En el Editor de código, abre el archivo *data_lake_to_mart.py*. Lee los comentarios en el archivo que explican lo que hace el código. Este código unirá dos tablas y propagará los datos resultantes en BigQuery.

*data_lake_to_mart.py*
```
# Copyright 2017 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

""" data_lake_to_mart.py demonstrates a Dataflow pipeline which reads a
large BigQuery Table, joins in another dataset, and writes its contents to a
BigQuery table.
"""


import argparse
import logging
import os
import traceback

import apache_beam as beam
from apache_beam.io.gcp.bigquery import parse_table_schema_from_json
from apache_beam.options.pipeline_options import PipelineOptions
from apache_beam.pvalue import AsDict


class DataLakeToDataMart:
    """A helper class which contains the logic to translate the file into
    a format BigQuery will accept.

    This example uses side inputs to join two datasets together.
    """

    def __init__(self):
        dir_path = os.path.dirname(os.path.realpath(__file__))
        self.schema_str = ''
        # This is the schema of the destination table in BigQuery.
        schema_file = os.path.join(dir_path, 'resources', 'orders_denormalized.json')
        with open(schema_file) as f:
            data = f.read()
            # Wrapping the schema in fields is required for the BigQuery API.
            self.schema_str = '{"fields": ' + data + '}'

    def get_orders_query(self):
        """This returns a query against a very large fact table.  We are
        using a fake orders dataset to simulate a fact table in a typical
        data warehouse."""
        orders_query = """SELECT
            acct_number,
            col_number,
            col_number_1,
            col_number_10,
            col_number_100,
            col_number_101,
            col_number_102,
            col_number_103,
            col_number_104,
            col_number_105,
            col_number_106,
            col_number_107,
            col_number_108,
            col_number_109,
            col_number_11,
            col_number_110,
            col_number_111,
            col_number_112,
            col_number_113,
            col_number_114,
            col_number_115,
            col_number_116,
            col_number_117,
            col_number_118,
            col_number_119,
            col_number_12,
            col_number_120,
            col_number_121,
            col_number_122,
            col_number_123,
            col_number_124,
            col_number_125,
            col_number_126,
            col_number_127,
            col_number_128,
            col_number_129,
            col_number_13,
            col_number_130,
            col_number_131,
            col_number_132,
            col_number_133,
            col_number_134,
            col_number_135,
            col_number_136,
            col_number_14,
            col_number_15,
            col_number_16,
            col_number_17,
            col_number_18,
            col_number_19,
            col_number_2,
            col_number_20,
            col_number_21,
            col_number_22,
            col_number_23,
            col_number_24,
            col_number_25,
            col_number_26,
            col_number_27,
            col_number_28,
            col_number_29,
            col_number_3,
            col_number_30,
            col_number_31,
            col_number_32,
            col_number_33,
            col_number_34,
            col_number_35,
            col_number_36,
            col_number_37,
            col_number_38,
            col_number_39,
            col_number_4,
            col_number_40,
            col_number_41,
            col_number_42,
            col_number_43,
            col_number_44,
            col_number_45,
            col_number_46,
            col_number_47,
            col_number_48,
            col_number_49,
            col_number_5,
            col_number_50,
            col_number_51,
            col_number_52,
            col_number_53,
            col_number_54,
            col_number_55,
            col_number_56,
            col_number_57,
            col_number_58,
            col_number_59,
            col_number_6,
            col_number_60,
            col_number_61,
            col_number_62,
            col_number_63,
            col_number_64,
            col_number_65,
            col_number_66,
            col_number_67,
            col_number_68,
            col_number_69,
            col_number_7,
            col_number_70,
            col_number_71,
            col_number_72,
            col_number_73,
            col_number_74,
            col_number_75,
            col_number_76,
            col_number_77,
            col_number_78,
            col_number_79,
            col_number_8,
            col_number_80,
            col_number_81,
            col_number_82,
            col_number_83,
            col_number_84,
            col_number_85,
            col_number_86,
            col_number_87,
            col_number_88,
            col_number_89,
            col_number_9,
            col_number_90,
            col_number_91,
            col_number_92,
            col_number_93,
            col_number_94,
            col_number_95,
            col_number_96,
            col_number_97,
            col_number_98,
            col_number_99,
            col_number_num1,
            date,
            foo,
            num1,
            num2,
            num3,
            num5,
            num6,
            product_number,
            quantity
        FROM
            `qwiklabs-resources.python_dataflow_example.orders` orders
        LIMIT
            10
        """
        return orders_query

    def add_account_details(self, row, account_details):
        """add_account_details joins two datasets together.  Dataflow passes in the
        a row from the orders dataset along with the entire account details dataset.

        This works because the entire account details dataset can be passed in memory.

        The function then looks up the account details, and adds all columns to a result
        dictionary, which will be written to BigQuery."""
        result = row.copy()
        try:
            result.update(account_details[row['acct_number']])
        except KeyError as err:
            traceback.print_exc()
            logging.error("Account Not Found error: %s", err)
        return result


def run(argv=None):
    """The main function which creates the pipeline and runs it."""
    parser = argparse.ArgumentParser()
    # Here we add some specific command line arguments we expect.   S
    # This defaults the output table in your BigQuery you'll have
    # to create the example_data dataset yourself using bq mk temp
    parser.add_argument('--output', dest='output', required=False,
                        help='Output BQ table to write results to.',
                        default='lake.orders_denormalized_sideinput')

    # Parse arguments from the command line.
    known_args, pipeline_args = parser.parse_known_args(argv)

    # DataLakeToDataMart is a class we built in this script to hold the logic for
    # transforming the file into a BigQuery table.
    data_lake_to_data_mart = DataLakeToDataMart()

    p = beam.Pipeline(options=PipelineOptions(pipeline_args))
    schema = parse_table_schema_from_json(data_lake_to_data_mart.schema_str)
    pipeline = beam.Pipeline(options=PipelineOptions(pipeline_args))

    # This query returns details about the account, normalized into a
    # different table.  We will be joining the data in to the main orders dataset in order
    # to create a denormalized table.
    account_details_source = (
        pipeline
        | 'Read Account Details from BigQuery ' >> beam.io.Read(
            beam.io.BigQuerySource(query="""
                SELECT
                  acct_number,
                  acct_company_name,
                  acct_group_name,
                  acct_name,
                  acct_org_name,
                  address,
                  city,
                  state,
                  zip_code,
                  country
                FROM
                  `qwiklabs-resources.python_dataflow_example.account`""",
                                   # This next stage of the pipeline maps the acct_number to a single row of
                                   # results from BigQuery.  Mapping this way helps Dataflow move your data around
                                   # to different workers.  When later stages of the pipeline run, all results from
                                   # a given account number will run on one worker.
                                   use_standard_sql=True))
        | 'Account Details' >> beam.Map(
            lambda row: (
                row['acct_number'], row
            )))

    orders_query = data_lake_to_data_mart.get_orders_query()
    (p
     # Read the orders from BigQuery.  This is the source of the pipeline.  All further
     # processing starts with rows read from the query results here.
     | 'Read Orders from BigQuery ' >> beam.io.Read(
        beam.io.BigQuerySource(query=orders_query, use_standard_sql=True))
     # Here we pass in a side input, which is data that comes from outside our
     # main source.  The side input contains a map of states to their full name
     | 'Join Data with sideInput' >> beam.Map(data_lake_to_data_mart.add_account_details, AsDict(
        account_details_source))
     # This is the final stage of the pipeline, where we define the destination
     # of the data.  In this case we are writing to BigQuery.
     | 'Write Data to BigQuery' >> beam.io.Write(
        beam.io.BigQuerySink(
            # The table name is a required argument for the BigQuery sink.
            # In this case we use the value passed in from the command line.
            known_args.output,
            # Here we use the JSON schema read in from a JSON file.
            # Specifying the schema allows the API to create the table correctly if it does not yet exist.
            schema=schema,
            # Creates the table in BigQuery if it does not yet exist.
            create_disposition=beam.io.BigQueryDisposition.CREATE_IF_NEEDED,
            # Deletes all data in the BigQuery table before writing.
            write_disposition=beam.io.BigQueryDisposition.WRITE_TRUNCATE)))
    p.run().wait_until_finish()


if __name__ == '__main__':
    logging.getLogger().setLevel(logging.INFO)
    run()
```

### 14.- Ejecuta la canalización de Apache Beam para realizar la unión de datos y crear la tabla resultante en BigQuery

Ahora, ejecuta la canalización de Dataflow en la nube.

#### Ejecuta el siguiente bloque de código en Cloud Shell para iniciar los trabajadores necesarios y apagarlos cuando se complete la tarea:

```
python dataflow_python_examples/data_lake_to_mart.py \
  --worker_disk_type="compute.googleapis.com/projects//zones//diskTypes/pd-ssd" \
  --max_num_workers=4 \
  --project=$PROJECT \
  --runner=DataflowRunner \
  --machine_type=e2-standard-2 \
  --staging_location=gs://$PROJECT/test \
  --temp_location gs://$PROJECT/test \
  --save_main_session \
  --region=us-central1
```

#### Navega al menú de navegación > Dataflow y haz clic en el nombre de este nuevo trabajo para ver el estado. Esta canalización de Dataflow tardará aproximadamente cinco minutos en comenzar, completar el trabajo y, luego, apagarse.

#### Una vez que el Estado del trabajo sea Sin errores en la pantalla de estado del trabajo de Dataflow, navega a BigQuery para verificar que se hayan propagado tus datos.

#### Deberías ver la tabla orders_denormalized_sideinput debajo del conjunto de datos lake.

Haz clic en la tabla y navega a la sección Vista previa para ver ejemplos de datos de orders_denormalized_sideinput.

```
Se valida la tarea de progreso ✅
```
