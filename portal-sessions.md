## Portal Sessions

A Portal Session is created for a Contact. Portal Sessions support different `scanner_types`, like camera or hand scanner, adding flexibility on how to interact with the loyalty platform.

### Get Portal Session

Retrieve a Portal Session by supplying its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions/{{portal_session_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`portal_session_uuid` `string`

`REQUIRED`

The UUID of the Portal Session that you want to retrieve.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
` `{
    "data": {
        "uuid": "d02c7730-a1bb-471d-b293-bdc5add171e5",
        "url": "https://api.piggy.eu/portal-sessions?uuid=d02c7730-a1bb-471d-b293-bdc5add171e5",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 243,
            "uuid": "2222e1242t3324535",
            "reference": "",
            "name": "Sesame Street"
        },
        "created_at": "2023-10-19T15:11:13+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Create Portal Session

When creating a portal session, a valid `shop_uuid` specifying the shop for which the portal session is to be created must be provided. Optionally, a `contact_uuid` can be provided, which will pre-fill the contact's details in the portal session. If no contact UUID is provided, the contact can be added in the portal session itself.

It is also possible to specify the `scanner_type` that is being used. This can be either 'camera' or 'hand\_scanner', and defaults to 'camera' if this option is not set.

Additionally, it is possible to enable the option for contacts to use their email-address as identification. As well as the option to start a session by registering a new contact.

Finally, the portal session can also be configured to use an on-screen keyboard.

POST

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

A valid shop UUID for the portal session being created.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the portal session is to be created. This can also be set in the portal session itself.

`scanner_type` `string`

`OPTIONAL`

Specify the type of scanner that is being used. Option: 'camera' and 'hand\_scanner'. If not set, defaults to 'camera'.

`enable_email_identification` `boolean`

`OPTIONAL`

Enable the option for contacts to use their email-address as identification. If not set, defaults to false.

`enable_registration` `boolean`

`OPTIONAL`

Enable the option to register new contacts. If not set, defaults to false.

`enable_on_screen_keyboard` `boolean`

`OPTIONAL`

Enable the on-screen keyboard. If not set, defaults to false.

`camera_service` `integer`

`OPTIONAL`

Select which camera service to use. Options include 1, 2 or 3. Defaults to 3.

`facing_mode` `string`

`OPTIONAL`

MediaTrackConstraints facingMode. Specifies the default camera if applicable. Specify 'user' to use the front camera and 'environment' to use the back camera. Defaults to 'user'.

`primary_identification_method` `string`

`OPTIONAL`

Start the identification flow with either 'email' or 'identifier'. Defaults to 'identifier'.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
` `{
    "data": {
        "uuid": "5a2066f3-d52b-4216-8177-ee341dbd9849",
        "url": "https://api.piggy.eu/portal-sessions?uuid=5a2066f3-d52b-4216-8177-ee341dbd9849",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "events": [],
        "created_at": "2023-11-12T19:43:01+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`55031`

Contact not found.

`1008`

Shop not found.

### Create Credit Reception Portal Session

A portal session can also specifically be made for credit receptions. This is done by specifying the `credit-reception` endpoint. All other parameters are the same as when creating a regular portal session.

POST

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions/credit-reception`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

A valid shop UUID for the portal session being created.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the portal session is to be created. This can also be set in the portal session itself.

`scanner_type` `string`

`OPTIONAL`

Specify the type of scanner that is being used. Option: 'camera' and 'hand\_scanner'. If not set, defaults to 'camera'.

`enable_email_identification` `boolean`

`OPTIONAL`

Enable the option for contacts to use their email-address as identification. If not set, defaults to false.

`enable_registration` `boolean`

`OPTIONAL`

Enable the option to register new contacts. If not set, defaults to false.

`enable_on_screen_keyboard` `boolean`

`OPTIONAL`

Enable the on-screen keyboard. If not set, defaults to false.

`camera_service` `integer`

`OPTIONAL`

Select which camera service to use. Options include 1, 2 or 3. Defaults to 3.

`facing_mode` `string`

`OPTIONAL`

MediaTrackConstraints facingMode. Specifies the default camera if applicable. Specify 'user' to use the front camera and 'environment' to use the back camera. Defaults to 'user'.

`primary_identification_method` `string`

`OPTIONAL`

Start the identification flow with either 'email' or 'identifier'. Defaults to 'identifier'.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
` `{
    "data": {
        "uuid": "5a2066f3-d52b-4216-8177-ee341dbd9849",
        "url": "https://api.piggy.eu/portal-sessions?uuid=5a2066f3-d52b-4216-8177-ee341dbd9849",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "events": [],
        "created_at": "2023-11-12T19:43:01+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1008`

Shop not found.

`55031`

Contact not found.

### Create Redeem Giftcard Portal Session

A portal session can also specifically be made for redeeming giftcards. This is done by specifying the `redeem-giftcards` endpoint. This session type requires a purchase amount, `amount_in_cents`, to be provided. It is also possible to specify whether multiple giftcards can be used for a single transaction by supplying the `allow_multiple_giftcards` parameter (defaults to true).

POST

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions/redeem-giftcards`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`amount_in_cents` `number`

`REQUIRED`

Purchase amount in cents (€10.40 = 1040)

`shop_uuid` `string`

`REQUIRED`

A valid shop UUID for the portal session being created.

`allow_multiple_giftcards` `boolean`

`OPTIONAL`

Enable the use of multiple giftcards per transaction. If not set, defaults to true.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the portal session is to be created. This can also be set in the portal session itself.

`scanner_type` `string`

`OPTIONAL`

Specify the type of scanner that is being used. Option: 'camera' and 'hand\_scanner'. If not set, defaults to 'camera'.

`enable_email_identification` `boolean`

`OPTIONAL`

Enable the option for contacts to use their email-address as identification. If not set, defaults to false.

`enable_registration` `boolean`

`OPTIONAL`

Enable the option to register new contacts. If not set, defaults to false.

`enable_on_screen_keyboard` `boolean`

`OPTIONAL`

Enable the on-screen keyboard. If not set, defaults to false.

`camera_service` `integer`

`OPTIONAL`

Select which camera service to use. Options include 1, 2 or 3. Defaults to 3.

`facing_mode` `string`

`OPTIONAL`

MediaTrackConstraints facingMode. Specifies the default camera if applicable. Specify 'user' to use the front camera and 'environment' to use the back camera. Defaults to 'user'.

`primary_identification_method` `string`

`OPTIONAL`

Start the identification flow with either 'email' or 'identifier'. Defaults to 'identifier'.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
` `{
    "data": {
        "uuid": "5a2066f3-d52b-4216-8177-ee341dbd9849",
        "url": "https://api.piggy.eu/portal-sessions?uuid=5a2066f3-d52b-4216-8177-ee341dbd9849",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "events": [],
        "created_at": "2023-11-12T19:43:01+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1008`

Shop not found.

`55031`

Contact not found.

### Create Top Up Giftcard Portal Session

A portal session can also specifically be made for topping up giftcards. This is done by specifying the `top-up-giftcards` endpoint. All other parameters are the same as when creating a regular portal session.

POST

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions/top-up-giftcards`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

A valid shop UUID for the portal session being created.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the portal session is to be created. This can also be set in the portal session itself.

`scanner_type` `string`

`OPTIONAL`

Specify the type of scanner that is being used. Option: 'camera' and 'hand\_scanner'. If not set, defaults to 'camera'.

`enable_email_identification` `boolean`

`OPTIONAL`

Enable the option for contacts to use their email-address as identification. If not set, defaults to false.

`enable_registration` `boolean`

`OPTIONAL`

Enable the option to register new contacts. If not set, defaults to false.

`enable_on_screen_keyboard` `boolean`

`OPTIONAL`

Enable the on-screen keyboard. If not set, defaults to false.

`camera_service` `integer`

`OPTIONAL`

Select which camera service to use. Options include 1, 2 or 3. Defaults to 3.

`facing_mode` `string`

`OPTIONAL`

MediaTrackConstraints facingMode. Specifies the default camera if applicable. Specify 'user' to use the front camera and 'environment' to use the back camera. Defaults to 'user'.

`primary_identification_method` `string`

`OPTIONAL`

Start the identification flow with either 'email' or 'identifier'. Defaults to 'identifier'.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
` `{
    "data": {
        "uuid": "5a2066f3-d52b-4216-8177-ee341dbd9849",
        "url": "https://api.piggy.eu/portal-sessions?uuid=5a2066f3-d52b-4216-8177-ee341dbd9849",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "events": [],
        "created_at": "2023-11-12T19:43:01+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1008`

Shop not found.

`55031`

Contact not found.

### Create Pay Prepaid Portal Session

A portal session can also specifically be made for prepaid transactions. This is done by specifying the `pay-prepaid` endpoint. All other parameters are the same as when creating a regular portal session.

POST

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions/pay-prepaid`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

A valid shop UUID for the portal session being created.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the portal session is to be created. This can also be set in the portal session itself.

`scanner_type` `string`

`OPTIONAL`

Specify the type of scanner that is being used. Option: 'camera' and 'hand\_scanner'. If not set, defaults to 'camera'.

`enable_email_identification` `boolean`

`OPTIONAL`

Enable the option for contacts to use their email-address as identification. If not set, defaults to false.

`enable_registration` `boolean`

`OPTIONAL`

Enable the option to register new contacts. If not set, defaults to false.

`enable_on_screen_keyboard` `boolean`

`OPTIONAL`

Enable the on-screen keyboard. If not set, defaults to false.

`camera_service` `integer`

`OPTIONAL`

Select which camera service to use. Options include 1, 2 or 3. Defaults to 3.

`facing_mode` `string`

`OPTIONAL`

MediaTrackConstraints facingMode. Specifies the default camera if applicable. Specify 'user' to use the front camera and 'environment' to use the back camera. Defaults to 'user'.

`primary_identification_method` `string`

`OPTIONAL`

Start the identification flow with either 'email' or 'identifier'. Defaults to 'identifier'.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
` `{
    "data": {
        "uuid": "5a2066f3-d52b-4216-8177-ee341dbd9849",
        "url": "https://api.piggy.eu/portal-sessions?uuid=5a2066f3-d52b-4216-8177-ee341dbd9849",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "events": [],
        "created_at": "2023-11-12T19:43:01+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1008`

Shop not found.

`55031`

Contact not found.

### Create Top Up Prepaid Portal Session

A portal session can also specifically be made to top up a contact's prepaid balance. This is done by specifying the `top-up-prepaid` endpoint. All other parameters are the same as when creating a regular portal session.

POST

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions/top-up-prepaid`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

A valid shop UUID for the portal session being created.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the portal session is to be created. This can also be set in the portal session itself.

`scanner_type` `string`

`OPTIONAL`

Specify the type of scanner that is being used. Option: 'camera' and 'hand\_scanner'. If not set, defaults to 'camera'.

`enable_email_identification` `boolean`

`OPTIONAL`

Enable the option for contacts to use their email-address as identification. If not set, defaults to false.

`enable_registration` `boolean`

`OPTIONAL`

Enable the option to register new contacts. If not set, defaults to false.

`enable_on_screen_keyboard` `boolean`

`OPTIONAL`

Enable the on-screen keyboard. If not set, defaults to false.

`camera_service` `integer`

`OPTIONAL`

Select which camera service to use. Options include 1, 2 or 3. Defaults to 3.

`facing_mode` `string`

`OPTIONAL`

MediaTrackConstraints facingMode. Specifies the default camera if applicable. Specify 'user' to use the front camera and 'environment' to use the back camera. Defaults to 'user'.

`primary_identification_method` `string`

`OPTIONAL`

Start the identification flow with either 'email' or 'identifier'. Defaults to 'identifier'.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
` `{
    "data": {
        "uuid": "5a2066f3-d52b-4216-8177-ee341dbd9849",
        "url": "https://api.piggy.eu/portal-sessions?uuid=5a2066f3-d52b-4216-8177-ee341dbd9849",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "events": [],
        "created_at": "2023-11-12T19:43:01+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1008`

Shop not found.

`55031`

Contact not found.

### Create Voucher Portal Session

A portal session can also specifically be made for vouchers. This is done by specifying the `vouchers` endpoint. All other parameters are the same as when creating a regular portal session.

POST

`https://api.piggy.eu/api/v3/oauth/clients/portal-sessions/vouchers`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

A valid shop UUID for the portal session being created.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the portal session is to be created. This can also be set in the portal session itself.

`scanner_type` `string`

`OPTIONAL`

Specify the type of scanner that is being used. Option: 'camera' and 'hand\_scanner'. If not set, defaults to 'camera'.

`enable_email_identification` `boolean`

`OPTIONAL`

Enable the option for contacts to use their email-address as identification. If not set, defaults to false.

`enable_registration` `boolean`

`OPTIONAL`

Enable the option to register new contacts. If not set, defaults to false.

`enable_on_screen_keyboard` `boolean`

`OPTIONAL`

Enable the on-screen keyboard. If not set, defaults to false.

`camera_service` `integer`

`OPTIONAL`

Select which camera service to use. Options include 1, 2 or 3. Defaults to 3.

`facing_mode` `string`

`OPTIONAL`

MediaTrackConstraints facingMode. Specifies the default camera if applicable. Specify 'user' to use the front camera and 'environment' to use the back camera. Defaults to 'user'.

`primary_identification_method` `string`

`OPTIONAL`

Start the identification flow with either 'email' or 'identifier'. Defaults to 'identifier'.

Response Example

Show more

`1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
` `{
    "data": {
        "uuid": "5a2066f3-d52b-4216-8177-ee341dbd9849",
        "url": "https://api.piggy.eu/portal-sessions?uuid=5a2066f3-d52b-4216-8177-ee341dbd9849",
        "contact": {
            "uuid": "123"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "events": [],
        "created_at": "2023-11-12T19:43:01+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1008`

Shop not found.

`55031`

Contact not found.

### Related

[**Prepaid Transactions** \\
Use prepaid as a payment method using Prepaid Transactions](https://docs.piggy.eu/v3/oauth/prepaid) [**Giftcard Transactions** \\
Use giftcards as a payment method using Giftcard Transactions](https://docs.piggy.eu/v3/oauth/giftcard-transactions)
