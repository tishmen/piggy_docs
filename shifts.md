**Please note:** the _Shifts_ API is now in beta. Some functionalities may not yet be available at that time. Please note that the API may be subject to change.

### List Shifts

Returns a paginated list of Shifts.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shifts`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

The UUID of the Shop you want to create the Shift for.

`status` `string`

`REQUIRED`

The Status for this Shift.

`team_uuid` `string`

`OPTIONAL`

The UUID of the Team you want to create the Shift for.

`page` `integer`

`OPTIONAL`

The page number.

`limit` `integer`

`OPTIONAL`

The maximum number of Shifts to retrieve (default: 30, max: 100).

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
` `{
    "data": [\
        {\
            "uuid": "69fa84f5-1270-4479-a9f9-75737d6739bf",\
            "contact": [\
                'uuid': "33630ed6-fd17-455a-b1ed-7494c3503ab3",\
                'email': "john.doe@example.com",\
            ],\
            "external_id": "ABC-123",\
            "team_uuid": "d37b1e30-23a2-40b6-ad4c-fa30ab0e9aa7",\
            "shift_type_uuid": "1d1bf619-b097-4461-96a2-f8e7a5249519",\
            "status": "approved",\
            "break_in_minutes": 30,\
            "net_duration_in_minutes": 480,\
            "started_at": "2024-12-01T09:00:00+00:00",\
            "ended_at": "2024-12-01T17:30:00+00:00",\
            "created_at": "2024-12-08T09:00:00+00:00",\
        },\
        {\
            "uuid": "bf7e6de4-13d7-444d-bf90-a4b68cb71f11",\
            "contact": {\
                "uuid": "bf12c2bb-8ba9-49fd-aa76-a7aa35f07280",\
                "email": "jane.doe@example.com"\
            },\
            "external_id": "XYZ-789",\
            "team_uuid": "4e13e25d-74da-443c-adb6-3826199de000",\
            "shift_type_uuid": "b5701f1b-5e31-49b6-b94f-2f388e72730a",\
            "status": "pending",\
            "break_in_minutes": 45,\
            "net_duration_in_minutes": 420,\
            "started_at": "2024-12-02T08:00:00+00:00",\
            "ended_at": "2024-12-02T15:30:00+00:00",\
            "created_at": "2024-12-08T09:15:00+00:00"\
        },\
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

`1008`

Shop not found.

`80531`

Shift status not found.

`80540`

Shift team not found.

### Create Shift

Creates a new Shift.

POST

`https://api.piggy.eu/api/v3/oauth/clients/shifts`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_uuid` `string`

`REQUIRED`

The UUID of the Contact you want to create the Shift for.

`shop_uuid` `string`

`REQUIRED`

The UUID of the Shop you want to create the Shift for.

`started_at` `string`

`REQUIRED`

The start date and time of the Shift in ISO 8601 format.

`ended_at` `string`

`REQUIRED`

The end date and time of the Shift in ISO 8601 format.

`status` `string`

`REQUIRED`

The Status for this Shift.

`external_id` `string`

`OPTIONAL`

An external identifier for the Shift.

`team_uuid` `string`

`OPTIONAL`

The UUID of the Team you want to create the Shift for.

`shift_type_uuid` `string`

`OPTIONAL`

The UUID of the Shift Team you want to create the Shift for.

`break_in_minutes` `integer`

`OPTIONAL`

The break in a minutes for this Shift.

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
` `{
    "data": {
        "uuid": "69fa84f5-1270-4479-a9f9-75737d6739bf",
        "contact": [\
            'uuid': "33630ed6-fd17-455a-b1ed-7494c3503ab3",\
            'email': "john.doe@example.com",\
        ],
        "external_id": "ABC-123",
        "team_uuid": "d37b1e30-23a2-40b6-ad4c-fa30ab0e9aa7",
        "shift_type_uuid": "1d1bf619-b097-4461-96a2-f8e7a5249519",
        "status": "approved",
        "break_in_minutes": 30,
        "net_duration_in_minutes": 480,
        "started_at": "2024-12-01T09:00:00+00:00",
        "ended_at": "2024-12-01T17:30:00+00:00",
        "created_at": "2024-12-08T09:00:00+00:00",
    },
    "meta": {}
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

`80530`

Shift not found.

`80531`

Shift status not found.

`80540`

Shift team not found.

`80550`

Shift type not found.

`80531`

Shift with external id already exists for this account.

### Find Shift by External ID

Returns all relevant information for a Shift.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shifts/find`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`external_id` `string`

`REQUIRED`

The external identifier for this Shift.

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
` `{
    "data": {
        "uuid": "69fa84f5-1270-4479-a9f9-75737d6739bf",
        "contact": [\
            'uuid': "33630ed6-fd17-455a-b1ed-7494c3503ab3",\
            'email': "john.doe@example.com",\
        ],
        "external_id": "ABC-123",
        "team_uuid": "d37b1e30-23a2-40b6-ad4c-fa30ab0e9aa7",
        "shift_type_uuid": "1d1bf619-b097-4461-96a2-f8e7a5249519",
        "status": "approved",
        "break_in_minutes": 30,
        "net_duration_in_minutes": 480,
        "started_at": "2024-12-01T09:00:00+00:00",
        "ended_at": "2024-12-01T17:30:00+00:00",
        "created_at": "2024-12-08T09:00:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`80530`

Shift not found.

### Get Shift

Retrieve a Shift using its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shifts/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`uuid` `string`

`REQUIRED`

The UUID for this Shift.

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
` `{
    "data": {
        "uuid": "69fa84f5-1270-4479-a9f9-75737d6739bf",
        "contact": [\
            'uuid': "33630ed6-fd17-455a-b1ed-7494c3503ab3",\
            'email': "john.doe@example.com",\
        ],
        "external_id": "ABC-123",
        "team_uuid": "d37b1e30-23a2-40b6-ad4c-fa30ab0e9aa7",
        "shift_type_uuid": "1d1bf619-b097-4461-96a2-f8e7a5249519",
        "status": "approved",
        "break_in_minutes": 30,
        "net_duration_in_minutes": 480,
        "started_at": "2024-12-01T09:00:00+00:00",
        "ended_at": "2024-12-01T17:30:00+00:00",
        "created_at": "2024-12-08T09:00:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`80530`

Shift not found.

### Update Shift

Updates the attributes of a Shift. Shop and contact cannot be updated.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/shifts/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`uuid` `string`

`REQUIRED`

The UUID for this Shift.

Body

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact.

`team_uuid` `string`

`OPTIONAL`

The UUID of the Team.

`started_at` `string`

`REQUIRED`

The start date and time of the Shift in ISO 8601 format.

`ended_at` `string`

`REQUIRED`

The end date and time of the Shift in ISO 8601 format.

`status` `string`

`REQUIRED`

The Status for this Shift.

`break_in_minutes` `integer`

`OPTIONAL`

The break in a minutes for this Shift.

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
` `{
    "data": {
        "uuid": "69fa84f5-1270-4479-a9f9-75737d6739bf",
        "contact": [\
            'uuid': "33630ed6-fd17-455a-b1ed-7494c3503ab3",\
            'email': "john.doe@example.com",\
        ],
        "external_id": "ABC-123",
        "team_uuid": "d37b1e30-23a2-40b6-ad4c-fa30ab0e9aa7",
        "shift_type_uuid": "1d1bf619-b097-4461-96a2-f8e7a5249519",
        "status": "approved",
        "break_in_minutes": 30,
        "net_duration_in_minutes": 480,
        "started_at": "2024-12-01T09:00:00+00:00",
        "ended_at": "2024-12-01T17:30:00+00:00",
        "created_at": "2024-12-08T09:00:00+00:00",
    },
    "meta": {}
}`

Possible errors

Code

Message

`80530`

Shift not found.

`80531`

Shift status not found.

`1003`

Invalid input.

### Delete Shift

Deletes an existing Shift.

DELETE

`https://api.piggy.eu/api/v3/oauth/clients/shifts/{{shift_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`shift_uuid` `string`

`REQUIRED`

The Shift's UUID

Possible errors

Code

Message

`80530`

Shift not found.

### Related

[**Teams** \\
Create or Update Teams for Shifts](https://docs.piggy.eu/v3/oauth/shift-teams) [**Shift Types** \\
Create or Update Shift Types](https://docs.piggy.eu/v3/oauth/shift-types)
