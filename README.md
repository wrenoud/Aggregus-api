See [Issues](#issues) below

## General ##

### `_details` ###

All models have a `_details` member which can be viewable by document owners but can only be modified server side by trigger events. These are internal flags.

i.e. If a booking is confirmed (`/api/booking/{id}/confirm`) by the host, server side the `_details.confirmed` will be set to true, but this is not accepted as a request parameter. This will then trigger the charge server side which is a seperate flow.

### `_owners` ###

All models (with the exception of the User model) have a `_owners` member which lists document owners by their roles. Each role defines how the owner can access and modify the document and is referenced in the API documentation. The `_owners` member can only be modified server side.

[User](./User)
----------------------------------------------------------------------

**[Model](./User/Model.md) | [API](./User/API.md)**

* [Create User](./User/API.md#create-user): `POST /api/user`
* [Retrieve User](./User/API.md#retrieve-user): `GET /api/user/{User ID}`
* [Update User](./User/API.md#update-user): `POST /api/user/{User ID}`
* [Delete User](./User/API.md#delete-user): `DEL /api/user/{User ID}`


[Experience](./Experience)
----------------------------------------------------------------------

**[Model](./Experience/Model.md) | [API](./Experience/API.md)**

* [Create Experience](./Experience/API.md#create-experience):
  `POST /api/experience`
* [Retrieve Experience](./Experience/API.md#retrieve-experience):
  `GET /api/experience/{Experience ID}`
* [Update Experience](./Experience/API.md#update-experience):
  `POST /api/experience/{Experience ID}`
* [Publish Experience](./Experience/API.md#publish-experience): `POST /api/experience/{Exp. ID}/publish`
* [Approve Experience](./Experience/API.md#approve-experience): `POST /api/experience/{Exp. ID}/approve`
* [Archive Experience](./Experience/API.md#archive-experience): `POST /api/experience/{Exp. ID}/archive`
* [Delete Experience](./Experience/API.md#delete-experience):
  `DEL /api/experience/{Experience ID}`

[Booking](./Booking)
----------------------------------------------------------------------

**[Model](./Booking/Model.md) | [API](./Booking/API.md)**

* [Create Booking](./Booking/API.md#create-booking): `POST /api/booking`
* [Retrieve Booking](./Booking/API.md#retrieve-booking):
  `GET /api/booking/{Booking ID}`
* [Update Booking](./Booking/API.md#update-booking):
  `POST /api/booking/{Booking ID}`
* [Confirm Booking](./Booking/API.md#confirm-booking):
  `POST /api/booking/{Booking ID}/confirm`
* [Delete Booking](./Booking/API.md#delete-booking):
  `DEL /api/booking/{Booking ID}`

Issues
----------------------------------------------------------------------

_to be addressed_

### Photos ###

Need to figure out endpoints and image handling for Users and Experiences.