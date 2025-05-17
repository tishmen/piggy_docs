## Collectable Rewards

Collectable Rewards represent Physical Rewards that have been claimed but are yet to be collected. These Rewards are stored at your Shop and await pickup by the Contact who claimed them. This section of the API deals with managing and tracking these Rewards.

### List Collectable Rewards

Retrieve a collection of Physical Rewards that still need to be picked up at the Shop by the Contact that has claimed them remotely.

GET

`https://api.piggy.eu/api/v3/oauth/clients/collectable-rewards`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

Retrieve an overview of all uncollected rewards for the Contact using their UUID.

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
` `{
    "data": [\
        {\
            "contact": {\
                "uuid": "123",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "balance": 250\
                }\
            },\
            "created_at": "2023-11-13T14:37:04+00:00",\
            "uuid": "abc012-def789-u4k3l2-890xyz",\
            "title": "Cup of coffee",\
            "reward": {\
                "id": 20002,\
                "uuid": "539483d0-fab4-40db-add5-2ec65ef12901",\
                "title": "Cup of coffee",\
                "description": "Espresso or black coffee",\
                "reward_type": "PHYSICAL",\
                "media": {\
                    "type": "image",\
                    "value": "http://images.google.com/image/coffee/300x200.jpg"\
                },\
                "required_credits": 2,\
                "pre_redeemable": true,\
                "expiration_duration": 6307200,\
                "is_active": true,\
                "cost_price": null,\
                "stock": null,\
            },\
            "expires_at": "2024-01-25T14:37:04+00:00",\
            "has_been_collected": false\
        }\
    ],
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`55031`

Contact not found.

### Collect Reward

To collect a Reward, the UUID of the Loyalty Transaction needs to be stated as query parameter inside the URI. It updates the existing Reward resource and sets the `has_been_collected` property to true.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/collectable-rewards/collect/{{loyalty_transaction_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`loyalty_transaction_uuid` `string`

`REQUIRED`

UUID of the Loyalty Transaction that was generated when the Reward was claimed by the Contact.

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
` `{
    "data": {
        "contact": {
            "uuid": "123abc-lmnopq-3242a-8899aa",
            "email": "john@doe.com"
        },
        "created_at": "2023-11-13T14:12:12+00:00",
        "uuid": "13ac4cc1-3522-4dfc-ae4a-a821e5090108",
        "title": "Cup of coffee",
        "reward": {
            "id": 20001,
            "uuid": "0fb363f8-6746-4e9b-ab31-1d4324ed2021",
            "title": "Cup of coffee",
            "description": "Espresso or black coffee",
            "reward_type": "PHYSICAL",
            "media": {
                "type": "image",
                "value": "http://images.google.com/image/300x225.jpg"
            },
            "required_credits": 10,
            "pre_redeemable": true,
            "expiration_duration": 2592000,
            "is_active": true,
            "cost_price": 17,
            "stock": 99,
        },
        "expires_at": "2023-12-13T14:12:12+00:00",
        "has_been_collected": true
    },
    "meta": {}
}`

Possible errors

Code

Message

`6003`

Reward not found.

`80300`

Reward not found.

`80300`

Reward is expired.

### Related

[**Rewards** \\
List or update your Rewards](https://docs.piggy.eu/v3/oauth/rewards) [**Loyalty Transactions** \\
List your Loyalty Transactions by filtering them on Contact, Shop or Loyalty Transaction type.](https://docs.piggy.eu/v3/oauth/loyalty-transactions)
