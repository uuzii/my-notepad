# Summary
Los objetivos de este documento son:
* Tener herramientas para gestionar/reparar un servidor
* Gestionar usuarios y permisos
* Bases de seguridad informática
* Realizar escaneos de un sitio de manera controlada

# Distros más usadas de linux
Las distros que utilizaremos serán
* Ubuntu server (18.04) este se denota como LTS porque es soporte extendido.
* CentOS server (7) Proviene de la familia de Red Hat, pero este es de paga y se utiliza mayormente en aplicaciones empresariales, mayormente banking y retail.

La siguiente [página](https://w3techs.com/technologies/overview/operating_system) lleva una estadística de los servers más usados a lolargo de la web. Donde notamos que de las más extendidas son Ubuntu y CentOS.

Hoy en día, manejar estas distribuciones es una habilidad muy deseada en el mercado.

# Instalación de Ubuntu server
Utilizaremos un virtualizador [VirtualBox](https://www.virtualbox.org/wiki/Downloads). Hacemos la instalación y veremos una pantalla donde seleccionaremos:
* Crear nueva.
* Escribimos un nombre `Ubuntu` si queremos que se autoseleccione el OS.
* Determinamos el tamaño de la mamoria, para fines didácticos 1 GB es factible pues no haremos uso de interfaz gráfica.
* Seleccionamos crear un disco duro virtual.
* Seleccionamos VDI como tipo de disco duro.
* Seleccionamos reservado dinámicamente para que el disco crezca según las necesidades.

Posteriormente necesitaremos una imagen ISO para bootear el OS en esta [página](https://ubuntu.com/download/server) siempre buscaremos las versiones LTS.

Una vez descargada la imagen, iremos nuevamente a VBox para configurar:
* Parámetros de red:
  * Conectado a: Adapdador puente
  * Avanzadas: wifi
  * Seleccionar tipo de adaptador
  * Modo promiscuo: permitir todo
  * Cable conectado (si es por cable)
* Almacenamiento
  * Controlador:IDE > click en Vacío > click en Unidad óptica
  * Seleccionar archivo de disco çoptico virtual
  * Seleccionar nuestra ISO

Damos click en iniciar y veremos que inicia el sistema en la VM. Cuando estamos instalando en servidores físicos se lelva a cabo un proceso de comprobación de memoria.
* Seleccionamos el idioma
* Seleccionamos Install Ubuntu Server
* Seleccionamos el idioma del teclado
* Seleccionamos Instlar ubuntu
* Confirmamos la dirección IP por defecto, a menos que queramos hacer algun cambio
* Si tenemos algún proxy, lo configuramos
* Aceptamos la dirección espejo a menos que tengamos problemas para la descarga de archivos
* Seleccionamos la instalación de un disco entero (más adelante revisaremos las particiones)
* Confirmamos el resumen
* Nombramos nuestro server y elegimos usuario y contraseña (debe ser robusta)
* Si queremos que el server sea accedido desde la web, instalaremos SSD sin importar de momento la identidad
* No usaremos ninguna configuración por defecto para aprender a hacerlo, saltamos con TAB
* Esperamos que termine la configuración
* Reiniciamos