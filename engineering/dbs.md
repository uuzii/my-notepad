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
