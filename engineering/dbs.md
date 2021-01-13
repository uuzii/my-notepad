🚀 v0.1.0

# Historia de la persistencia de la información

Históricamente, la información solo se pasaba de boca en boca, pero el ser humano se dió cuenta de que era necesario almacenar la información en algún sitio para que no se alterara, es así que nació la escritura.
Al inicio se utilizó la arcilla, el papiro, luego la revolución del papel. Tuvieron que pasar muchos años para que llegar la siguiente revolución, que fue el *microfilm*, que podía almacenar grandes cantidades de datos de manera muy segura. La siguiente forma de almacenar archivos, fue la digital, que en discos ópticos, permitió guardar unos y ceros como secuencias de datos que conforman información. El último avance en este sentido, es la nube, que es un sitio en el que todos podemos accesar a información digital.

Justo las bases de datos se introducen con la información digital. Las bases de datos se dividen en dos grupos y las compañías que la han promovido:
* **Bases de datos relacionales:** SQL Server, Oracle, PostgreSQL, MySQL, MariaDB (fork de MySQL en open surce).
* **Bases de datos no relacionales:** MemCatch, Cassandra, DynamoDB, ElasticSearch, neo4j, MongoDB, Firestore.

Tenemos también dos clasificaciones de los servicios de bases de datos:
* **Auto administrados.** Las instalamos en nuestro servidor, nos encargamos de actualizarlas y darles mantenimieto.
* **Administrados.** Servicios que ofrecen las nubes modernas: AmazonWS, Azure, GoogleCloud.

Hoy en día, no hay industrias que no utilicen bases de datos, pues esto significaría un atraso respecto al mercado actual.

# Historia de las RDB (Bases de datos relacionales)

Las bases de datos surgen de la necesidad de almacenar información. En la arquitectura de computadoras Vonn-Neuman, no se considera el almacenamiento de datos en memoria, sino solamente la memoria RAM. Las bases de datos originalmente eran basadas en archivos, básicamente eran archivos de texto plano que almacenaban datos, pero en este esquema, no era fácil tener orden ni hacer ciertas consultas, es así como nacen las bases de datos relaconales.

El inventor de estas bases fue **Edgar Codd**, que escribió doce [reglas](https://es.wikipedia.org/wiki/12_reglas_de_Codd) que encapsulan la filosofía de las bases de datos y que prevaleciera un estándar. Codd inventó el *álgebra relacional*, consiste en cómo tenemos datos que se pueden empezar a mezclar a través de ciertas propiedades y características.

# Entidades y atributos

Uno de los objetos principales de las bases de datos, son las entidades. Una entidad, es algo muy parecido a un *objeto* en los lenguajes de POO. La idea de un objeto, es representar  un objeto del mundo real. Las entidades, por convención se nombran en plural y poseen:
* Atributos sencillos
* Atributos multivaluados (que hay varios de ellos)
* Atributos compuestos (tienen más atributos dentro de sí)
* Atributos inferidos
* Métodos de entrada
* Atributo llave (diferencía a elementos idénticos unos de otros)
  * Naturales. Número de serie asignado
  * Clave artificial. Se le asigna de manera arbitraria

En el siguiente ejemplo, vemos un ejemplo de entidades y atributos:

* **Automóvil.** Es la entidad, se representa con un rectángulo.
* **Volante y modelo.** Son atributos simples.
* **Llantas.** Es un atributo *multivaluado*, es decir que tiene más de uno, se representa con un óvalo doble.
* **Motor.** Es un atributo *compuesto* pues está formado por múltiples atributos.
* **Antigüedad.** Es un atributo autoinferido, pues se deduce del modelo.
* **No. de serie.** Es un dato que lo diferenciará de otras entidades que sean idénticas.

![Entidades y atributos](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/entities-attributes.jpeg?raw=true)

### Entidades débiles y fuertes

Las *entidades fuertes*, se denominan así porque no dependen de ninguna otra entidad para existir, las débiles son justo lo opuesto: las *entidades débiles*, son entidades que dependen de otra entidad para tener sentido, en el siguiente ejemplo y se representan con un rectángulo doble.

![Entidades fuertes y débiles](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/weak-strong.jpeg?raw=true)

Pueden determinarse débiles por dos motivos:
  * Por identidad. No se direnecían entre sí, más que por la clave de su entidad fuerte, en el siguient ejemplo, vemos que los ejemplares (débiles) no se pueden diferenciar entre sí, más que por el id de su entidad fuerte

  **Ejemplar** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Libro**
  | id       | title             | libro_id | localización | edición  |
  | -------- | ----------------- | -------- | ------------ | -------- |
  | LKJ89KAS | Viaje al centr... | LKJ89KAS | pasillo 1    | 1        |
  | KCO31OKJ | De la tierra a... | KCO31OKJ | pasillo 2    | 1        |
  | NSDJOH12 | Rebelión en la... | NSDJOH12 | pasillo 1    | 3        |
  | 09KSIHBD | El señor de lo... | 09KSIHBD | pasillo 1    | 3        |

  * Por existencia. Para que no dependan del identificador de la entidad fuerte, se les asigna además una clave propia, aunque conceptualmente, seguirán dependiendo de ella. En el siguiente ejemplo, se le ha asignado una clave a cada ejemplar:

  **Ejemplar** &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **Libro**
  | id       | title             | libro_id   | localización | edición  |
  | -------- | ----------------- | ---------- | ------------ | -------- |
  | LKJ89KAS | Viaje al centr... | SDKJSD0012 | pasillo 1    | 1        |
  | KCO31OKJ | De la tierra a... | NVDUNDLO08 | pasillo 2    | 1        |
  | NSDJOH12 | Rebelión en la... | KNDEKNEU57 | pasillo 1    | 3        |
  | 09KSIHBD | El señor de lo... | SDKNSDKJD6 | pasillo 1    | 3        |

En estos ejemplos, se representa *ejemplares* como entidad débil de *libro*. Dicho de otro modo, una entidad débil no es otra cosa que una entidad que posee la definición de una entidad fuerte, solo se diferenciarán por el *id*.

# Relaciones

Es el otro elemento importante en los diagramas que hemos visto, se llama relación, nos indica cuántos elementos de una entidad corresponden a cuántos elementos de otra. Las relaciones se representan con un rombo.

![Diagrama ER (Relación)](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/relations.png?raw=true)
## Cardinalidad

La cardinalidad nos propeorciona información acerca de las relaciones,
* Uno a uno (1:1). Ejemplo, un automóvil solo tiene un dueño.
* Cero a uno (0:1) ó 1:1 opcional. Ejemplo, un usuario puede o no tener una sesión activa.
* Uno a n (1:n). Ejemplo, un dueño puede tener muchos automóviles.
* Cero a n (0:n) ó 0 a n opcional. Ejemplo, un paciente puede estar internado en una habitación, pero no necesariamente una habitación debe tener un paciente.
* Muchos a muchos (n:n). Ejemplo, un alumno puede estar inscrito a muchas clases, a su vez una clase puede tener muchos alumnos.

![Cardinalidades](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/cardinalities.jpeg?raw=true)

# Diagrama Entidad - Relación

Nos permite visualizar cómo interaccionan las entidades conectadas por sus respectivas cardinalidades, en el siguiente ejemplo, vemos un diagrama de un blogpost:

![Diagrama entidad relación](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/er-diagram.jpeg?raw=true)

# Diagrama físico

Una vez teniendo claras las entidades que van a interactuar en el sistema, nos hace falta aterrizar nuestro diseño de una manera más precisa para llevarlo a la práctica, esto lo haremos con el diagrama físico, que nos ayudará para transicionar eventualmente a una base de datos. Aquí veremos los símbolos de las cardinalidades y también los tipos de datos:

## Tipos de datos

### De texto
* **CHAR(n).** Nos permite almacenar caracteres y strings
* **VARCHAR(n).** Es parecido a CHAR(n), toma un espacio inicial en memoria pero su tamaño es dinámico, es decir que puede crecer conforme se requiera (hasta 250 caractéres).
* **TEXT.** Permite almacenar cadenas de más de 250 carcatéres.

### Números
* **INTEGER.** Son números que no tienen partes fraccionarias.
  *** BIGINT.** Nos permite para declarar enteros que de antemano sabemos que será muy grande.
  *** SMALLINT.** Nos permite para declarar enteros que de antemano sabemos que será muy chico.
* **DECIMAL(n, s).** Recibe dos parámetros, n: el número y s: sus decimales, nos servirá para manejar números con parte fraccionaria
* **NUMERIC(n, s).** Misma definición que DECIMAL(n, s).

### Fecha/hora
* **DATE.** Solamente almacena fecha (año, mes, día)
* **TIME.** Alamacena la hora del día
* **DATETIME.** Almacena la hora aún con milisegundos, es muy precisa
* **TIMESTAMP.** Misma definición que DATETIME.

### Lógicos
* **BOOLEAN.** Dato binario que almacena cierto/falso, 1/0 nos servirá para almacenar banderas

## Constraints

Son las reglas que le definimos a una base de datos:
*** NOT NULL.** Se asegura que la columna no tenga valores nulos, nos ayudará a definir los datos obligatorios del sistema.
* **UNIQUE.** Se asegura que cada valor en la caloumna se repita, se utiliza por ejemplo, para validar que no se repita un correo electrónico.
* **PRIMARY KEY.** Es una combinación de NOT NULL y UNIQUE, valida que sea único un elemento y que no esté vacío.
* **FOREGIN KEY.** Identifica de manera única una tupla en otra tabla (convivirá quizá con una PRIMARY KEY, pero sí se puede repetir).
* **CHECK.** Se asegura que el valor en la columna cumpla una condición dada
* **DEFAULT.** Coloca un valor por defecto cuando no hay un valor especificado (que no sea NULL).
* **INDEX.** Se crea por columna para permitir búsquedas más rápidas, su desventaja es que si la base de datos crece mucho, se realentiza, se usa mejor cuando la tabal es estática.

## Normalización

Nos permite separar los componentes de una base de datos para que obedezcan a las reglas de Codd. Llegar a este punto es la aspiración de todo diseñador de base de datos.

Veamos una tabla sin normalizar:

| alumno  | nivel_curso  | nombre_curso     | materia_1 | materia_2 |
| ------- | ------------ | ---------------- | --------- | --------- |
| Juanito | Maestría     | Data engineering | MySQL     | Python    |
| Pepito  | Licenciatura | Programación     | MySQL     | Python    |

Para ir separando la información y normalizándola se usan las siguientes formas:

### Primera forma normal (1FN)
Esta norma nos habla acerca de atributos atómicos, quiere decir que no puede haber campos repetidos, aplicando esta norma, tendríamos este resultado, reduciendo dos columnas (materias 1 y 2) a una:

| alumno_id | alumno  | nivel_curso  | nombre_curso     | materia   |
| --------- | ------- | ------------ | ---------------- | --------- |
| 1         | Juanito | Maestría     | Data engineering | MySQL     |
| 1         | Juantio | Maestría     | Data engineering | Python    |
| 1         | Pepito  | Licenciatura | Programación     | MySQL     |
| 1         | Pepito  | Licenciatura | Programación     | Python    |

### Segunda forma normal (2FN)
Indica que primero debe estar en la primera forma normal y cada campo en la tabla debe depender de una clave única, aplicándola, tendríamos ahora dos tablas, una para guardar la relación de los alumnos y otra para las materias:

**alumnos:**

| alumno_id | alumno  | nivel_curso  | nombre_curso     |
| --------- | ------- | ------------ | ---------------- |
| 1         | Juanito | Maestría     | Data engineering |
| 2         | Pepito  | Licenciatura | Programación     |

**materias:**

|materia_id|alumno_id|materia|
|-|-|-|
|1|1|MySQL|
|2|1|Python|
|3|2|MySQL|
|4|2|Python|

Vemos que esta tabla tiene mayor sentido, ya que loas alumnos y materias no representan las mismas entidades en el mundo real.

### Tercera forma normal (3FN)
Nos dice que se deben cumplir las primeras dos formas normales y que los campos que no son clave, no deben tener dependencias, aplicándolo a las tablas anteriores, tendríamos los siguientes resultados (separando alumnos de cursos):


**alumnos:**

|alumno_id|alumno|cusro_id|
|-|-|-|
|1|Juanito|1|
|2|Pepito|2|

**cursos:**

|cusro_id|nivel_curso|nombre_curso|
|-|-|-|
|1|Maestría|Data engineering|
|2|Licenciatura|Programación|

**materias:**

|materia_id|alumno_id|materia|
|-|-|-|
|1|1|MySQL|
|2|1|Python|
|3|2|MySQL|
|4|2|Python|

### Cuarta forma normal (4FN)
Debe incluir las primeras tres formas normales, pero además, los campos multivaluados, se identificarçan por una clave única, veamos cómo nos quedaría (agregamos una tabla con un id artificial y los números que relacionan a los alumnos con cada materia):

**alumnos:**

|alumno_id|alumno|cusro_id|
|-|-|-|
|1|Juanito|1|
|2|Pepito|2|

**cursos:**

|cusro_id|nivel_curso|nombre_curso|
|-|-|-|
|1|Maestría|Data engineering|
|2|Licenciatura|Programación|

**materias:**

|materia_id|materia|
|-|-|
|1|MySQL|
|2|Python|

**materias_por_alumno:**

|mpa_id|materia_id|alumno_id|
|-|-|-|
|1|1|1|
|2|2|1|
|3|1|2|
|4|2|2|

> Aunque pareciera un diseño más rebuscado, nos ayudará a que la computadora pueda identificar de mejor manera todas las entidades y que no haya datos repetidos, eventualmente también nos permitirá hacer querys más precisos.

Después de este proceso, ya tenemos todos los elementos para hacer nuestro diagrama físico.

![Diagrama físico](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/physic-er.jpeg?raw=true)

# RDBMS

Son siglas de **R**elational **D**ata**B**ase **M**anagement **S**ystem, es decir, un manejador de bases de datos, son programas que nos permiten llevar a la realidad nuestras bases de datos. Las más importantes son:

* MySQL
* PostgreSQL
* Oracle

Para descargar MySQL, podemos buscar un instablable en este [link](https://downloads.mysql.com/archives/community/)

También requerimos el **WorkBench**, que nos permitirá visualizar de manera más fácil nuestras bases de datos. A este se le denomina *cliente gráfico* la forma más simple de abstraer una base de datos, es a modo de una tabla, para eso nos ayudan los clientes gráficos.

Al abrirlo, seleccionamos nuestro servidor, y vemos que podemos crear *schemas* que no son otra cosas sino bases de datos, seleccionamos el encoding y si lo requerimos para algún idioma específico.

# Servicios administrados

Los servicios *cloud* o servicios administrados, son aquellos que no están en equipos propios o de la empresa, sino que se encuentran en equipos de terceros, nos evitan preocuparnos por la seguridad y configuraciones y así concentrarnos en las bases de datos per se. Los servicios más populares que se proporcionan en el mercado son:
* AWS
* Google Cloud Platform
* Microsoft Azure

# SQL

Es un lenguaje que nos permite hacer consultas a bases de datos, llamado así por sus siglas **S**tructured **Q**uery **L**anguage, surge de la necesidad de hacer un estándar para hacer consultas a bases de datos. Anteriormente existían múltiples RDBS, también muchos trabataban de leer archivos de texto o de diferentes tipos, por ello era necesario estandarizarlo.

## NOSQL

Hoy en día existen algunos manejadores de bases de datos que no son relacionales, utilizan lenguajes más amplios, pero muchas veces, tienen como base SQL, a éstas se les denomina **NOSQL**, pero no porque no tengan nada que ver con SQL, sino porque son *Not Only* - SQL, ejemplos de ello son:
* Big query
* Cassandra

## DDL
Es un sub-lenguje de SQL, se denomina por las siglas de **D**ata **D**efinition **L**anguage, éste nos ayuda a crear la estructura de una base de datos: entidades y relaciones. Existen tres grandes comandos en DDL:
* **Create.** Nos permitirá crear bases de datos, tablas, índices, etc.
* **Alter**. Nos permite modificar DBs, por ejemplo agregar columnas, quitándolas, cambiando el tipo de dato, etc.
* **Drop.** Nos ayuda a borrar elementos, debemos tener mucho cuidado con ella.

Los elementos que vamos a estar manejando son:
* **Database.** Es la base de datos.
* **Table.** Es la proyección o tradcción a SQL de nuestras entidades.
* **View.** Son las proyecciones de nuestras bases de datos, de modo que son entendibles.

### Crear una Base de Datos (CREATE DATABASE)

Para crear una base de datos y usarla usaremos esta sintaxis:

```sql
CREATE DATABASE test_db;

USE DATABASE test_db;
```

Un equivalente en Workbench, damos click derecho en la columna SCHEMAS y luego click  en *Create Schema*, nombremos nuestra base de datos, y veremos cómo se escribe el comando CREATE con un cierto encoding. Para usarlo por default, damos click derecho a nuestro Schema y seleccionamos *Set as default schema*.

### Crear una tabla (CREATE TABLE)

Los comandos para crear una tabla son más complejos, pues implican más campos (constrains), esta sería la forma de hacerlo:

```sql
CREATE TABLE people (
  person_id int,
  last_name varchar(255),
  fisrt_name varchar(255),
  address varchar(255),
  city varchar(255)
)
```

En la consola, desplegamos nuestra schema, y damos click derecho en Tablas luego en la opción Create Table. Colocamos un nombre a nuestra tabla y veremos que por default nos crea un campo id, lo modificamos a nuestro gusto, generalmente para un id, seleccionaremos el campo AI (automatic increment) que lo incrementará automáticamente cuando se agregue más registros, agregamos los campos mencionados arriba con sus tipos y al darle apply, veremos algo así:

```sql
CREATE TABLE `platziblog`.`people` (
  `person_id` INT NOT NULL AUTO_INCREMENT,
  `last_name` VARCHAR(255) NULL,
  `first_name` VARCHAR(255) NULL,
  `address` VARCHAR(255) NULL,
  `city` VARCHAR(255) NULL,
  PRIMARY KEY (`person_id`));
```

Posteriormente, dentro de Tables, veremos que ya existe la tabla people. Para hacer una consulta, podemos dar click derecho y hacer click en Select Rows - Limit 1000 para ver los primeros 1000 registros.

### Crear una vista (CREATE VIEW)

Los views toman datos de una base de datos, los ponen en una forma presentable y los convierten en una consulta recurrente.

```sql
CREATE VIEW v_brasil_customers AS
  SELECT customer_name, contact_name
  FROM customers
  WHERE country = "Brasil"
```

Donde:
* El profijo `v`se antepone como un estándar para identificar que no es una base de datos sino una vista
* SELECT y las demás sentencias nos ayudarán a consultar solo los clientes de Brasil.

Para hacerlo en la consola seguimos estos pasos:
1. Hacemos una consulta de los primeros 1000 registros dando click derecho a nuestra tabla > Select rows limit 1000
2. Copiamos el comando de esta selección
3. Damos click derecho en Views > create view
4. Pegamos nuestro SELECT en la segunda línea
5. Editamos el nombre de la vista
6. Damos click en apply y revisamos
7. Veremos nuestro view guardado y podemos consultarlo

Estas vistas nos ayudarán para crear querys más avanzados que combinen tablas, hagan búsquedas avanzadas y que se hagan recurrentemente.

### Modificar una tabla (ALTER TABLE)

Son comandos que nos permitirán modificar valores en una tabla

```sql
ALTER TABLE people
ADD date_of_birth date;

ALTER TABLE people
ALTER COLUMN date_of_birth year;

ALTER TABLE people
DROP COLUMN date_of_birth;
```

Para hacerlo en Workbench:
1. Damos click derecho en la tabla > alter table
2. Veremos el modo de edición y podremos añadir columnas
3. Pulsamos apply y revisamos
4. Veremos que hemos alterado la tabla

De esta manera podemos alterar la tabla, agregando, quitando columnas (DROP), modificando los tipos de datos, etc.

### Eliminar tabla/base de datos (DROP)

Esta sentencia es muy delicada, pues consiste en eliminar datos de nuestra base de datos. Ejemplo, para borrar una tabla o una base de datos respectivamente, se usa:

```sql
DROP TABLE people;

DROP DATABASE test_db;
```

Para borrar una tabla en Workbench:
1. Damos click derecho en la tabla > drop table
2. Revisamos la sentencia
3. Ejecutamos

Para borrar una base de datos/schema en Workbench:
1. Damos click derecho en la db > drop schema
2. Revisamos la sentencia
3. Ejecutamos

Los comandos DDL, se usan mayormente al inicio del proyecto, posteriormente, usaremos los comandos **DML**.

## DML
Es otro sub-lenguaje de SQL, que tiene que ver con el manejo del contenido de la base de datos, no tanto de la base de datos per se. Quiere decir *Data Manipulation Language*, algunas de las acciones principales son:
* Insert
* Update
* Delete
* Select

### INSERT
Agrega nuevos registro o tuplas en una tabla de una base de datos, sería un equivalente a agregar *filas* a una tabla:

```sql
INSERT INTO people (last_name, first_name, address, city)
VALUES ('Rodriguez', 'Uzi', 'Calle X', 'HMO')
```

* `INSERT` indica en qué tabla se van a introducir los datos y cuáles serán los argumentos
* `VALUES` representa una de las *filas* a insertar, en este punto es muy importante mantener el orden en el que se introducen los campos

PAra hacerlo en workbench, introducimos directamente el comando anterior en la tabla.

### UPDATE
Nos permitirá modificar datos ya existentes en nuestra tabla

```sql
UPDATE people
SET last_name = 'Alcantara', city = 'MEX'
WHERE person_id = 1;

UPDATE people
SET last_name = 'Alcantara'
WHERE city = 'MEX';

UPDATE people
SET first_name = 'Uzi';
```

Donde:
* `UPDATE` contiene la tabla
* `SET` especifica qué campo se tiene que cambiar y por qué valor
* `WHERE` detalla qué condiciones debe cumplir el ítem que se va modificar

En el primer caso, cambiamos apllido y ciudad de un row puntual, en el segundo caso afectamos a todos los rows que sean de la ciudad MEX; en el tercer caso no especificamos qué ítem se debe modificar.

### DELETE
Puede borrar el contenido de una tabla, en el primer caso, borra un ítem puntual, si no especificamos dónde como en el segundo caso de abajo, se puede borrar toda la tabla.

```sql
DELETE FROM people
WHERE person_id = 1;

DELETE FROM poeple;
```
## SELECT
Nos consulta información de una base de datos:

```sql
SELECT first_name, last_name
FROM people;
```

Donde:
* `SELECT` nos permitirá ver los campos indicados
* `FROM` especifica la tabla
* `WHERE` especifica argumentos de los ítems que queremos consultar

## ¿Qué tan estándar es SQL?
El lenguaje SQL, unificó la forma en la que se hacían consultas a una base de datos. Aún actualmente, muchas tecnologías modernas siguen usando su estructura sintáctica.

> 💡 Para todos los manejadores de bases de datos relacionales que usan SQL, el lenguaje DDL y DML se utilizan igual

# Creando una base de datos para un Blogpost
Para crear la DD del diagrama físico que tenemos arriba, necesitamos seguir los siguientes pasos:
1. Identificar qué entidades NO poseen llaves fóraneas, pues éstas entidades, dependen de otras.
2. Proceder a crear las tablas de las entidades que no poseen llaves primarias, cuidando que tengan bien asignados sus tipos de dato, valores por defecto, así como sus constrains, que pueden ser:
  * **PK** PRIMARY KEY
  * **NN** NOT NULL
  * **UQ** UNIQUE
  * **AI** AUTO INCREMENT
3. Proceder a crear las *tablas dependientes*, es decir, las que tienen llaves foráneas cuidando que éstas últimas correspondan a entidades de las que ya tenemos tabla creada.
4. Configurar las llaves foráneas especificando las tablas referenciadas y sus acciones, que pueden ser:
  * **RESTRICT** Para impedir que se borre información debajo de este ítem.
  * **CASCADE** Para re-etiquetar todos los elementos de una columna.
  * **SET NULL** Para poner un null en lugar del ítem (cuidado con los campos NN).
  * **NOT ACTION** Para no hacer nada.
  Éste es un ejemplo de la forma SQL de crear una FK:
  ```sql
  ALTER TABLE `uziblog`.`posts` -- Modificando la tabla
  ADD INDEX `posts_users_idx` (`user_id` ASC); -- Añadiendo un index al campo user_id
  ;
  ALTER TABLE `uziblog`.`posts` -- Altera la tabla añadiendo un constrain
  ADD CONSTRAINT `posts_users`
    FOREIGN KEY (`user_id`)
    REFERENCES `uziblog`.`users` (`id`)
    ON DELETE NO ACTION
    ON UPDATE CASCADE;
  ```
5. Crear las tablas que tienen llaves foráneas de tablas dependientes
6. Crear las *tablas transitivas*, es decir, aquellas que en nuestro diagrama físico, descomponen las relaciones N:N. Dichas tablas, no poseen información per se, sino solo contendrán llaves foráneas a modo de pivote para generar las relaciones N:N entre dos entidades.

> 💡  Pro tip: En Workbench, una vez generada nuestra BD, podemos usar Database/Reverse engineer... para generar el diagrama físico de nuestra base de datos si es que estamos en un lugar en el que no conocemos a detalle cómo se ha diseñado una instancia.

# Consultas o querys
Es importante saber hacer consultas correctas a las bases de datos, muchas veces, puede exisitr una gran cantidad de información distribuida en múltiples tablas, lo cuál de manera estática muchas veces carece de sentido, pero cuando sabemos consultar esa información para presentarla de modos útiles, podemos aportar mucho valor a una organización. Hacer query's es el quehacer diario del administrador de bases de datos.

> Cualquier duda de negocio se puede resolver haciendo un buen query

## Estructura básica de un query
Las partes esenciales de una consulta son: `WHERE`y `FROM`. El siguiente es un ejemplo de un query:
```sql
SELECT city, count(*) AS total -- proyectamos ciertos datos como total
FROM people -- de la tabla people
WHERE active = true -- agregamos una condición
GROUP BY city -- agrupamos por ciudad
ORDER BY total DESC -- ordenamos de manera descendente
HAVING total >= 2; -- filtra el total de los que tienen más de dos
```

Veamos a detalle algunas consultas básicas para nuestro blog:
* Consulta básica de todos los elementos de una tabla
  ```sql
  SELECT *
  FROM posts;
  ```
* Consulta de todos los elementos que sean de fechas menores a 2020:
  ```sql
  SELECT *
  FROM posts
  WHERE post_date < '2020';
  ```

## SELECT
`SELECT` nos ayuda a hacer las diferentes proyecciones de nuestra base de datos. La forma mas básica de usar `SELECT` es:
```sql
SELECT * FROM [table];
```

Si qusiéramos seleccionar solo ciertos campos de una tabla:
```sql
SELECT [column1], ..., [columnN] FROM [table];
```

Si queremos cambiar el nombre que se le da al campo en la consulta:
```sql
SELECT [column1] AS [alias1], ..., [columnN] AS [aliasN] FROM [table];
```

Si queremos hacer una agrupación sencilla, que nos arrojaría un conteo de los elementos de una tabla:
```sql
SELECT COUNT(*) AS [alias] FROM [table]
```

De este modo vemos que, si bien podemos consultar y filtrar datos de nuestra base de datos, también podemos construir *datos on the flight*.

## FROM
`FROM`, como hemos visto, nos ayuda a indicar de dónde se van a traer los datos. Hacer consultas a una sola tabla es sencillo, pero cuando queremos unir tablas para hacer proyecciones más específicas, requeriremos ser más minuciosos. Para esto, usaremos otra sentencia que es inseparable de `FROM` y esta es `JOIN`. Para entender esto, usaremos diagramas de Venn:

### Diferencia
Para la diferencia, hay dos tipos de `JOIN` que nos permiten proyectarla: *LEFT JOIN* y *RIGHT JOIN*. Consideremos que estamos haciendo una base de datos para un blog, sea **A** la tabla que trae los posts y **B** la tabla que trae los usuarios:

![diferencia](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/join-1.jpg?raw=true)

* El el primer caso de `LEFT JOIN`, traeríamos todos los usuarios, tengan o no tengan posts. En SQL ejecutaríamos
  ```sql
  SELECT *
  FROM schema.users
    LEFT JOIN schemaname.posts ON users.id = posts.user_id;
  ```
* En el segundo caso de `LEFT JOIN`, solo traeríamos los usuarios que no tengan posts. En SQL ejecutaríamos:
  ```sql
  SELECT *
  FROM schemaname.users
    LEFT JOIN schemaname.posts ON users.id = posts.user_id
    WHERE posts.user_id IS NULL;
  ```
* El el primer caso de `RIGHT JOIN`, traeríamos todos los posts, aunque ningún usuario los haya creado. En SQL:
  ```sql
  SELECT *
  FROM schemaname.users
    RIGHT JOIN schemaname.posts ON users.id = posts.user_id;
  ```
* El el segundo caso de `RIGH JOIN` traeríamos todos los posts que ningún usuario haya creado.
```sql
  SELECT *
  FROM schemaname.users
    RIGHT JOIN schemaname.posts ON users.id = posts.user_id
    WHERE posts.user_id IS NULL;
  ```

### Intersección
Para la intersección, existe el *INNER JOIN*, que trerá los datos que compartan ambas tablas y el *OUTER JOIN*, que se presenta en dos casos: unión (traería todos los datos de las tablas) y la diferencia simétrica, que traería todos los datos de las tablas, excepto aquellos que tengan en común. Volviendo al ejemplo de la tabla **A** y **B**, veríamos lo siguiente:

![imagen](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/join-2.jpg?raw=true)

* El `INNER JOIN` traería los usuarios que tengan algún post asociado o viceversa. En SQL:
  ```sql
  SELECT *
  FROM schemaname.users
	  INNER JOIN schemaname.posts ON users.id = posts.user_id;
  ```
* El `OUTER JOIN` en el caso unión, traería todos los usuarios y todos los posts.
  ```sql
  SELECT *
  FROM schemaname.users
    LEFT JOIN schemaname.posts ON users.id = posts.user_id
  UNION
  SELECT *
  FROM schemaname.users
    RIGHT JOIN schemaname.posts ON users.id = posts.user_id;
  ```
* El `OUTER JOIN` en el caso diferencia simétrica, traería todos los usuarios que no tengan posts asociados y todos los posts que no tengan usuario asociado. En SQL:
  ```sql
  SELECT *
  FROM uziblog.users
    LEFT JOIN uziblog.posts ON users.id = posts.user_id
  WHERE posts.user_id IS NULL
  UNION
  SELECT *
  FROM uziblog.users
    RIGHT JOIN uziblog.posts ON users.id = posts.user_id
  WHERE posts.user_id IS NULL;
  ```

## WHERE
`WHERE` nos ayuda a filtrar tuplas de acuerdo a criterios específicos como fechas, cantidades, etc. Ejemplo, la siguiente consulta nos permitirá consultar todos los posts que tengan un id menor a 50:
```sql
SELECT *
FROM schemaname.posts
WHERE id < 50;
```
En estos casos, nos será muy útil tener bien definidos los tipos de dato de nuestros campos.

Si queremos ingresar criterios de los que no tenemos el valor preciso, podemos usar la sentencia `LIKE`:
```sql
SELECT *
FROM schemaname.posts
WHERE titulo LIKE '%[someWordBetweenFieldValue]%';
```

Para especificar que el valor buscado esté al inicio o al final, removemos el signo de `%` del inicio o del final para cada opción respectivamente:
```sql
SELECT *
FROM schemaname.posts
WHERE titulo LIKE '[someWordBetweenFieldValue]%'; -- para buscar la palabra al inicio
```

Podemos validar también con fechas:
```sql
SELECT *
FROM schemaname.posts
WHERE post_date > '2025-01-01';
```

O elementos en intervalos con las sentencias `BETWEEN` y `AND` (estas sentencias se pueden usar para establecer intervalos a otros tipos de dato):
```sql
SELECT *
FROM schemaname.posts
WHERE post_date BETWEEN '2023-01-01' AND '2025-01-01';
```

Podemos usar la función `YEAR`, por mencionar un ejemplo, para no escribir toda la fecha sino solo el año y filtrar los elementos de acuerdo a alguno de sus campos que maneje un timestamp:
```sql
SELECT *
FROM schemaname.posts
WHERE YEAR(post_date) BETWEEN '2023' AND '2025';
```