# SQL Murder Mystery

Este proyecto es una solución al misterio de asesinato utilizando SQL.

## Abrir en Colab

<a href="https://colab.research.google.com/github/Mcarrobof24/SQL_Murde_Mystery/blob/main/Camila_Arrobo_SQL_Murder_Mystery.ipynb" target="_parent">
    <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
</a>

Adapted By: Martin Arroyo

Elaborate By: Camila Arrobo

![Detective making connections between points](https://github.com/freestackinitiative/coop_sql_notebooks/blob/main/assets/sleuth.png?raw=1)

**Credit**

This material was adapted from the [SQL Murder Mystery by Knight Lab](https://mystery.knightlab.com/) under [Creative Commons CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/). The SQL Murder Mystery was originally created by [Joon Park](https://twitter.com/joonparkmusic) and [Cathy He](https://twitter.com/Cathy_MeiyingHe) while they were Knight Lab fellows. See the [GitHub repository](https://github.com/NUKnightLab/sql-mysteries) for more information.

## Escenario
¡Se ha cometido un crimen y los detectives necesitan tu ayuda! Te dieron el informe de la escena del crimen, pero de alguna manera lo perdiste. Recuerdas vagamente que el crimen fue un `murder` _(asesinato)_ que ocurrió en algún momento el 15 de Enero de 2018 y que tuvo lugar en `SQL City`. Depende de ti averiguar quién es el asesino usando solo tus habilidades en SQL y tu ingenio. Te proporcionan una conexión a la base de datos del Departamento de Policía, que tiene todas las pistas que necesitarás para atrapar al asesino.

Usa las habilidades que desarrollaste en SQL 101 y 102, junto con cualquier recurso que desees, para resolver el `SQL Murder Mystery!"`

## Conéctate a la base de datos del Departamento de Policía

Para comenzar y ejecutar tus consultas, presiona play en la celda de abajo para conectarte a la base de datos del Departamento de Policía.

Para ejecutar consultas, crea una nueva celda de `Code` y escribe `%%sql` en la parte superior. Luego puedes escribir tus consultas debajo. Ve el ejemplo a continuación:

```python
%%sql

SELECT *
FROM table
```

```python
%%capture
# @title Press Play { display-mode: "form" }
# Install `teachdb` and `coop_grader`
print("Installing `teachdb` and its dependencies...")
!pip install --quiet --upgrade git+https://github.com/freestackinitiative/teachingdb.git git+https://github.com/martinmarroyo/coop_grader.git
print("Successfully installed `teachdb`")
import pandas as pd
from teachdb.teachdb import connect_teachdb
from coop_grader.sql_murder_mystery.check_suspect import check_suspect
# Set configurations for notebook
%load_ext sql
%config SqlMagic.autopandas = True
%config SqlMagic.feedback = False
%config SqlMagic.displaycon = False
pd.set_option('display.max_rows', None)
pd.set_option('display.max_columns', None)
pd.set_option('display.width', None)
pd.set_option('display.max_colwidth', 99)
# Load data
con = connect_teachdb(database="sql_murder_mystery")

%sql con
```

## Descubriendo tablas en la base de datos
Comenzamos nuestra búsqueda para encontrar al asesino explorando la base de datos del Departamento de Policía. Pero aún no has visto la base de datos y no sabes cuáles son las tablas, así que, ¿cómo sabes qué buscar?

Afortunadamente, la mayoría de los sistemas de gestión de bases de datos relacionales tienen esta información almacenada en un lugar donde puedes consultarla. Muy a menudo, se utiliza un esquema especial conocido como [`information_schema`](https://en.wikipedia.org/wiki/Information_schema) para almacenar información sobre las tablas y columnas en tu base de datos (también conocido como metadata). La base de datos del Departamento de Policía tiene un information schema, con la vista `tables` que te muestra qué tablas están disponibles, y la vista `columns` que te muestra todas las columnas de cada tabla y sus tipos de datos.

## Listando todas las tablas en la base de datos de la Policía
Primero, veremos todas las tablas disponibles para nosotros consultando la vista `information_schema.tables`. Te daremos la primera consulta para empezar, pero de aquí en adelante tendrás que idear las consultas restantes utilizando tu conocimiento de SQL y tu ingenio.

Aquí está la consulta necesaria para mostrarte las tablas en la base de datos del Departamento de Policía. Cópiala/Pégala en la celda de abajo y ejecútala para ver las tablas disponibles para ti:

```python
%%sql
SELECT *
FROM information_schema.tables
```

| table_catalog | table_schema | table_name               | table_type | self_referencing_column_name | reference_generation | user_defined_type_catalog | user_defined_type_schema | user_defined_type_name | is_insertable_into | is_typed | commit_action | TABLE_COMMENT |
|---------------|--------------|--------------------------|------------|-----------------------------|----------------------|--------------------------|-------------------------|-----------------------|-------------------|----------|---------------|---------------|
| memory        | main         | crime_scene_report       | BASE TABLE | None                        | None                 | None                     | None                    | None                  | YES               | NO       | None          | None          |
| memory        | main         | drivers_license          | BASE TABLE | None                        | None                 | None                     | None                    | None                  | YES               | NO       | None          | None          |
| memory        | main         | facebook_event_checkin   | BASE TABLE | None                        | None                 | None                     | None                    | None                  | YES               | NO       | None          | None          |
| memory        | main         | get_fit_now_check_in     | BASE TABLE | None                        | None                 | None                     | None                    | None                  | YES               | NO       | None          | None          |
| memory        | main         | get_fit_now_member       | BASE TABLE | None                        | None                 | None                     | None                    | None                  | YES               | NO       | None          | None          |
| memory        | main         | income                   | BASE TABLE | None                        | None                 | None                     | None                    | None                  | YES               | NO       | None          | None          |
| memory        | main         | interview                | BASE TABLE | None                        | None                 | None                     | None                    | None                  | YES               | NO       | None          | None          |
| memory        | main         | person                   | BASE TABLE | None                        | None                 | None                     | None                    |                       | YES               | NO       | None          | None          |
	
