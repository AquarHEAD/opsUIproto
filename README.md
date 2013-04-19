opsUIproto
==========

Prototype UI for the personal account management module of a online payment system.

用户名/电子邮箱登录 

## Interface

### Terminology

*Private* means (should) only available to other services in the system, but not end-user's clients.

*Public* means end-user's clients might be redirected to those API.

`service_id` & `service_secret` is generated for other services "manually", for identifying who's requesting our resources.

### Authorization

`/token/new?service_id=&service_secret=`

*Private*

Generate a auth token.

Since the end-user's client won't access this API (can even block that via local network api), it's safe to pass the secret directly in the request.

Before redirect to our login UI, other services should check cookies first, and check if the token hasn't expired.

`/token/info/[token_key]?service_id=&service_secret=`

*Private*

Get the token info specified by its `token_key` value.

The token would contain these fields:

- id: (maybe unnecessary)
- key: the token key.
- status: new, authed, expired, failed (or just a boolean authed?)
- uid: if authed then set to the authed user's id

### Login

`/login?token=&redirect=`

*Public*

A login UI, if login succeed will redirect to `redirect`, otherwise keep `token` and `redirect` and reload.

`/logout`

*Public*

Just the logout. (maybe need rethink)

### Payment

`/payment/new?service_id=&service_secret=`

*Private*

Create a new payment.

Needed arguments:

- Payment Name: some meaningful name describing the payment
- Payment From: the other one involved in the payment
- Detail Link(optional): a link to where user can view the details


`/payment/info/[order_id]?service_id=&service_secret=`

*Private*

Mainly for checking payment status asynchronously.

`/pay/[order_id]`

*Public*

After a payment is created, should redirect user to this UI.

### User Info

`/user/[id]`

*Private*

Return the user's info whose `id` is specified in the request.

## Internals

`/`

the welcome page.

`/home`

the user's home page.

`/signup`

sign up page.

`/edit`

edit user info page.

`/charge`

add money page.

`/history`

activity history page.

## Models

### User

- username: cannot contain '@'
- email:
- password:
- status:
- join_date:
- 

### AuthToken


### ResetToken


### Activity


### Payment
