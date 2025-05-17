**Please note:** the _Shift Teams_ API is now in beta. Some functionalities may not yet be available at that time. Please note that the API may be subject to change.

### List Teams

Returns a paginated list of Teams for Shifts.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shift-teams`

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

The maximum number of Teams to retrieve (default: 30, max: 100).

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
` `{
    "data": [\
        {\
            "uuid": "b5701f1b-5e31-49b6-b94f-2f388e72730a",\
            "name": "Kitchen Staff",\
            "created_at": "2024-12-08T09:00:00+00:00",\
            "updated_at": "2024-12-08T09:00:00+00:00",\
        },\
        {\
            "uuid": "90597ad4-a00e-4d3c-aca0-3ca6ec222f23",\
            "name": "Housekeeping",\
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

### Create Team

Creates a new Team for Shifts.

POST

`https://api.piggy.eu/api/v3/oauth/clients/shift-teams`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

The Name of the Team you want to create.

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
` `{
    "data": {
        "uuid": "b91e16b3-f4bc-4cdb-a2ef-68b596b3ce0f",
        "name": "Management",
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

### Find Team by Name

Returns all relevant information for a Team.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shift-teams/find`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

The Name of this Team.

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
` `{
    "data": {
        "uuid": "b91e16b3-f4bc-4cdb-a2ef-68b596b3ce0f",
        "name": "Management",
        "created_at": "2024-12-08T09:00:00+00:00",
        "updated_at": "2024-12-08T17:30:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`80540`

Shift team not found.

### Get Team

Retrieve a Team using its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shift-teams/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`uuid` `string`

`REQUIRED`

The UUID for this Team.

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
` `{
    "data": {
        "uuid": "b91e16b3-f4bc-4cdb-a2ef-68b596b3ce0f",
        "name": "Management",
        "created_at": "2024-12-08T09:00:00+00:00",
        "updated_at": "2024-12-08T17:30:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`80540`

Shift team not found.

### Update Team

Updates the attributes of a Team.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/shift-teams/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`uuid` `string`

`REQUIRED`

The UUID for this Team.

Body

`name` `string`

`REQUIRED`

The Name for this Team.

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
` `{
    "data": {
        "uuid": "b91e16b3-f4bc-4cdb-a2ef-68b596b3ce0f",
        "name": "Management",
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

`80540`

Shift team not found.

### Related

[**Shifts** \\
List, Update or Create Shifts](https://docs.piggy.eu/v3/oauth/shifts) [**Shift Types** \\
Create or Update Shift Types](https://docs.piggy.eu/v3/oauth/shift-types)
