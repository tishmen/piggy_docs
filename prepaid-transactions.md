## Prepaid Transactions

Create contact-specific Prepaid Balances to settle refunds or reward extra credits. It works for both on-sight or online stores, allowing quick and easy redemption. The Prepaid Balance of a Contact is incrementally or decrementally updated after a Prepaid Transaction is made for that Contact. The creation of a transaction is validated by supplying the Contact details from whom the transaction is made and by ensuring the transaction is associated with the correct Shop, which is also supplied.

### Create Prepaid Transaction

This API call generates a Prepaid Transaction for a specified Contact. Necessary parameters include either the Contact's UUID or their unique identifier value, the amount in cents for the transaction, and the Shop UUID where the transaction is occurring. Using this method ensures and accurate and up-to-date record of the Contact's Prepaid balance.

POST

`https://api.piggy.eu/api/v3/oauth/clients/prepaid-transactions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_uuid` `string`

`REQUIRED_WITHOUT`

The UUID of the Contact you want to create the Prepaid Transaction for. Required without contact\_identifier\_value.

`contact_identifier_value` `string`

`REQUIRED_WITHOUT`

The Contact Identifier used to conduct the transaction. Required without contact\_uuid.

`amount_in_cents` `integer`

`REQUIRED`

The amount for the transaction, supply a round number in cents.

`shop_uuid` `string`

`REQUIRED`

The UUID of the Shop where the transaction takes place.

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
` `{
    "data": {
        "uuid": "3cff98cb-644e-4148-bf34-bac842ac602e",
        "amount_in_cents": -1000,
        "prepaid_balance": {
            "balance_in_cents": 4000,
            "balance": "€40.00"
        },
        "created_at": "2023-11-12T19:49:22+00:00",
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "contact_identifier": {
            "name": "Loyalty Card",
            "value": "cilwjefwnsvidjfkjskjf",
            "active": true
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

`60003`

Contact identifier not found.

`60004`

Contact identifier inactive.

`1008`

Shop not found.

### List Prepaid Transactions

Returns a paginated list of Prepaid Transactions, which can be filtered either by Contact or Shop.

GET

`https://api.piggy.eu/api/v3/oauth/clients/prepaid-transactions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`OPTIONAL`

The Contact's UUID for whom the Prepaid Transactions are to be retrieved.

`shop_uuid` `string`

`OPTIONAL`

The Shop's UUID for which the Prepaid Transactions are to be retrieved.

`limit` `string`

`OPTIONAL`

Limit of Transactions to be retrieved (max: 30, default: 10).

`page` `string`

`OPTIONAL`

Page of Transactions to retrieve (default: 1).

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
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
67
68
69
70
71
72
73
74
75
76
77
78
79
80
81
82
83
` `{
    "data": {
        [\
            "uuid": "abcde-123-789-uio",\
            "amount_in_cents": -1000,\
            "prepaid_balance": {\
                "balance_in_cents": 4000,\
                "balance": "€40.00"\
            },\
            "created_at": "2023-11-12T19:49:22+00:00",\
            "shop": {\
                "id": 15,\
                "uuid": "123123",\
                "reference": null,\
                "name": "Amsterdam Pop-up Store"\
            },\
            "contact_identifier": {\
                "name": "Loyalty Card",\
                "value": "7890UIOL",\
                "active": true\
            },\
            "contact": {\
                "uuid": "12345-abcdef-poiuy-0876",\
                "email": "john@doe.com"\
            }\
        ],
        [\
            "uuid": "asdf-123-uio-789e",\
            "amount_in_cents": 1500,\
            "prepaid_balance": {\
                "balance_in_cents": 1500,\
                "balance": "€15.00"\
            },\
            "created_at": "2023-11-14T19:49:22+00:00",\
            "shop": {\
                "id": 15,\
                "uuid": "123123",\
                "reference": null,\
                "name": "Amsterdam Pop-up Store"\
            },\
            "contact_identifier": {\
                "name": "Loyalty Card",\
                "value": "ABCDE123",\
                "active": true\
            },\
            "contact": {\
                "uuid": "poiuy-abcdef-12345-0876",\
                "email": "jane@doe.com"\
            }\
        ],
        [\
            "uuid": "890-123-uioas-789e",\
            "amount_in_cents": -3400,\
            "prepaid_balance": {\
                "balance_in_cents": 22010,\
                "balance": "€220.10"\
            },\
            "created_at": "2023-11-19T19:49:22+00:00",\
            "shop": {\
                "id": 15,\
                "uuid": "123123",\
                "reference": null,\
                "name": "Amsterdam Pop-up Store"\
            },\
            "contact_identifier": {\
                "name": "Loyalty Card",\
                "value": "UNO768",\
                "active": true\
            },\
            "contact": {\
                "uuid": "0876-abcdef-12345-abcdef",\
                "email": "joseph@doe.com"\
            }\
        ],
    },
    "meta": [\
        "current_page": 1,\
        "from": 1,\
        "last_page": 1,\
        "per_page": 3,\
        "total": 3\
    ]
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

`1008`

Shop not found.

### Get Prepaid Balance

Use this function to retrieve the Prepaid Balance of a particular Contact, identified by their UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}/prepaid-balance`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID

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
        "balance_in_cents": 5000,
        "balance": "€50.00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

### Reverse Prepaid Transaction

At times, an initial Prepaid Transaction needs to be reversed, because of a refund for instance. This will create a new Prepaid Transaction with the same data as initially used, but for the negative amount issued.

POST

`https://api.piggy.eu/api/v3/oauth/clients/prepaid-transactions/{{prepaid_transaction_uuid}}/reverse`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`prepaid_transaction_uuid` `string`

`REQUIRED`

The UUID of the Prepaid Transaction that is to be reversed.

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
` `{
    "data": {
        "uuid": "3cff98cb-644e-4148-bf34-bac842ac602e",
        "amount_in_cents": -1000,
        "prepaid_balance": {
            "balance_in_cents": 4000,
            "balance": "€40.00"
        },
        "created_at": "2023-11-12T19:49:22+00:00",
        "shop": {
            "id": 15,
            "uuid": "123123",
            "reference": null,
            "name": "Amsterdam Pop-up Store"
        },
        "contact_identifier": {
            "name": "Loyalty Card",
            "value": "cilwjefwnsvidjfkjskjf",
            "active": true
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

`60003`

Contact identifier not found.

`60004`

Contact identifier inactive.

`1008`

Shop not found.

### Related

[**Giftcards** \\
Retrieve all the information stored on Giftcards](https://docs.piggy.eu/v3/oauth/giftcards) [**Giftcard Transactions** \\
Use giftcards as a payment method using Giftcard Transactions](https://docs.piggy.eu/v3/oauth/giftcard-transactions)
