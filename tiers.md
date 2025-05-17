## Tiers

You can now offer your Contacts an extra bonus by using Tiers. Tiers allow you to automatically assign Contacts a certain status. For example, consider Bronze, Silver and Gold as Tiers, where a higher total credits received or number of transactions leads to a higher Tier. You can then apply alternative loyalty rules that apply to a specific Tiers.

### List Tiers

This API call fetches a list of all Tiers available in your Loyalty Program. It's a handy way to view the different status levels that Contacts can achieve.

GET

`https://api.piggy.eu/api/v3/oauth/clients/tiers`

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
` `{
    "data": [\
        {\
            "name": "Gold",\
            "description": "VIP Members only",\
            "position": 1,\
            "media": null,\
            "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839"\
        }\
    ],
    "meta": []
}`

### Get Tier

Use this endpoint to retrieve the current loyalty Tier of a specific Contact. It’s useful for understanding a Contact's loyalty status and offering them tailored Rewards based on their Tier.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}/tier`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID from whom you want to retrieve the Tier.

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
        "name": "Gold",
        "description": "VIP Members only",
        "position": 1,
        "media": null,
        "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839"
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

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
