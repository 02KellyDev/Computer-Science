# POR LA FACULTAD DE INGENIERÍA, UNIVERSIDAD DE SINALOA PARA LA UNIVERSIDAD DE LA COSTA — CUC

Este repositorio documenta una práctica presencial realizada por el Dr. Jose Alfonso Aguilar Calderón de la Unversidad Autónoma de Sinaloa (UAS) desde el **23 al 30 de noviembre de 2026** en **Mazatlán, Sinaloa, México**.

Durante la práctica se preparó el entorno de desarrollo, se creó una primera página con PHP y XAMPP, y se construyó una API REST para consultar productos almacenados en una base de datos MySQL. Las herramientas que se utilizaron fueron Postman, XAMPP, Apache, PHP, MySQL y Git.

## Objetivos de aprendizaje
Al finalizar esta práctica, el estudiante podrá:

- Crear y sincronizar un repositorio local con GitHub.
- Instalar y comprobar el funcionamiento de XAMPP.
- Ejecutar archivos PHP dentro del servidor web Apache.
- Importar una base de datos y sus registros en MySQL.
- Comprender la arquitectura básica Modelo-Controlador-Rutas de una API.
- Consultar los endpoints con Postman.
- Diferenciar una página web tradicional de una respuesta JSON.

## Herramientas utilizadas
- **Git y GitHub:** control de versiones y sincronización del repositorio.
- **XAMPP:** paquete local con Apache, PHP y MySQL.
- **Apache:** servidor web para ejecutar el proyecto.
- **PHP:** lenguaje utilizado para generar contenido dinámico y construir la API.
- **MySQL/MariaDB:** almacenamiento de la información de productos.
- **Postman:** ejecución y comprobación de las solicitudes HTTP.

## Estructura del proyecto

```text
Computer-Science/
├── ejemplo-1/
│   ├── config/
│   │   └── database.php
│   ├── controllers/
│   │   └── ProductoController.php
│   ├── models/
│   │   └── Producto.php
│   ├── public/
│   │   ├── .htaccess
│   │   └── index.php
│   ├── routes/
│   │   └── api.php
│   └── sql/
│       └── productos.sql
├── practica1/
│   └── file.php
├── postman/
└── .postman/
```

### Archivos principales

- `practica1/file.php`: muestra información de PHP mediante `phpinfo()` y sirve como prueba inicial del entorno.
- `ejemplo-1/sql/productos.sql`: crea la base de datos `rest`, la tabla `productos` y tres registros de ejemplo.
- `ejemplo-1/config/database.php`: contiene la conexión a MySQL mediante PDO.
- `ejemplo-1/models/Producto.php`: consulta los productos en la base de datos.
- `ejemplo-1/controllers/ProductoController.php`: procesa las respuestas de la API.
- `ejemplo-1/routes/api.php`: registra las rutas disponibles.
- `ejemplo-1/public/index.php`: recibe todas las solicitudes y dirige cada una a su ruta correspondiente.
- `ejemplo-1/public/.htaccess`: permite usar URLs limpias en Apache.

## Paso 1. Crear el repositorio local

1. Abre una terminal dentro de la carpeta del proyecto.
2. Inicializa Git si todavía no lo has hecho:

   ```bash
   git init
   ```

3. Configura tu nombre y correo. Usa los mismos datos asociados a tu cuenta de GitHub:

   ```bash
   git config --global user.name "Tu Nombre"
   git config --global user.email "tu-correo@example.com"
   ```

4. Revisa que los datos se hayan guardado correctamente:

   ```bash
   git config --global --list
   ```

5. Agrega los archivos, crea el primer registro y sincroniza el repositorio remoto:

   ```bash
   git add .
   git commit -m "Creación inicial del proyecto"
   git remote add origin URL_DEL_REPOSITORIO
   git push -u origin main
   ```

> Si el repositorio remoto ya existe o utiliza otra rama, consulta su dirección y nombre de rama antes de ejecutar el último comando.

## Paso 2. Instalar y configurar XAMPP

1. Descarga XAMPP desde su sitio oficial.
2. Ejecuta el instalador y acepta los permisos que solicite el sistema.
3. Instala al menos los componentes **Apache**, **MySQL** y **PHP**.
4. Abre el Panel de Control de XAMPP.
5. Inicia los servicios **Apache** y **MySQL**.
6. Comprueba que Apache esté activo y que no exista otro programa ocupando los puertos `80` o `443`.

La carpeta principal del servidor normalmente se encuentra en:

```text
C:\xampp\htdocs\
```

## Paso 3. Colocar el proyecto en XAMPP

Copia la carpeta `Computer-Science` dentro de `htdocs` o crea una conexión desde tu ubicación actual hasta esa carpeta.

Una estructura posible sería:

```text
C:\xampp\htdocs\Computer-Science\
```

No es obligatorio que el proyecto tenga el nombre original, pero debes conservar la ruta que utilices en las instrucciones y en la configuración de Apache.

## Paso 4. Comprobación inicial de PHP

El archivo `practica1/file.php` contiene una prueba básica y muestra la configuración de PHP.

1. Abre XAMPP y verifica que Apache esté iniciado.
2. Accede desde el navegador a:

   ```text
   http://localhost/Computer-Science/practica1/file.php
   ```

3. Si la página muestra información sobre PHP, la instalación y la ejecución de PHP fueron exitosas.

> La información mostrada por `phpinfo()` se usa únicamente en entornos locales de práctica. No debe publicarse en un servidor real porque revela datos de configuración.

## Paso 5. Crear la base de datos

1. En el navegador, abre phpMyAdmin:

   ```text
   http://localhost/phpmyadmin
   ```

2. Abre la pestaña **SQL**.
3. Copia el contenido de `ejemplo-1/sql/productos.sql` y ejecútalo.

También puedes importar el archivo desde **Importar** en phpMyAdmin.

El script realiza lo siguiente:

- Crea la base de datos `rest`.
- Crea la tabla `productos`.
- Inserta tres productos de ejemplo.

Las columnas principales son:

| Columna | Descripción |
|---|---|
| `id_producto` | Identificador automático del producto |
| `nombre` | Nombre del producto |
| `descripcion` | Descripción opcional |
| `precio_compra` | Costo de compra |
| `precio_venta` | Precio de venta |
| `existencia` | Cantidad disponible |

## Paso 6. Configurar la conexión a MySQL

El archivo `ejemplo-1/config/database.php` utiliza estas variables de entorno:

| Variable | Valor predeterminado |
|---|---|
| `DB_HOST` | `127.0.0.1` |
| `DB_PORT` | `3306` |
| `DB_NAME` | `diplomado` |
| `DB_USER` | `root` |
| `DB_PASS` | cadena vacía |

Como el script SQL crea la base de datos llamada `rest`, el valor de `DB_NAME` debe ser `rest`. Puedes modificar `database.php` temporalmente para definir ese valor:

```php
$name = getenv('DB_NAME') ?: 'rest';
```

> En una configuración real se recomienda definir las variables de entorno y no escribir contraseñas directamente en el código.

## Paso 7. Configurar las rutas en Apache

El proyecto puede probarse con el enrutamiento básico sin modificar el archivo `.htaccess`:

```text
http://localhost/Computer-Science/ejemplo-1/public/index.php?ruta=/productos
http://localhost/Computer-Science/ejemplo-1/public/index.php?ruta=/productos/1
```

Para utilizar rutas amigables como `/productos` y `/productos/1`, Apache debe tener activado `mod_rewrite`.

El valor de `RewriteBase` en `ejemplo-1/public/.htaccess` debe coincidir con la ubicación real del proyecto. Por ejemplo:

```apache
RewriteBase /Computer-Science/ejemplo-1/public/
```

Si la carpeta se encuentra dentro de `htdocs` con otro nombre, reemplaza `Computer-Science` por el nombre real.

## Paso 8. Instalar y sincronizar Postman

1. Instala Postman desde su sitio oficial.
2. Registra o inicia sesión en tu cuenta.
3. Crea un nuevo espacio de trabajo o abre el proyecto correspondiente.
4. Si el repositorio ya contiene la colección, abre Postman y sincroniza la carpeta o colección disponible en `postman/collections/`.

## Paso 9. Probar la API con Postman

### Obtener todos los productos

- Método: `GET`
- URL:

  ```text
  http://localhost/Computer-Science/ejemplo-1/public/index.php?ruta=/productos
  ```

En la respuesta se recibirá un objeto JSON con el mensaje, el total de registros y el arreglo `data`.

Ejemplo simplificado:

```json
{
  "mensaje": "Lista de productos",
  "total": 3,
  "data": [
    {
      "id_producto": 1,
      "nombre": "Mouse inalámbrico",
      "precio_venta": "199.00",
      "existencia": 25
    }
  ]
}
```

### Obtener un producto por identificador

- Método: `GET`
- URL:

  ```text
  http://localhost/Computer-Science/ejemplo-1/public/index.php?ruta=/productos/1
  ```

Resultados esperados:

- `200 OK`: el producto fue encontrado.
- `400 Bad Request`: el identificador no es un número entero positivo.
- `404 Not Found`: no existe un producto con ese identificador.
- `405 Method Not Allowed`: se intentó utilizar un método no configurado.

## Explicación del flujo de la aplicación

La práctica sigue un flujo parecido al siguiente:

1. El navegador o Postman envía una solicitud HTTP.
2. `public/index.php` recibe el método y determina la ruta solicitada.
3. `routes/api.php` indica qué ruta está disponible.
4. `ProductoController.php` procesa la solicitud.
5. `Producto.php` ejecuta consultas preparadas contra MySQL.
6. `database.php` abre la conexión mediante PDO.
7. El controlador devuelve los datos como JSON.

Esta organización permite separar responsabilidades:

- **Modelo:** acceso y consulta de datos.
- **Vista o cliente:** navegador o Postman.
- **Controlador:** decide qué acción realizar.
- **Rutas:** relacionan URLs y métodos HTTP con acciones.

## Solución de problemas

### Apache no inicia

- Revisa si Apache está intentando utilizar un puerto ocupado.
- Cambia el puerto de Apache en XAMPP o detén el programa que usa el puerto `80`.
- Ejecuta XAMPP como administrador si el sistema lo solicita.

### Error de conexión con MySQL

Comprueba que:

- El servicio MySQL esté iniciado.
- La base de datos `rest` exista.
- `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER` y `DB_PASS` sean correctos.
- La extensión PDO MySQL esté habilitada en PHP.

### La página muestra `Endpoint no encontrado`

Verifica:

- Que la ruta incluya `/productos`.
- Que la ruta coincida exactamente con la ruta configurada.
- Que el proyecto esté dentro de la ruta utilizada en el navegador.

### La página muestra `Producto no encontrado`

Confirma que:

- La base de datos contiene registros.
- El identificador utilizado existe.
- La tabla `productos` fue creada dentro de la base `rest`.

## Guardar y entregar los cambios

Después de realizar modificaciones:

```bash
git add .
git commit -m "Actualiza la práctica y la documentación"
git push
```

Antes de entregar, confirma que el proyecto contenga estos puntos:

- Repositorio local y remoto sincronizados.
- XAMPP con Apache y MySQL funcionando.
- Base de datos `rest` importada.
- Archivo PHP de prueba ejecutado.
- Endpoints probados con Postman.
- Colección o captura de las respuestas disponibles en el equipo.

## Créditos

Práctica académica realizada presencialmente por estudiantes de la CUC durante la semana del **15 al 29 de noviembre de 2026**, en Mazatlán, Sinaloa, México.
