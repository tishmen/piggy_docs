## Reward Receptions

Reward Receptions allow end-users to redeem Rewards using the credits they've accumulated in a Loyalty Program. These Rewards can range from products, services, or any other special offers provided by the business.

### Create Reward Reception

Create a Reward Reception by specifying the Contact's UUID and the desired Reward's UUID. It enables a Contact to claim a Reward. Our backend manages various scenarios, like insufficient credits, stock limitations, Reward limits, and inactivity of Rewards, ensuring a correct Reward claiming process.

**Important:** Our own Piggy products require some sort of validation to ensure the user is the owner of the email address for reward claiming (either through the loyalty card – which needs to be activated by email address – or by logging in). If you intend to have Contacts claim rewards by finding their ID's through their email address, please make sure they're authenticated in some way.

POST

`https://api.piggy.eu/api/v3/oauth/clients/reward-receptions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_uuid` `string`

`REQUIRED`

The UUID of the Contact you want to create the Reward Reception for.

`reward_uuid` `string`

`REQUIRED`

The Reward's UUID that is related to the reception.

`contact_identifier_value` `string`

`OPTIONAL`

The Contact Identifier used by the Contact.

`shop_uuid` `string`

`REQUIRED`

The UUID of the Shop you want to create the Reward Reception for.

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
` `{
    "data": {
        "title": "Lekkerbekkie",
        "type": "reward_reception",
        "uuid": "55537e6c-23e8-4c0c-bbce-65f587fb543a",
        "credits": -2,
        "contact": {
            "uuid": "123",
            "email": "nizlslzvzrt_42@hytmxil.cym",
            "credit_balance": {
                "id": 2625740,
                "balance": 2883
            }
        },
        "shop": {
            "id": 15,
            "uuid": "123123",
            "type": "physical",
            "name": "Visjes",
            "address": {
                "id": 59,
                "shop_id": 15,
                "street": null,
                "housenumber": null,
                "postalcode": null,
                "city": null,
                "state": null,
                "country": null,
                "latitude": null,
                "longitude": null,
                "house_number": null,
                "postal_code": null
            }
        },
        "contact_identifier": null,
        "created_at": "2023-11-23T15:27:26+00:00",
        "reward": {
            "id": 20002,
            "uuid": "539483d0-fab4-40db-add5-2ec65ef12901",
            "title": "Lekkerbekkie",
            "description": null,
            "reward_type": "PHYSICAL",
            "media": {
                "type": "image",
                "value": "http://localhost:8001/storage/accounts/ea8b731d-9db1-4deb-8e12-e0d9d89b194e/uploads/1699885865/conversions/300x200.jpg"
            },
            "required_credits": 2,
            "pre_redeemable": true,
            "expiration_duration": 6307200,
            "is_active": true,
            "cost_price": null,
            "stock": null,
            "eennaam": "false",
            "middle_name": "",
            "middle_name1223434": "",
            "asdfjkl": "",
            "something": ""
        },
        "expires_at": "2024-02-04T15:27:26+00:00",
        "has_been_collected": false
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

`6003`

Reward not found.

`1017`

Onvoldoende punten \| Insufficient credits.

`1018`

The stock on this reward is empty.

`1019`

You have reached your claim limit on this reward.

`1052`

Reward is inactive.

### Reverse Reward Reception

At times, a Reward redemption needs to be reversed, because of a canceled order for instance. This will create a new Credit Reception with the same data as initially used, to reissue the credits used.

POST

`https://api.piggy.eu/api/v3/oauth/clients/reward-receptions/{{reward_reception_uuid}}/reverse`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`reward_reception_uuid` `string`

`REQUIRED`

The UUID of the Reward Reception that needs to be reversed

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
` `{
            "data": {
                "type": "credit_reception",
                "contact_identifier": null,
                "contact": {
                    "uuid": "123",
                    "email": "john@doe.com",
                    "credit_balance": {
                        "id": 2625740,
                        "balance": 1642
                    }
                },
                "shop": {
                    "id": 15,
                    "uuid": "123123",
                    "type": "physical",
                    "name": "Amsterdam Pop-up Store",
                    "address": {
                        "id": 59,
                        "shop_id": 15,
                        "street": "Sesame Street",
                        "housenumber": 12,
                        "postalcode": "9999YZ",
                        "city": "Duckburg",
                        "state": "NH",
                        "country": "NL",
                        "latitude": null,
                        "longitude": null,
                        "house_number": null,
                        "postal_code": null
                    }
                },
                "credits": 10,
                "created_at": "2023-11-06T09:12:32+00:00",
                "uuid": "294183c2-20f2-4f86-a5e5-17e71c4b0660",
                "unit": {
                    "id": 112,
                    "name": "test",
                    "label": "Calories",
                    "prefix": "",
                    "is_default": true
                },
                "unit_value": null,
                "someAttribute": null,
                "someAttribute2": null,
                "someAttribute21123": null
            },
            "meta": {}
        }

    }`

Possible errors

Code

Message

`1003`

Invalid input.

### Related

[**Contacts** \\
Get information and create contacts using our public API](https://docs.piggy.eu/v3/oauth/contacts) [**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards)
