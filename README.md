# Proyecto Modular PHP

Este proyecto sigue una estructura modular por aplicaciones para mantener el código organizado y facilitar el mantenimiento y la escalabilidad.

## Estructura de Carpetas

```plaintext
project/
│
├── apps/
│   ├── NotaCredito/
│   │   ├── Controllers/
│   │   │   └── NotaCreditoController.php
│   │   ├── Models/
│   │   │   └── NotaCredito.php
│   │   ├── Services/
│   │   │   └── NotaCreditoService.php
│   │   └── Views/
│   │       └── NotaCreditoView.php
│   ├── AnotherApp/
│   │   ├── Controllers/
│   │   ├── Models/
│   │   ├── Services/
│   │   └── Views/
│   │       └── ...
│
├── config/
│   └── database.php
│
├── public/
│   └── index.php
│
├── routes/
│   └── web.php
│
└── vendor/
    └── ...
```

Ejemplo de Implementación Modular
NotaCredito.php (Modelo)

```php
<?php

namespace App\NotaCredito\Models;

class NotaCredito {
    public $nro_documento;
    public $cod_empresa;
    public $cod_documento;

    public function __construct($nro_documento, $cod_empresa, $cod_documento) {
        $this->nro_documento = $nro_documento;
        $this->cod_empresa = $cod_empresa;
        $this->cod_documento = $cod_documento;
    }
}
?>

```
NotaCreditoService.php (Servicio)

```php
<?php

namespace App\NotaCredito\Services;

use App\NotaCredito\Models\NotaCredito;

class NotaCreditoService {
    public function checkEstadoFirmado(NotaCredito $documento) {
        // Aquí podría haber lógica para verificar el estado firmado
        if ($documento->nro_documento % 2 == 0) {
            return 'Firmado';
        } else {
            return 'No Firmado';
        }
    }
}
?>

```
NotaCreditoController.php (Controlador)

```php
<?php

namespace App\NotaCredito\Controllers;

use App\NotaCredito\Models\NotaCredito;
use App\NotaCredito\Services\NotaCreditoService;

class NotaCreditoController {
    protected $notaCreditoService;

    public function __construct() {
        $this->notaCreditoService = new NotaCreditoService();
    }

    public function show($nro_documento, $cod_empresa, $cod_documento) {
        $notaCredito = new NotaCredito($nro_documento, $cod_empresa, $cod_documento);
        $estado = $this->notaCreditoService->checkEstadoFirmado($notaCredito);
        
        // Aquí iría la lógica para enviar datos a la vista
        echo 'El estado del documento es: ' . $estado;
    }
}
?>

```

web.php (Rutas)

```php
<?php

require_once '../vendor/autoload.php';

use App\NotaCredito\Controllers\NotaCreditoController;

// Aquí va la lógica para inicializar el enrutador y manejar las solicitudes

?>

```
Explicación de la Arquitectura y Patrones
Modularidad:

Cada aplicación tiene su propio conjunto de controladores, modelos, servicios y vistas.
Esto facilita la gestión y el mantenimiento del código, permitiendo que cada módulo evolucione independientemente.
Modelo (NotaCredito.php):

Define la estructura de datos.
Servicio (NotaCreditoService.php):

Contiene la lógica de negocio, como la verificación del estado firmado.
Controlador (NotaCreditoController.php):

Gestiona las solicitudes y las respuestas, y utiliza los servicios para aplicar la lógica de negocio.
Rutas (web.php):

Define las rutas y las acciones correspondientes para cada aplicación.
Punto de Entrada (index.php):

Inicializa la aplicación y el enrutador.
Esta estructura modular te permite mantener tu aplicación organizada, con una clara separación de responsabilidades, lo que facilita la escalabilidad y el mantenimiento. Cada aplicación puede tener su propio ciclo de vida y desarrollo independiente.

Clase de Conexión a Base de Datos MySQL
Database.php

```php
<?php

namespace App\Config;

use PDO;
use PDOException;

class Database {
    private $host = 'localhost';
    private $db_name = 'your_database';
    private $username = 'your_username';
    private $password = 'your_password';
    public $conn;

    public function getConnection() {
        $this->conn = null;

        try {
            $this->conn = new PDO("mysql:host=" . $this->host . ";dbname=" . $this->db_name, $this->username, $this->password);
            $this->conn->exec("set names utf8");
        } catch (PDOException $exception) {
            echo "Connection error: " . $exception->getMessage();
        }

        return $this->conn;
    }
}
?>
```

Uso de la Clase de Conexión

```php
<?php

require_once 'path/to/Database.php';

use App\Config\Database;

$database = new Database();
$db = $database->getConnection();

if ($db) {
    echo "Connected successfully.";
} else {
    echo "Connection failed.";
}
?>

```

Routing en PHP
web.php
```php

<?php

use App\NotaCredito\Controllers\NotaCreditoController;

// Router simple usando un array de rutas
$routes = [
    '/nota-credito' => [NotaCreditoController::class, 'show']
];

$request_uri = explode('?', $_SERVER['REQUEST_URI'], 2)[0];

foreach ($routes as $path => $handler) {
    if ($path === $request_uri) {
        list($controller, $method) = $handler;
        $params = array_slice(explode('/', $request_uri), 2);
        call_user_func_array([new $controller, $method], $params);
        exit();
    }
}

// Ruta por defecto si no se encuentra ninguna coincidencia
http_response_code(404);
echo "404 Not Found";

?>


```

index.php


```php
<?php

require_once '../vendor/autoload.php';

use App\NotaCredito\Controllers\NotaCreditoController;
use App\Config\Database;

// Aquí va la lógica para inicializar el enrutador y manejar las solicitudes
require_once '../routes/web.php';
?>



```
