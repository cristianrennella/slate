---
title: API GOcuotas

language_tabs:
  - json

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

search: true
---

# Introducción

Bienvenido a la API de GOcuotas!

El proceso completo consiste en los siguientes 4 pasos:

1) Autenticarse con email y contraseña. Esto le devolverá un token.

2) Crear el recurso `checkout`.

3) Redireccionar al usuario a la `url_init` indicada por el `checkout` recién creado.

4) Una vez que el usuario finaliza el proceso de pago es redireccionado de vuelta a su tienda.

<aside class="success">
  API endpoint: https://www.gocuotas.com/api/v1/
</aside>

Por cualquier duda por favor contactarse con cristian@gocuotas.com

# Autenticación

> Para autentificarse usa el siguiente formato:

> **Request Body**

```json
  {
    "email": "nombre@comercio.com",
    "password": "123"
  }
```
> Asegúrate de reemplazar el email y la contraseña con el provisto por GOcuotas para tu comercio.

> **Response Body**

```json
  {
    "token": "authToken",
  }
```

### HTTP Request
`POST https://www.gocuotas.com/api/v1/authentication`

Las credenciales del comercio deberán ser obtenidas de los administradores de GOcuotas.

Con las credenciales, se deberá obtener un token de corta duración que permitirá autenticar una futura request a la API de GOcuotas con `email` y `password`.

Una vez que obtienes un token válido, deberá ser utilizado en el `Header` de las siguientes request con el formato:

<aside class="notice">
  Authorization: Bearer authToken
</aside>

# Proceso de pago a través de la redirección del usuario
### HTTP Request
`POST https://www.gocuotas.com/api/v1/checkouts`

Este checkout endpoint crea un checkout en nuestro sistema y retorna una URL a la cual debes redireccionar a tu usuario.

> **Request Header**

```json
     {
       "Authorization": "Bearer authToken",
       "Content-Type": "application/json"
     },
```

> **Request Body**

```json
     {
       "amount_in_cents": 300000,
       "order_reference_id": "00025",
       "url_success": "http://tu-comercio.com/gocuotas/success",
       "url_failure": "http://tu-comercio.com/gocuotas/failure",
     }
```
> Asegúrate de reemplazar `authToken` con tu propio token.

### Parámetros del Request

Parámetro | Description
--------- | -----------
amount_in_cents | Monto total de la compra expresado en centavos. Si la venta es de $3.000,00 (tres mil pesos) entonces el valor sería 300000.
order_reference_id | Es indicador de la compra que sea de utilidad para tu comercio. Pueden definir ustedes el valor de acuerdo a su preferencia.
url_success | Es a donde se redireccionará al cliente una vez que complete el proceso de pago con éxito en nuestra plataforma.
url_failure | Es a donde se redireccionará al cliente en el caso de falla por cualquier motivo que no se haya podido concretar el pago.

> **Response Body**

```json
     {
       "url_init": "https://www.gocuotas.com/checkouts/123"
     }
```

### Parámetros del Response

Parámetro | Description
--------- | -----------
url_init | Es a donde debes redireccionar al cliente para que inicie el pago en GOcuotas.
