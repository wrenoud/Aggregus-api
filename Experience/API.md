* [Create Experience](#create-experience): `POST /api/experience`
* [Retrieve Experience](#retrieve-experience): `GET /api/experience/{Exp. ID}`
* [Update Experience](#update-experience): `POST /api/experience/{Exp. ID}`
* [Publish Experience](#publish-experience): `POST /api/experience/{Exp. ID}/publish`
* [Approve Experience](#approve-experience): `POST /api/experience/{Exp. ID}/approve`
* [Archive Experience](#archive-experience): `POST /api/experience/{Exp. ID}/archive`
* [Delete Experience](#delete-experience): `DEL /api/experience/{Exp. ID}`


Workflow
----------------------------------------------------------------------

![Experience workflow](https://raw.github.com/wrenoud/Aggregus-api/master/Experience/Experience.png)


Model States
----------------------------------------------------------------------

* **[New](#new)**: This is an _Experience_ that a _Host_ can continue to _Update_ until they're ready to _Publish_ or _Delete_.
* **[Published](#published)**: This is an _Experience_ that the _Host_ feels ready to share on the website, and is pending _Approval_.
* **[Approved](#approved)**: This is an _Experience_ that is visible in the marketplace that has never been _Booked_.
* **[Booked](#booked)**: This is an _Experience_ that has been _Booked_ previously and is visible in the marketplace.
* **[Withdrawn](#withdrawn)**: This is an _Approved_ _Experience_ that the _Host_ has withdrawn from the marketplace.
* **[Archived](#archived)**: This is an _Approved_ and _Booked_ _Experience_ which has been permanently removed from the marketplace and from the _Hosts_ dashboard.
* **[Deleted](#Deleted)**: This is an _Experience_ which was never _Booked_ which has been permanently removed from the marketplace and the _Hosts_ dashboard.

### New

This is an _Experience_ that a _Host_ can continue to _Update_ until they're ready to _Publish_ or _Delete_.

    _details: {
        published: false,
        approved: false,
        booked: false,
        archived: false,
        deleted: false
    }

Can transition to:

* [New](#new) via [Update Experience](#update-experience) (Host)
* [Published](#published) via [Publish Experience](#publish-experience) (Host)
* [Deleted](#deleted) via [Delete Experience](#delete-experience) (Host)

### Published

This is an _Experience_ that the _Host_ feels ready to share on the website, and is pending _Approval_. This can still be edited but will need to be _Published_ after an _Update_.

    _details: {
        published: true,
        approved: false,
        booked: false,
        archived: false,
        deleted: false
    }

Can transition to:

* [New](#new) via [Update Experience](#update-experience) (Host)
* [Approved](#approved) via [Approve Experience](#approve-experience) (Admin)
* [Deleted](#deleted) via [Delete Experience](#delete-experience) (Host)


### Approved

This is an _Experience_ that is visible in the marketplace that has never been _Booked_.

    _details: {
        published: true,
        approved: true,
        booked: false,
        archived: false,
        deleted: false
    }

Can transition to:

* [New](#new) via [Update Experience](#update-experience) (Host)
* [Published](#published) via [Approve Experience](#approve-experience) (Admin) 
* [Withdrawn](#withdrawn) via [Publish Experience](#publish-experience) (Host)
* [Booked](#booked) via [Create Booking](../Booking/API.md#create-booking) (Host)
* [Deleted](#deleted) via [Delete Experience](#delete-experience) (Host)


### Booked

This is an _Experience_ that has been _Booked_ previously and is visible in the marketplace.

    _details: {
        published: true,
        approved: true,
        booked: true,
        archived: false,
        deleted: false
    }

Can transition to:

* [Withdrawn](#withdrawn) via [Publish Experience](#publish-experience) (Host)
* [Archived](#archived) via [Archive Experience](#archive-experience) (Host)
* [Archived](#archived) via [Update Experience](#update-experience) (Host) _Note_ that this forces a [Create Experience](#create-experience) and will return a model with a _new_ id.


### Withdrawn

This is an _Approved_ _Experience_ that the _Host_ has withdrawn from the marketplace. Available transitions are dependant on the _Booked_ state.

    _details: {
        published: false,
        approved: true,
        booked: [true|false],
        archived: false,
        deleted: false
    }

Can transition to:

> If `booked: false`
>
>* [New](#new) via [Update Experience](#update-experience) (Host)
>* [Approved](#approved) via [Publish Experience](#publish-experience) (Host)
>* [Deleted](#deleted) via [Delete Experience](#delete-experience) (Host)
>
> If `booked: true`
>
>* [Booked](#booked) via [Publish Experience](#publish-experience) (Host)
>* [Archived](#archived) via [Archive Experience](#archive-experience) (Host)


### Archived

This is an _Approved_ and _Booked_ _Experience_ which has been permanently removed from the marketplace and from the _Hosts_ dashboard but is still referenceable for any existing bookings.

    _details: {
        published: [true|false],
        approved: true,
        booked: true,
        archived: true,
        deleted: false
    }

Terminal state.

### Deleted

This is an _Experience_ which was never _Booked_ which has been permanently removed from the marketplace and the _Hosts_ dashboard.

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
