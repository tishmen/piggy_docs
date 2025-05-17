## Shops

Shops play a central role in most transactions within our system. They can represent physical storefronts or digital web shops. The API calls below enable you to retrieve Shop data by listing Shops or retrieving a single Shop.

### List Shops

Retrieve all Shops belonging to an Account. In case this call is used for a Loyalty flow, then please check for a Loyalty program. None of the Loyalty API calls where a Shop ID is required will work if the Shop used is not linked to a Loyalty Program.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shops`

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
` `{
    "data": [\
        {\
            "uuid": "123123",\
            "name": "Amsterdam Pop-up Store",\
            "type": "physical",\
            "reference": null\
        },\
        {\
            "uuid": "12312312312312",\
            "name": "Web Shop",\
            "type": "web",\
            "reference": ""\
        },\
        {\
            "uuid": "2222e1242t3324535",\
            "name": "Sesame Street",\
            "type": "physical",\
            "reference": ""\
        },\
    ],
    "meta": {}
}`

### Get Shop

Retrieve information of your Shop. This call is useful for getting insights into individual shop settings and attributes.

GET

`https://api.piggy.eu/api/v3/oauth/clients/shops/{{shop_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`shop_uuid` `string`

`REQUIRED`

The UUID of the Shop you want to retrieve.

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
        "uuid": "123123",
        "name": "Amsterdam Pop-up Store",
        "type": "physical",
        "reference": null
    },
    "meta": {}
}`

Possible errors

Code

Message

`1008`

Shop not found.

### Related

[**Contacts** \\
Get information and create contacts using our public API](https://docs.piggy.eu/v3/oauth/contacts) [**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards)
