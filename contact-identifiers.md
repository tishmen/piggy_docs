## Contact Identifiers

Mostly, Contacts are identified by their email address. However, Contact Identifiers can also be used to retrieve a Contact's information. Most current Contact Identifiers are Loyalty Cards, which have been around since the start of Piggy. However, if needed, virtually anything can be used as a Contact Identifier, provided they use a unique value (within the Account).

### Find Contact Identifier

Finds the Contact Identifier. Returns `contact` as null if not yet linked to a Contact.

GET

`https://api.piggy.eu/api//contact-identifiers/find?contact_identifier_value={{contact_identifier_value}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_identifier_value` `string`

`REQUIRED`

The unique value for the Contact Identifier.

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
        "name": "Loyalty Card",
        "value": "ABCDE12356-90",
        "active": true,
        "contact": {
            "uuid": "123abc-def789-qwerty-34567",
            "email": "john@doe.com
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60003`

Contact identifier not found.

`60004`

Contact identifier inactive.

### Create Contact Identifier

Creates a Contact Identifier. You can link it directly to a Contact when you send a `contact_uuid` along.

POST

`https://api.piggy.eu/api//contact-identifiers`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_identifier_value` `string`

`REQUIRED`

The unique value for the Contact Identifier, which will serve as the identifier.

`contact_uuid` `string`

`OPTIONAL`

The Contact's UUID for whom the identifier is to be created. If you want to link a contact later, you can leave this empty.

`contact_identifier_name` `string`

`OPTIONAL`

A name for the Contact Identifier. When left out, it receives the same value as contact\_identifier\_value.

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
        "name": "Loyalty Card",
        "value": "ABCDE12356-90",
        "active": true,
        "contact": {
            "uuid": "123abc-def789-qwerty-34567",
            "email": "john@doe.com
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

`60006`

Contact identifier already exists.

### Link Contact Identifier

Links an existing, unlinked Contact Identifier to an existing Contact.

PUT

`https://api.piggy.eu/api//contact-identifiers/link`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_identifier_value` `string`

`REQUIRED`

The unique value of the Contact Identifier that you want to link to the Contact.

`contact_uuid` `string`

`REQUIRED`

The Contact UUID that needs to be linked to the Contact Identifier.

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
        "name": "Loyalty Card",
        "value": "ABCDE12356-90",
        "active": true,
        "contact": {
            "uuid": "123abc-def789-qwerty-34567",
            "email": "john@doe.com
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60003`

Contact identifier not found.

`60004`

Contact identifier inactive.

`55031`

Contact not found.

`60014`

Contact identifier is already linked.

### Unlink Contact Identifier

Removes a linked Contact from a Contact Identifier.

PUT

`https://api.piggy.eu/api//contact-identifiers/unlink`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_identifier_value` `string`

`REQUIRED`

The unique value of the Contact Identifier that you want to unlink the contact from.

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
        "name": "Loyalty Card",
        "value": "ABCDE12356-90",
        "active": true,
        "contact": null
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60003`

Contact identifier not found.

`60004`

Contact identifier inactive.

### Update Contact Identifier

Updates a Contact Identifier. Note: only the identifier's `name` and `active` properties can be updated. The value cannot be updated.

PUT

`https://api.piggy.eu/api//contact-identifiers`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_identifier_value` `string`

`REQUIRED`

The unique value of the Contact Identifier.

`contact_identifier_name` `string`

`OPTIONAL`

A name for the Contact Identifier.

`contact_identifier_active` `boolean`

`OPTIONAL`

Active state of the contact identifier.

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
        "name": "Loyalty Card",
        "value": "ABCDE12356-90",
        "active": true,
        "contact": {
            "uuid": "123abc-def789-qwerty-34567",
            "email": "john@doe.com
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

`60006`

Contact identifier already exists.

### Delete Contact Identifier

Deletes an Contact Identifier.

DELETE

`https://api.piggy.eu/api//contact-identifiers`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_identifier_value` `string`

`REQUIRED`

The unique value of the Contact Identifier that you want to delete.

Possible errors

Code

Message

`1003`

Invalid input.

`60003`

Contact identifier not found.

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/oauth/contacts) [**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/oauth/rewards)
