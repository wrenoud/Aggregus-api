![Experience workflow](./Eperience.png)

* [Create Experience](#create-experience): `POST /api/experience`
* [Retrieve Experience](#retrieve-experience): `GET /api/experience/{Exp. ID}`
* [Update Experience](#update-experience): `POST /api/experience/{Exp. ID}`
* [Publish Experience](#update-experience): `POST /api/experience/{Exp. ID}/publish`
* [Approve Experience](#approve-experience): `POST /api/experience/{Exp. ID}/approve`
* [Archive Experience](#archive-experience): `POST /api/experience/{Exp. ID}/archive`
* [Delete Experience](#delete-experience): `DEL /api/experience/{Exp. ID}`


Create Experience:
-----------------------------------------
**Route:**  `POST /api/experience`

Creates a new experience.

Note that this will always set `_details.published: false` by default. Thus a separate call to `POST /api/experience/{Exp. ID}/publish` to initiate the approval process would be required.

**Authentication:** Any

**Private:** Yes

**Request:**

>* `name` - name of the experience
>* `description` - description of the experience
>* `guest_instructions` - Instructions delivered to the guest on confirmation
>* `experience_details` - see [Experience](./Model.md) model
>* `photos` - **This may be handled elsewhere**
>* `location` - see [Experience](./Model.md) model
>* `price` - see [Experience](./Model.md) model

**Server Action:**

> Server will determine new `_id` and add the populated document to the database.

**Response:**

> Populated [Experience](./Model.md) model


Retrieve Experience:
----------------------------------------------------------
**Route:** `GET /api/experience/{Exp. ID}`

Retrieves the specific experience document.

**Note** that with Any or No Auth. only experiences with the follow settings can be returned

* `_details.approved = true`
* `_details.published = true`
* `_details.deleted = false`

**Authentication:** None, Any, Host

**Private:** No

**Request:**

**Response:**

> Populated [Experience](Model.md) model with the following restrictions
>
> *Host Authentication:*
>
>     {}
>
> *Any Authentication:*
>
>     {_details: 0}
>
> *No Authentication:*
>
>     {_details: 0}


Update Experience:
---------------------------------------------------------
**Route:** `POST /api/experience/{Exp. ID}`

Description

**Authentication:** Host

**Private:** Yes

**Request:**

>* `name` - name of the experience
>* `description` - description of the experience
>* `guest_instructions` - Instructions delivered to the guest on confirmation
>* `experience_details` - see [Experience](./Model.md) model
>* `photos` - **This may be handled elsewhere**
>* `location` - see [Experience](./Model.md) model
>* `price` - see [Experience](./Model.md) model

**Server Action:**

> Any updates to the experience with switch `_details.approved` to false, requiring re-approval.

**Response:**

> Populated [Experience](./Model.md) model


Publish Experience:
-----------------------------------------
**Route:**  `POST /api/experience/{Exp. ID}/publish`

Change published status.

If the Experience is transitioned from false to true this will put the experience up for approval unless `_details.approved` is still set to `true` in which case it will go live immediately.

**Authentication:** Host

**Private:** Yes

**Request:**

>     published: [true|false]

**Server Action:**

**Response:**


Approve Experience:
---------------------------------------------------------
**Route:** `POST /api/experience/{_Blank ID}/approve`

Description

**Authentication:**

**Private:**

**Request:**

**Server Action:**

**Response:**


Archive Experience:
---------------------------------------------------------
**Route:** `POST /api/experience/{Experience ID}/archive`

Description

**Authentication:**

**Private:**

**Request:**

**Server Action:**

**Response:**


Delete Experience:
--------------------------------------------------------
**Route:** `DEL /api/experience/{Exp. ID}`

Deletes (hides) an experience, which is to say flips the `_detail.deleted` flag.

Note that this won't cancel related bookings.

**Authentication:** Host

**Private:** Yes

**Request:**

**Response:**
