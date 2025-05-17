**Please note:** the _Bookings_ API is now in beta. Some functionalities may not yet be available at that time. Please note that the API may be subject to change.

### Create Booking

Creates a new Booking.

POST

`https://api.piggy.eu/api/v3/oauth/clients/bookings`

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

The UUID of the Contact who made the booking

`external_id` `string`

`OPTIONAL`

An external identifier for the Booking

`source` `string`

`OPTIONAL`

Source from where the Booking was created

`starts_at` `string`

`OPTIONAL`

The start date and time of the Booking in ISO 8601 format

`ends_at` `string`

`OPTIONAL`

The end date and time of the Booking in ISO 8601 format

`checked_in_at` `string`

`OPTIONAL`

The date and time when the party checked into the booking in ISO 8601 format

`number_of_people` `integer`

`OPTIONAL`

The size of the party in the booking

`company_name` `string`

`OPTIONAL`

The name of the company for which the booking was made

`status` `string`

`OPTIONAL`

Status of the booking (e.g. created, in progress, canceled, completed)

`prepaid_amount` `integer`

`OPTIONAL`

(in cents) The amount that was paid prior to the start of the booking

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

### Update Booking

Updates the attributes of a Booking. Shop and contact cannot be updated.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/bookings/{{uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`external_id` `string`

`OPTIONAL`

An external identifier for the Booking

`source` `string`

`OPTIONAL`

Source from where the Booking was created

`starts_at` `string`

`OPTIONAL`

The start date and time of the Booking in ISO 8601 format

`ends_at` `string`

`OPTIONAL`

The end date and time of the Booking in ISO 8601 format

`checked_in_at` `string`

`OPTIONAL`

The date and time when the party checked into the booking in ISO 8601 format

`number_of_people` `integer`

`OPTIONAL`

The size of the party in the booking

`company_name` `string`

`OPTIONAL`

The name of the company for which the booking was made

`status` `string`

`OPTIONAL`

Status of the booking (e.g. created, in progress, canceled, completed)

`prepaid_amount` `integer`

`OPTIONAL`

(in cents) The amount that was paid prior to the start of the booking

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
` `{
    "data": {
        "uuid": "bfada889-6a69-4fcf-aed6-42933ae9084d",
        "contact": {
            "uuid": "12345-abcdefg-6789-yuiop",
            "email": "john@doe.com"
        },
        "external_id": "V-0012",
        "source": "Widget",
        "starts_at": "2024-05-01T20:00:00+00:00",
        "ends_at": "2024-05-01T22:30:00+00:00",
        "checked_in_at": "2024-05-01T20:14:46+00:00",
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
