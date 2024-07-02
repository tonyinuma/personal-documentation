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
