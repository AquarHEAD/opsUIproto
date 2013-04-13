opsUIproto
==========

Prototype UI for the personal account management module of a online payment system.

用户名/电子邮箱登录 用户名不能包含@

密码重置token的Model

## Interface

*Private* means (should) only available to other services in the system, but not end-user's clients.

*Public* means end-user's clients might be redirected to those API.

### Authorization

`/token/new?service_secret=`

*Private*

Generate a auth token, the `service_secret` is generated for other services "manually".

Since the end-user's client won't access this API (can even block that via local network api), it's safe to pass the secret directly in the request.

`/token/info?service_secret=&token=[key]`

*Private*

Get the token info specified by its `key` value.

The token would contain these fields:

- id: (maybe unnecessary)
- key: the token key.
- status: new, authed, expired, failed (or just a boolean authed?)
- uid: if authed then set to the authed user's id

### Login

`/login?token=&redirect=`

*Public*

A login UI, if login succeed will redirect to `redirect`, otherwise keep `token` and `redirect` and reload.

Should check cookies first, if user has logined then set the `token` and redirect immediately, otherwise show the UI.

`/logout`

*Public*

Just the logout. (maybe need rethink)

### Payment

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


### AuthToken


### ResetToken


### Activity
