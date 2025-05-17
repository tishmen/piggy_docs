**Please note:** the _Shift Types_ API is now in beta. Some functionalities may not yet be available at that time. Please note that the API may be subject to change.

### List Shift Types

Returns a paginated list of Shift Types.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shift-types`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`page` `integer`

`OPTIONAL`

The page number.

`limit` `integer`

`OPTIONAL`

The maximum number of Shift Types to retrieve (default: 30, max: 100).

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
` `{
    "data": [\
        {\
            "uuid": "8880c45e-36ef-4ef5-ac10-8dd428260907",\
            "name": "Night",\
            "description": "Shifts between 23:00 and 06:00.",\
            "created_at": "2024-12-08T09:00:00+00:00",\
            "updated_at": "2024-12-08T17:30:00+00:00",\
        },\
        {\
            "uuid": "ec92b3cd-d896-43ac-ae8c-c8eafeb90288",\
            "name": "Weekend",\
            "description": "Shifts on Saturday or Sunday.",\
            "created_at": "2024-12-01T09:00:00+00:00",\
            "updated_at": "2024-12-08T09:00:00+00:00",\
        },\
    ],
    "meta": {
        "page": 1,
        "per_page": 30,
        "last_page": 10,
        "total": 300
    }
}`

### Create Shift Type

Creates a new Shift Type.

POST

`https://api.piggy.eu/api/v3/oauth/clients/shift-types`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

The Name of the Shift Type you want to create.

`description` `string`

`REQUIRED`

The Description of the Shift Type you want to create.

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
        "uuid": "8880c45e-36ef-4ef5-ac10-8dd428260907",
        "name": "Night",
        "description": "Shifts between 23:00 and 06:00",
        "created_at": "2024-12-08T09:00:00+00:00",
        "updated_at": "2024-12-08T17:30:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Find Shift Type by Name

Returns all relevant information for a Shift Type.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shift-types/find`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

The Name of this Shift Type.

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
        "uuid": "8880c45e-36ef-4ef5-ac10-8dd428260907",
        "name": "Night",
        "description": "Shifts between 23:00 and 06:00",
        "created_at": "2024-12-08T09:00:00+00:00",
        "updated_at": "2024-12-08T17:30:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`80550`

Shift type not found.

### Get Shift Type

Retrieve a Shift Type using its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shift-types/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`uuid` `string`

`REQUIRED`

The UUID for this Shift Type.

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
        "uuid": "8880c45e-36ef-4ef5-ac10-8dd428260907",
        "name": "Night",
        "description": "Shifts between 23:00 and 06:00",
        "created_at": "2024-12-08T09:00:00+00:00",
        "updated_at": "2024-12-08T17:30:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`80550`

Shift type not found.

### Update Shift Type

Updates the attributes of a Shift Type.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/shift-types/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`uuid` `string`

`REQUIRED`

The UUID for this Shift Type.

Body

`name` `string`

`REQUIRED`

The Name for this Shift Type.

`description` `string`

`REQUIRED`

The Description for this Shift Type.

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
        "uuid": "8880c45e-36ef-4ef5-ac10-8dd428260907",
        "name": "Night",
        "description": "Shifts between 23:00 and 06:00",
        "created_at": "2024-12-08T09:00:00+00:00",
        "updated_at": "2024-12-08T17:30:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80550`

Shift type not found.

### Related

[**Shifts** \\
List, Update or Create Shifts](https://docs.piggy.eu/v3/oauth/shifts) [**Teams** \\
Create or Update Teams for Shifts](https://docs.piggy.eu/v3/oauth/shift-teams)
