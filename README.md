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

Aquí está la consulta necesaria para mostrarte las tablas en la base de datos del Departamento de Policía.

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
	

## Listando todas las tablas y sus columnas en la base de datos de la Policía
¡Genial! Ahora que sabes qué tablas están disponibles, es hora de averiguar las columnas que tiene cada tabla junto con el tipo de datos de cada columna. Escribe una consulta que muestre el nombre de la tabla, el nombre de la columna y el tipo de datos para cada tabla en la base de datos del Departamento de Policía utilizando la vista information_schema.columns. Asegúrate de que la salida esté ordenada por nombre de tabla y luego por nombre de columna (ascendente):

Pistas opcionales - ¡intenta usarlas solo si te quedas REALMENTE atascado!

__Pista 1__: Estructura tu consulta de la misma manera que lo hicimos en la consulta anterior donde miramos `information_schema.tables`

__Pista 2__: Asegúrate de revisar el enlace para la descripción de information_schema.columns. Te dirá los nombres de las columnas que debes usar para la consulta. Recuerda, queremos el nombre de la tabla, el nombre de la columna y el tipo de datos - ¡revisa la descripción para encontrar los nombres apropiados!

__Pista 3__: No olvides usar la declaración ORDER BY para ordenar los resultados de tu búsqueda. Estamos buscando ordenar ambas columnas en orden ascendente, lo cual se puede especificar usando la palabra clave ASC, sin embargo, también es el orden de clasificación predeterminado, por lo que ASC no es necesariamente requerido.

```python
%%sql
SELECT *
FROM information_schema.columns
ORDER BY table_name, column_name
```
## Diagrama de Relaciones de Entidad (Opcional)
Después de hacer un poco de trabajo de detective y encontrar las tablas en la base de datos del Departamento de Policía, descubres que hay un ERD (Diagrama de Relaciones de Entidad). Esto puede ser realmente útil en tu búsqueda para encontrar al asesino. Si lo prefieres, desafíate a ti mismo (y tus habilidades en SQL) a continuar solo consultando el `information_schema` según sea necesario. De lo contrario, puedes hacer clic en el desplegable a continuación para revelar el ERD que te ayudará a ver las tablas y las relaciones en la base de datos del Departamento de Policía de un vistazo:
![imagen](https://camo.githubusercontent.com/88ba7d26819f20ec27e926f525a9b28ebfb30a67f5d0abbfcea1aee593305844/68747470733a2f2f6769746875622e636f6d2f66726565737461636b696e69746961746976652f636f6f705f73716c5f6e6f7465626f6f6b732f626c6f622f6d61696e2f6173736574732f6d75726465725f6d7973746572795f736368656d612e706e673f7261773d31)

## ESPACIO DE TRABAJO
Usa las celdas de abajo para escribir tus consultas y trabajar en resolver el misterio. Cuando tengas un sospechoso, verifica tu respuesta y ejecuta su nombre a través de la función `check_suspect`. Si encuentras al asesino, la función te lo dirá.

## 1. FILTRAR DATOS DE LA TABLAR CRIME_SCENE_REPORT
Se filtran los datos de la tabla __crime_scene_report__ por su tipo (__type__), fecha (__date__) y la ciudad (__city__) donde sucedió el crimen en este caso sería SQL City.
```python
%%sql
SELECT * FROM crime_scene_report
WHERE type= 'murder' AND city = 'SQL City' AND date =20180115
```
| date     | type   | description                                                                                       | city     |
|----------|--------|---------------------------------------------------------------------------------------------------|----------|
| 20180115 | murder | Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave" | SQL City |

## 2. ENCONTRAR INFORMACIÓN DE LOS TESTIGOS DEL ASESINATO
__Encontrar información de los testigos del asesinato__. De acuerdo con las imágenes de seguridad hay 2 testigos. El primer testigo vive en la última casa de "Northwestern Dr" y la segunda testigo se llama Annabel, vive en algún lugar de "Franklin Ave".

```python
%%sql
SELECT * FROM person
WHERE address_street_name IN('Northwestern Dr')
ORDER BY address_number DESC
LIMIT 1;
```

|id |	  name        |	license_id | address_number|	address_street_name |	ssn|
|----|------------------------|------------|-------|--------------|------------|
| 14887 |	Morty Schapiro	| 118009 | 4919 |	Northwestern Dr	| 111564949 |

```python
%%sql
SELECT * FROM person
WHERE name LIKE 'Annabel %' AND address_street_name IN('Franklin Ave')
```
| id |	name |	license_id |	address_number | address_street_name |	ssn |
|-----|------|-----------|-----------|-------------|----------|
| 16371 | Annabel Miller | 490173 | 103 | Franklin Ave | 318771143 |

## 3. EXAMINAR LAS ENTREVISTAS DE LOS TESTIGOS
El primer testigo es Morty Schapiro id = 14887 y la segunda testigo se llama Annabel Miller y su id = 16371. De acuerdo con sus ids se revisa su entrevista.

```python
%%sql
SELECT *
FROM interview JOIN person
ON id=person_id
WHERE id IN (14887, 16371)
```
| person_id | transcript | id | name | license_id | address_number | address_street_name | ssn |
|---------|-----------|-----|-------|-----------|----------|---------|------|
| 14887 | I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W". | 14887 | Morty Schapiro | 118009 | 4919 |	Northwestern Dr | 111564949 |
| 16371 | I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th. | 16371 | Annabel Miller | 490173 | 103 | Franklin Ave | 318771143 |

## 4. ANALIZANDO LAS PISTAS
De acuerdo con las entrevistas a los testigos el asesino es un hombre que asiste al gimnasio. Llevaba una bolsa con número de membresía __gold que comenzaba con "48Z" y placa del auto "H42W"__. El sospechoso asistió al gimnasio el __09 de enero.__

```python
%%sql
SELECT X.id, X.person_id, X.name, X.membership_status
FROM get_fit_now_member X
JOIN get_fit_now_check_in Y ON X.id = Y.membership_id
WHERE X.membership_status='gold' AND X.id LIKE '48Z%' AND Y.check_in_date= 20180109
```
| id |	person_id |	name |	membership_status |
|-----|----------|-------------|-----------|
| 48Z7A | 28819 | Joe Germuska | gold |
| 48Z55 | 67318 | Jeremy Bowers | gold |

```python
%%sql
SELECT X.name, X.id, X.license_id, Y.plate_number
FROM person X
JOIN drivers_license Y ON X.license_id=Y.id
WHERE plate_number LIKE 'H42W%'
```
| name | id | license_id | plate_number |
|------|----|---------|---------|
| Maxine Whitely | 78193 | 183779 | H42W0X |

__El sospechoso huyó en el carro de Maxine Whitely.__

## 5. BUSCANDO INFORMACIÓN DE LOS SOSPECHOSOS
De acuerdo a las consultas hay dos sospechosos que cumplen con las pistas brindadas por los testigos, ellos son __Joe Germuska con el person_id= 28819 y Jeremy Bowers con el person_id= 67318.__ El sospechoso se subió al auto de __Maxine Whitely con el person_id= 78193__ para huir de la escena del crimen.

Se requiere revisar las entrevistas de estos tres sospechoso del crimen.

```python
%%sql
SELECT id, name, transcript FROM
interview JOIN person
ON person_id=id
WHERE person_id IN (28819, 67318, 78193)
```
| id | name | transcript |
|-----|---------|----------|
| 67318 | Jeremy Bowers | I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.|

```python
check_suspect("Jeremy Bowers")

Congrats, you found the murderer! But wait, there's more...
If you think you're up for a challenge,try querying the interview transcript of the murderer to find the real villain behind this crime.
If you feel especially confident in your SQL skills, try to complete this final step with no more than 2 queries.
Use this same `check_suspect` function with your new suspect to check your answer.
True
```

## 6. ENCONTRANDO A LA AUTORA INTELECTUAL DEL CRIMEN
Con base a la entrevista realizada a __JEREMY BOWERS__ se conoce que es el asesino a sueldo. Pero existe una autora intelectual del crimen y debemos encontrarla finalmente resolver el caso y darlo por terminado.

La sospechaso es una mujer que mide alrededor 5'5" (65") o 5'7" (67"), tiene cabello rojo y conduce un auto Tesla Modelo S.

```python
%%sql
SELECT *
FROM drivers_license
WHERE gender='female' AND hair_color='red' AND car_make='Tesla' AND car_model='Model S'
```
| id | age | height | eye_color | hair_color | gender | plate_number | car_make	| car_model |
|------|-----|-----|-------|------|-------|-------|--------|-------|
| 202298 | 68 |	66 | green | red | female | 500123 | Tesla | Model S |
| 291182 | 65 |	66 | blue | red | female | 08CM64 | Tesla | Model S |
| 918773 | 48 |	65 | black | red | female | 917UU3 | Tesla | Model S |

```python
%%sql
SELECT *
FROM person X
JOIN drivers_license Y ON X.license_id=Y.id
WHERE Y.id IN (202298, 291182, 918773)
```

| id | name | license_id | address_number | address_street_name | ssn | 	id_1 |	age |	height | eye_color | hair_color | gender | plate_number	| car_make | car_model |
|-------|-----|-----|------|-----|-----|----|-------|------|------|-----|-----|----|-----|-----|
| 78881 | Red Korb | 918773 | 107 | Camerata Dr | 961388910 | 918773 |	48 |	65 |	black |	red |	female | 917UU3 | Tesla | Model S |
| 90700 | Regina George| 291182 | 332 |	Maple Ave | 337169072 |	291182 |	65 |	66 |	blue |	red |	female | 08CM64	| Tesla	| Model S |
| 99716 | Miranda Priestly | 202298 | 1883 | Golden Ave | 987756388 | 202298 |	68 | 66 | green | red |	female | 500123 | Tesla | Model S |

```python
%%sql
SELECT * FROM
facebook_event_checkin
WHERE person_id IN (78881, 90700, 99716)
```
| person_id | event_id | event_name | date |
|----------|--------|--------|------|
| 99716 | 1143 | SQL Symphony Concert |	20171206 |
| 99716 | 1143 | SQL Symphony Concert |	20171212 |
| 99716 | 1143 | SQL Symphony Concert |	20171229 |

```python
%%sql
SELECT X.id, X.name, Y.ssn, Y.annual_income
FROM person X
JOIN income Y ON X.ssn=Y.ssn
WHERE X.ssn= 987756388
```

| id |	name |	ssn |	annual_income |
|------|------|------|-------|
| 99716 | Miranda Priestly |	987756388 |	310000 |

```python
check_suspect("Miranda Priestly")
Congrats, you found the brains behind the murder!
Everyone in SQL City hails you as the greatest SQL detective of all time.
Time to break out the champagne!
True
```

### CASO RESUELTO
__Miranda Priestly es la autora intelectual del asesinato__. Es una mujer millonaria, que tiene cabello rojo, posee un auto marca Tesla Model S. Además que según registros en la tabla facebook_event_checkin asistió al evento SQL Symphony Concert tres veces en diciembre del 2017. Cumple con todas las características descrita por el asesino a sueldo Jeremy Bowers.



