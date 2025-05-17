## Webhooks

For a number of events, you can subscribe to Webhooks to receive event data. Most information concerning the event itself is on the Webhook data. For most related entities, you may only receive its UUID, which then requires an API call to retrieve more data if needed.

A list of available Webhook events can be found below. If there's an event missing, you can request the addition of said event with our support team.

Available webhook events

Event

Description

`contact_created`

New contact has been created.

`contact_updated`

Contact has been updated.

`credit_reception_created`

New credit reception has been created for a contact.

`prepaid_transaction_created`

New prepaid transaction has been created for a contact.

`voucher_created`

New voucher has been created.

`voucher_updated`

Voucher has been updated.

`voucher_redeemed`

Contact has redeemed a voucher.

`form_submission_completed`

Contact has submitted and completed a form.

`referral_completed`

Contact has completed a referral.

`contact_identifier_created`

New contact identifier created for a contact.

`reward_updated`

Reward has been updated.

`promotion_updated`

Promotion has been updated.

### List Webhook Subscriptions

Lists an Account's current Webhook Subscriptions.

GET

`https://api.piggy.eu/api/v3/oauth/clients/webhook-subscriptions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`event_type` `string`

`OPTIONAL`

Only retrieve Webhook Subscription of a specific event type (options: see available webhook event types).

`status` `string`

`OPTIONAL`

Only retrieve Webhook Subscription of a specific status (options: ACTIVE, INACTIVE).

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
` `{
    "data": [\
        {\
            "uuid": "f1dc2d67-caa8-4919-a537-66491a94ed2f",\
            "name": "Contact created",\
            "event_type": "contact_created",\
            "url": "http://api.example.com/piggy/webhooks",\
            "status": "ACTIVE",\
            "version": "V1",\
            "created_at": "2023-10-20T11:46:22+00:00"\
        },\
        {\
            "uuid": "5062eeac-bbeb-4903-b81e-2422793ff151",\
            "name": "Promotion updated",\
            "event_type": "promotion_updated",\
            "url": "http://api.example.com/piggy/webhooks",\
            "status": "ACTIVE",\
            "version": "V1",\
            "created_at": "2023-10-20T11:48:43+00:00"\
        },\
        {\
            "uuid": "33ee3999-4230-4e3e-b320-cbcbe719bc45",\
            "name": "Referral completed",\
            "event_type": "referral_completed",\
            "url": "http://api.example.com/piggy/webhooks",\
            "status": "INACTIVE",\
            "version": "V1",\
            "created_at": "2023-10-20T11:49:19+00:00"\
        },\
    ],
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Create Webhook Subscription

Subscribes to a specific Webhook event.

POST

`https://api.piggy.eu/api/v3/oauth/clients/webhook-subscriptions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`name` `string`

`REQUIRED`

A name given to the subscription for internal purposes.

`event_type` `string`

`REQUIRED`

The event type to listen to, can only choose from available event types.

`url` `string`

`REQUIRED`

The endpoint to which webhook data is to be sent.

`secret` `string`

`OPTIONAL`

The event type to listen to, can only choose from available event types.

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
    "data": {
        "uuid": "59165ef0-1afd-4baf-b1f7-ffee287aff32",
        "name": "Contact created",
        "event_type": "contact_created",
        "url": "http://api.example.com/piggy/webhooks",
        "status": "ACTIVE",
        "version": "V1",
        "created_at": "2023-11-13T12:32:43+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Get Webhook Subscription

Retrieve a specific Webhook Subscription by specifying its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/webhook-subscriptions/{{webhook_subscription_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`webhook_subscription_uuid` `string`

`REQUIRED`

The UUID of the Webhook Subscription you want to retrieve.

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
` `{
    "data": {
        "uuid": "59165ef0-1afd-4baf-b1f7-ffee287aff32",
        "name": "Some Webhook",
        "event_type": "contact_created",
        "url": "http://yourwebsite.com/somethingsomething",
        "properties": [],
        "status": "ACTIVE",
        "version": "V1",
        "created_at": "2023-11-13T12:32:43+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`80300`

Webhook subscription not found.

### Update Webhook Subscription

Updates an existing Webhook Subscription. The subscription's `event_type` and `secret` cannot be altered after creation.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/webhook-subscriptions/{{webhook_subscription_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`webhook_subscription_uuid` `string`

`REQUIRED`

The UUID of the Webhook Subscription you want to update.

Body

`name` `string`

`OPTIONAL`

A name given to the subscription for internal purposes.

`url` `string`

`OPTIONAL`

The endpoint to which webhook data is to be sent.

`status` `string`

`OPTIONAL`

The subscription's status (options: ACTIVE, INACTIVE).

`attributes` `array`

`REQUIRED | OPTIONAL`

Required if event\_type is contact\_updated, array of one or more contact attributes. Optional when event\_type is not contact\_updated.

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
` `{
    "data": {
        "uuid": "59165ef0-1afd-4baf-b1f7-ffee287aff32",
        "name": "Updated Webhook",
        "event_type": "contact_created",
        "url": "http://yourwebsite.com/somethingsomething",
        "properties": [],
        "status": "ACTIVE",
        "version": "V1",
        "created_at": "2023-11-13T12:32:43+00:00"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80300`

Webhook subscription not found.

### Delete Webhook Subscription

Removes an existing Webhook Subscription.

DELETE

`https://api.piggy.eu/api/v3/oauth/clients/webhook-subscriptions/{{webhook_subscription_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`webhook_subscription_uuid` `string`

`REQUIRED`

The UUID of the Webhook Subscription you want to delete.

Response Example

Show more

`1
2
3
4
` `{
    "data": [],
    "meta": []
}`

Possible errors

Code

Message

`80300`

Webhook subscription not found.
