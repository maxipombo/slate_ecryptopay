---
title: API ecryptopay

language_tabs: # must be one of https://git.io/vQNgJ
# - shell

# toc_footers:
#   - <a href='#'>Sign Up for a Developer Key</a>
#   - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
# - errors

search: false

code_clipboard: true

meta:
  - name: ecryptopay
    content: Procesador de pagos de criptomonedas con Bitso para B2B.
---

# Introducción

Bienvenido a la documentación de la API de ecryptopay! Puedes usarla para acceder a los endpoints en los que podrás obtener la información necesaria para implementar la generación de formatos de pago con criptomonedas.

Actualmente la API se encuentra en fase experimental por lo que podremos tener cambios mayores en la implementación, igualmente el equipo estará disponible para resolver cualquier duda.
# Autenticación

Con ecryptopay usamos la autenticación con un token Bearer que lo puedes generar haciendo una petición con el email y password de tu cuenta usando el `client_id` y `client_secret` establecido en el dashboard de tu cuenta en [configuración de api](https://ecryptopay.com/user/account/api).

### HTTP Request

`POST https://ecryptopay.com/api/v1/oauth/token`

### Body Parameters

Body parameters should be JSON encoded and should be exactly the same as the JSON payload used to construct the signature.

Parameter | Default | Required | Description
--------- | ------- | -------- | -----------
grant_type | password | true | -
email | - | true | Email de la cuenta de acceso al dashboard.
password | - | true | Password de la cuenta de acceso al dashboard.
client_id | - | true | -
client_secret | - | true | -

### JSON Response Payload

Returns a JSON object representing the order:

Field Name | Type | Description | Units
---------- | ---- | ----------- | -----
access_token | String | - | -
token_type | String | - | -
expires_in | Integer | - | -
refresh_token | String | - | -
created_at | Integer | Timestamp at which the token request was created | -

# Formatos de pago

## Get All Payments

This endpoint retrieves all payments.

### HTTP Request

`GET https://ecryptopay.com/api/v1/payments`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
authorization | Bearer `<access_token>` | See [Autenticación](#autenticacion)

### JSON Response Payload

Returns a JSON Array of payments. Every element in the array is a JSON object:

Field Name | Type | Description | Units
---------- | ---- | ----------- | -----
id | Integer | - | -
currency | String | The cryptocurrency of convertion | -
amount | String | The original amount on Mexican Peso | -
created_at | String | - | -
updated_at | String | - | -
state | String | The state of the payment  | -
currency_amount | String | The amount on the currency selected | -

## Get a Specific Payment

This endpoint retrieves a specific payment.
### HTTP Request

`GET https://ecryptopay.com/api/v1/payments/<ID>`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
authorization | Bearer `<access_token>` | See [Autenticación](#autenticacion)

### JSON Response Payload

Returns a payment.

Field Name | Type | Description | Units
---------- | ---- | ----------- | -----
id | Integer | - | -
currency | String | The cryptocurrency of convertion | -
amount | String | The original amount on Mexican Peso | -
created_at | String | - | -
updated_at | String | - | -
state | String | The status of the payment  | -
currency_amount | String | The amount on the currency selected | -
qr_image_path | String | The url of the QR code generated | -

## Post a Payment

This endpoint allows you create a payment
### HTTP Request

`GET https://ecryptopay.com/api/v1/payments/<ID>`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
authorization | Bearer `<access_token>` | See [Autenticación](#autenticacion)

### Body Parameters

Parameter | Default | Description
--------- | ------- | -----------
currency | `btc` | -
amount | - | The amount on Mexican Peso

### JSON Response Payload

Returns the payment created.

Field Name | Type | Description | Units
---------- | ---- | ----------- | -----
id | Integer | - | -
currency | String | The cryptocurrency of convertion | -
amount | String | The original amount on Mexican Peso | -
created_at | String | - | -
updated_at | String | - | -
currency_amount | String | The amount on the currency selected | -
qr_image_path | String | The url of the QR code generated | -

# Webhooks

## Registering URL

Users can register a callback url that will get hit with payloads corresponding to certain events described below. Puedes registrarla por medio del apartado de tu dashboard de [configuración de api](https://ecryptopay.com/user/account/api).

<aside class="warning">
Ten en cuenta que solo podrás registrar una URL solamente y si necesitas cambiarla deberás contactar a soporte.
</aside>

## Payments

Users that register a webhook will get a POST payload to that URL with the following fields on payments.

### JSON Response Payload

Returns a JSON object with the following fields:

Field Name | Type | Description | Units
---------- | ---- | ----------- | -----
id | Integer | - | -
currency | String | The cryptocurrency of convertion | -
amount | String | The original amount on Mexican Peso | -
created_at | String | - | -
updated_at | String | - | -
state | String | The status of the payment  | -
currency_amount | String | The amount on the currency selected | -
