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
- Payment Recipient: the other one involved in the payment
- Money Amount: how much money to pay
- Success Callback: callback on success
- Failure Callback: callback on failure
- Detail Link(optional): a link to where user can view the details
- Expire Time: how long is a payment valid

`/payment/info/[order_id]?service_id=&service_secret=`

*Private*

For checking payment status asynchronously.

`/pay/[payment_token]`

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

- id: `int`
- username: `string` cannot contain '@'
- email: `string`
- password: `string` encrypted with bcrypt
- realname: `string`
- id_number: `string`
- payment_password: `string` encrypted with bcrypt
- bind_phone: `string` (optional)
- sec_question: `string` (optional)
- sec_answer: `string` (optional)
- actived: `boolean`
- join_date: `datetime`
- grow_points: `int` or `float` (may need this extra info when pay for thing)
- balance: `float`

### AuthToken

- key: `string` a unique token
- status: `string` fresh, authed, expired
- expire_date: `datetime` cannot be used later than this
- uid: `foreignkey` if authed then set to the authed user's id
- login_ip: `string` (optional)
- login_date: `datetime`

### ResetToken

- key: `string`
- uid: `foreignkey` -> user.id
- used: `boolean`
- used_date: `datetime`
- expire_date: `datetime`

### Payment

- payment_token: `string`
- payment_name: `string`
- payment_type(maybe): `string` 收款/付款?
- recipient: `string` to whom the payment is made
- money_amount: `float`
- status: `string` pending, succeed, canceled, expired
- created_date: `datetime`
- finished_date: `datetime`

### PrepaidCard

- identifier: `string`
- password: `string`
- value: `float`
- created_date: `datetime`
- used_date: `datetime`
- expire_date: `datetime`

### ServiceProvider

- service_id: `string`
- service_secret: `string`
- service_name: `string`