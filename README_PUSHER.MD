# Instalación de Pusher

Para instalar Pusher usando Composer, sigue los siguientes pasos:

1. Abre una terminal y navega al directorio de tu proyecto.
2. Ejecuta el siguiente comando para instalar la biblioteca de Pusher:

    ```bash
    composer require pusher/pusher-php-server
    ```

¡Y eso es todo! Ahora has instalado y configurado Pusher en tu proyecto PHP.
## Ejemplo de Uso en Laravel 9

Para integrar Pusher en un proyecto Laravel 9, puedes seguir estos pasos adicionales:

### Crear un Evento

Primero, crea un evento que se encargará de la transmisión de mensajes. Puedes hacerlo ejecutando el siguiente comando:

```bash
php artisan make:event PusherBroadcast
```

Luego, edita el archivo del evento generado en `app/Events/PusherBroadcast.php` para que se vea así:

```php
<?php

namespace App\Events;

use Illuminate\Broadcasting\InteractsWithSockets;
use Illuminate\Contracts\Broadcasting\ShouldBroadcast;
use Illuminate\Foundation\Events\Dispatchable;
use Illuminate\Queue\SerializesModels;

class PusherBroadcast implements ShouldBroadcast
{
    use Dispatchable, InteractsWithSockets, SerializesModels;

    public string $message;

    public function __construct(string $message)
    {
        $this->message = $message;
    }

    public function broadcastOn(): array
    {
        return ['public'];
    }

    public function broadcastAs(): string
    {
        return 'chat';
    }
}
```

### Crear un Controlador

A continuación, crea un controlador para manejar las solicitudes de transmisión. Puedes hacerlo ejecutando el siguiente comando:

```bash
php artisan make:controller PusherController
```

Luego, edita el archivo del controlador generado en `app/Http/Controllers/PusherController.php` para que se vea así:

```php
<?php

namespace App\Http\Controllers;

use App\Events\PusherBroadcast;
use Illuminate\Contracts\Foundation\Application;
use Illuminate\Contracts\View\Factory;
use Illuminate\Contracts\View\View;
use Illuminate\Http\Request;

class PusherController extends Controller
{
    public function index()
    {
        return view('index');
    }

    public function broadcast(Request $request)
    {
        broadcast(new PusherBroadcast($request->get('message')))->toOthers();

        return view('broadcast', ['message' => $request->get('message')]);
    }

    public function receive(Request $request)
    {
        return view('receive', ['message' => $request->get('message')]);
    }
}
```

### Configurar Pusher

Finalmente, asegúrate de configurar las credenciales de Pusher en tu archivo `.env`:

```
PUSHER_APP_ID=your-app-id
PUSHER_APP_KEY=your-app-key
PUSHER_APP_SECRET=your-app-secret
PUSHER_APP_CLUSTER=mt1
```

Y eso es todo. Ahora has integrado Pusher en tu proyecto Laravel 9.