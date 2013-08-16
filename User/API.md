* [Create User](#Create): `POST /api/user`
* [Retrieve User](#Retrieve): `GET /api/user/{User ID}`
* [Update User](#Update): `POST /api/user/{User ID}`
* [Delete User](#Delete): `DEL /api/user/{User ID}`


Create User: `POST /api/user` [Create]
-----------------------------

Creates user object, only fails on email conflict, and returns populated user object

**Authentication:** None

**Private:** Yes

**Response:**

> Populated [User](Model.md) model


<a id="Retrieve"></a>Retrieve User: `GET /api/user/{User ID}`
----------------------------------------

Retrieves user object, note results dependant on auth status.

**Authentication:** None, Any, User

**Private:** No

**Response:**

> Populated [User](Model.md) model with the following restrictions

> *User Authentication:*
>
>     {password: 0}
>
> *Any Authentication:*
>
>     {_details: 0, password: 0, financial: 0, contact: 0}
>
> *No Authentication:*
>
>     {_details: 0, password: 0, financial: 0, contact: 0, location: 0}


<a id="Update"></a>Update User: `POST /api/user/{User ID}`
---------------------------------------

Updates user info.

Only editable by authenticated user with same id, note only admin can update balance owed

**Authentication:** User

**Private:** Yes

**Request:**

> All query fields are optional
>
>- `contact.email` - server will update and re-send validation email
>- `bank_account_token` - server will update the recipient with bank account token
>- `credit_card_token` - server will update customer with credit card token
>- `name` - see [User](Model.md) model
>- `description` - see [User](Model.md) model
>- `location` - see [User](Model.md) model
>- `contact` - see [User](Model.md) model
>- `social` - see [User](Model.md) model

**Response:**

> Populated [User](Model.md) model (as per GET)


<a id="Delete"></a>Delete User: `DEL /api/user/{User ID}`
--------------------------------------

Deletes a user account, which is to say flips the _detail.deleted flag

**Authentication:** User

**Private:** Yes
