## Giftcard Transactions

Our software only allows non-reincrementable Giftcards for transaction creation through OAuth, due to security reasons. This ensures that Giftcards cannot have negative balances; in other words, they cannot be decremented by an amount larger than their current stored value. Each transaction is linked to a Giftcard, represented by a hash or QR-code and an ID, offering secure and reliable tracking of Giftcard usage.

### List Giftcard Transactions

This API call provides a list of all Giftcard Transactions. It offers filtering options based on per-page count, page number, Giftcard hash, Giftcard Program UUID, and Shop UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/giftcard-transactions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`perPage` `string`

`OPTIONAL`

The number of Giftcard Transactions to retrieve (min: 0, max: 100, default: 10)

`page` `number`

`OPTIONAL`

Page number (default: 1).

`gifcard_hash` `string`

`REQUIRED_IF`

Required if giftcard\_program\_uuid or shop\_uuid is not specified.

`giftcard_program_uuid` `string`

`REQUIRED_IF`

Required if giftcard\_hash or shop\_uuid is not specified.

`shop_uuid` `string`

`REQUIRED_IF`

Required if giftcard\_hash or giftcard\_program\_uuid is not specified.

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
132
133
134
135
136
137
138
139
140
141
142
143
144
145
146
147
148
149
150
151
152
153
154
155
156
157
158
159
160
161
162
163
164
165
166
167
168
169
170
171
172
173
174
175
176
177
178
179
180
181
182
183
184
185
186
187
188
189
190
191
192
` `{
    "data": [\
        {\
            "id": 1,\
            "uuid": null,\
            "amount_in_cents": 2500,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 1397,\
                "uuid": "37485734857385",\
                "name": "De Groentewinkel"\
            },\
            "settlements": [],\
            "created_at": "2016-11-29T18:42:34+00:00"\
        },\
        {\
            "id": 2,\
            "uuid": null,\
            "amount_in_cents": -2200,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 1397,\
                "uuid": "37485734857385",\
                "name": "De Groentewinkel"\
            },\
            "settlements": [],\
            "created_at": "2016-11-29T18:43:13+00:00"\
        },\
        {\
            "id": 3,\
            "uuid": null,\
            "amount_in_cents": 1000,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 1397,\
                "uuid": "37485734857385",\
                "name": "De Groentewinkel"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T09:25:26+00:00"\
        },\
        {\
            "id": 4,\
            "uuid": null,\
            "amount_in_cents": -500,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 1397,\
                "uuid": "37485734857385",\
                "name": "De Groentewinkel"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T09:25:43+00:00"\
        },\
        {\
            "id": 5,\
            "uuid": null,\
            "amount_in_cents": -800,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 1397,\
                "uuid": "37485734857385",\
                "name": "De Groentewinkel"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T09:26:08+00:00"\
        },\
        {\
            "id": 6,\
            "uuid": null,\
            "amount_in_cents": 10,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 161,\
                "uuid": "12312312312312",\
                "name": "Web Shop"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T12:48:00+00:00"\
        },\
        {\
            "id": 7,\
            "uuid": null,\
            "amount_in_cents": -9,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 161,\
                "uuid": "12312312312312",\
                "name": "Web Shop"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T12:48:22+00:00"\
        },\
        {\
            "id": 8,\
            "uuid": null,\
            "amount_in_cents": -1,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 161,\
                "uuid": "12312312312312",\
                "name": "Web Shop"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T12:48:36+00:00"\
        },\
        {\
            "id": 9,\
            "uuid": null,\
            "amount_in_cents": 2000,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 161,\
                "uuid": "12312312312312",\
                "name": "Web Shop"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T13:16:18+00:00"\
        },\
        {\
            "id": 10,\
            "uuid": null,\
            "amount_in_cents": -500,\
            "type": "STANDARD",\
            "settled": false,\
            "card": {\
                "id": 1,\
                "uuid": null\
            },\
            "shop": {\
                "id": 161,\
                "uuid": "12312312312312",\
                "name": "Web Shop"\
            },\
            "settlements": [],\
            "created_at": "2016-11-30T13:16:36+00:00"\
        }\
    ],
    "meta": {
        "page": 1,
        "limit": 10,
        "viewing_from": 1,
        "viewing_to": 10,
        "last_page": 120,
        "total": 1192
    }
}`

### Create Giftcard Transaction

This API call facilitates the creation of a transaction for a specific Giftcard. It handles both increment and decrement transactions. A negative amount will trigger a decrementation, adjusting the Giftcard's balance accordingly.

POST

`https://api.piggy.eu/api/v3/oauth/clients/giftcard-transactions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`giftcard_uuid` `string`

`REQUIRED`

The UUID of the Giftcard you want to create the transaction for.

`amount_in_cents` `integer`

`REQUIRED`

The amount in cents with which the Giftcard is to be incremented or decremented. Supply a negative amount for a decrementation.

`shop_uuid` `string`

`REQUIRED`

UUID of the Shop where the Giftcard Transaction has taken place.

`custom_attributes` `array`

`OPTIONAL`

Additional Custom Attributes as a key-value array.

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
` `{
    "data": {
        "id": 2271,
        "uuid": "355892c5-6bf7-4167-bae2-3bf2bf44c3dc",
        "amount_in_cents": -1000,
        "type": "STANDARD",
        "settled": false,
        "card": {
            "id": 41650,
            "uuid": "45d7ce10-16cc-4bb7-b37f-e8c4bc553fdf"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "name": "Amsterdam Pop-up Store"
        },
        "settlements": [],
        "created_at": "2023-11-12T16:10:15+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`5006`

Deze giftcard kan niet opgewaardeerd worden. \| This giftcard cannot be incremented.

`5008`

Maximale limiet op kaart bereikt. \| Max amount will be exceeded.

`5003`

Het bedrag op de kaart is niet toereikend. \| Insufficient amount on giftcard.

`5002`

Het bedrag op de kaart is verlopen. \| This giftcard has expired.

`1000`

Interne fout. \| Internal error.

### Get Giftcard Transaction

Retrieve details of a specific Giftcard Transaction. By providing the transaction's UUID, you can access comprehensive information about individual transactions, allowing you to see and keep track of Giftcard Transactions, making it easier to manage Giftcards.

GET

`https://api.piggy.eu/api/v3/oauth/clients/giftcard-transactions/{{giftcard_transaction_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`giftcard_transaction_uuid` `string`

`REQUIRED`

The UUID of the Giftcard Transaction that you want to retrieve.

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
` `
{
    "data": {
        "id": 2268,
        "uuid": "9e6426ef-1916-4f3c-a94f-4cf6af4f945b",
        "amount_in_cents": 5,
        "type": "STANDARD",
        "settled": false,
        "card": {
            "id": 41592,
            "uuid": "f12e8cd7-47de-4f6d-bdf3-e282ace87eb0"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "name": "Amsterdam Pop-up Store"
        },
        "settlements": [],
        "created_at": "2023-10-09T10:02:43+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`5010`

Giftcard transaction not found.

### Reverse Giftcard Transaction

At times, an initial Giftcard Transaction needs to be reversed, because of a refund for instance. This will create a new Giftcard Transaction with the same data as initially used, but for the negative amount issued.

POST

`https://api.piggy.eu/api/v3/oauth/clients/giftcard-transactions/{{giftcard_transaction_uuid}}/reverse`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`giftcard_transaction_uuid` `string`

`REQUIRED`

The UUID of the Giftcard Transaction that is to be reversed.

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
` `{
    "data": {
        "id": 2271,
        "uuid": "355892c5-6bf7-4167-bae2-3bf2bf44c3dc",
        "amount_in_cents": -1000,
        "type": "STANDARD",
        "settled": false,
        "card": {
            "id": 41650,
            "uuid": "45d7ce10-16cc-4bb7-b37f-e8c4bc553fdf"
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "name": "Amsterdam Pop-up Store"
        },
        "settlements": [],
        "created_at": "2023-11-12T16:10:15+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`5010`

Giftcard transaction not found.

`5006`

Deze giftcard kan niet opgewaardeerd worden. \| This giftcard cannot be incremented.

`5008`

Maximale limiet op kaart bereikt. \| Max amount will be exceeded.

`5003`

Het bedrag op de kaart is niet toereikend. \| Insufficient amount on giftcard.

`5002`

Het bedrag op de kaart is verlopen. \| This giftcard has expired.

`55025`

This is not the giftcards last transaction.

`1000`

Interne fout. \| Internal error.

### Related

[**Prepaid Transactions** \\
A new form of giftcards, now contact-based](https://docs.piggy.eu/v3/oauth/prepaid-transactions) [**Giftcards** \\
Retrieve all the information stored on Giftcards](https://docs.piggy.eu/v3/oauth/giftcards)
