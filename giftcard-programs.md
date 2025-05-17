## Giftcard Programs

The Piggy Giftcards system has two types of giftcards: physical and digital giftcards, which are tied to a specific Giftcard Program. Each program includes details like `card_count`, status, and `expiration_days`. For a more detailed description of what comprises a Giftcard, please checkout the Giftcard section.

### List Giftcard Programs

Retrieve a list of your Giftcard Programs.

GET

`https://api.piggy.eu/api/v3/oauth/clients/giftcard-programs`

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
` `{
    "data": [\
        {\
            "id": 1,\
            "card_count": 29928,\
            "settles": 0,\
            "uuid": "123",\
            "name": "My Giftcard program",\
            "active": true,\
            "expiration_days": 394,\
            "max_amount_in_cents": 51000,\
            "min_amount_in_cents": null,\
            "calculator_flow": "DEFAULT"\
        },\
        {\
            "id": 54,\
            "card_count": 81,\
            "settles": 0,\
            "uuid": "8fb59dcf-fb5b-4cee-88d8-35e361e55a12",\
            "name": "asdas",\
            "active": true,\
            "expiration_days": null,\
            "max_amount_in_cents": null,\
            "min_amount_in_cents": null,\
            "calculator_flow": "DEFAULT"\
        }\
    ],
    "meta": []
}`

### Related

[**Giftcard Transactions** \\
Use giftcards as a payment method using Giftcard Transactions](https://docs.piggy.eu/v3/oauth/giftcard-transactions) [**Prepaid Transactions** \\
A new form of giftcards, now contact-based](https://docs.piggy.eu/v3/oauth/prepaid-transactions)
