## Contact Subscriptions

Contact Subscriptions are the links between Subscription Types and Contacts. A Contact is either subscribed or unsubscribed, which decides whether or not they will receive emails sent through that Subscription Type. However, in the case of a Subscription Type with a double opt-in strategy, a third option comes into the mix: pending confirmation. In terms of whether they will receive emails for the Subscription Type, they will not. It is also not possible to fully subscribe a Contact for a double opt-in Subscription Type.

### List Contact Subscriptions

This function retrieves a comprehensive list of all subscriptions associated with a specific Contact, identified by their UUID. It provides an easy and efficient way to view the various subscriptions a Contact is enrolled in, ensuring you have up-to-date information on their engagement with your services.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contact-subscriptions/{{contact_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

UUID of the Contact for whom the Subscriptions are to be retrieved.

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
` `
    {
    "data": [\
        {\
            "is_subscribed": false,\
            "subscription_type": {\
                "id": 2,\
                "uuid" : null,\
                "name": "Functional",\
                "description": "Functionele emails",\
                "active": true,\
                "strategy": "OPT_OUT"\
            },\
            "status": 0\
        },\
        {\
            "is_subscribed": false,\
            "subscription_type": {\
                "id": 1,\
                "uuid" : null,\
                "name": "Marketing",\
                "description": "Marketing",\
                "active": true,\
                "strategy": "DOUBLE_OPT_IN"\
            },\
            "status": 0\
        }\
    ],
    "meta": []
}`

Possible errors

Code

Message

`55031`

Contact not found.

### Subscribe Contact

This call enables the subscription of a specific Contact to a chosen Subscription Type. By providing the Contact's UUID and the desired Subscription Type's UUID, you can easily enroll a Contact in a particular subscription.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/contact-subscriptions/{{contact_uuid}}/subscribe`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

UUID of the Contact to be subscribed.

Body

`subscription_type_uuid` `string`

`REQUIRED`

The Subscription Type's UUID.

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
` `
    {
    "data": {
        "is_subscribed": false,
        "status": "PENDING_CONFIRMATION",
        "subscription_type": {
            "name": "Marketing",
            "description": "Subscribe here for marketing purposes",
            "active": true,
            "strategy": "DOUBLE_OPT_IN",
            "uuid": "123123123",
            "id": 1
        }
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

`55030`

Subscription type not found.

### Unsubscribe Contact to Subscription Type

This function allows for the removal of a specific Contact from a particular Subscription Type. By specifying the Contact's UUID and the Subscription Type's UUID, you can easily manage the Contact's subscription status.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/contact-subscriptions/{{contact_uuid}}/unsubscribe`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

UUID of the Contact to be unsubscribed.

Body

`subscription_type_uuid` `string`

`REQUIRED`

The Subscription Type's UUID.

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
` `
    {
    "data": {
        "is_subscribed": false,
        "status": "UNSUBSCRIBED",
        "subscription_type": {
            "name": "Marketing",
            "description": "Subscribe here for marketing purposes",
            "active": true,
            "strategy": "DOUBLE_OPT_IN",
            "uuid": "123123123",
            "id": 1
        }
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

`55030`

Subscription type not found.

### Related

[**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards) [**Reward Receptions** \\
It's what Contacts are saving up for: Reward Receptions](https://docs.piggy.eu/v3/oauth/reward-receptions)
