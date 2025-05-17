## Rewards

Once end-users have saved up enough credits at a certain Loyalty Program, they can use those credits to redeem Rewards. These Rewards can be accessed through our API, which allows listing of available Rewards at a specific Shop or for a specific Contact. If claimable Rewards are found for a Contact, they can be redeemed through Reward Receptions.

### List Rewards

This API endpoint is used to list Rewards available for to be shown in-store or to list Rewards that a specific Contact can claim, with potentially a subsequent Create Reward Reception call when a Contact claims a Reward. The list of Rewards for a particular Contact might differ from the full list of Rewards.

GET

`https://api.piggy.eu/api/v3/oauth/clients/rewards`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`shop_uuid` `string`

`OPTIONAL`

The Shop UUID for which the Rewards you want to retrieve.

`contact_uuid` `string`

`OPTIONAL`

List the claimable rewards associated with the Contact by supplying their UUID.

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
` `
    {
    "data": [\
        {\
            "id": 1,\
            "uuid": "6bfe1e50-b13a-49ab-85c9-98cd8b734da2",\
            "title": "Donation to charity",\
            "type": "DIGITAL",\
            "is_active": true,\
            "reward_type": "DIGITAL",\
            "description": "Helping people",\
            "required_credits": 25,\
            "media": {\
                "type": "image",\
                "value": "https://images.unsplash.com/photo-1565945887714-d5139f4eb0ce"\
            },\
            "custom_attribute_one": "ABC",\
            "custom_attribute_two": "123"\
        },\
        {\
            "id": 2,\
            "uuid": "bcd51b4b-88be-4a5d-b658-0e59dc1dabbc",\
            "title": "Cup of coffee",\
            "type": "PHYSICAL",\
            "is_active": true,\
            "reward_type": "PHYSICAL",\
            "description": "Choice between lungo, espresso or cappuccino",\
            "required_credits": 50,\
            "media": {\
                "type": "image",\
                "value": "https://images.unsplash.com/photo-1565945887714-d5139f4eb0ce"\
            },\
            "custom_attribute_one": "DEF",\
            "custom_attribute_two": "456",\
            "pre_redeemable": false,\
            "expiration_duration": null\
        },\
        {\
            "id": 3,\
            "uuid": "539483d0-fab4-40db-add5-2ec65ef12901",\
            "title": "Coffee deluxe",\
            "type": "PHYSICAL",\
            "is_active": true,\
            "reward_type": "PHYSICAL",\
            "description": "Choice between frappuccino, latte or iced coffee",\
            "required_credits": 75,\
            "media": {\
                "type": "image",\
                "value": "https://images.unsplash.com/photo-1565945887714-d5139f4eb0ce"\
            },\
            "custom_attribute_one": "GHI",\
            "custom_attribute_two": "789",\
            "pre_redeemable": true,\
            "expiration_duration": 6307200\
        },\
        {\
            "id": 4,\
            "uuid": "31909cd8-a078-49a9-a7e1-aa95aa9e8c4d",\
            "title": "T-shirt",\
            "type": "PHYSICAL",\
            "is_active": true,\
            "reward_type": "PHYSICAL",\
            "description": "Size: S/M/L/XL",\
            "required_credits": 150,\
            "media": {\
                "type": "image",\
                "value": "https://images.unsplash.com/photo-1565945887714-d5139f4eb0ce"\
            },\
            "custom_attribute_one": "JKL",\
            "custom_attribute_two": "012",\
            "pre_redeemable": false,\
            "expiration_duration": null\
        }\
    ],
    "meta": {}
}`

Possible errors

Code

Message

`1008`

Shop not found.

`55031`

Contact not found.

### Get Reward

Retrieve a specific Reward by specifying its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/rewards/{{reward_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`reward_uuid` `string`

`REQUIRED`

The UUID of the Reward you want to retrieve.

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
        "uuid": "539483d0-fab4-40db-add5-2ec65ef12901",
        "title": "Cup of coffee",
        "description": null,
        "cost_price": null,
        "required_credits": 2,
        "media": {
            "type": "image",
            "value": "http://localhost:8001/storage/accounts/ea8b731d-9db1-4deb-8e12-e0d9d89b194e/uploads/1699885865/conversions/300x200.jpg"
        },
        "is_active": true,
        "active": true,
        "reward_type": "PHYSICAL",
        "stock": null,
        "example_attribute": "custom attributes for rewards are included like this"
    },
    "meta": {}
}`

Possible errors

Code

Message

`6003`

Reward not found.

### Update Reward

You can modify both global values and custom Reward Attributes. If you need to update a custom attribute—one that you've created specifically for a Reward—simply include it in the parameters, just as you would with the title, description, and required credits.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/rewards/{{reward_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`title` `string`

`OPTIONAL`

A new title for the Reward.

`description` `string`

`OPTIONAL`

A new description for the Reward.

`required_credits` `number`

`OPTIONAL`

Number of credits required to claim this Reward.

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
` `{
    "data": {
        "uuid": "539483d0-fab4-40db-add5-2ec65ef12901",
        "title": "Cup of coffee",
        "description": null,
        "cost_price": null,
        "required_credits": 2,
        "media": {
            "type": "image",
            "value": "http://localhost:8001/storage/accounts/ea8b731d-9db1-4deb-8e12-e0d9d89b194e/uploads/1699885865/conversions/300x200.jpg"
        },
        "is_active": true,
        "active": true,
        "reward_type": "PHYSICAL",
        "stock": null,
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`6003`

Reward not found.

### Related

[**Reward Attributes** \\
Create or list Reward Attributes for the Account in question.](https://docs.piggy.eu/v3/oauth/reward-attributes) [**Reward Reception** \\
Create reward receptions for end-users who saved up enough credits.](https://docs.piggy.eu/v3/oauth/reward-receptions)
