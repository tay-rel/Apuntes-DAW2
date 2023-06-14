# Enviar mail con Laravel

## Configuración del servidor de correo SMTP/API
En el contexto de Laravel, un servidor de correo puede ser un servidor **SMTP** o una **API** de correo. Un servidor SMTP (Simple Mail Transfer Protocol) es un estándar para el envío de correos electrónicos. Una API de correo es una interfaz de programación de aplicaciones que te permite enviar correos utilizando servicios externos.

### [Mailtrap](https://mailtrap.io/)

Mailtrap es un servidor de correo electrónico SMTP falso con la particularidad de que intercepta los mails enviados desde su propia bandeja de entrada. Esto es útil para el desarrollo ya que no origina spam en un correo real al realizar las pruebas de envío de correo.

Para utilizar Mailtrap, es necesario disponer de una cuenta, y una vez dentro encontraremos la información necesaria en la sección **My Inbox > Show credentials** para configurar las siguientes claves del archivo de entorno:

```env
# .env

MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=your_port
MAIL_USERNAME=your_username_code
MAIL_PASSWORD=your_password_code
MAIL_ENCRYPTION=null
```

> El servidor de correo falso **Mailtrap** debe cambiarse por un servidor de correo real al pasar a producción.
{.is-warning}


## Creación de la clase Mailable

En Laravel, la clase `Mailable` se utiliza para definir la estructura y el contenido de los correos electrónicos que se enviarán.

```php
php artisan make:mail CreateFormMailable
```

Tras ejecutar el comando se creará un fichero dentro del directorio `app/Mail`, si no existe el directorio, se crea.

La nueva clase contendrá la lógica necesaria para configurar los detalles del correo, como el remitente, el destinatario y el contenido.

## Escribir un Mailable

### Constructor de clase

En el constructor recibimos el objeto o el array de datos que esperamos recibir del formulario para enviar el mail:

```php
    public function __construct(
        protected array $contact,
    ) {
    }
````

### Remitente y asunto

En el método `envelope()` configuramos el remitente y el asunto, con los datos que nos llegan del constructor:

```php
    public function envelope(): Envelope
    {
        return new Envelope(
            from: new Address($this->contact['email'], $this->contact['name'] . ' ' . $this->contact['lastName']),
            subject: $this->contact['subject'],
        );
    }
```

### Contenido

El método `content()` define el contenido del correo electrónico.

Si se desea personalizar el formato de los datos que se enviarán en el correo electrónico, una manera de hacerlo es pasándole manualmente datos a la vista a través del parámetro `with` de la definición de contenido.

> Para ello se deben establecer estos datos en propiedades protegidas o privadas en el constructor para que los datos no se pongan automáticamente a disposición de la plantilla.
{.is-info}


```php
    public function content(): Content
    {
        return new Content(
            markdown: 'emails.contact-us',
            with: [
                'name' => $this->contact['name'] . ' ' . $this->contact['lastName'],
                'email' => $this->contact['email'],
                'subject' => $this->contact['subject'],
                'message' => $this->contact['message'],
            ],
        );
    }
```

### Diseñar la vista del mail con Markdown

Al utilizar la clave `markdown` en vez de `view`, se consigue que la vista presente el mail utilizando los componentes pre-construidos de los que dispone Laravel con Blade para Markdown.

Se crea la vista en `resources/views/emails.contact-us` (podría ser otra ruta pero siempre dentro de `resources/views/`) y se diseña la estructura del mail, por ejemplo:

```html
<x-mail::message>
<img src="{{ asset('images/logo-black.png') }}" alt="escuela-salud-logo" width="40px" height="40px" style="margin: auto; width: fit-content; display: block; margin-bottom: 5px">
<h1 style="margin: auto; width: fit-content;">¡Nuevo mensaje recibido!</h1>
<x-mail::panel>
**Remitente**: {{ $name }} <br>
**Email**: {{ $email }} <br>
**Asunto**: {{ $subject }}
</x-mail::panel>
{{ $message }}
</x-mail::message>
```

Se utilizan los componentes `<x-mail::message>` y `</x-mail::panel>` y se accede a las variables definidias en el método `contact()` con `with`. Además se aplican algunos estilos con CSS inline, aunque podría hacerse en su propia hoja de estilos para mails.

> Es importante que no haya sangría ya que podría ocasionar que el editor de Markdown no interprete bien los elementos.
{.is-warning}

## Utilización de la clase Mailable

En el método que procese el envío del botón `submit` del formulario para enviar el correo, debemos llamar a nuestra nueva clase Mailable pasándole los datos del formulario para el constructor, que en este caso se presentan en forma de `array`:

```php
use Illuminate\Support\Facades\Mail;
use App\Mail\ContactFormMailable;

public function submitForm()
{
    $contact = $this->validate();

    Mail::to('info@escueladesaludmurcia.example')->send(new ContactFormMailable($contact));
    
    // ...
}
```

## [Documentación oficial Mail Laravel 10](https://laravel.com/docs/10.x/mail)
