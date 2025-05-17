## Units

Units can be seen as the input values for Credit Receptions. An input value goes in, a certain amount of credits comes out. For most Piggy Clients, 'purchase\_amount' will be the default value, but it can be virtually anything (calories, kilometers, visits; anything countable).

An Account always has at least one Unit, and one of the Units is the Account's default (fallback) Unit. This default Unit is used whenever the Unit to be used is not specified.

### List Units

Lists all Units for an account. These can subsequently be used to create Credit Receptions, where the unit\_name parameter needs to be supplied if the default unit purchase\_amount is not wished to be used.

GET

`https://api.piggy.eu/api/v3/oauth/clients/units`

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
` `{
    "data": [\
        {\
            "name": "purchase_amount",\
            "label": "Purchase Amount",\
            "prefix": "",\
            "is_default": false\
        },\
        {\
            "name": "test",\
            "label": "Calories",\
            "prefix": "",\
            "is_default": true\
        }\
    ],
    "meta": {}
}`

### Create Unit

Create a new Unit for your Account with this API call. By default, a Unit always exists for an Account. If `is_default` is set to `true`, the newly created Unit will be the default Unit for the Account, meaning this will be used for Credit Receptions if Unit is not specified. For integrating purposes, however, we recommend the Unit to be explicitly set if a certain Unit should be used (while creating a Credit Reception for example), and not rely on it being the default on creation, as it can later be changed.

POST

`https://api.piggy.eu/api/v3/oauth/clients/units`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

Internal and unique value for the Unit. Cannot be altered after creation.

`label` `string`

`REQUIRED`

Label of the Unit, to be outputted to end users.

`is_default` `boolean`

`OPTIONAL`

Specifies whether this Unit is to be the Account's default. Accepts: true, false, 1, 0, yes, no. Defaults to false if not set.

`prefix` `string`

`OPTIONAL`

The prefix of the Unit, this will be outputted to end users. Defaults to the first character of the name, if not set.

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
` `{
    "data": {
        "loyalty_program_id": 3545,
        "name": "test2",
        "label": "Calories",
        "is_default": true,
        "prefix": "t",
        "format": "int",
        "updated_at": "2023-11-06T14:39:24.000000Z",
        "created_at": "2023-11-06T14:39:24.000000Z",
        "id": 298
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55045`

Unit already exists.

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts) [**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards)
