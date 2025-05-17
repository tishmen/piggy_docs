**Please note:** the _Visits_ API is now in beta. Some functionalities may not yet be available at that time. Please note that the API may be subject to change.

### Create Visit

Creates a new Visit.

POST

`https://api.piggy.eu/api/v3/oauth/clients/visits`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`shop_uuid` `string`

`REQUIRED`

The UUID of the Shop you want to create the Reward Reception for.

`contact_uuid` `string`

`REQUIRED`

The UUID of the visiting Contact

`external_id` `string`

`UNIQUE | OPTIONAL`

A unique, external identifier for the Visit

`source` `string`

`OPTIONAL`

Source from where the Visit was created

`starts_at` `string`

`OPTIONAL`

The start date and time of the Visit in ISO 8601 format

`ends_at` `string`

`OPTIONAL`

The end date and time of the Visit in ISO 8601 format

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
        "uuid": "bfada889-6a69-4fcf-aed6-42933ae9084d",
        "contact": {
            "uuid": "12345-abcdefg-6789-yuiop",
            "email": "john@doe.com"
        },
        "external_id": "V-0012",
        "source": "Website",
        "starts_at": "2024-05-01T20:00:00+00:00",
        "ends_at": "2024-05-01T22:30:00+00:00",
        "created_at": "2024-04-25T13:35:09+00:00"
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

`1008`

Shop not found.

### Update Visit

Updates the attributes of a Visit. Shop and contact cannot be updated.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/visits/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`external_id` `string`

`OPTIONAL`

An external identifier for the Visit

`source` `string`

`OPTIONAL`

Source from where the Visit was created

`starts_at` `string`

`OPTIONAL`

The start date and time of the Visit in ISO 8601 format

`ends_at` `string`

`OPTIONAL`

The end date and time of the Visit in ISO 8601 format

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
        "uuid": "bfada889-6a69-4fcf-aed6-42933ae9084d",
        "contact": {
            "uuid": "12345-abcdefg-6789-yuiop",
            "email": "john@doe.com"
        },
        "external_id": "V-0012",
        "source": "Website",
        "starts_at": "2024-05-01T20:00:00+00:00",
        "ends_at": "2024-05-01T22:30:00+00:00",
        "created_at": "2024-04-25T13:35:09+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Related

[**Loyalty Tokens** \\
Create or Claim a Loyalty Token](https://docs.piggy.eu/v3/oauth/loyalty-tokens) [**Reward Receptions** \\
It's what Contacts are saving up for: Reward Receptions](https://docs.piggy.eu/v3/oauth/reward-receptions)
