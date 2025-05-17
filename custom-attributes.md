## Custom Attributes

Custom Attributes are flexible, tailored attributes that allow you to extend the entities in the Piggy system.

Currently, Custom Attributes can be created for the following entities:

- `business_profile`
- `booking`
- `contact`
- `giftcard`
- `giftcard_transaction`
- `promotion`
- `reward`
- `voucher`

If you're missing any entity here, please reach out so we can see if we can make it happen on short notice.

### List Custom Attributes

Get a list of Custom Attributes for a specified entity.

GET

`https://api.piggy.eu/api/v3/oauth/clients/custom-attributes`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`entity` `string`

`REQUIRED`

The entity of the Custom Attributes you want to retrieve.

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
` `{
    "data": [\
        {\
            "id": 2234,\
            "entity": "voucher",\
            "name": "rens_123",\
            "label": "Rens123",\
            "type": "select",\
            "group_name": null,\
            "group": {\
                "id": 141,\
                "name": "General",\
                "created_by_user": null,\
                "position": 1\
            },\
            "is_piggy_defined": false,\
            "is_soft_read_only": false,\
            "is_hard_read_only": false,\
            "field_type": null,\
            "has_unique_value": false,\
            "description": "some description",\
            "options": [\
                {\
                    "value": "100",\
                    "label": "Blue",\
                    "description": null,\
                    "media": null\
                },\
                {\
                    "value": "200",\
                    "label": "Red",\
                    "description": null,\
                    "media": null\
                },\
                {\
                    "value": "300",\
                    "label": "Yellow",\
                    "description": null,\
                    "media": null\
                }\
            ],\
            "position": 1,\
            "created_at": "2023-11-13T16:04:14+00:00",\
            "meta": [],\
            "can_be_deleted": true,\
            "last_used_date": null,\
            "created_by_user": null\
        }\
    ]
}`

### Create Custom Attribute

Creates a new Custom Attribute for a specified entity. You need to provide essential details like the entity type, name, label, and data type of the attribute. There's also the option to add a description and, for select attributes, an array of options.

POST

`https://api.piggy.eu/api/v3/oauth/clients/custom-attributes`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`entity` `string`

`REQUIRED`

The entity that you want to create the Custom Attributes for.

`name` `string`

`REQUIRED`

The unique value for the Custom Attribute, used for internal purposes.

`label` `string`

`REQUIRED`

The label for the attribute, which is outputted.

`type` `string`

`REQUIRED`

The data type of the attribute, possible options: url, text, date, phone, float, color, email, number, select, boolean, rich\_text, date\_time, long\_text, date\_range, time\_range, identifier, birth\_date, file\_upload, media\_upload, multi\_select or license\_plate.

`options` `array`

`OPTIONAL`

Applicable for (multi)selects only. Array consisting of name and value arrays.

`description` `string`

`OPTIONAL`

A description for the attribute.

`group_name` `string`

`OPTIONAL`

Name of the Group to which the attribute belongs.

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
` `{
    "data": {
        "id": 2234,
        "entity": "voucher",
        "name": "rens_123",
        "label": "Rens123",
        "type": "select",
        "group_name": null,
        "group": {
            "id": 141,
            "name": "General",
            "created_by_user": null,
            "position": 1
        },
        "is_piggy_defined": false,
        "is_soft_read_only": false,
        "is_hard_read_only": false,
        "field_type": null,
        "has_unique_value": false,
        "description": "some description",
        "options": [\
            {\
                "value": "100",\
                "label": "Blue",\
                "description": null,\
                "media": null\
            },\
            {\
                "value": "200",\
                "label": "Red",\
                "description": null,\
                "media": null\
            },\
            {\
                "value": "300",\
                "label": "Yellow",\
                "description": null,\
                "media": null\
            }\
        ],
        "position": 1,
        "created_at": "2023-11-13T16:04:14+00:00",
        "meta": [],
        "can_be_deleted": true,
        "last_used_date": null,
        "created_by_user": null
    },
    "meta": []
}`

### Update Custom Attribute

Updates the properties of a Custom Attribute. Note that only the `label`, `description`, `options` for selects and multiselects, `group` can be updated. Attributes defined by the system cannot be updated.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/custom-attributes/{{custom_attribute_name}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`entity` `string`

`REQUIRED`

The entity that you want to create the Custom Attributes for.

`label` `string`

`OPTIONAL`

The label for the attribute, which is outputted.

`options` `array`

`OPTIONAL`

Applicable for (multi)selects only. Array consisting of name and value arrays.

`description` `string`

`OPTIONAL`

A description for the attribute.

`group_name` `string`

`OPTIONAL`

Name of the Group to which the attribute belongs.

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
` `{
    "data": {
        "id": 2234,
        "entity": "voucher",
        "name": "rens_123",
        "label": "Rens123",
        "type": "select",
        "group_name": null,
        "group": {
            "id": 141,
            "name": "General",
            "created_by_user": null,
            "position": 1
        },
        "is_piggy_defined": false,
        "is_soft_read_only": false,
        "is_hard_read_only": false,
        "field_type": null,
        "has_unique_value": false,
        "description": "some description",
        "options": [\
            {\
                "value": "100",\
                "label": "Blue",\
                "description": null,\
                "media": null\
            },\
            {\
                "value": "200",\
                "label": "Red",\
                "description": null,\
                "media": null\
            },\
            {\
                "value": "300",\
                "label": "Yellow",\
                "description": null,\
                "media": null\
            }\
        ],
        "position": 1,
        "created_at": "2023-11-13T16:04:14+00:00",
        "meta": [],
        "can_be_deleted": true,
        "last_used_date": null,
        "created_by_user": null
    },
    "meta": []
}`

### Related

[**Contacts** \\
Contacts are the basis of the loyalty system, for whom any action is created](https://docs.piggy.eu/v3/oauth/contacts) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
