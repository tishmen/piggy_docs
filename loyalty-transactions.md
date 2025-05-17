## Loyalty Transactions

Loyalty Transaction is the umbrella term for both Credit Receptions and Reward Receptions, representing all the changes to a Credit Balance.

### List Loyalty Transactions

Returns transactions for a Loyalty Program. The results can be filtered by Contact, Shop and Loyalty Transaction `type`.

GET

`https://api.piggy.eu/api/v3/oauth/clients/loyalty-transactions`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`OPTIONAL`

UUID of the Contact for whom Loyalty Transactions are to be retrieved.

`shop_uuid` `string`

`OPTIONAL`

UUID of the Shop for which Loyalty Transactions are to be retrieved.

`type` `string`

`OPTIONAL`

Type of Loyalty Transactions to be retrieved, options: 'credit\_receptions' and 'reward\_receptions'. If not set, loyalty transactions of all types are retrieved.

`limit` `number`

`OPTIONAL`

Number of Loyalty Transactions to be retrieved (min. 0, max. 100, default 10).

`page` `number`

`OPTIONAL`

Page within list to retrieve.

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
77
78
79
80
81
82
83
84
85
86
87
88
89
90
91
92
93
94
95
96
97
98
99
100
101
102
103
104
105
106
107
108
109
110
111
112
113
114
115
116
117
118
119
120
121
122
123
124
125
126
127
128
129
130
131
132
133
134
135
136
137
138
139
140
141
142
143
144
145
146
147
148
149
150
151
152
153
154
155
156
157
158
159
160
161
162
163
164
165
166
167
168
169
170
171
172
173
174
175
176
177
178
179
180
181
182
183
184
185
186
187
188
189
190
191
192
193
194
195
196
197
198
199
200
201
202
203
204
205
206
207
208
209
210
211
212
213
214
215
216
217
218
219
220
221
222
223
224
225
226
227
228
229
230
231
232
233
234
235
236
237
238
239
240
241
242
243
244
245
246
247
248
249
250
251
252
253
254
255
256
257
258
259
260
261
262
263
264
265
266
267
268
269
270
271
272
273
274
275
276
277
278
279
280
281
282
283
284
285
286
287
288
289
290
291
292
293
294
295
296
297
298
299
300
301
302
303
304
305
306
307
308
309
310
311
312
313
314
315
316
317
318
319
320
321
322
323
324
325
326
327
328
329
330
331
332
333
334
335
336
337
338
339
340
341
342
343
344
345
346
347
348
349
350
351
352
353
354
355
356
357
358
359
360
361
362
363
364
365
366
367
368
369
370
371
372
373
374
375
376
377
378
379
380
381
382
383
384
385
386
387
388
389
390
391
392
393
394
395
396
397
398
399
400
401
402
403
404
405
406
407
408
409
410
411
412
413
414
415
416
417
418
419
420
421
422
423
424
425
426
427
428
429
430
431
432
433
434
435
436
437
438
439
440
441
442
` `{
    "data": [\
        {\
            "uuid": "daa78841-b321-4564-9dd3-91c6ccd9d600",\
            "credits": 1177,\
            "created_at": "2023-11-12T19:27:49+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": 1.15,\
            "unit": {\
                "id": 298,\
                "name": "test2",\
                "label": "Calories",\
                "prefix": "t",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "9423bcfa-c2b1-41e6-998b-5c4182d1e259",\
            "credits": 20,\
            "created_at": "2023-11-12T17:37:41+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "57589e9f-0107-4cdc-afb6-03b8c89d892e",\
            "credits": 0,\
            "created_at": "2023-11-10T16:47:40+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "a45f1a18-bed8-4451-aba5-1a9e83fa5ea4",\
            "credits": 0,\
            "created_at": "2023-11-10T16:46:12+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123abc-def789-qwerty-34567",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "a06e0546-48d4-4faf-91c6-f66989f5a49f",\
            "credits": 0,\
            "created_at": "2023-11-10T16:23:38+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "e64ad300-668b-428d-850d-f4eb9b37268b",\
            "credits": 10,\
            "created_at": "2023-11-09T19:24:58+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123abc-def789-qwerty-34567",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "8630aa53-0097-4beb-8504-4938f4ef724e",\
            "credits": 10,\
            "created_at": "2023-11-09T14:13:36+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123abc-def789-qwerty-34567",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "ec2e9f1f-2573-4937-90f7-b610e881059b",\
            "credits": 10,\
            "created_at": "2023-11-09T14:12:44+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123abc-def789-qwerty-34567",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "e075a8a4-6d50-4b30-a62b-142fb8a91a30",\
            "credits": 10,\
            "created_at": "2023-11-09T14:12:29+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123abc-def789-qwerty-34567",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123abc-def789-qwerty-34567",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        },\
        {\
            "uuid": "13d3f57a-5566-42d0-8a63-328958f49144",\
            "credits": 10,\
            "created_at": "2023-11-09T14:11:32+00:00",\
            "type": "credit_reception",\
            "contact_identifier": null,\
            "contact": {\
                "uuid": "123",\
                "email": "john@doe.com",\
                "credit_balance": {\
                    "id": 2625740,\
                    "balance": 2929\
                }\
            },\
            "shop": {\
                "id": 15,\
                "uuid": "123123",\
                "type": "physical",\
                "name": "Amsterdam Pop-up Store",\
                "address": {\
                    "id": 59,\
                    "shop_id": 15,\
                    "street": "Sesame Street",\
                    "housenumber": 12,\
                    "postalcode": "9999YZ",\
                    "city": "Duckburg",\
                    "state": "NH",\
                    "country": "NL",\
                    "latitude": null,\
                    "longitude": null,\
                    "house_number": null,\
                    "postal_code": null\
                }\
            },\
            "unit_value": null,\
            "unit": {\
                "id": 112,\
                "name": "test",\
                "label": "Calories",\
                "prefix": "",\
                "is_default": true\
            }\
        }\
    ],
    "meta": {
        "page": 1,
        "limit": 10,
        "viewing_from": 1,
        "viewing_to": 10,
        "last_page": 6,
        "total": 57
    }
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

### Related

[**Rewards** \\
List your rewards and update rewards](https://docs.piggy.eu/v3/oauth/rewards) [**Reward Receptions** \\
It's what Contacts are saving up for: Reward Receptions](https://docs.piggy.eu/v3/oauth/reward-receptions)
