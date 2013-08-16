* Create User: `POST /api/user`
* Retrieve User: `GET /api/user/{User ID}`
* Update User: `POST /api/user/{User ID}`
* Delete User: `DEL /api/user/{User ID}`

## Create User: `POST /api/user` ##

Creates user object, only fails on email conflict, and returns populated user object

**Authentication:** None

**Private:** Yes

**Response:**

> Populated [User](Model.md) model

## Retrieve User: `GET /api/user/{User ID}` ##

Retrieves user object, note results dependant on auth status.

Authentication: None, Any, User
Private: No
Response:

User Authentication: {password: 0}
Any Authentication: {_details: 0, password: 0, financial: 0, contact: 0}
No Authentication: {_details: 0, password: 0, financial: 0, contact: 0, location: 0}

## Update User: `POST /api/user/{User ID}` ##

Updates user info.

Only editable by authenticated user with same id, note only admin can update balance owed

Authentication: User
Private: Yes
Request: all optional

contact.email: server will update and re-send validation email
bank_account_token: server will update the recipient with bank account token
credit_card_token: server will update customer with credit card token
name
description
location
contact
social

Response:

Populated User model (as per GET)

## Delete User: `DEL /api/user/{User ID}` ##

Deletes a user account, which is to say flips the _detail.deleted flag

Authentication: User
Private: Yes