## Perks

Perks are benefits associated with different Tiers in our system, designed to enhance customer engagement and loyalty Each tier, such as Bronze, Silver, and Gold, comes with specific perks like percentage discounts or free delivery. These Perks can be retrieved via our API to implement and manage the associated benefits, like applying set discount Perks.

### List Perks

This API call fetches a list of all Perks with their Perk Options.

GET

`https://api.piggy.eu/api/v3/oauth/clients/perks`

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
` `{
    "data": [\
        {\
            "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",\
            "label": "Discount",\
            "name": "discount",\
            "data_type": "single_select",\
            "options": [\
                {\
                    "label": "No discount",\
                    "value": "0_off",\
                    "default": true,\
                },\
                {\
                    "label": "10% off",\
                    "value": "10_off",\
                    "default": false,\
                },\
                {\
                    "label": "25% off",\
                    "value": "25_off",\
                    "default": false,\
                }\
            ]\
        }\
    ]
}`

### Create Perk

Creates a new Perk.

POST

`https://api.piggy.eu/api/v3/oauth/clients/perks`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`label` `string`

`REQUIRED`

The label of the perk.

`name` `string`

`REQUIRED`

The name of the perk (used internally).

`data_type` `string`

`REQUIRED`

The data type of the perk, available options: single\_select or boolean.

`options` `array`

`REQUIRED`

The options of the perk (should contain a label, value and default key with values). When data\_type is boolean, only true and false values are allowed. A default option is required: this one will be assigned to your existing tiers.

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
` `{
    "data": {
        "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",
        "label": "Discount",
        "name": "discount",
        "data_type": "single_select",
        "options": []
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Get Perk

Retrieve a Perk with its Perk Options using its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/perks/{{perk_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`perk_uuid` `string`

`REQUIRED`

The Perk's UUID

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
        "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",
        "label": "Discount",
        "name": "discount",
        "data_type": "single_select",
        "options": [\
            {\
                "label": "No discount",\
                "value": "0_off",\
                "default": true,\
            },\
            {\
                "label": "10% off",\
                "value": "10_off",\
                "default": false,\
            },\
            {\
                "label": "25% off",\
                "value": "25_off",\
                "default": false,\
            }\
        ]
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`110000`

Perk not found.

### Update Perk

Updates an existing Perk.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/perks/{{perk_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`perk_uuid` `string`

`REQUIRED`

The Perk's UUID

Body

`label` `string`

`REQUIRED`

The label of the perk.

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
        "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",
        "label": "Discount",
        "name": "discount",
        "data_type": "single_select",
        "options": [\
            {\
                "label": "No discount",\
                "value": "0_off",\
                "default": true,\
            },\
            {\
                "label": "10% off",\
                "value": "10_off",\
                "default": false,\
            },\
            {\
                "label": "25% off",\
                "value": "25_off",\
                "default": false,\
            }\
        ]
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`110000`

Perk not found.

### Delete Perk

Deletes an existing Perk.

DELETE

`https://api.piggy.eu/api/v3/oauth/clients/perks/{{perk_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`perk_uuid` `string`

`REQUIRED`

The Perk's UUID

Response Example

Show more

`1
2
3
4
` `{
    "data": null,
    "meta": {}
}`

Possible errors

Code

Message

`110000`

Perk not found.

### Related

[**Tiers** \\
You can offer your Contacts an extra bonus by using Tiers](https://docs.piggy.eu/v3/oauth/tiers) [**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts)
