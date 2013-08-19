* [Create Experience](#create-experience): `POST /api/experience`
* [Retrieve Experience](#retrieve-experience): `GET /api/experience/{Exp. ID}`
* [Update Experience](#update-experience): `POST /api/experience/{Exp. ID}`
* [Publish Experience](#update-experience): `POST /api/experience/{Exp. ID}/publish`
* [Approve Experience](#approve-experience): `POST /api/experience/{Exp. ID}/approve`
* [Archive Experience](#archive-experience): `POST /api/experience/{Exp. ID}/archive`
* [Delete Experience](#delete-experience): `DEL /api/experience/{Exp. ID}`


Workflow
----------------------------------------------------------------------

![Experience workflow](https://raw.github.com/wrenoud/Aggregus-api/master/Experience/Experience.png)


State List
----------------------------------------------------------------------

### New

    _details: {
        published: false,
        approved: false,
        booked: false,
        archived: false,
        deleted: false
    }

Can transition to [Published](#published) or [Deleted](#deleted)

### Published

    _details: {
        published: true,
        approved: false,
        booked: false,
        archived: false,
        deleted: false
    }

Can transition to [New](#new), [Approved](#approved) or [Deleted](#deleted)

### Unpublished

    _details: {
        published: false,
        approved: true,
        booked: [true|false],
        archived: false,
        deleted: false
    }

Can transition to [New](#new) if booked is false, [Approved](#approved) if booked is false, [Booked](#booked) if booked is true, [Archived](#archived) if booking is true, or [Deleted](#deleted) if booked is false.

### Approved

    _details: {
        published: true,
        approved: true,
        booked: false,
        archived: false,
        deleted: false
    }

Can transition to [Published](#published), [Unpublished](#unpublished), [Booked](#booked), or [Deleted](#deleted)

### Booked

    _details: {
        published: true,
        approved: true,
        booked: true,
        archived: false,
        deleted: false
    }

Can transition to [Unpublished](#unpublished), or [Archived](#archived)

### Archived

    _details: {
        published: [true|false],
        approved: true,
        booked: true,
        archived: true,
        deleted: false
    }

Terminal state.

### Deleted

    _details: {
        published: [true|false],
        approved: [true|false],
        booked: false,
        archived: false,
        deleted: true
    }

Terminal state.


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

**Note** that with _Any_ or _No Auth._ only experiences with the follow settings can be returned

* `_details.published = true`
* `_details.approved = true`
* `_details.archived = false`
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

Update the experience.

Not that this will only be succesful if `_details.booked: false` and will reset published and approved flags.

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

> Any updates to the experience with switch `_details.published` and `_details.approved` to false, requiring re-approval.

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

> Populated [Experience](./Model.md) model


Approve Experience:
---------------------------------------------------------
**Route:** `POST /api/experience/{_Blank ID}/approve`

Approves an Experience for display on the site. This is only accessible by Admins and can only be executed on experiences with

    _details: {
        published: true,
        booked: false,
        archived: false,
        deleted: false
    }

**Authentication:** Admin

**Private:** Yes

**Request:**

>* `approved: [true|false]`
>* `message:` - string message to be sent with the approval message to Host

**Server Action:**

> This will trigger email to the host informing them of the status change.

**Response:**

> Populated [Experience](./Model.md) model


Archive Experience:
---------------------------------------------------------
**Route:** `POST /api/experience/{Experience ID}/archive`

This removes the Experience from the site, much like setting `_detail.published: false`. This is the only option after the Experience has been booked as an alternate to deletion.

**Authentication:** Host

**Private:** Yes

**Request:**

> This is a unidirectional process so there is options.

**Server Action:**

**Response:**


Delete Experience:
--------------------------------------------------------
**Route:** `DEL /api/experience/{Exp. ID}`

Deletes (hides) an experience, which is to say flips the `_detail.deleted` flag.

This is only available while `_details.booking: false`

**Authentication:** Host

**Private:** Yes

**Request:**

> no options

**Response:**

