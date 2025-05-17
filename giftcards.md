## Giftcards

The Piggy Giftcards system has two types of Giftcards: physical and digital Giftcards. Each Giftcard is uniquely identified by a hash or QR-code, along with an ID. A Giftcard Program can hold any number of Giftcards.

### List Giftcards

Returns a paginated list of Giftcards.

GET

`https://api.piggy.eu/api/v3/oauth/clients/giftcards`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`giftcard_program_uuid` `string`

`OPTIONAL`

The UUID of the Giftcard Program for which you want to retrieve Giftcards.

`page` `integer`

`OPTIONAL`

The page number.

`limit` `integer`

`OPTIONAL`

The maximum number of Giftcards to retrieve (default: 30, max: 100).

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
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
` `{
    "data": [\
    {\
        "uuid": "1646fa16-09f7-43d4-a361-a8660208485c",\
        "type": "DIGITAL",\
        "hash": "PIGGY123",\
        "expiration_date": null,\
        "active": true,\
        "upgradeable": true,\
        "amount_in_cents": 1500,\
        "giftcard_program": {\
            "uuid": "a203bee9-2523-4a19-8b7e-2d282d272b6f",\
            "name": "My Giftcards"\
        }\
    },\
    {\
        "uuid": "3a112128-0bf9-43c1-a154-e782a6ad9659",\
        "type": "DIGITAL",\
        "hash": "PIGGY345",\
        "expiration_date": null,\
        "active": true,\
        "upgradeable": true,\
        "amount_in_cents": 2500,\
        "giftcard_program": {\
            "uuid": "f6be82ff-bb19-46fa-ab5b-b55b03332bbe",\
            "name": "My Giftcards"\
        }\
    },\
    {\
        "uuid": "4a83975d-91a4-4e27-ada7-5d16bf5b68fa",\
        "type": "DIGITAL",\
        "hash": "PIGGY789",\
        "expiration_date": null,\
        "active": true,\
        "upgradeable": true,\
        "amount_in_cents": 5000,\
        "giftcard_program": {\
            "uuid": "77bde4c4-4aa1-4766-a498-3e291c41bddb",\
            "name": "My Giftcards"\
        }\
    }\
    ],
    "meta": {
        "page": 1,
        "per_page": 30,
        "last_page": 10,
        "total": 300
    }
}`

Possible errors

Code

Message

`1003`

Invalid input.

`1100`

Giftcard Program not found.

### Get Giftcard

Finds a Giftcard by its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/giftcards/{{giftcard_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

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
` `{
    "data": {
        "uuid": "d694e2a2-e982-4bfc-8269-e79dc98b38e4",
        "type": "DIGITAL",
        "hash": "GFTCRD123",
        "expiration_date": null,
        "active": true,
        "upgradeable": true,
        "amount_in_cents": 2500,
        "display_amount": "€ 25,00",
        "giftcard_program": {
            "name": "Web Giftcards"
        }
    },
}`

Possible errors

Code

Message

`1003`

Invalid input.

`5001`

Giftcard not found.

### Find Giftcard by Hash

Returns all relevant information for a Giftcard. Required for a create Giftcard Transaction call, as it provides all the necessary details.

GET

`https://api.piggy.eu/api/v3/oauth/clients/giftcards/find-one-by`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`hash` `string`

`REQUIRED`

The Giftcard's unique hash or QR-code.

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
` `{
    "data": {
        "uuid": "d694e2a2-e982-4bfc-8269-e79dc98b38e4",
        "type": "DIGITAL",
        "hash": "GFTCRD123",
        "expiration_date": null,
        "active": true,
        "upgradeable": true,
        "amount_in_cents": 2500,
        "display_amount": "€ 25,00",
        "giftcard_program": {
            "name": "Web Giftcards"
        }
    },
}`

Possible errors

Code

Message

`1003`

Invalid input.

`5001`

Giftcard not found.

### Create Giftcard

This API call allows creation of Digital Giftcards, as the creation of Physical Giftcards is currently not supported. To create a Digital Giftcard, use `type: 1` in your request.

POST

`https://api.piggy.eu/api/v3/oauth/clients/giftcards`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`type` `number`

`REQUIRED`

Giftcard type - 0: Physical, 1: Digital. Only the creation of Digital Giftcards is supported at this time.

`giftcard_program_uuid` `string`

`REQUIRED`

The UUID of the Giftcard Program to which the newly created Giftcard is to be linked.

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
19
20
21
22
23
24
25
26
` `{
    "data": {
        "id": 41654,
        "uuid": "5b3dab1c-bdde-4313-a74d-67de815c751c",
        "type": "DIGITAL",
        "hash": "41654",
        "amount_in_cents": 0,
        "upgradeable": true,
        "active": true,
        "expiration_date": "2024-12-10T16:26:20+00:00",
        "amount": 0,
        "url": "https://piggy.eu/giftcard-balance?hash=41654",
        "display_amount": "€0.00",
        "formatted_expiration_date": "2024-12-10",
        "giftcard_program": {
            "id": 1,
            "name": "My Giftcard program",
            "uuid": "123",
            "max_amount_in_cents": 51000,
            "calculator_flow": "DEFAULT",
            "expiration_days": 394
        },
        "giftcard_recipients": []
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55021`

Only Digital giftcards can by created using the public API.

`1100`

Giftcard Program not found.

### Send Giftcard by Email

This API call sends out an email containing giftcard information. The email is to be preconfigured in the Account and adhere to the following requirements: (1) must be a valid Automated email, (2) must be of category 'Giftcard recipient', (3) subscription type must be configured and Contact must be subscribed to said subscription type and (4) content must contain 'giftcard.hash' personalisation token. The email to be sent can be specified. If not specified, the system will attempt to find an email which adheres to aforementioned requirements. If none can be found, the request will fail.

POST

`https://api.piggy.eu/api/v3/oauth/clients/giftcards/{{uuid}}/send-by-email`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_uuid` `string`

`REQUIRED`

The Contact to whom the giftcard email is to be sent.

`email_uuid` `string`

`OPTIONAL`

The email that is to be used to send the giftcard. If none is given, the system will attempt to use any valid email in the account. Email must contain the 'giftcard.hash' personalisation token.

`merge_tags` `array`

`OPTIONAL`

An associative array of custom merge tags, to replace those in the email. Keys must start with 'custom.' (Example: custom.message: 'Hello')

Response Example

Show more

`1
2
3
4
5
6
7
` `{
    "data": {
        "message": "success",
        "data": "Email successfully sent",
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`5001`

Giftcard not found.

### Related

[**Giftcard Transactions** \\
Use giftcards as a payment method using Giftcard Transactions](https://docs.piggy.eu/v3/oauth/giftcard-transactions) [**Prepaid Transactions** \\
A new form of giftcards, now contact-based](https://docs.piggy.eu/v3/oauth/prepaid-transactions)
