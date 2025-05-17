## Credit Receptions

Credit Receptions are the backbone of the Piggy Loyalty system. It lets our end-users collect credits and it's therefore the basis for virtually every loyalty integration. Before starting to build your integration, we ask you read the recommended flows.

### Refunds and reversing Credit Receptions

At times, initially issued credits need to be reverted, when an order was refunded for instance. In such a case, it's important to note that creating a Credit Reception with a negative `unit_value` is inadvisable. Due to the possibility of Loyalty Rules dependent on variables (e.g. time-based filters or Tier-based filters) being present, the resulting number of credits issued can differ from the initial transaction to be corrected.

**Instead, use the `Reverse Credit Reception` API call to reverse an initial transaction.**

#### Partial refunds

In case of partial refunds, it gets a bit more tricky. To account for time-based and attribute-based filters as best as possible, we recommend to calculate the percentage of the order that was refunded, take that percentage of the _credits_ initially issued, and create a Credit Reception with that negative amount of `credits` (do _not_ use `unit_value` (e.g. initial transaction of €50.00 issued 80 credits, €20.00 was refunded, so new Credit Reception for -32 credits).

### Create Credit Reception

In creating a Credit Reception, you may either choose to set the `unit_value` alongside the relevant `unit_name`, or the `credits`, or both. In any case, one of either must be set. If only purchase amount is set, then the number of credits are determined by any Loyalty Rules set on the Loyalty Program. Loyalty Rules are rules that Clients can set in their Business Dashboard, which can apply multiplication or rounding rules for instance.

To mimic in-store behaviour as closely as possible, we recommend setting `unit_name` and `unit_value` only. If `unit_name` is not set, then the default unit will be used (purchase\_amount).

If, however, the number of `credits` to be given should be set explicitly - for instance when the Credit Reception doesn't concern a transaction or a (partial) refund needs to be made - then the credits can be set. In this case, `unit_value` is optional.

When using purchase amount as unit\_name, use purchase amount as a float for unit\_value (e.g. €10,40 translates to unit\_value: 10.40).

POST

`https://api.piggy.eu/api/v3/oauth/clients/credit-receptions`

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

The UUID of the Contact for whom the Credit Reception is to be created.

`credits` `number`

`REQUIRED_IF`

Number of Credits, required if unit\_value is not set. Overrides any Loyalty Rules.

`unit_value` `number`

`REQUIRED_IF`

Value of the Unit to be given. This parameter is required if credits is not set. Subject to any Loyalty Rules.

`unit_name` `string`

`OPTIONAL`

Name of the Unit for the corresponding value. If both unit\_name and credits are not set, then the default unit (purchase\_amount) is used.

`contact_identifier_value` `string`

`OPTIONAL`

The value of the Contact Identifier linked to the Contact for whom the Credit Reception is to be created.

`pos_transaction_uuid` `string`

`OPTIONAL`

The UUID of the POS transaction that you want to link the Credit Reception to.

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

`1008`

Shop not found.

`55031`

Contact not found.

`60003`

Contact identifier not found.

`60007`

Unit not found.

`1015`

Maximum amount of credits reached.

`60001`

Credit reception creating in progress.

`55046`

No default unit found.

`1027`

POS Transaction not found.

`6001`

The POS transaction is already linked to a credit reception.

### Reverse Credit Reception

At times, an initial Credit Reception needs to be reversed, because of a refund for instance. This will create a new Credit Reception with the same data as initially used, but for the negative number of credits issued.

POST

`https://api.piggy.eu/api/v3/oauth/clients/credit-receptions/{{credit_reception_uuid}}/reverse`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`credit_reception_uuid` `string`

`REQUIRED`

The UUID of the Credit Reception that needs to be reversed

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

### Calculate Credits

Without actually creating a Credit Reception, this API call can be used to calculate how many credits would be issued if a Credit Reception was to be made with the given values. Please note, that there is a (small) possibility of a divergence in credits between the calculation and the actual Credit Reception, if loyalty rule filters are in place that are time-based (e.g. a happy hour rule).

GET

`https://api.piggy.eu/api/v3/oauth/clients/credit-receptions/calculate`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`shop_uuid` `string`

`REQUIRED`

The UUID of the Shop you want to create the Reward Reception for.

`contact_uuid` `string`

`OPTIONAL`

The UUID of the Contact for whom the credits are to be calculated.

`unit_value` `number`

`REQUIRED`

Value for which credits are to be calculated. Subject to any Loyalty Rules.

`unit_name` `string`

`OPTIONAL`

Name of the unit for the corresponding value. If both unit\_name and credits are not set, then the default unit (purchase\_amount) is used.

Response Example

Show more

`1
2
3
4
5
6
` `{
    "data": {
        "credits": 10240
    },
    "meta": {}
}`

Possible errors

Code

Message

`1003`

Invalid input.

`1008`

Shop not found.

`55031`

Contact not found.

`60007`

Unit not found.

### Related

[**Loyalty Tokens** \\
Create or Claim a Loyalty Token](https://docs.piggy.eu/v3/oauth/loyalty-tokens) [**Reward Receptions** \\
It's what Contacts are saving up for: Reward Receptions](https://docs.piggy.eu/v3/oauth/reward-receptions)
