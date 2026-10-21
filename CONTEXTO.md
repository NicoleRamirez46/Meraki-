CONTEXTO.md — Merakié

1. Descripción del proyecto

Merakié es una plataforma web que conecta clientes con salones de belleza, centros de estética y profesionales independientes que ofrecen servicios de belleza y cuidado personal. Permitirá consultar servicios, precios y disponibilidad; buscar establecimientos; reservar, consultar y cancelar citas; y, de forma opcional, comprar productos de belleza. La plataforma contempla una página de descuentos y, como propuesta del proyecto, una membresía llamada Merakié Pass.

El proyecto se basa en la filosofía Kaizen, enfocada en la mejora continua de la experiencia de los usuarios, los profesionales y la plataforma.

El desarrollo implementará primero las funciones centrales acordadas en el plan de trabajo. Algunas funciones de la ficha se documentarán en el modelo de clases, aunque no tengan una pantalla funcional en esta primera versión.

2. Integrantes

|Integrante      |Responsabilidad  |
|----------------|-----------------|
|Nicole Ramirez  |  Administradora |
|                |                 |

3. Herramientas y tecnologías

Se mantienen las herramientas definidas en el plan:

|Capa o propósito        |Herramienta                |Uso                                                                   |
|------------------------|---------------------------|----------------------------------------------------------------------|
|Sistema operativo       |Windows                    |Entorno de desarrollo                                                 |
|Editor                  |Visual Studio Code         |Escribir y organizar el código                                        |
|Frontend                |HTML y CSS                 |Reutilizar y adaptar el prototipo visual                              |
|Plantillas              |Jinja (integrado con Flask)|Mostrar contenido dinámico en las páginas HTML                        |
|Backend                 |Python y Flask             |Implementar rutas, lógica de la aplicación, formularios y sesiones    |
|Base de datos           |SQLite mediante sqlite3  |Guardar y consultar los datos sin configurar un servidor independiente|
|Seguridad de contraseñas|werkzeug.security        |Guardar hashes de contraseñas en lugar de contraseñas en texto plano  |
|Control de versiones    |Git y GitHub               |Registrar cambios y compartir el código                               |
|Publicación             |PythonAnywhere             |Publicar la aplicación web                                            |

No se utilizará un ORM: las consultas SQL se escribirán directamente con sqlite3.

4. Estructura de archivos

La organización acordada del proyecto es:

merakie/
├── app.py
├── modelos.py
├── db.py
├── schema.sql
├── templates/
└── static/

* app.py: rutas de Flask y control del flujo de la aplicación.
* modelos.py: clases del proyecto y lógica asociada a la programación orientada a objetos.
* db.py: conexión con SQLite y funciones para ejecutar consultas SQL.
* schema.sql: creación de las tablas de la base de datos.
* templates/: páginas HTML del prototipo adaptadas a plantillas Jinja.
* static/: hojas de estilo CSS e imágenes.

5. Modelo de clases y POO

El código debe evidenciar el uso de la programación orientada a objetos, incluida la herencia y la validación de atributos.

Clases previstas

* Usuario: clase base para los tipos de usuario que comparten datos y comportamiento.
* Cliente: representa a quien busca servicios, reserva citas y puede comprar productos.
* Administrador: representa a quien supervisa usuarios, perfiles, reservas y contenido administrativo.
* TrabajadorIndependiente: representa a profesionales que ofrecen servicios de belleza de forma independiente.
* Proveedor: representa a quien suministra productos e insumos de belleza.
* ServicioBelleza: representa los servicios ofrecidos por salones o profesionales.

Herencia

Usuario
├── Cliente
├── Administrador
└── TrabajadorIndependiente

Justificación: un cliente, un administrador y un trabajador independiente son usuarios de la plataforma. Las clases Proveedor y ServicioBelleza también estarán documentadas en el modelo, según la ficha del proyecto.

Validación de edad

La clase Cliente debe validar la edad mediante un setter: solo se aceptan edades entre 18 y 100 años, inclusive. Los valores fuera de ese rango deben rechazarse.

6. Base de datos

Se conservarán las tablas y relaciones definidas en el plan de trabajo.

|Tabla      |Columnas principales                                                               |Propósito                                                                                                                          |
|-----------|-----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
|usuarios |id, nombre, documento, edad, correo, telefono, contrasena_hash, rol|Guardar los datos de clientes y administradores, diferenciados por rol. El correo será único y la contraseña se guardará como hash.|
|salones  |id, nombre, zona, calificacion                                             |Guardar la información de los establecimientos.                                                                                    |
|servicios|id, salon_id, nombre, precio                                               |Guardar los servicios que ofrece cada salón. salon_id referencia a salones.id.                                                 |
|citas    |id, usuario_id, servicio_id, fecha, hora, modalidad, estado          |Relacionar clientes con servicios y registrar las citas. El estado será activa o cancelada.                                    |
|productos|id, nombre, descripcion, precio                                            |Tabla opcional para el catálogo de la tienda, si se alcanza a implementar.                                                         |

Relaciones principales

* usuarios 1 a N citas.
* salones 1 a N servicios.
* servicios 1 a N citas.

Cancelar una cita cambiará su estado a cancelada; no se eliminará el registro, para conservar el historial. Las horas disponibles deben calcularse excluyendo las citas canceladas.

7. Módulos y orden de desarrollo

Se mantiene el orden definido en el plan de trabajo. Cada módulo debe integrar pantalla, lógica y datos cuando corresponda, y se publicará al terminar.

|Módulo|Contenido                                                                       |Semana prevista|
|------|--------------------------------------------------------------------------------|---------------|
|M0    |Publicar una página que muestre «Hola»                                          |1              |
|M1    |Catálogo de salones con datos leídos desde SQLite                               |1              |
|M2    |Registro, inicio de sesión y clase Usuario                                    |2              |
|M3    |Reservar, ver y cancelar citas                                                  |3              |
|M4    |Administración básica: consultar usuarios y citas, y agregar salones y servicios|4              |
|M5    |Tienda con catálogo y carrito guardado en sesión; opcional                      |4              |
|Cierre|Pruebas, informe, diagrama de clases y ensayo de sustentación                   |4              |

Módulo actual

Módulo actual:

8. Alcance de la primera versión

Funciones principales previstas

* Registro e inicio de sesión de clientes.
* Catálogo de salones y servicios.
* Reserva, consulta y cancelación de citas.
* Panel de administración básico.
* Página de descuentos estática, de acuerdo con el prototipo.
* Tienda con catálogo, únicamente si el tiempo disponible lo permite.

Funciones que quedan fuera de la implementación principal

Por el tiempo disponible, estas funciones se recortan o quedan como parte del modelo/documentación, de acuerdo con el plan:

* Gestión de proveedores y venta al por mayor.
* Profesionales independientes y servicios a domicilio reales.
* Pagos reales; los pagos del prototipo eran simulados.
* Membresía Merakié Pass.
* Exportación de citas a .ics.
* Notificaciones.

La tienda (M5) es opcional y debe descartarse primero si se presentan retrasos. Las clases de la ficha deben quedar documentadas aunque algunas no tengan una pantalla funcional en esta versión.

9. Criterios básicos de funcionamiento y seguridad

* Rechazar el registro de clientes con edad inferior a 18 o superior a 100 años.
* Guardar contraseñas mediante hash con werkzeug.security, nunca en texto plano.
* Utilizar sesiones de Flask para mantener el inicio de sesión.
* Proteger las rutas que requieren autenticación.
* Permitir el acceso a /admin únicamente a usuarios cuyo rol sea administrador; ocultar un botón no sustituye la validación en el servidor.
* Al cancelar una cita, actualizar su estado sin borrar el historial.
* No ofrecer como disponible una hora ocupada por una cita activa.
* Usar una ruta fiable para el archivo SQLite al publicar la aplicación.

10. Publicación y control de versiones

El código se gestionará con Git y GitHub y se publicará en PythonAnywhere, verificando las condiciones vigentes del plan gratuito antes de depender de él. Después de actualizar el código publicado, se debe recargar la aplicación web cuando sea necesario.

11. Pruebas y sustentación

Antes del cierre del proyecto se realizarán:

* Pruebas de registro, inicio y cierre de sesión.
* Pruebas de acceso a las rutas protegidas.
* Pruebas de reserva y cancelación con dos cuentas para comprobar la disponibilidad.
* Pruebas de administración con una cuenta de cliente y una de administrador.
* Pruebas generales de la aplicación publicada.
* Documentación de las clases y elaboración del diagrama de clases con herencia.
* Ensayo de sustentación en el que cada integrante pueda explicar el código, incluidos los módulos desarrollados por la otra persona.

Principio para la sustentación: el modelo de clases cubre la ficha del proyecto y la aplicación implementa el núcleo funcional acordado.
