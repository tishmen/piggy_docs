## Promotions

Vouchers are directly connected to specific Promotions, just like Giftcards are connected to Giftcard Programs. Each Promotion can have its own `name` and `description` which show up for the Contact when they get a Voucher. Also, Promotions can be set up with different rules and ways to make Vouchers. This makes it easy to fit Promotions to your business and marketing needs.

### List Promotions

Retrieves a list of all Promotions linked to an Account, providing detailed insights into each promotion's attributes such as name, description, voucher limits, and expiration periods.

GET

`https://api.piggy.eu/api/v3/oauth/clients/promotions`

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
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122
123
124
125
126
127
128
129
130
131
` `{
    "data": [\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": "meer"\
            },\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "voucher_limit": 25012,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "123"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "Gratis Smoothie",\
            "description": "Super lekker",\
            "voucher_limit": null,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "983a8692-f9de-4aa5-9fde-138d5f8f153b"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "New Voucher",\
            "description": "Gekke shit",\
            "voucher_limit": null,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "9c966b16-6212-4169-ae2f-a523566b533f"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "Test 123",\
            "description": "description 123",\
            "voucher_limit": null,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "c847707f-7f63-4132-aa77-9011702011a1"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "promotion zonder vouchers ",\
            "description": "use existing false",\
            "voucher_limit": 250,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "4f3e6cd0-55e8-4286-acb0-ba18a806573b"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "promotion met vouchers",\
            "description": "use existing true",\
            "voucher_limit": null,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "89ab7398-64d9-4e3c-b684-f55c5f666ffa"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "meh",\
            "description": "nee",\
            "voucher_limit": null,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "22f26b1c-5c24-4a13-8285-0dcb27eef05f"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "123",\
            "description": "222",\
            "voucher_limit": null,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "35d9b2a0-9f4b-401a-a59e-91263de57230"\
        },\
        {\
            "attributes": {\
                "joker": null,\
                "joker2": null,\
                "jokertje1233": null,\
                "someName2": null\
            },\
            "name": "some name for a promotion campaign",\
            "description": "free stuff !@#!!!",\
            "voucher_limit": null,\
            "limit_per_contact": null,\
            "expiration_duration": null,\
            "uuid": "b0ec7fd1-f6dd-4f39-a48c-a9573aea02ba"\
        },\
    ],
    "meta": []
}`

### Create Promotion

Globally create a Promotion for the Account in question by providing a `name` and `description` Match the Promotion to your business needs by stating a `voucher_limit`, `expiration_duration` or a `limit_per_contact`

POST

`https://api.piggy.eu/api/v3/oauth/clients/promotions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

The name for the attribute. This name is displayed (as this entity has no 'label').

`description` `string`

`REQUIRED`

The description of the Promotion.

`voucher_limit` `integer`

`OPTIONAL`

The maximum amount of Vouchers this Promotion can hold.

`limit_per_contact` `integer`

`OPTIONAL`

The maximum amount of Vouchers can be assigned to a Contact for this particular Promotion.

`expiration_duration` `integer`

`OPTIONAL`

The amount of time - expressed in seconds - a Voucher is valid before it expires.

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
        "attributes": {
            "joker": null,
            "joker2": null,
            "jokertje1233": null,
            "someName2": null
        },
        "name": "Some Promotion Name",
        "description": "Some Promotion Description",
        "voucher_limit": 30,
        "limit_per_contact": 10,
        "expiration_duration": null,
        "uuid": "dc2f0b37-02fc-496e-aa13-f78ee308c6f5"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Get Promotion

Retrieve a specific Promotion by specifying its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/promotions/{{promotion_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`promotion_uuid` `string`

`REQUIRED`

The UUID of the Promotion you want to retrieve.

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
        "attributes": {
            "attributeOne": null,
            "attributeTwo": null
        },
        "name": "Example promotion",
        "description": "Example promotion description",
        "voucher_limit": 12,
        "limit_per_contact": 3,
        "expiration_duration": 86400,
        "uuid": "5c3d59c7-7799-4d41-894a-8f46e50edfda"
    },
    "meta": []
}`

Possible errors

Code

Message

`80002`

Promotion not found.

### Related

[**Vouchers** \\
Create Vouchers for Promotions for the Contacts to use](https://docs.piggy.eu/v3/oauth/vouchers) [**Promotion Attributes** \\
Customize your Promotions and fully integrate with your software](https://docs.piggy.eu/v3/oauth/promotion-attributes)
