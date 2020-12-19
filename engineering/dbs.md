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

> Ejemplo de entidad: un automovil que tiene volante, llantas (atributo multivaluado), motor (atributo compuesto), antigüedad (atributo inferido)

### Entidades fuertes
Son entidades que pueden sobrevivir por sí solas
> Ej. Libro > Ejemplares, el libro es la entidad fuerte que sostiene el sentido de los ejemplares.

### Entidades fuertes
No pueden existir sin una entidad fuerte
* Por identidad. No se diferencían entre sí más que por la clave de su entidad fuerte
* Por existencia. Se les asigna una clave propia

