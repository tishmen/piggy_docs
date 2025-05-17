## Referrals

Get insights in your referral program and retrieve all Referrals made by your Contact. The referring Contact is the one who shares their unique Referral code, enabling other contacts to sign up using that code. The referred Contact is the Contact that has used a unique Referral code to sign up.

### List Referrals

Retrieve a list of Referrals from your referral program.

GET

`https://api.piggy.eu/api/v3/oauth/clients/referrals`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`referring_contact_uuid` `string`

`OPTIONAL`

The uuid of the Contact who has referred another Contact.

`status` `string`

`OPTIONAL`

The Referral status can be one of the following: pending, completed, or failed, indicating the current state of the Referral.

`limit` `number`

`OPTIONAL`

Maximum number of items to retrieve. Default value = 30, limit max = 100.

`page` `number`

`OPTIONAL`

Page within list to retrieve

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
` `{
    "data": [\
        {\
            "uuid": "d1fd45ac-0993-46c1-9ae7-334bcd9b0ad7",\
            "referring_contact": {\
                "uuid": "2e70ea71-631f-4b3c-8097-8e0284751c96",\
                "email": "referring-contact-1@example.eu"\
            },\
            "referred_contact": {\
                "uuid": "435438ef-7e62-43aa-87c7-3f6d3deb0a72",\
                "email": "referred-contact-1@example.eu",\
            }\
            "status": "completed"\
        },\
        {\
            "uuid": "d1fd45ac-0993-46c1-9ae7-334bcd9b0ad7",\
            "referring_contact": {\
                "uuid": "8ff157c6-9808-46b9-ac43-ae14809d8f81",\
                "email": "referring-contact-2@example.eu"\
            },\
            "referred_contact": {\
                "uuid": "3ba05ab0-28f6-465d-a345-aaeccd83a24f",\
                "email": "referred-contact-2@example.eu",\
            }\
            "status": "completed"\
        },\
        {\
            "uuid": "d1fd45ac-0993-46c1-9ae7-334bcd9b0ad7",\
            "referring_contact": {\
                "uuid": "2e70ea71-631f-4b3c-8097-8e0284751c96",\
                "email": "referring-contact-3@example.eu"\
            },\
            "referring_contact": {\
                "uuid": "5bc55776-dfac-41b4-a257-0aa2d2dac7b5",\
                "email": "referred-contact-3@example.eu",\
            }\
            "status": "completed"\
        }\
    ],
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.
