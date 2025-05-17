## Contacts

Contacts are the end-users of Clients. Contacts are provided with basic attributes, such as an email address, first name, last name, and so forth. Any custom attributes can also be created, either in the Business Dashboard or via API. Their email address is the standard unique identifier, via which Contacts can be found. Aside from the email address, Contact Identifiers can also be used to find a Contact.

### List Contacts

Retrieve a list of Contacts with pagination. The pagination has default values, but you can alter those by supplying the `page` and `limit` parameters including values with the call.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`page` `number`

`OPTIONAL`

Page number (default: 1)

`limit` `number`

`OPTIONAL`

Maximum number of Contacts to list (default: 30, max: 100)

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
443
444
445
446
447
448
449
450
451
452
453
454
455
456
457
458
459
460
461
462
463
464
465
466
467
468
469
470
471
472
473
474
475
476
477
478
479
480
481
482
483
484
485
486
487
488
489
490
491
492
493
494
495
496
497
498
499
500
501
502
503
504
505
506
507
508
509
510
511
512
513
514
515
516
517
518
519
520
521
522
523
524
525
526
527
528
529
530
531
532
533
534
535
536
537
538
539
540
541
542
543
544
545
546
547
548
549
550
551
552
553
554
555
556
557
558
559
560
561
562
563
564
565
566
567
568
569
570
571
572
573
574
575
576
577
578
579
580
581
582
583
584
585
586
587
588
589
590
591
592
593
594
595
596
597
598
599
600
601
602
603
604
605
606
607
608
609
610
611
612
613
614
615
616
617
618
619
620
621
622
623
624
625
626
627
628
629
630
631
632
633
634
635
636
637
638
639
640
641
642
643
644
645
646
647
648
649
650
651
652
653
654
655
656
657
658
659
660
661
662
663
664
665
666
667
668
669
670
671
672
673
674
675
676
677
678
679
680
681
682
683
684
685
686
687
688
689
690
691
692
693
694
695
696
697
698
699
700
701
702
703
704
705
706
707
708
709
710
711
712
713
714
715
716
717
718
719
720
721
722
723
724
725
726
727
728
729
730
731
732
733
734
735
736
737
738
739
740
741
742
743
744
745
746
747
748
749
750
751
752
753
754
755
756
757
758
759
760
761
762
763
764
765
766
767
768
769
770
771
772
773
774
775
776
777
778
779
780
781
782
783
784
785
786
787
788
789
790
791
792
793
794
795
796
797
798
799
800
801
802
803
804
805
806
807
808
809
810
811
812
813
814
815
816
817
818
819
820
821
822
823
824
825
826
827
828
829
830
831
832
833
834
835
836
837
838
839
840
841
842
843
844
845
846
847
848
849
850
851
852
853
854
855
856
857
858
859
860
861
862
863
864
865
866
867
868
869
870
871
872
873
874
875
876
877
878
879
880
881
882
883
884
885
886
887
888
889
890
891
892
893
894
895
896
897
898
899
900
901
902
903
904
905
906
907
908
909
910
911
912
913
914
915
916
917
918
919
920
921
922
923
924
925
926
927
928
929
930
931
932
933
934
935
936
937
938
939
940
941
942
943
944
945
946
947
948
949
950
951
952
953
954
955
956
957
958
959
960
961
962
963
964
965
966
967
968
969
970
971
972
973
974
975
976
977
978
979
980
981
982
983
984
985
986
987
988
989
990
991
992
993
994
995
996
997
998
999
1000
1001
1002
1003
1004
1005
1006
1007
1008
1009
1010
1011
1012
1013
1014
1015
1016
1017
1018
1019
1020
1021
1022
1023
1024
1025
1026
1027
1028
1029
1030
1031
1032
1033
1034
1035
1036
1037
1038
1039
1040
1041
1042
1043
1044
1045
1046
1047
1048
1049
1050
1051
1052
1053
1054
1055
1056
1057
1058
1059
1060
1061
1062
1063
1064
1065
1066
1067
1068
1069
1070
1071
1072
1073
1074
1075
1076
1077
1078
1079
1080
1081
1082
1083
1084
1085
1086
1087
1088
1089
1090
1091
1092
1093
1094
1095
1096
1097
1098
1099
1100
1101
1102
1103
1104
1105
1106
1107
1108
1109
1110
1111
1112
1113
1114
1115
1116
1117
1118
1119
1120
1121
1122
1123
1124
1125
1126
1127
1128
1129
1130
1131
1132
1133
1134
1135
1136
1137
1138
1139
1140
1141
1142
1143
1144
1145
1146
1147
1148
1149
1150
1151
1152
1153
1154
1155
1156
1157
1158
1159
1160
1161
1162
1163
1164
1165
1166
1167
1168
1169
1170
1171
1172
1173
1174
1175
1176
1177
1178
1179
1180
1181
1182
1183
1184
1185
1186
1187
1188
1189
1190
1191
1192
1193
1194
1195
1196
1197
1198
1199
1200
1201
1202
1203
1204
1205
1206
1207
1208
1209
1210
1211
1212
1213
1214
1215
1216
1217
1218
1219
1220
1221
1222
1223
1224
1225
1226
1227
1228
1229
1230
1231
1232
1233
1234
1235
1236
1237
1238
1239
1240
1241
1242
1243
1244
1245
1246
1247
1248
1249
1250
1251
1252
1253
1254
1255
1256
1257
1258
1259
1260
1261
1262
1263
1264
1265
1266
1267
1268
1269
1270
1271
1272
1273
1274
1275
1276
1277
1278
1279
1280
1281
1282
1283
1284
1285
1286
1287
1288
1289
1290
1291
1292
1293
1294
1295
1296
1297
1298
1299
1300
1301
1302
1303
1304
1305
1306
1307
1308
1309
1310
1311
1312
1313
1314
1315
1316
1317
1318
1319
1320
1321
1322
1323
1324
1325
1326
1327
1328
1329
1330
1331
1332
1333
1334
1335
1336
1337
1338
1339
1340
1341
1342
1343
1344
1345
1346
1347
1348
1349
1350
1351
1352
1353
1354
1355
1356
1357
1358
1359
1360
1361
1362
1363
1364
1365
1366
1367
1368
1369
1370
1371
1372
1373
1374
1375
1376
1377
1378
1379
1380
1381
1382
1383
1384
1385
1386
1387
1388
1389
1390
1391
1392
1393
1394
1395
1396
1397
1398
1399
1400
1401
1402
1403
1404
1405
1406
1407
1408
1409
1410
1411
1412
1413
1414
1415
1416
1417
1418
1419
1420
1421
1422
1423
1424
1425
1426
1427
1428
1429
1430
1431
1432
1433
1434
1435
1436
1437
1438
1439
1440
1441
1442
1443
1444
1445
1446
1447
1448
1449
1450
1451
1452
1453
1454
1455
1456
1457
1458
1459
1460
1461
1462
1463
1464
1465
1466
1467
1468
1469
1470
1471
1472
1473
1474
1475
1476
1477
1478
1479
1480
1481
1482
1483
1484
1485
1486
1487
1488
1489
1490
1491
1492
1493
1494
1495
1496
1497
1498
1499
1500
1501
1502
1503
1504
1505
1506
1507
1508
1509
1510
1511
1512
1513
1514
1515
1516
1517
1518
1519
1520
1521
1522
1523
1524
1525
1526
1527
1528
1529
1530
1531
1532
1533
1534
1535
1536
1537
1538
1539
1540
1541
1542
1543
1544
1545
1546
1547
1548
1549
1550
1551
1552
1553
1554
1555
1556
1557
1558
1559
1560
1561
1562
1563
1564
1565
1566
1567
1568
1569
1570
1571
1572
1573
1574
1575
1576
1577
1578
1579
1580
1581
1582
1583
1584
1585
1586
1587
1588
1589
1590
1591
1592
1593
1594
1595
1596
1597
1598
1599
1600
1601
1602
1603
1604
1605
1606
1607
1608
1609
1610
1611
1612
1613
1614
1615
1616
1617
1618
1619
1620
1621
1622
1623
1624
1625
1626
1627
1628
1629
1630
1631
1632
1633
1634
1635
1636
1637
1638
1639
1640
1641
1642
1643
1644
1645
1646
1647
1648
1649
1650
1651
1652
1653
1654
1655
1656
1657
1658
1659
1660
1661
1662
1663
1664
1665
1666
1667
1668
1669
1670
1671
1672
1673
1674
1675
1676
1677
1678
1679
1680
1681
1682
1683
1684
1685
1686
1687
1688
1689
1690
1691
1692
1693
1694
1695
1696
1697
1698
1699
1700
1701
1702
1703
1704
1705
1706
1707
1708
1709
1710
1711
1712
1713
1714
` `{
    "data": [\
        {\
            "id": 21,\
            "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
            "email": "john@doe.com",\
            "attributes": [\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2204,\
                        "type": "media_upload",\
                        "field_type": "media_upload",\
                        "name": "avatar",\
                        "label": "Avatar",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2205,\
                        "type": "text",\
                        "field_type": "text",\
                        "name": "firstname",\
                        "label": "First name",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2206,\
                        "type": "text",\
                        "field_type": "text",\
                        "name": "lastname",\
                        "label": "Last name",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "john@doe.com",\
                    "attribute": {\
                        "id": 2203,\
                        "type": "email",\
                        "field_type": "email",\
                        "name": "email",\
                        "label": "Email",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "0",\
                    "attribute": {\
                        "id": 2220,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "previous_loyalty_balance",\
                        "label": "Previous credit balance",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "0",\
                    "attribute": {\
                        "id": 2219,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "loyalty_balance",\
                        "label": "Credit balance",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "0",\
                    "attribute": {\
                        "id": 2223,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "loyalty_number_of_transactions",\
                        "label": "Number of transactions",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2221,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "loyalty_first_transaction_date",\
                        "label": "First credit reception date",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2222,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "loyalty_last_transaction_date",\
                        "label": "Last transaction date",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2207,\
                        "type": "text",\
                        "field_type": "text",\
                        "name": "address",\
                        "label": "Address",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2208,\
                        "type": "phone",\
                        "field_type": "phone",\
                        "name": "phonenumber",\
                        "label": "Telephone number",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2209,\
                        "type": "birth_date",\
                        "field_type": "birth_date",\
                        "name": "birthdate",\
                        "label": "Birth date",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2210,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "channel",\
                        "label": "Channel",\
                        "options": [\
                            {\
                                "value": "BUSINESS_APP",\
                                "label": "BUSINESS_APP",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "API",\
                                "label": "API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "BUSINESS_DASHBOARD",\
                                "label": "BUSINESS_DASHBOARD",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "CUSTOMER_APP",\
                                "label": "CUSTOMER_APP",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "WEBSHOP_API",\
                                "label": "WEBSHOP_API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "IMPORT",\
                                "label": "IMPORT",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "WEBSITE_FORM",\
                                "label": "WEBSITE_FORM",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "REGISTER_API",\
                                "label": "REGISTER_API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "OAUTH_API",\
                                "label": "OAUTH_API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIPTION_TYPE",\
                                "label": "SUBSCRIPTION_TYPE",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "CUSTOM_APP",\
                                "label": "CUSTOM_APP",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "AUTOMATIONS",\
                                "label": "AUTOMATIONS",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "FORMS",\
                                "label": "FORMS",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "WIDGET",\
                                "label": "WIDGET",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2211,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "age",\
                        "label": "Age",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2212,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "locale",\
                        "label": "Preferred language",\
                        "options": [\
                            {\
                                "value": "en",\
                                "label": "English",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "nl",\
                                "label": "Nederlands",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "de",\
                                "label": "Deutsch",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "es",\
                                "label": "Español",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "fr",\
                                "label": "Francés",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "ACTIVE",\
                    "attribute": {\
                        "id": 2213,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "status",\
                        "label": "Status",\
                        "options": [\
                            {\
                                "value": "ACTIVE",\
                                "label": "Active",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "INACTIVE",\
                                "label": "Inactive",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "2023-11-13T12:56:00+00:00",\
                    "attribute": {\
                        "id": 2214,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "updated_at",\
                        "label": "Updated at",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "2021-10-20T18:06:09+00:00",\
                    "attribute": {\
                        "id": 2215,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "created_at",\
                        "label": "Created at",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2216,\
                        "type": "boolean",\
                        "field_type": "boolean",\
                        "name": "is_anonymous",\
                        "label": "Anonymous",\
                        "options": [\
                            {\
                                "value": "true",\
                                "label": "True",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "false",\
                                "label": "False",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "0",\
                    "attribute": {\
                        "id": 2217,\
                        "type": "float",\
                        "field_type": "float",\
                        "name": "prepaid_balance",\
                        "label": "Prepaid balance",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2218,\
                        "type": "multi_select",\
                        "field_type": "multi_select",\
                        "name": "loyalty_associated_shops",\
                        "label": "Associated shops",\
                        "options": [\
                            {\
                                "value": "123123",\
                                "label": "Amsterdam Pop-up Store",\
                                "description": null,\
                                "media": {\
                                    "type": "image",\
                                    "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                                }\
                            },\
                            {\
                                "value": "12312312312312",\
                                "label": "Web Shop",\
                                "description": null,\
                                "media": {\
                                    "type": "image",\
                                    "value": "https://static.piggy.nl/business/profile_types/webshop.svg"\
                                }\
                            },\
                            {\
                                "value": "2222e1242t3324535",\
                                "label": "Sesame Street",\
                                "description": null,\
                                "media": {\
                                    "type": "image",\
                                    "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                                }\
                            },\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "0",\
                    "attribute": {\
                        "id": 2224,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "loyalty_total_credits_received",\
                        "label": "Total credits received",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2225,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "tier",\
                        "label": "Tier",\
                        "options": [\
                            {\
                                "value": "1",\
                                "label": "Gold",\
                                "description": "VIP Members only",\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2226,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_3",\
                        "label": "Subscription status: Monthly offers",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2227,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_1",\
                        "label": "Subscription status: Marketing",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2228,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_5",\
                        "label": "Subscription status: Product updates",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2229,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_4",\
                        "label": "Subscription status: Events",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2233,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "favorite_color",\
                        "label": "Your Favorite Color",\
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
                        "is_piggy_defined": false,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2236,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "favorite_color1",\
                        "label": "Your Favorite Color1",\
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
                        "is_piggy_defined": false,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                }\
            ],\
            "tier": null,\
            "subscriptions": [\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Marketing",\
                        "description": "Subscribe here for marketing purposes",\
                        "active": true,\
                        "strategy": "DOUBLE_OPT_IN",\
                        "uuid": "123123123",\
                        "id": 1\
                    }\
                },\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Monthly offers",\
                        "description": "A monthly email blast with our best offers!",\
                        "active": true,\
                        "strategy": "DOUBLE_OPT_IN",\
                        "uuid": null,\
                        "id": 3\
                    }\
                },\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Events",\
                        "description": "Stay in the loop for all our special events",\
                        "active": true,\
                        "strategy": "OPT_IN",\
                        "uuid": null,\
                        "id": 4\
                    }\
                },\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Product updates",\
                        "description": "Never miss out on any of our newest features",\
                        "active": true,\
                        "strategy": "OPT_OUT",\
                        "uuid": null,\
                        "id": 5\
                    }\
                }\
            ],\
            "credit_balance": {\
                "balance": 0,\
                "id": 2625749\
            },\
            "prepaid_balance": {\
                "balance_in_cents": 0\
            },\
            "current_values": {\
                "age": null,\
                "city": "Duckburg",\
                "dier": null,\
                "tier": null,\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com",\
                "avatar": "",\
                "locale": null,\
                "status": "ACTIVE",\
                "address": "",\
                "channel": null,\
                "lastname": "",\
                "birthdate": null,\
                "firstname": "",\
                "created_at": "2021-10-20T18:06:09+00:00",\
                "postalcode": "9999YZ",\
                "updated_at": "2023-11-13T12:56:00+00:00",\
                "housenumber": 12,\
                "phonenumber": "",\
                "is_anonymous": false,\
                "add_to_wallet": "http://piggy.eu/add-to-wallet/loyalty?uuid=9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "a_custom_attribute": null,\
                "loyalty_balance": 0,\
                "prepaid_balance": 0,\
                "custom_app_login_url": "http://piggy.eu/home?token=eyJpdiI6IklhRnV6cDNkRUVieTZ2YzN2NlM0Y2c9PSIsInZhbHVlIjoieUZyMlJrRU5MVXRLZDh0dDErWWwxWjN6cnI2VFg3YmVWSWYvQTZhQXRWd1UrQXBqVTcwVmp1ZHo0bDNaK3FncTlGZWdBOEV3ZHkyY0pHemgrSitLSHRyblI0bEJwNnE4eEsyZ0cvN2dEeDQ2ekRjRVlzN05vWW9PWkZ1VjR5NUlCdDdFZUZwUmNHZ2pLTlp5OGdNWnRIaHFpVDdrVzljWC9JU2U4Vk1POFQ4RU9qdU5GbFAyc1ZSaStEekNGUkhyUjRINDRRSHkwa24yVmxkN25lVVBkdz09IiwibWFjIjoiOTcyMTRhZmRhNzRmYWQ3M2NjODY2YTYyOTgxYWFlZWY5OWMzMWY2ZTM0OWJiNDQxNTc1M2ViNjE4ODJjMDVjNyIsInRhZyI6IiJ9",\
                "favorite_color": null,\
                "custom_app_login_code": 973756,\
                "subscription_status_1": "UNSUBSCRIBED",\
                "subscription_status_3": "UNSUBSCRIBED",\
                "subscription_status_4": "UNSUBSCRIBED",\
                "subscription_status_5": "UNSUBSCRIBED",\
                "loyalty_associated_shops": "",\
                "previous_loyalty_balance": 0,\
                "default_contact_identifier": null,\
                "subscription_preferences_url": "http://piggy.eu/subscription-preferences/9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "loyalty_last_transaction_date": null,\
                "loyalty_first_transaction_date": null,\
                "loyalty_number_of_transactions": 0,\
                "loyalty_total_credits_received": 0,\
                "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=9504d4b2-03c5-41d8-8423-4b1821b6aa00&subscription_type=1",\
                "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=9504d4b2-03c5-41d8-8423-4b1821b6aa00&subscription_type=3",\
            },\
            "current_display_values": {\
                "age": "",\
                "city": "",\
                "dier": "False",\
                "tier": "",\
                "uuid": "",\
                "email": "john@doe.com",\
                "avatar": "",\
                "locale": "",\
                "status": "Active",\
                "address": "",\
                "channel": "",\
                "lastname": "",\
                "birthdate": "",\
                "firstname": "",\
                "created_at": "2021-10-20, 20:06",\
                "postalcode": "",\
                "updated_at": "2023-11-13, 13:56",\
                "housenumber": "",\
                "phonenumber": "",\
                "is_anonymous": "False",\
                "add_to_wallet": "http://piggy.eu/add-to-wallet/loyalty?uuid=9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "a_custom_attribute": "",\
                "loyalty_balance": "0",\
                "prepaid_balance": "0",\
                "custom_app_login_url": "http://piggy.eu/home?token=eyJpdiI6Ikt2N3d5RXJRdEx3VXNqdFZpc2tNaVE9PSIsInZhbHVlIjoiSmpXeVRqZlJ5S2c1cUhOUGtzeXRmMmNQcEpBTzU3K1RjaFpzNnJxY2NBc2xFbVZDNXd5R2k1WDRyQWVQRUt3TDUrRnVPTnp5MURMakFjN0RITjNKQWtaRXpWK3V1V2lqOThTVnEvK3pyZkhHWm42VXdKSkkxSjc1MHZuTFBaK1hFdTZIRHBKaGdmaFR5clczelhYbHhkM0RaL2kzV0l1T0JtRVNqSk5lRm53TWpyWGlNYWExSzlEdXNCMmZyQi9YOU0xOUpvZjdYN2dZRzBJNnFFeVpMdz09IiwibWFjIjoiYmI5MzE3MGU5YmQxODUwOWI1ZTVjNjFjMWRjN2U0Y2FhOGEyOTI5ZDcwNWI5NzY3ZGUwN2U3NzQyNDEyZjAxMyIsInRhZyI6IiJ9",\
                "favorite_color": "",\
                "custom_app_login_code": "973756",\
                "subscription_status_1": "Unsubscribed",\
                "subscription_status_3": "Unsubscribed",\
                "subscription_status_4": "Unsubscribed",\
                "subscription_status_5": "Unsubscribed",\
                "loyalty_associated_shops": "",\
                "previous_loyalty_balance": "0",\
                "default_contact_identifier": "",\
                "subscription_preferences_url": "http://piggy.eu/subscription-preferences/9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "loyalty_last_transaction_date": "",\
                "loyalty_first_transaction_date": "",\
                "loyalty_number_of_transactions": "0",\
                "loyalty_total_credits_received": "0",\
                "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=9504d4b2-03c5-41d8-8423-4b1821b6aa00&subscription_type=1",\
                "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=9504d4b2-03c5-41d8-8423-4b1821b6aa00&subscription_type=3",\
            }\
        },\
        {\
            "id": 22,\
            "uuid": "123",\
            "email": "john@doe.com",\
            "attributes": [\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2204,\
                        "type": "media_upload",\
                        "field_type": "media_upload",\
                        "name": "avatar",\
                        "label": "Avatar",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "Niels",\
                    "attribute": {\
                        "id": 2205,\
                        "type": "text",\
                        "field_type": "text",\
                        "name": "firstname",\
                        "label": "First name",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "de Vries",\
                    "attribute": {\
                        "id": 2206,\
                        "type": "text",\
                        "field_type": "text",\
                        "name": "lastname",\
                        "label": "Last name",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "john@doe.com",\
                    "attribute": {\
                        "id": 2203,\
                        "type": "email",\
                        "field_type": "email",\
                        "name": "email",\
                        "label": "Email",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "2897",\
                    "attribute": {\
                        "id": 2220,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "previous_loyalty_balance",\
                        "label": "Previous credit balance",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "2895",\
                    "attribute": {\
                        "id": 2219,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "loyalty_balance",\
                        "label": "Credit balance",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "77",\
                    "attribute": {\
                        "id": 2223,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "loyalty_number_of_transactions",\
                        "label": "Number of transactions",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "2023-01-09T15:03:37+00:00",\
                    "attribute": {\
                        "id": 2221,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "loyalty_first_transaction_date",\
                        "label": "First credit reception date",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "2023-11-13T14:37:04+00:00",\
                    "attribute": {\
                        "id": 2222,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "loyalty_last_transaction_date",\
                        "label": "Last transaction date",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2207,\
                        "type": "text",\
                        "field_type": "text",\
                        "name": "address",\
                        "label": "Address",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2208,\
                        "type": "phone",\
                        "field_type": "phone",\
                        "name": "phonenumber",\
                        "label": "Telephone number",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2209,\
                        "type": "birth_date",\
                        "field_type": "birth_date",\
                        "name": "birthdate",\
                        "label": "Birth date",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2210,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "channel",\
                        "label": "Channel",\
                        "options": [\
                            {\
                                "value": "BUSINESS_APP",\
                                "label": "BUSINESS_APP",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "API",\
                                "label": "API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "BUSINESS_DASHBOARD",\
                                "label": "BUSINESS_DASHBOARD",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "CUSTOMER_APP",\
                                "label": "CUSTOMER_APP",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "WEBSHOP_API",\
                                "label": "WEBSHOP_API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "IMPORT",\
                                "label": "IMPORT",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "WEBSITE_FORM",\
                                "label": "WEBSITE_FORM",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "REGISTER_API",\
                                "label": "REGISTER_API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "OAUTH_API",\
                                "label": "OAUTH_API",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIPTION_TYPE",\
                                "label": "SUBSCRIPTION_TYPE",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "CUSTOM_APP",\
                                "label": "CUSTOM_APP",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "AUTOMATIONS",\
                                "label": "AUTOMATIONS",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "FORMS",\
                                "label": "FORMS",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "WIDGET",\
                                "label": "WIDGET",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2211,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "age",\
                        "label": "Age",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2212,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "locale",\
                        "label": "Preferred language",\
                        "options": [\
                            {\
                                "value": "en",\
                                "label": "English",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "nl",\
                                "label": "Nederlands",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "de",\
                                "label": "Deutsch",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "es",\
                                "label": "Español",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "fr",\
                                "label": "Francés",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "ACTIVE",\
                    "attribute": {\
                        "id": 2213,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "status",\
                        "label": "Status",\
                        "options": [\
                            {\
                                "value": "ACTIVE",\
                                "label": "Active",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "INACTIVE",\
                                "label": "Inactive",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "2023-11-13T14:37:00+00:00",\
                    "attribute": {\
                        "id": 2214,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "updated_at",\
                        "label": "Updated at",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "2021-10-20T18:06:09+00:00",\
                    "attribute": {\
                        "id": 2215,\
                        "type": "date_time",\
                        "field_type": "date_time",\
                        "name": "created_at",\
                        "label": "Created at",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2216,\
                        "type": "boolean",\
                        "field_type": "boolean",\
                        "name": "is_anonymous",\
                        "label": "Anonymous",\
                        "options": [\
                            {\
                                "value": "true",\
                                "label": "True",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "false",\
                                "label": "False",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "40",\
                    "attribute": {\
                        "id": 2217,\
                        "type": "float",\
                        "field_type": "float",\
                        "name": "prepaid_balance",\
                        "label": "Prepaid balance",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",\
                    "attribute": {\
                        "id": 2218,\
                        "type": "multi_select",\
                        "field_type": "multi_select",\
                        "name": "loyalty_associated_shops",\
                        "label": "Associated shops",\
                        "options": [\
                            {\
                                "value": "123123",\
                                "label": "Amsterdam Pop-up Store",\
                                "description": null,\
                                "media": {\
                                    "type": "image",\
                                    "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                                }\
                            },\
                            {\
                                "value": "12312312312312",\
                                "label": "Web Shop",\
                                "description": null,\
                                "media": {\
                                    "type": "image",\
                                    "value": "https://static.piggy.nl/business/profile_types/webshop.svg"\
                                }\
                            },\
                            {\
                                "value": "2222e1242t3324535",\
                                "label": "Sesame Street",\
                                "description": null,\
                                "media": {\
                                    "type": "image",\
                                    "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                                }\
                            },\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "3014",\
                    "attribute": {\
                        "id": 2224,\
                        "type": "number",\
                        "field_type": "number",\
                        "name": "loyalty_total_credits_received",\
                        "label": "Total credits received",\
                        "options": [],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "1",\
                    "attribute": {\
                        "id": 2225,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "tier",\
                        "label": "Tier",\
                        "options": [\
                            {\
                                "value": "1",\
                                "label": "Gold",\
                                "description": "VIP Members only",\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": true\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2226,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_3",\
                        "label": "Subscription status: Monthly offers",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2227,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_1",\
                        "label": "Subscription status: Marketing",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2228,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_5",\
                        "label": "Product updates",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "UNSUBSCRIBED",\
                    "attribute": {\
                        "id": 2229,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "subscription_status_4",\
                        "label": "Subscription status: Events",\
                        "options": [\
                            {\
                                "value": "UNSUBSCRIBED",\
                                "label": "Unsubscribed",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "PENDING_CONFIRMATION",\
                                "label": "Pending confirmation",\
                                "description": null,\
                                "media": null\
                            },\
                            {\
                                "value": "SUBSCRIBED",\
                                "label": "Subscribed",\
                                "description": null,\
                                "media": null\
                            }\
                        ],\
                        "is_piggy_defined": true,\
                        "is_soft_read_only": true,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2233,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "favorite_color",\
                        "label": "Your Favorite Color",\
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
                        "is_piggy_defined": false,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                },\
                {\
                    "value": "",\
                    "attribute": {\
                        "id": 2236,\
                        "type": "select",\
                        "field_type": "select",\
                        "name": "favorite_color1",\
                        "label": "Your Favorite Color1",\
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
                        "is_piggy_defined": false,\
                        "is_soft_read_only": false,\
                        "is_hard_read_only": false\
                    }\
                }\
            ],\
            "tier": {\
                "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",\
                "name": "Gold",\
                "description": "VIP Members only",\
                "position": 1,\
                "media": null\
            },\
            "subscriptions": [\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Marketing",\
                        "description": "Subscribe here for marketing purposes",\
                        "active": true,\
                        "strategy": "DOUBLE_OPT_IN",\
                        "uuid": "123123123",\
                        "id": 1\
                    }\
                },\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Monthly offers",\
                        "description": "A monthly email blast with our best offers!",\
                        "active": true,\
                        "strategy": "DOUBLE_OPT_IN",\
                        "uuid": null,\
                        "id": 3\
                    }\
                },\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Events",\
                        "description": "Stay in the loop for all our special events",\
                        "active": true,\
                        "strategy": "OPT_IN",\
                        "uuid": null,\
                        "id": 4\
                    }\
                },\
                {\
                    "is_subscribed": false,\
                    "status": "UNSUBSCRIBED",\
                    "subscription_type": {\
                        "name": "Product updates",\
                        "description": "Never miss out on any of our newest features",\
                        "active": true,\
                        "strategy": "OPT_OUT",\
                        "uuid": null,\
                        "id": 5\
                    }\
                }\
            ],\
            "credit_balance": {\
                "balance": 2895,\
                "id": 2625740\
            },\
            "prepaid_balance": {\
                "balance_in_cents": 4000\
            },\
            "current_values": {\
                "age": null,\
                "city": "Duckburg",\
                "dier": null,\
                "tier": "1",\
                "uuid": "123",\
                "email": "john@doe.com",\
                "avatar": "",\
                "locale": null,\
                "status": "ACTIVE",\
                "address": "",\
                "channel": null,\
                "lastname": "de Vries",\
                "birthdate": null,\
                "firstname": "Niels",\
                "created_at": "2021-10-20T18:06:09+00:00",\
                "postalcode": "9999YZ",\
                "updated_at": "2023-11-13T14:37:00+00:00",\
                "housenumber": 12,\
                "phonenumber": "",\
                "is_anonymous": false,\
                "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",\
                "a_custom_attribute": null,\
                "loyalty_balance": 2895,\
                "prepaid_balance": 40,\
                "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6ImxTKzI5cXNMdlNXTnM0QXFpMUFVWHc9PSIsInZhbHVlIjoiWEFFK3JYb3lXVDlicC82SkJ0SWNQMUd6S0pUVDROY0ZiY2kvVWVxYzNNOTRoTmswSHcxSkZMOHZqTFRrdmsySm1HL0NJS3BybmNrNW50RkpyL2V2VndZUWVGTEcwK1ZXWWptcnp3MmE4M3kycmlleGRRY0VsaDZ3eXRGd1VkMHRlMGhzdWtic0U5c21Kbm5iTW9qWDcvU0pQSzNEUVRtYnhRWG9ucU1HR21JPSIsIm1hYyI6Ijc2MjY2M2YzNmY3NjNlODMwNWVhNDA2ZjIwZDAxMWExZDNjOWVkZDU4NDYwMGExMTJiYzBhODVkOGJlZGMyMTIiLCJ0YWciOiIifQ==",\
                "favorite_color": null,\
                "custom_app_login_code": 206820,\
                "subscription_status_1": "UNSUBSCRIBED",\
                "subscription_status_3": "UNSUBSCRIBED",\
                "subscription_status_4": "UNSUBSCRIBED",\
                "subscription_status_5": "UNSUBSCRIBED",\
                "loyalty_associated_shops": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",\
                "previous_loyalty_balance": 2897,\
                "default_contact_identifier": "123123",\
                "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",\
                "loyalty_last_transaction_date": "2023-11-13T14:37:04+00:00",\
                "loyalty_first_transaction_date": "2023-01-09T15:03:37+00:00",\
                "loyalty_number_of_transactions": 77,\
                "loyalty_total_credits_received": 3014,\
                "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",\
                "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3",\
            },\
            "current_display_values": {\
                "age": "",\
                "city": "",\
                "dier": "False",\
                "tier": "Gold",\
                "uuid": "",\
                "email": "john@doe.com",\
                "avatar": "",\
                "locale": "",\
                "status": "Active",\
                "address": "",\
                "channel": "",\
                "lastname": "de Vries",\
                "birthdate": "",\
                "firstname": "Niels",\
                "created_at": "2021-10-20, 20:06",\
                "postalcode": "",\
                "updated_at": "2023-11-13, 15:37",\
                "housenumber": "",\
                "phonenumber": "",\
                "is_anonymous": "False",\
                "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",\
                "a_custom_attribute": "",\
                "loyalty_balance": "2895",\
                "prepaid_balance": "40",\
                "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Ijh6SnNUM2hIL1lqTStaNFVSbEtPZWc9PSIsInZhbHVlIjoiM0o4K2x4ajlOazl2bW90UzRGbkJnbVNjbDFaall1cWhOL1ZaRjE2d0VhR1BsSll6OG43dTU1dHlMVkxQMnlmOWZ6TFZaWTI3enZOTVFiWmxrNldNUitEMHRvQjIvWjUrbE1UNmJPaFRBb3R4cUxuMzNPUjdEWFZmZFRxUlJOQUs2MEdIK0llR1BQd0dncGhKanZSd0JYYVB3VWplek9FdnpPSUlKczBBTmVzPSIsIm1hYyI6ImYzOTFhMjA0ODE0ZjY3YmExYTY2ODM0YzhkNzFlNjFhNmM2M2Q1MjFhNDA2NDcxMjEzOGI1MGZkYmE1YjdlZWEiLCJ0YWciOiIifQ==",\
                "favorite_color": "",\
                "custom_app_login_code": "206820",\
                "subscription_status_1": "Unsubscribed",\
                "subscription_status_3": "Unsubscribed",\
                "subscription_status_4": "Unsubscribed",\
                "subscription_status_5": "Unsubscribed",\
                "loyalty_associated_shops": "Amsterdam Pop-up Store and Sesame Street",\
                "previous_loyalty_balance": "2897",\
                "default_contact_identifier": "123123",\
                "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",\
                "loyalty_last_transaction_date": "2023-11-13, 15:37",\
                "loyalty_first_transaction_date": "2023-01-09, 16:03",\
                "loyalty_number_of_transactions": "77",\
                "loyalty_total_credits_received": "3014",\
                "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",\
                "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3",\
            }\
        }\
    ],
    "meta": {
        "page": 1,
        "limit": "2",
        "viewing_from": 1,
        "viewing_to": 2,
        "last_page": 31,
        "total": 62
    }
}`

Possible errors

Code

Message

`1003`

Invalid input.

### Create Contact

If a Contact needs to be created, only the email address is needed. To add any additional data to the Contact, a subsequent [Update Contact](https://docs.piggy.eu/v3/oauth/contacts#update-contact) API call can be used.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contacts`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`email` `string`

`REQUIRED`

The email address of the Contact to be created.

`referral_code` `string`

`OPTIONAL`

A referral code used for Piggy's referral programme.

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
        "uuid": "cd183615-d1cb-402a-b618-6472d5e13890"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60005`

Contact already exists.

### Create Contact Async

If a Contact needs to be created _quickly_ and any system values that are normally created upon creation aren't relevant, this function can be used. This API call is much faster than the 'regular' creation API call, focusing on basic Contact creation.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contacts/async`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`email` `string`

`REQUIRED`

The email address of the Contact to be created.

`referral_code` `string`

`OPTIONAL`

A referral code used for Piggy's referral programme.

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
        "uuid": "cd183615-d1cb-402a-b618-6472d5e13890"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60005`

Contact already exists.

`60000`

Usage limit reached.

### Create Anonymous Contact

Creates an anonymous Contact with a fictitious email address. This can be used whenever a new Contact cannot (yet) be identified by their email address, but some actions are already desired (e.g. creation of Credit Receptions). The Contact in question can later create a Contact with their actual email address, and subsequently 'claim' the anonymous Contact, which is then merged with the new Contact.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contacts/anonymous`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`contact_identifier_value` `string`

`OPTIONAL`

The unique value for the Contact Identifier, which will serve as the identifier

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
        "uuid": "cd183615-d1cb-402a-b618-6472d5e13890"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60005`

Contact already exists.

`60014`

Contact identifier is already linked.

`60003`

Contact identifier not found.

`60000`

Usage limit reached.

### Find Contact

Find a Contact by email address. To find a Contact using an identifier, see [Contact Identifiers.](https://docs.piggy.eu/v3/oauth/contact-identifiers)

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/find-one-by`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`email` `string`

`REQUIRED`

The Contact's email address

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
443
444
445
446
447
448
449
450
451
452
453
454
455
456
457
458
459
460
461
462
463
464
465
466
467
468
469
470
471
472
473
474
475
476
477
478
479
480
481
482
483
484
485
486
487
488
489
490
491
492
493
494
495
496
497
498
499
500
501
502
503
504
505
506
507
508
509
510
511
512
513
514
515
516
517
518
519
520
521
522
523
524
525
526
527
528
529
530
531
532
533
534
535
536
537
538
539
540
541
542
543
544
545
546
547
548
549
550
551
552
553
554
555
556
557
558
559
560
561
562
563
564
565
566
567
568
569
570
571
572
573
574
575
576
577
578
579
580
581
582
583
584
585
586
587
588
589
590
591
592
593
594
595
596
597
598
599
600
601
602
603
604
605
606
607
608
609
610
611
612
613
614
615
616
617
618
619
620
621
622
623
624
625
626
627
628
629
630
631
632
633
634
635
636
637
638
639
640
641
642
643
644
645
646
647
648
649
650
651
652
653
654
655
656
657
658
659
660
661
662
663
664
665
666
667
668
669
670
671
672
673
674
675
676
677
678
679
680
681
682
683
684
685
686
687
688
689
690
691
692
693
694
695
696
697
698
699
700
701
702
703
704
705
706
707
708
709
710
711
712
713
714
715
716
717
718
719
720
721
722
723
724
725
726
727
728
729
730
731
732
733
734
735
736
737
738
739
740
741
742
743
744
745
746
747
748
749
750
751
752
753
754
755
756
757
758
759
760
761
762
763
764
765
766
767
768
769
770
771
772
773
774
775
776
777
778
779
780
781
782
783
784
785
786
787
788
789
` `{
    "data": {
        "id": 22,
        "uuid": "123",
        "email": "john@doe.com",
        "attributes": [\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2204,\
                    "type": "media_upload",\
                    "field_type": "media_upload",\
                    "name": "avatar",\
                    "label": "Avatar",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2205,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "firstname",\
                    "label": "First name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2206,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "lastname",\
                    "label": "Last name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "john@doe.com",\
                "attribute": {\
                    "id": 2203,\
                    "type": "email",\
                    "field_type": "email",\
                    "name": "email",\
                    "label": "Email",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "1617",\
                "attribute": {\
                    "id": 2220,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "previous_loyalty_balance",\
                    "label": "Previous credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1612",\
                "attribute": {\
                    "id": 2219,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_balance",\
                    "label": "Credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "55",\
                "attribute": {\
                    "id": 2223,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_number_of_transactions",\
                    "label": "Number of transactions",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-01-09T15:03:37+00:00",\
                "attribute": {\
                    "id": 2221,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_first_transaction_date",\
                    "label": "First credit reception date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "2023-11-06T11:32:00+00:00",\
                "attribute": {\
                    "id": 2222,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_last_transaction_date",\
                    "label": "Last transaction date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2207,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "address",\
                    "label": "Address",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2208,\
                    "type": "phone",\
                    "field_type": "phone",\
                    "name": "phonenumber",\
                    "label": "Telephone number",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2209,\
                    "type": "birth_date",\
                    "field_type": "birth_date",\
                    "name": "birthdate",\
                    "label": "Birth date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2210,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "channel",\
                    "label": "Channel",\
                    "options": [\
                        {\
                            "value": "BUSINESS_APP",\
                            "label": "BUSINESS_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "API",\
                            "label": "API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "BUSINESS_DASHBOARD",\
                            "label": "BUSINESS_DASHBOARD",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOMER_APP",\
                            "label": "CUSTOMER_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSHOP_API",\
                            "label": "WEBSHOP_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "IMPORT",\
                            "label": "IMPORT",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSITE_FORM",\
                            "label": "WEBSITE_FORM",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "REGISTER_API",\
                            "label": "REGISTER_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "OAUTH_API",\
                            "label": "OAUTH_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIPTION_TYPE",\
                            "label": "SUBSCRIPTION_TYPE",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOM_APP",\
                            "label": "CUSTOM_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "AUTOMATIONS",\
                            "label": "AUTOMATIONS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "FORMS",\
                            "label": "FORMS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WIDGET",\
                            "label": "WIDGET",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2211,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "age",\
                    "label": "Age",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2212,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "locale",\
                    "label": "Preferred language",\
                    "options": [\
                        {\
                            "value": "en",\
                            "label": "English",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "nl",\
                            "label": "Nederlands",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "de",\
                            "label": "Deutsch",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "es",\
                            "label": "Español",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "fr",\
                            "label": "Francés",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "ACTIVE",\
                "attribute": {\
                    "id": 2213,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "status",\
                    "label": "Status",\
                    "options": [\
                        {\
                            "value": "ACTIVE",\
                            "label": "Active",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "INACTIVE",\
                            "label": "Inactive",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-11-06T16:47:00+00:00",\
                "attribute": {\
                    "id": 2214,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "updated_at",\
                    "label": "Updated at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2021-10-20T18:06:09+00:00",\
                "attribute": {\
                    "id": 2215,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "created_at",\
                    "label": "Created at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2216,\
                    "type": "boolean",\
                    "field_type": "boolean",\
                    "name": "is_anonymous",\
                    "label": "Anonymous",\
                    "options": [\
                        {\
                            "value": "true",\
                            "label": "True",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "false",\
                            "label": "False",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "20",\
                "attribute": {\
                    "id": 2217,\
                    "type": "float",\
                    "field_type": "float",\
                    "name": "prepaid_balance",\
                    "label": "Prepaid balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",\
                "attribute": {\
                    "id": 2218,\
                    "type": "multi_select",\
                    "field_type": "multi_select",\
                    "name": "loyalty_associated_shops",\
                    "label": "Associated shops",\
                    "options": [\
                        {\
                            "value": "123123",\
                            "label": "Amsterdam Pop-up Store",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                        {\
                            "value": "12312312312312",\
                            "label": "Web Shop",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/webshop.svg"\
                            }\
                        },\
                        {\
                            "value": "2222e1242t3324535",\
                            "label": "Sesame Street",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1697",\
                "attribute": {\
                    "id": 2224,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_total_credits_received",\
                    "label": "Total credits received",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1",\
                "attribute": {\
                    "id": 2225,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "tier",\
                    "label": "Tier",\
                    "options": [\
                        {\
                            "value": "1",\
                            "label": "Gold",\
                            "description": "VIP Members only",\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2226,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_3",\
                    "label": "Subscription status: Monthly offers",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2227,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_1",\
                    "label": "Subscription status: Marketing",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2228,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_5",\
                    "label": "Subscription status: Product updates",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2229,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_4",\
                    "label": "Subscription status: Events",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            }\
        ],
        "tier": {
            "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",
            "name": "Gold",
            "description": "VIP Members only",
            "position": 1,
            "media": null
        },
        "subscriptions": [\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Marketing",\
                    "description": "Subscribe here for marketing purposes",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": "123123123",\
                    "id": 1\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Monthly offers",\
                    "description": "A monthly email blast with our best offers!",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": null,\
                    "id": 3\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Events",\
                    "description": "Stay in the loop for all our special events",\
                    "active": true,\
                    "strategy": "OPT_IN",\
                    "uuid": null,\
                    "id": 4\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Product updates",\
                    "description": "Never miss out on any of our newest features",\
                    "active": true,\
                    "strategy": "OPT_OUT",\
                    "uuid": null,\
                    "id": 5\
                }\
            }\
        ],
        "credit_balance": {
            "balance": 1612,
            "id": 2625740
        },
        "prepaid_balance": {
            "balance_in_cents": 2000
        },
        "current_values": {
            "age": null,
            "city": "Duckburg",
            "dier": null,
            "tier": "1",
            "uuid": "123",
            "email": "john@doe.com",
            "avatar": "",
            "locale": null,
            "status": "ACTIVE",
            "address": "",
            "channel": null,
            "lastname": "",
            "birthdate": null,
            "firstname": "",
            "created_at": "2021-10-20T18:06:09+00:00",
            "postalcode": "9999YZ",
            "updated_at": "2023-11-06T16:47:00+00:00",
            "housenumber": 12,
            "phonenumber": "",
            "is_anonymous": false,
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": null,
            "loyalty_balance": 1612,
            "prepaid_balance": 20,
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Ikh3ZW8yRW9pSU9Gc25SYmxRUFVVWUE9PSIsInZhbHVlIjoianhuYncxVHBlK0dqMTN4RHhXNzdZNVJ3aGJqcERFdmVWQlcxYjFNZVZYSVdoWVVMWk54K1UvQ2lFek1nMmtGaTJZNGRNOG1rUUxwRGNnU25JcitpN0d5RWVKN21kS1gvSmMvNTA2bmxqU0Z0cmxoR2pPT0Ewc1hXeEJsMkVJNGFDQlNlSWpHRXVpWndyd3g3eTVXck90Z0tlL3J2RHNRKzRjRkNGb0tmVmVzPSIsIm1hYyI6ImZlMzc0ZDYzZDVjNTNjOTc3YTc1ZTMxN2RmMzM4MDFkZWQ2YWI2MDkwNDY4OWQ1MDk0ZWRkN2Y2ZTdhZjY5ZmEiLCJ0YWciOiIifQ==",
            "custom_app_login_code": 198169,
            "subscription_status_1": "UNSUBSCRIBED",
            "subscription_status_3": "UNSUBSCRIBED",
            "subscription_status_4": "UNSUBSCRIBED",
            "subscription_status_5": "UNSUBSCRIBED",
            "loyalty_associated_shops": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",
            "previous_loyalty_balance": 1617,
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06T11:32:00+00:00",
            "loyalty_first_transaction_date": "2023-01-09T15:03:37+00:00",
            "loyalty_number_of_transactions": 55,
            "loyalty_total_credits_received": 1697,
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
        },
        "current_display_values": {
            "age": "",
            "city": "",
            "dier": "False",
            "tier": "Gold",
            "uuid": "",
            "email": "john@doe.com",
            "avatar": "",
            "locale": "",
            "status": "Active",
            "address": "",
            "channel": "",
            "lastname": "",
            "birthdate": "",
            "firstname": "",
            "created_at": "2021-10-20, 20:06",
            "postalcode": "",
            "updated_at": "2023-11-06, 17:47",
            "housenumber": "",
            "phonenumber": "",
            "is_anonymous": "False",
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": "",
            "loyalty_balance": "1612",
            "prepaid_balance": "20",
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Imx6TzJVV1ZwNlBRd3VkQmV1NnhKbnc9PSIsInZhbHVlIjoiVnlKZWM4OG5Vdjl0QmNEY242YmZ3OFNpWjQwQXkwRThmMCttWHlwQmNSTFluNzdpVkQwY0ZEcVo5TktvUXRHSVBYUE43S2dscFpyMUJnazFrWWZWVHI3cDNLTVJsZDcwWU9zQWdIejh5YjRQOGx4RjV6aDJjR3FTWlI1RTNlQlRrZEVNcURZeFlaZlYvQ2R2STdSU1RDdFdZT0RRdkxNN01vOXhLQnhaZkNVPSIsIm1hYyI6ImJjMzczNWQ2NGEwNzZlNjZhZTU0OTk3MzIxOTczODYzZjMxZjYwNzM2MjY5ZWJkODRkMTEzODU0NmQxMDU4OTQiLCJ0YWciOiIifQ==",
            "custom_app_login_code": "198169",
            "subscription_status_1": "Unsubscribed",
            "subscription_status_3": "Unsubscribed",
            "subscription_status_4": "Unsubscribed",
            "subscription_status_5": "Unsubscribed",
            "loyalty_associated_shops": "Amsterdam Pop-up Store and Sesame Street",
            "previous_loyalty_balance": "1617",
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06, 12:32",
            "loyalty_first_transaction_date": "2023-01-09, 16:03",
            "loyalty_number_of_transactions": "55",
            "loyalty_total_credits_received": "1697",
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
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

### Find or Create Contact

Find Contact by email address or – if no Contact exists yet for this email address – our API creates a new Contact for the email address you just supplied and returns the newly created Contact in the response.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/find-or-create`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`email` `string`

`REQUIRED`

The Contact's email address.

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
443
444
445
446
447
448
449
450
451
452
453
454
455
456
457
458
459
460
461
462
463
464
465
466
467
468
469
470
471
472
473
474
475
476
477
478
479
480
481
482
483
484
485
486
487
488
489
490
491
492
493
494
495
496
497
498
499
500
501
502
503
504
505
506
507
508
509
510
511
512
513
514
515
516
517
518
519
520
521
522
523
524
525
526
527
528
529
530
531
532
533
534
535
536
537
538
539
540
541
542
543
544
545
546
547
548
549
550
551
552
553
554
555
556
557
558
559
560
561
562
563
564
565
566
567
568
569
570
571
572
573
574
575
576
577
578
579
580
581
582
583
584
585
586
587
588
589
590
591
592
593
594
595
596
597
598
599
600
601
602
603
604
605
606
607
608
609
610
611
612
613
614
615
616
617
618
619
620
621
622
623
624
625
626
627
628
629
630
631
632
633
634
635
636
637
638
639
640
641
642
643
644
645
646
647
648
649
650
651
652
653
654
655
656
657
658
659
660
661
662
663
664
665
666
667
668
669
670
671
672
673
674
675
676
677
678
679
680
681
682
683
684
685
686
687
688
689
690
691
692
693
694
695
696
697
698
699
700
701
702
703
704
705
706
707
708
709
710
711
712
713
714
715
716
717
718
719
720
721
722
723
724
725
726
727
728
729
730
731
732
733
734
735
736
737
738
739
740
741
742
743
744
745
746
747
748
749
750
751
752
753
754
755
756
757
758
759
760
761
762
763
764
765
766
767
768
769
770
771
772
773
774
775
776
777
778
779
780
781
782
783
784
785
786
787
788
789
` `{
    "data": {
        "id": 22,
        "uuid": "123",
        "email": "john@doe.com",
        "attributes": [\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2204,\
                    "type": "media_upload",\
                    "field_type": "media_upload",\
                    "name": "avatar",\
                    "label": "Avatar",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2205,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "firstname",\
                    "label": "First name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2206,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "lastname",\
                    "label": "Last name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "john@doe.com",\
                "attribute": {\
                    "id": 2203,\
                    "type": "email",\
                    "field_type": "email",\
                    "name": "email",\
                    "label": "Email",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "1617",\
                "attribute": {\
                    "id": 2220,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "previous_loyalty_balance",\
                    "label": "Previous credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1612",\
                "attribute": {\
                    "id": 2219,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_balance",\
                    "label": "Credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "55",\
                "attribute": {\
                    "id": 2223,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_number_of_transactions",\
                    "label": "Number of transactions",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-01-09T15:03:37+00:00",\
                "attribute": {\
                    "id": 2221,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_first_transaction_date",\
                    "label": "First credit reception date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "2023-11-06T11:32:00+00:00",\
                "attribute": {\
                    "id": 2222,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_last_transaction_date",\
                    "label": "Last transaction date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2207,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "address",\
                    "label": "Address",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2208,\
                    "type": "phone",\
                    "field_type": "phone",\
                    "name": "phonenumber",\
                    "label": "Telephone number",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2209,\
                    "type": "birth_date",\
                    "field_type": "birth_date",\
                    "name": "birthdate",\
                    "label": "Birth date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2210,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "channel",\
                    "label": "Channel",\
                    "options": [\
                        {\
                            "value": "BUSINESS_APP",\
                            "label": "BUSINESS_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "API",\
                            "label": "API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "BUSINESS_DASHBOARD",\
                            "label": "BUSINESS_DASHBOARD",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOMER_APP",\
                            "label": "CUSTOMER_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSHOP_API",\
                            "label": "WEBSHOP_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "IMPORT",\
                            "label": "IMPORT",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSITE_FORM",\
                            "label": "WEBSITE_FORM",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "REGISTER_API",\
                            "label": "REGISTER_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "OAUTH_API",\
                            "label": "OAUTH_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIPTION_TYPE",\
                            "label": "SUBSCRIPTION_TYPE",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOM_APP",\
                            "label": "CUSTOM_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "AUTOMATIONS",\
                            "label": "AUTOMATIONS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "FORMS",\
                            "label": "FORMS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WIDGET",\
                            "label": "WIDGET",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2211,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "age",\
                    "label": "Age",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2212,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "locale",\
                    "label": "Preferred language",\
                    "options": [\
                        {\
                            "value": "en",\
                            "label": "English",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "nl",\
                            "label": "Nederlands",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "de",\
                            "label": "Deutsch",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "es",\
                            "label": "Español",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "fr",\
                            "label": "Francés",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "ACTIVE",\
                "attribute": {\
                    "id": 2213,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "status",\
                    "label": "Status",\
                    "options": [\
                        {\
                            "value": "ACTIVE",\
                            "label": "Active",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "INACTIVE",\
                            "label": "Inactive",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-11-06T16:47:00+00:00",\
                "attribute": {\
                    "id": 2214,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "updated_at",\
                    "label": "Updated at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2021-10-20T18:06:09+00:00",\
                "attribute": {\
                    "id": 2215,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "created_at",\
                    "label": "Created at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2216,\
                    "type": "boolean",\
                    "field_type": "boolean",\
                    "name": "is_anonymous",\
                    "label": "Anonymous",\
                    "options": [\
                        {\
                            "value": "true",\
                            "label": "True",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "false",\
                            "label": "False",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "20",\
                "attribute": {\
                    "id": 2217,\
                    "type": "float",\
                    "field_type": "float",\
                    "name": "prepaid_balance",\
                    "label": "Prepaid balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",\
                "attribute": {\
                    "id": 2218,\
                    "type": "multi_select",\
                    "field_type": "multi_select",\
                    "name": "loyalty_associated_shops",\
                    "label": "Associated shops",\
                    "options": [\
                        {\
                            "value": "123123",\
                            "label": "Amsterdam Pop-up Store",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                        {\
                            "value": "12312312312312",\
                            "label": "Web Shop",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/webshop.svg"\
                            }\
                        },\
                        {\
                            "value": "2222e1242t3324535",\
                            "label": "Sesame Street",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1697",\
                "attribute": {\
                    "id": 2224,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_total_credits_received",\
                    "label": "Total credits received",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1",\
                "attribute": {\
                    "id": 2225,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "tier",\
                    "label": "Tier",\
                    "options": [\
                        {\
                            "value": "1",\
                            "label": "Gold",\
                            "description": "VIP Members only",\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2226,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_3",\
                    "label": "Subscription status: Monthly offers",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2227,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_1",\
                    "label": "Subscription status: Marketing",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2228,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_5",\
                    "label": "Subscription status: Product updates",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2229,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_4",\
                    "label": "Subscription status: Events",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            }\
        ],
        "tier": {
            "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",
            "name": "Gold",
            "description": "VIP Members only",
            "position": 1,
            "media": null
        },
        "subscriptions": [\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Marketing",\
                    "description": "Subscribe here for marketing purposes",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": "123123123",\
                    "id": 1\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Monthly offers",\
                    "description": "A monthly email blast with our best offers!",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": null,\
                    "id": 3\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Events",\
                    "description": "Stay in the loop for all our special events",\
                    "active": true,\
                    "strategy": "OPT_IN",\
                    "uuid": null,\
                    "id": 4\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Product updates",\
                    "description": "Never miss out on any of our newest features",\
                    "active": true,\
                    "strategy": "OPT_OUT",\
                    "uuid": null,\
                    "id": 5\
                }\
            }\
        ],
        "credit_balance": {
            "balance": 1612,
            "id": 2625740
        },
        "prepaid_balance": {
            "balance_in_cents": 2000
        },
        "current_values": {
            "age": null,
            "city": "Duckburg",
            "dier": null,
            "tier": "1",
            "uuid": "123",
            "email": "john@doe.com",
            "avatar": "",
            "locale": null,
            "status": "ACTIVE",
            "address": "",
            "channel": null,
            "lastname": "",
            "birthdate": null,
            "firstname": "",
            "created_at": "2021-10-20T18:06:09+00:00",
            "postalcode": "9999YZ",
            "updated_at": "2023-11-06T16:47:00+00:00",
            "housenumber": 12,
            "phonenumber": "",
            "is_anonymous": false,
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": null,
            "loyalty_balance": 1612,
            "prepaid_balance": 20,
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Ikh3ZW8yRW9pSU9Gc25SYmxRUFVVWUE9PSIsInZhbHVlIjoianhuYncxVHBlK0dqMTN4RHhXNzdZNVJ3aGJqcERFdmVWQlcxYjFNZVZYSVdoWVVMWk54K1UvQ2lFek1nMmtGaTJZNGRNOG1rUUxwRGNnU25JcitpN0d5RWVKN21kS1gvSmMvNTA2bmxqU0Z0cmxoR2pPT0Ewc1hXeEJsMkVJNGFDQlNlSWpHRXVpWndyd3g3eTVXck90Z0tlL3J2RHNRKzRjRkNGb0tmVmVzPSIsIm1hYyI6ImZlMzc0ZDYzZDVjNTNjOTc3YTc1ZTMxN2RmMzM4MDFkZWQ2YWI2MDkwNDY4OWQ1MDk0ZWRkN2Y2ZTdhZjY5ZmEiLCJ0YWciOiIifQ==",
            "custom_app_login_code": 198169,
            "subscription_status_1": "UNSUBSCRIBED",
            "subscription_status_3": "UNSUBSCRIBED",
            "subscription_status_4": "UNSUBSCRIBED",
            "subscription_status_5": "UNSUBSCRIBED",
            "loyalty_associated_shops": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",
            "previous_loyalty_balance": 1617,
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06T11:32:00+00:00",
            "loyalty_first_transaction_date": "2023-01-09T15:03:37+00:00",
            "loyalty_number_of_transactions": 55,
            "loyalty_total_credits_received": 1697,
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
        },
        "current_display_values": {
            "age": "",
            "city": "",
            "dier": "False",
            "tier": "Gold",
            "uuid": "",
            "email": "john@doe.com",
            "avatar": "",
            "locale": "",
            "status": "Active",
            "address": "",
            "channel": "",
            "lastname": "",
            "birthdate": "",
            "firstname": "",
            "created_at": "2021-10-20, 20:06",
            "postalcode": "",
            "updated_at": "2023-11-06, 17:47",
            "housenumber": "",
            "phonenumber": "",
            "is_anonymous": "False",
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": "",
            "loyalty_balance": "1612",
            "prepaid_balance": "20",
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Imx6TzJVV1ZwNlBRd3VkQmV1NnhKbnc9PSIsInZhbHVlIjoiVnlKZWM4OG5Vdjl0QmNEY242YmZ3OFNpWjQwQXkwRThmMCttWHlwQmNSTFluNzdpVkQwY0ZEcVo5TktvUXRHSVBYUE43S2dscFpyMUJnazFrWWZWVHI3cDNLTVJsZDcwWU9zQWdIejh5YjRQOGx4RjV6aDJjR3FTWlI1RTNlQlRrZEVNcURZeFlaZlYvQ2R2STdSU1RDdFdZT0RRdkxNN01vOXhLQnhaZkNVPSIsIm1hYyI6ImJjMzczNWQ2NGEwNzZlNjZhZTU0OTk3MzIxOTczODYzZjMxZjYwNzM2MjY5ZWJkODRkMTEzODU0NmQxMDU4OTQiLCJ0YWciOiIifQ==",
            "custom_app_login_code": "198169",
            "subscription_status_1": "Unsubscribed",
            "subscription_status_3": "Unsubscribed",
            "subscription_status_4": "Unsubscribed",
            "subscription_status_5": "Unsubscribed",
            "loyalty_associated_shops": "Amsterdam Pop-up Store and Sesame Street",
            "previous_loyalty_balance": "1617",
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06, 12:32",
            "loyalty_first_transaction_date": "2023-01-09, 16:03",
            "loyalty_number_of_transactions": "55",
            "loyalty_total_credits_received": "1697",
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60000`

Usage limit reached.

### Find or Create Contact Async

If a Contact needs to be retrieved or created _quickly_ and any system values that are normally created upon contact creation aren't relevant, this can be used. This API call is much faster than the 'regular' find or create API call; only the Contact's UUID will be returned on the response in this instance.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/find-or-create/async`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`email` `string`

`REQUIRED`

The email address of the Contact to be created or to be found.

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
        "uuid": "cd183615-d1cb-402a-b618-6472d5e13890"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`60000`

Usage limit reached.

### Get Contact

Retrieve a specific Contact with the the using its UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID

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
443
444
445
446
447
448
449
450
451
452
453
454
455
456
457
458
459
460
461
462
463
464
465
466
467
468
469
470
471
472
473
474
475
476
477
478
479
480
481
482
483
484
485
486
487
488
489
490
491
492
493
494
495
496
497
498
499
500
501
502
503
504
505
506
507
508
509
510
511
512
513
514
515
516
517
518
519
520
521
522
523
524
525
526
527
528
529
530
531
532
533
534
535
536
537
538
539
540
541
542
543
544
545
546
547
548
549
550
551
552
553
554
555
556
557
558
559
560
561
562
563
564
565
566
567
568
569
570
571
572
573
574
575
576
577
578
579
580
581
582
583
584
585
586
587
588
589
590
591
592
593
594
595
596
597
598
599
600
601
602
603
604
605
606
607
608
609
610
611
612
613
614
615
616
617
618
619
620
621
622
623
624
625
626
627
628
629
630
631
632
633
634
635
636
637
638
639
640
641
642
643
644
645
646
647
648
649
650
651
652
653
654
655
656
657
658
659
660
661
662
663
664
665
666
667
668
669
670
671
672
673
674
675
676
677
678
679
680
681
682
683
684
685
686
687
688
689
690
691
692
693
694
695
696
697
698
699
700
701
702
703
704
705
706
707
708
709
710
711
712
713
714
715
716
717
718
719
720
721
722
723
724
725
726
727
728
729
730
731
732
733
734
735
736
737
738
739
740
741
742
743
744
745
746
747
748
749
750
751
752
753
754
755
756
757
758
759
760
761
762
763
764
765
766
767
768
769
770
771
772
773
774
775
776
777
778
779
780
781
782
783
784
785
786
787
788
789
` `{
    "data": {
        "id": 22,
        "uuid": "123",
        "email": "john@doe.com",
        "attributes": [\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2204,\
                    "type": "media_upload",\
                    "field_type": "media_upload",\
                    "name": "avatar",\
                    "label": "Avatar",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2205,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "firstname",\
                    "label": "First name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2206,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "lastname",\
                    "label": "Last name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "john@doe.com",\
                "attribute": {\
                    "id": 2203,\
                    "type": "email",\
                    "field_type": "email",\
                    "name": "email",\
                    "label": "Email",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "1617",\
                "attribute": {\
                    "id": 2220,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "previous_loyalty_balance",\
                    "label": "Previous credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1612",\
                "attribute": {\
                    "id": 2219,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_balance",\
                    "label": "Credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "55",\
                "attribute": {\
                    "id": 2223,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_number_of_transactions",\
                    "label": "Number of transactions",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-01-09T15:03:37+00:00",\
                "attribute": {\
                    "id": 2221,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_first_transaction_date",\
                    "label": "First credit reception date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "2023-11-06T11:32:00+00:00",\
                "attribute": {\
                    "id": 2222,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_last_transaction_date",\
                    "label": "Last transaction date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2207,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "address",\
                    "label": "Address",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2208,\
                    "type": "phone",\
                    "field_type": "phone",\
                    "name": "phonenumber",\
                    "label": "Telephone number",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2209,\
                    "type": "birth_date",\
                    "field_type": "birth_date",\
                    "name": "birthdate",\
                    "label": "Birth date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2210,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "channel",\
                    "label": "Channel",\
                    "options": [\
                        {\
                            "value": "BUSINESS_APP",\
                            "label": "BUSINESS_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "API",\
                            "label": "API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "BUSINESS_DASHBOARD",\
                            "label": "BUSINESS_DASHBOARD",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOMER_APP",\
                            "label": "CUSTOMER_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSHOP_API",\
                            "label": "WEBSHOP_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "IMPORT",\
                            "label": "IMPORT",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSITE_FORM",\
                            "label": "WEBSITE_FORM",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "REGISTER_API",\
                            "label": "REGISTER_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "OAUTH_API",\
                            "label": "OAUTH_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIPTION_TYPE",\
                            "label": "SUBSCRIPTION_TYPE",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOM_APP",\
                            "label": "CUSTOM_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "AUTOMATIONS",\
                            "label": "AUTOMATIONS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "FORMS",\
                            "label": "FORMS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WIDGET",\
                            "label": "WIDGET",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2211,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "age",\
                    "label": "Age",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2212,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "locale",\
                    "label": "Preferred language",\
                    "options": [\
                        {\
                            "value": "en",\
                            "label": "English",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "nl",\
                            "label": "Nederlands",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "de",\
                            "label": "Deutsch",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "es",\
                            "label": "Español",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "fr",\
                            "label": "Francés",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "ACTIVE",\
                "attribute": {\
                    "id": 2213,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "status",\
                    "label": "Status",\
                    "options": [\
                        {\
                            "value": "ACTIVE",\
                            "label": "Active",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "INACTIVE",\
                            "label": "Inactive",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-11-06T16:47:00+00:00",\
                "attribute": {\
                    "id": 2214,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "updated_at",\
                    "label": "Updated at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2021-10-20T18:06:09+00:00",\
                "attribute": {\
                    "id": 2215,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "created_at",\
                    "label": "Created at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2216,\
                    "type": "boolean",\
                    "field_type": "boolean",\
                    "name": "is_anonymous",\
                    "label": "Anonymous",\
                    "options": [\
                        {\
                            "value": "true",\
                            "label": "True",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "false",\
                            "label": "False",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "20",\
                "attribute": {\
                    "id": 2217,\
                    "type": "float",\
                    "field_type": "float",\
                    "name": "prepaid_balance",\
                    "label": "Prepaid balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",\
                "attribute": {\
                    "id": 2218,\
                    "type": "multi_select",\
                    "field_type": "multi_select",\
                    "name": "loyalty_associated_shops",\
                    "label": "Associated shops",\
                    "options": [\
                        {\
                            "value": "123123",\
                            "label": "Amsterdam Pop-up Store",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                        {\
                            "value": "12312312312312",\
                            "label": "Web Shop",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/webshop.svg"\
                            }\
                        },\
                        {\
                            "value": "2222e1242t3324535",\
                            "label": "Sesame Street",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1697",\
                "attribute": {\
                    "id": 2224,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_total_credits_received",\
                    "label": "Total credits received",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1",\
                "attribute": {\
                    "id": 2225,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "tier",\
                    "label": "Tier",\
                    "options": [\
                        {\
                            "value": "1",\
                            "label": "Gold",\
                            "description": "VIP Members only",\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2226,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_3",\
                    "label": "Subscription status: Monthly offers",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2227,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_1",\
                    "label": "Subscription status: Marketing",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2228,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_5",\
                    "label": "Product updates",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2229,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_4",\
                    "label": "Subscription status: Events",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            }\
        ],
        "tier": {
            "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",
            "name": "Gold",
            "description": "VIP Members only",
            "position": 1,
            "media": null
        },
        "subscriptions": [\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Marketing",\
                    "description": "Subscribe here for marketing purposes",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": "123123123",\
                    "id": 1\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Monthly offers",\
                    "description": "A monthly email blast with our best offers!",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": null,\
                    "id": 3\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Events",\
                    "description": "Stay in the loop for all our special events",\
                    "active": true,\
                    "strategy": "OPT_IN",\
                    "uuid": null,\
                    "id": 4\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Product updates",\
                    "description": "Never miss out on any of our newest features",\
                    "active": true,\
                    "strategy": "OPT_OUT",\
                    "uuid": null,\
                    "id": 5\
                }\
            }\
        ],
        "credit_balance": {
            "balance": 1612,
            "id": 2625740
        },
        "prepaid_balance": {
            "balance_in_cents": 2000
        },
        "current_values": {
            "age": null,
            "city": "Duckburg",
            "dier": null,
            "tier": "1",
            "uuid": "123",
            "email": "john@doe.com",
            "avatar": "",
            "locale": null,
            "status": "ACTIVE",
            "address": "",
            "channel": null,
            "lastname": "",
            "birthdate": null,
            "firstname": "",
            "created_at": "2021-10-20T18:06:09+00:00",
            "postalcode": "9999YZ",
            "updated_at": "2023-11-06T16:47:00+00:00",
            "housenumber": 12,
            "phonenumber": "",
            "is_anonymous": false,
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": null,
            "loyalty_balance": 1612,
            "prepaid_balance": 20,
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Ikh3ZW8yRW9pSU9Gc25SYmxRUFVVWUE9PSIsInZhbHVlIjoianhuYncxVHBlK0dqMTN4RHhXNzdZNVJ3aGJqcERFdmVWQlcxYjFNZVZYSVdoWVVMWk54K1UvQ2lFek1nMmtGaTJZNGRNOG1rUUxwRGNnU25JcitpN0d5RWVKN21kS1gvSmMvNTA2bmxqU0Z0cmxoR2pPT0Ewc1hXeEJsMkVJNGFDQlNlSWpHRXVpWndyd3g3eTVXck90Z0tlL3J2RHNRKzRjRkNGb0tmVmVzPSIsIm1hYyI6ImZlMzc0ZDYzZDVjNTNjOTc3YTc1ZTMxN2RmMzM4MDFkZWQ2YWI2MDkwNDY4OWQ1MDk0ZWRkN2Y2ZTdhZjY5ZmEiLCJ0YWciOiIifQ==",
            "custom_app_login_code": 198169,
            "subscription_status_1": "UNSUBSCRIBED",
            "subscription_status_3": "UNSUBSCRIBED",
            "subscription_status_4": "UNSUBSCRIBED",
            "subscription_status_5": "UNSUBSCRIBED",
            "loyalty_associated_shops": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",
            "previous_loyalty_balance": 1617,
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06T11:32:00+00:00",
            "loyalty_first_transaction_date": "2023-01-09T15:03:37+00:00",
            "loyalty_number_of_transactions": 55,
            "loyalty_total_credits_received": 1697,
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
        },
        "current_display_values": {
            "age": "",
            "city": "",
            "dier": "False",
            "tier": "Gold",
            "uuid": "",
            "email": "john@doe.com",
            "avatar": "",
            "locale": "",
            "status": "Active",
            "address": "",
            "channel": "",
            "lastname": "",
            "birthdate": "",
            "firstname": "",
            "created_at": "2021-10-20, 20:06",
            "postalcode": "",
            "updated_at": "2023-11-06, 17:47",
            "housenumber": "",
            "phonenumber": "",
            "is_anonymous": "False",
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": "",
            "loyalty_balance": "1612",
            "prepaid_balance": "20",
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Imx6TzJVV1ZwNlBRd3VkQmV1NnhKbnc9PSIsInZhbHVlIjoiVnlKZWM4OG5Vdjl0QmNEY242YmZ3OFNpWjQwQXkwRThmMCttWHlwQmNSTFluNzdpVkQwY0ZEcVo5TktvUXRHSVBYUE43S2dscFpyMUJnazFrWWZWVHI3cDNLTVJsZDcwWU9zQWdIejh5YjRQOGx4RjV6aDJjR3FTWlI1RTNlQlRrZEVNcURZeFlaZlYvQ2R2STdSU1RDdFdZT0RRdkxNN01vOXhLQnhaZkNVPSIsIm1hYyI6ImJjMzczNWQ2NGEwNzZlNjZhZTU0OTk3MzIxOTczODYzZjMxZjYwNzM2MjY5ZWJkODRkMTEzODU0NmQxMDU4OTQiLCJ0YWciOiIifQ==",
            "custom_app_login_code": "198169",
            "subscription_status_1": "Unsubscribed",
            "subscription_status_3": "Unsubscribed",
            "subscription_status_4": "Unsubscribed",
            "subscription_status_5": "Unsubscribed",
            "loyalty_associated_shops": "Amsterdam Pop-up Store and Sesame Street",
            "previous_loyalty_balance": "1617",
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06, 12:32",
            "loyalty_first_transaction_date": "2023-01-09, 16:03",
            "loyalty_number_of_transactions": "55",
            "loyalty_total_credits_received": "1697",
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
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

### Update Contact

Contact's attributes. Please note that system values or system defined attributes, such as 'credit balance' or 'created at', cannot be updated.

Example payload:

`1
2
3
4
5
6
` `{
    "attributes": {
        "firstname": "John",
        "lastname": "Doe"
    }
}`

PUT

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID

Body

`attributes` `array`

`REQUIRED`

An array of the attributes to be updated.

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
443
444
445
446
447
448
449
450
451
452
453
454
455
456
457
458
459
460
461
462
463
464
465
466
467
468
469
470
471
472
473
474
475
476
477
478
479
480
481
482
483
484
485
486
487
488
489
490
491
492
493
494
495
496
497
498
499
500
501
502
503
504
505
506
507
508
509
510
511
512
513
514
515
516
517
518
519
520
521
522
523
524
525
526
527
528
529
530
531
532
533
534
535
536
537
538
539
540
541
542
543
544
545
546
547
548
549
550
551
552
553
554
555
556
557
558
559
560
561
562
563
564
565
566
567
568
569
570
571
572
573
574
575
576
577
578
579
580
581
582
583
584
585
586
587
588
589
590
591
592
593
594
595
596
597
598
599
600
601
602
603
604
605
606
607
608
609
610
611
612
613
614
615
616
617
618
619
620
621
622
623
624
625
626
627
628
629
630
631
632
633
634
635
636
637
638
639
640
641
642
643
644
645
646
647
648
649
650
651
652
653
654
655
656
657
658
659
660
661
662
663
664
665
666
667
668
669
670
671
672
673
674
675
676
677
678
679
680
681
682
683
684
685
686
687
688
689
690
691
692
693
694
695
696
697
698
699
700
701
702
703
704
705
706
707
708
709
710
711
712
713
714
715
716
717
718
719
720
721
722
723
724
725
726
727
728
729
730
731
732
733
734
735
736
737
738
739
740
741
742
743
744
745
746
747
748
749
750
751
752
753
754
755
756
757
758
759
760
761
762
763
764
765
766
767
768
769
770
771
772
773
774
775
776
777
778
779
780
781
782
783
784
785
786
787
788
789
` `{
    "data": {
        "id": 22,
        "uuid": "123",
        "email": "john@doe.com",
        "attributes": [\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2204,\
                    "type": "media_upload",\
                    "field_type": "media_upload",\
                    "name": "avatar",\
                    "label": "Avatar",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2205,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "firstname",\
                    "label": "First name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2206,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "lastname",\
                    "label": "Last name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "john@doe.com",\
                "attribute": {\
                    "id": 2203,\
                    "type": "email",\
                    "field_type": "email",\
                    "name": "email",\
                    "label": "Email",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "1617",\
                "attribute": {\
                    "id": 2220,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "previous_loyalty_balance",\
                    "label": "Previous credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1612",\
                "attribute": {\
                    "id": 2219,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_balance",\
                    "label": "Credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "55",\
                "attribute": {\
                    "id": 2223,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_number_of_transactions",\
                    "label": "Number of transactions",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-01-09T15:03:37+00:00",\
                "attribute": {\
                    "id": 2221,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_first_transaction_date",\
                    "label": "First credit reception date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "2023-11-06T11:32:00+00:00",\
                "attribute": {\
                    "id": 2222,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_last_transaction_date",\
                    "label": "Last transaction date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2207,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "address",\
                    "label": "Address",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2208,\
                    "type": "phone",\
                    "field_type": "phone",\
                    "name": "phonenumber",\
                    "label": "Telephone number",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2209,\
                    "type": "birth_date",\
                    "field_type": "birth_date",\
                    "name": "birthdate",\
                    "label": "Birth date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2210,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "channel",\
                    "label": "Channel",\
                    "options": [\
                        {\
                            "value": "BUSINESS_APP",\
                            "label": "BUSINESS_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "API",\
                            "label": "API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "BUSINESS_DASHBOARD",\
                            "label": "BUSINESS_DASHBOARD",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOMER_APP",\
                            "label": "CUSTOMER_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSHOP_API",\
                            "label": "WEBSHOP_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "IMPORT",\
                            "label": "IMPORT",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSITE_FORM",\
                            "label": "WEBSITE_FORM",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "REGISTER_API",\
                            "label": "REGISTER_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "OAUTH_API",\
                            "label": "OAUTH_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIPTION_TYPE",\
                            "label": "SUBSCRIPTION_TYPE",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOM_APP",\
                            "label": "CUSTOM_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "AUTOMATIONS",\
                            "label": "AUTOMATIONS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "FORMS",\
                            "label": "FORMS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WIDGET",\
                            "label": "WIDGET",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2211,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "age",\
                    "label": "Age",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2212,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "locale",\
                    "label": "Preferred language",\
                    "options": [\
                        {\
                            "value": "en",\
                            "label": "English",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "nl",\
                            "label": "Nederlands",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "de",\
                            "label": "Deutsch",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "es",\
                            "label": "Español",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "fr",\
                            "label": "Francés",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "ACTIVE",\
                "attribute": {\
                    "id": 2213,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "status",\
                    "label": "Status",\
                    "options": [\
                        {\
                            "value": "ACTIVE",\
                            "label": "Active",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "INACTIVE",\
                            "label": "Inactive",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-11-06T16:47:00+00:00",\
                "attribute": {\
                    "id": 2214,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "updated_at",\
                    "label": "Updated at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2021-10-20T18:06:09+00:00",\
                "attribute": {\
                    "id": 2215,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "created_at",\
                    "label": "Created at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2216,\
                    "type": "boolean",\
                    "field_type": "boolean",\
                    "name": "is_anonymous",\
                    "label": "Anonymous",\
                    "options": [\
                        {\
                            "value": "true",\
                            "label": "True",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "false",\
                            "label": "False",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "20",\
                "attribute": {\
                    "id": 2217,\
                    "type": "float",\
                    "field_type": "float",\
                    "name": "prepaid_balance",\
                    "label": "Prepaid balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",\
                "attribute": {\
                    "id": 2218,\
                    "type": "multi_select",\
                    "field_type": "multi_select",\
                    "name": "loyalty_associated_shops",\
                    "label": "Associated shops",\
                    "options": [\
                        {\
                            "value": "123123",\
                            "label": "Amsterdam Pop-up Store",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                        {\
                            "value": "12312312312312",\
                            "label": "Web Shop",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/webshop.svg"\
                            }\
                        },\
                        {\
                            "value": "2222e1242t3324535",\
                            "label": "Sesame Street",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1697",\
                "attribute": {\
                    "id": 2224,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_total_credits_received",\
                    "label": "Total credits received",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "1",\
                "attribute": {\
                    "id": 2225,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "tier",\
                    "label": "Tier",\
                    "options": [\
                        {\
                            "value": "1",\
                            "label": "Gold",\
                            "description": "VIP Members only",\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2226,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_3",\
                    "label": "Subscription status: Monthly offers",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2227,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_1",\
                    "label": "Subscription status: Marketing",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2228,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_5",\
                    "label": "Product updates",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2229,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_4",\
                    "label": "Subscription status: Events",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            }\
        ],
        "tier": {
            "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839",
            "name": "Gold",
            "description": "VIP Members only",
            "position": 1,
            "media": null
        },
        "subscriptions": [\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Marketing",\
                    "description": "Subscribe here for marketing purposes",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": "123123123",\
                    "id": 1\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Monthly offers",\
                    "description": "A monthly email blast with our best offers!",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": null,\
                    "id": 3\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Events",\
                    "description": "Stay in the loop for all our special events",\
                    "active": true,\
                    "strategy": "OPT_IN",\
                    "uuid": null,\
                    "id": 4\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Product updates",\
                    "description": "Never miss out on any of our newest features",\
                    "active": true,\
                    "strategy": "OPT_OUT",\
                    "uuid": null,\
                    "id": 5\
                }\
            }\
        ],
        "credit_balance": {
            "balance": 1612,
            "id": 2625740
        },
        "prepaid_balance": {
            "balance_in_cents": 2000
        },
        "current_values": {
            "age": null,
            "city": "Duckburg",
            "dier": null,
            "tier": "1",
            "uuid": "123",
            "email": "john@doe.com",
            "avatar": "",
            "locale": null,
            "status": "ACTIVE",
            "address": "",
            "channel": null,
            "lastname": "",
            "birthdate": null,
            "firstname": "",
            "created_at": "2021-10-20T18:06:09+00:00",
            "postalcode": "9999YZ",
            "updated_at": "2023-11-06T16:47:00+00:00",
            "housenumber": 12,
            "phonenumber": "",
            "is_anonymous": false,
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": null,
            "loyalty_balance": 1612,
            "prepaid_balance": 20,
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Ikh3ZW8yRW9pSU9Gc25SYmxRUFVVWUE9PSIsInZhbHVlIjoianhuYncxVHBlK0dqMTN4RHhXNzdZNVJ3aGJqcERFdmVWQlcxYjFNZVZYSVdoWVVMWk54K1UvQ2lFek1nMmtGaTJZNGRNOG1rUUxwRGNnU25JcitpN0d5RWVKN21kS1gvSmMvNTA2bmxqU0Z0cmxoR2pPT0Ewc1hXeEJsMkVJNGFDQlNlSWpHRXVpWndyd3g3eTVXck90Z0tlL3J2RHNRKzRjRkNGb0tmVmVzPSIsIm1hYyI6ImZlMzc0ZDYzZDVjNTNjOTc3YTc1ZTMxN2RmMzM4MDFkZWQ2YWI2MDkwNDY4OWQ1MDk0ZWRkN2Y2ZTdhZjY5ZmEiLCJ0YWciOiIifQ==",
            "custom_app_login_code": 198169,
            "subscription_status_1": "UNSUBSCRIBED",
            "subscription_status_3": "UNSUBSCRIBED",
            "subscription_status_4": "UNSUBSCRIBED",
            "subscription_status_5": "UNSUBSCRIBED",
            "loyalty_associated_shops": "123123;5fd3322e-671e-430f-aa38-5ed02e600ff7",
            "previous_loyalty_balance": 1617,
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06T11:32:00+00:00",
            "loyalty_first_transaction_date": "2023-01-09T15:03:37+00:00",
            "loyalty_number_of_transactions": 55,
            "loyalty_total_credits_received": 1697,
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
        },
        "current_display_values": {
            "age": "",
            "city": "",
            "dier": "False",
            "tier": "Gold",
            "uuid": "",
            "email": "john@doe.com",
            "avatar": "",
            "locale": "",
            "status": "Active",
            "address": "",
            "channel": "",
            "lastname": "",
            "birthdate": "",
            "firstname": "",
            "created_at": "2021-10-20, 20:06",
            "postalcode": "",
            "updated_at": "2023-11-06, 17:47",
            "housenumber": "",
            "phonenumber": "",
            "is_anonymous": "False",
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=123",
            "a_custom_attribute": "",
            "loyalty_balance": "1612",
            "prepaid_balance": "20",
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Imx6TzJVV1ZwNlBRd3VkQmV1NnhKbnc9PSIsInZhbHVlIjoiVnlKZWM4OG5Vdjl0QmNEY242YmZ3OFNpWjQwQXkwRThmMCttWHlwQmNSTFluNzdpVkQwY0ZEcVo5TktvUXRHSVBYUE43S2dscFpyMUJnazFrWWZWVHI3cDNLTVJsZDcwWU9zQWdIejh5YjRQOGx4RjV6aDJjR3FTWlI1RTNlQlRrZEVNcURZeFlaZlYvQ2R2STdSU1RDdFdZT0RRdkxNN01vOXhLQnhaZkNVPSIsIm1hYyI6ImJjMzczNWQ2NGEwNzZlNjZhZTU0OTk3MzIxOTczODYzZjMxZjYwNzM2MjY5ZWJkODRkMTEzODU0NmQxMDU4OTQiLCJ0YWciOiIifQ==",
            "custom_app_login_code": "198169",
            "subscription_status_1": "Unsubscribed",
            "subscription_status_3": "Unsubscribed",
            "subscription_status_4": "Unsubscribed",
            "subscription_status_5": "Unsubscribed",
            "loyalty_associated_shops": "Amsterdam Pop-up Store and Sesame Street",
            "previous_loyalty_balance": "1617",
            "default_contact_identifier": "hash123456",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/123",
            "loyalty_last_transaction_date": "2023-11-06, 12:32",
            "loyalty_first_transaction_date": "2023-01-09, 16:03",
            "loyalty_number_of_transactions": "55",
            "loyalty_total_credits_received": "1697",
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=123&subscription_type=3"
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

### Delete Contact

Deletes a Contact. This action is irreversible, the Contact's Credit Balance and any relation to other entities within the system will be removed, although the entities themselves may remain. When using 'DEFAULT' as the deletion type, a log is created containing detailed information. If 'GDPR' is used as the deletion type, any reference to personal data is removed.

**Note:** This API call schedules the removal of the Contact, this may take a few minutes.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}/delete`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID.

Body

`type` `string`

`REQUIRED`

The deletion type. Options are DEFAULT and GDPR.

Response Example

Show more

`1
2
3
4
` `{
    "data": null,
    "meta": {}
}`

Possible errors

Code

Message

`55031`

Contact not found.

### Claim Anonymous Contact

This function enables the conversion of an anonymous Contact (registered with a fictional email address) into a recognized Contact with a real email address. If a Contact already exists with the provided real email address, the anonymous and existing Contacts will be merged.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{anonymous_contact_uuid}}/claim`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`anonymous_contact_uuid` `string`

`OPTIONAL`

The anonymous Contact's UUID that contains a fictional email address

Body

`email` `string`

`OPTIONAL`

The real email address for the Contact that is about to be not anonymous anymore.

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
443
444
445
446
447
448
449
450
451
452
453
454
455
456
457
458
459
460
461
462
463
464
465
466
467
468
469
470
471
472
473
474
475
476
477
478
479
480
481
482
483
484
485
486
487
488
489
490
491
492
493
494
495
496
497
498
499
500
501
502
503
504
505
506
507
508
509
510
511
512
513
514
515
516
517
518
519
520
521
522
523
524
525
526
527
528
529
530
531
532
533
534
535
536
537
538
539
540
541
542
543
544
545
546
547
548
549
550
551
552
553
554
555
556
557
558
559
560
561
562
563
564
565
566
567
568
569
570
571
572
573
574
575
576
577
578
579
580
581
582
583
584
585
586
587
588
589
590
591
592
593
594
595
596
597
598
599
600
601
602
603
604
605
606
607
608
609
610
611
612
613
614
615
616
617
618
619
620
621
622
623
624
625
626
627
628
629
630
631
632
633
634
635
636
637
638
639
640
641
642
643
644
645
646
647
648
649
650
651
652
653
654
655
656
657
658
659
660
661
662
663
664
665
666
667
668
669
670
671
672
673
674
675
676
677
678
679
680
681
682
683
684
685
686
687
688
689
690
691
692
693
694
695
696
697
698
699
700
701
702
703
704
705
706
707
708
709
710
711
712
713
714
715
716
717
718
719
720
721
722
723
724
725
726
727
728
729
730
731
732
733
734
735
736
737
738
739
740
741
742
743
744
745
746
747
748
749
750
751
752
753
754
755
756
757
758
759
760
761
762
763
764
765
766
767
768
769
770
771
772
773
774
775
776
777
778
779
780
781
782
783
784
` `
    {
    "data": {
        "id": 45842,
        "uuid": "999",
        "email": "henkvandenvelden@hotmail.com",
        "attributes": [\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2204,\
                    "type": "media_upload",\
                    "field_type": "media_upload",\
                    "name": "avatar",\
                    "label": "Avatar",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2205,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "firstname",\
                    "label": "First name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2206,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "lastname",\
                    "label": "Last name",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "new_contac6@piggy.eu",\
                "attribute": {\
                    "id": 2203,\
                    "type": "email",\
                    "field_type": "email",\
                    "name": "email",\
                    "label": "Email",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "0",\
                "attribute": {\
                    "id": 2220,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "previous_loyalty_balance",\
                    "label": "Previous credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "0",\
                "attribute": {\
                    "id": 2219,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_balance",\
                    "label": "Credit balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "0",\
                "attribute": {\
                    "id": 2223,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_number_of_transactions",\
                    "label": "Number of transactions",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2221,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_first_transaction_date",\
                    "label": "First credit reception date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2222,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "loyalty_last_transaction_date",\
                    "label": "Last transaction date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2207,\
                    "type": "text",\
                    "field_type": "text",\
                    "name": "address",\
                    "label": "Address",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2208,\
                    "type": "phone",\
                    "field_type": "phone",\
                    "name": "phonenumber",\
                    "label": "Telephone number",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2209,\
                    "type": "birth_date",\
                    "field_type": "birth_date",\
                    "name": "birthdate",\
                    "label": "Birth date",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "OAUTH_API",\
                "attribute": {\
                    "id": 2210,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "channel",\
                    "label": "Channel",\
                    "options": [\
                        {\
                            "value": "BUSINESS_APP",\
                            "label": "BUSINESS_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "API",\
                            "label": "API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "BUSINESS_DASHBOARD",\
                            "label": "BUSINESS_DASHBOARD",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOMER_APP",\
                            "label": "CUSTOMER_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSHOP_API",\
                            "label": "WEBSHOP_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "IMPORT",\
                            "label": "IMPORT",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WEBSITE_FORM",\
                            "label": "WEBSITE_FORM",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "REGISTER_API",\
                            "label": "REGISTER_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "OAUTH_API",\
                            "label": "OAUTH_API",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIPTION_TYPE",\
                            "label": "SUBSCRIPTION_TYPE",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "CUSTOM_APP",\
                            "label": "CUSTOM_APP",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "AUTOMATIONS",\
                            "label": "AUTOMATIONS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "FORMS",\
                            "label": "FORMS",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "WIDGET",\
                            "label": "WIDGET",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2211,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "age",\
                    "label": "Age",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2212,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "locale",\
                    "label": "Preferred language",\
                    "options": [\
                        {\
                            "value": "en",\
                            "label": "English",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "nl",\
                            "label": "Nederlands",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "de",\
                            "label": "Deutsch",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "es",\
                            "label": "Español",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "fr",\
                            "label": "Francés",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": false,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "ACTIVE",\
                "attribute": {\
                    "id": 2213,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "status",\
                    "label": "Status",\
                    "options": [\
                        {\
                            "value": "ACTIVE",\
                            "label": "Active",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "INACTIVE",\
                            "label": "Inactive",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-11-01T15:05:00+00:00",\
                "attribute": {\
                    "id": 2214,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "updated_at",\
                    "label": "Updated at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "2023-11-01T14:59:14+00:00",\
                "attribute": {\
                    "id": 2215,\
                    "type": "date_time",\
                    "field_type": "date_time",\
                    "name": "created_at",\
                    "label": "Created at",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "1",\
                "attribute": {\
                    "id": 2216,\
                    "type": "boolean",\
                    "field_type": "boolean",\
                    "name": "is_anonymous",\
                    "label": "Anonymous",\
                    "options": [\
                        {\
                            "value": "true",\
                            "label": "True",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "false",\
                            "label": "False",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "0",\
                "attribute": {\
                    "id": 2217,\
                    "type": "float",\
                    "field_type": "float",\
                    "name": "prepaid_balance",\
                    "label": "Prepaid balance",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2218,\
                    "type": "multi_select",\
                    "field_type": "multi_select",\
                    "name": "loyalty_associated_shops",\
                    "label": "Associated shops",\
                    "options": [\
                        {\
                            "value": "123123",\
                            "label": "Amsterdam Pop-up Store",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                        {\
                            "value": "12312312312312",\
                            "label": "Web Shop",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/webshop.svg"\
                            }\
                        },\
                        {\
                            "value": "2222e1242t3324535",\
                            "label": "Sesame Street",\
                            "description": null,\
                            "media": {\
                                "type": "image",\
                                "value": "https://static.piggy.nl/business/profile_types/on_site.svg"\
                            }\
                        },\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "0",\
                "attribute": {\
                    "id": 2224,\
                    "type": "number",\
                    "field_type": "number",\
                    "name": "loyalty_total_credits_received",\
                    "label": "Total credits received",\
                    "options": [],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "",\
                "attribute": {\
                    "id": 2225,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "tier",\
                    "label": "Tier",\
                    "options": [\
                        {\
                            "value": "1",\
                            "label": "Gold",\
                            "description": "VIP Members only",\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": true\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2226,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_3",\
                    "label": "Subscription status: Monthly offers",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2227,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_1",\
                    "label": "Subscription status: Marketing",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2228,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_5",\
                    "label": "Product updates",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
            {\
                "value": "UNSUBSCRIBED",\
                "attribute": {\
                    "id": 2229,\
                    "type": "select",\
                    "field_type": "select",\
                    "name": "subscription_status_4",\
                    "label": "Subscription status: Events",\
                    "options": [\
                        {\
                            "value": "UNSUBSCRIBED",\
                            "label": "Unsubscribed",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "PENDING_CONFIRMATION",\
                            "label": "Pending confirmation",\
                            "description": null,\
                            "media": null\
                        },\
                        {\
                            "value": "SUBSCRIBED",\
                            "label": "Subscribed",\
                            "description": null,\
                            "media": null\
                        }\
                    ],\
                    "is_piggy_defined": true,\
                    "is_soft_read_only": true,\
                    "is_hard_read_only": false\
                }\
            },\
        ],
        "tier": null,
        "subscriptions": [\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Marketing",\
                    "description": "Subscribe here for marketing purposes",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": "123123123",\
                    "id": 1\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Monthly offers",\
                    "description": "A monthly email blast with our best offers!",\
                    "active": true,\
                    "strategy": "DOUBLE_OPT_IN",\
                    "uuid": null,\
                    "id": 3\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Events",\
                    "description": "Stay in the loop for all our special events",\
                    "active": true,\
                    "strategy": "OPT_IN",\
                    "uuid": null,\
                    "id": 4\
                }\
            },\
            {\
                "is_subscribed": false,\
                "status": "UNSUBSCRIBED",\
                "subscription_type": {\
                    "name": "Product updates",\
                    "description": "Never miss out on any of our newest features",\
                    "active": true,\
                    "strategy": "OPT_OUT",\
                    "uuid": null,\
                    "id": 5\
                }\
            }\
        ],
        "credit_balance": {
            "balance": 0,
            "id": 2625795
        },
        "prepaid_balance": {
            "balance_in_cents": 0
        },
        "current_values": {
            "age": null,
            "city": "Duckburg",
            "dier": "false",
            "tier": null,
            "uuid": "3a685add-1711-425d-9184-fc6db18e0e1d",
            "email": "new_contac6@piggy.eu",
            "avatar": "",
            "locale": null,
            "status": "ACTIVE",
            "address": "",
            "channel": "OAUTH_API",
            "lastname": "",
            "birthdate": null,
            "firstname": "",
            "created_at": "2023-11-01T14:59:14+00:00",
            "postalcode": "9999YZ",
            "updated_at": "2023-11-01T15:05:00+00:00",
            "housenumber": 12,
            "phonenumber": "",
            "is_anonymous": true,
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=3a685add-1711-425d-9184-fc6db18e0e1d",
            "a_custom_attribute": null,
            "loyalty_balance": 0,
            "prepaid_balance": 0,
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6Ikx2dDRqSTF4U3phV21PL1BIcG1STWc9PSIsInZhbHVlIjoiVnc0dUdhblAzZDdod29NQnhmVDlqbFdxYkF3S041WnhlN3hJQ3hrYklaa3BVVzhGbkIrM2R3citUWDVRQnRFL0IrSFJtY1JoQ09WRmR4N3hrZGFzdXBUL2pQVDF4aW9nWFEwQTFUcmliQk5EYXVBaEVpODZiMzh6Qm9Kb0E5QjZaOXM4bjQ3bFhIVEY3SVovRzRnejJZZlVYZUp0ZnA2RUNlUHVYYWlYNytucER5QndxYVNUSnRYVm9na1FrempOTDBmRFE0QkhJOFFIYTM2SnMvcTJ2QT09IiwibWFjIjoiN2JiZTQ5ZDVjMjgyMjY3NDkzYjE2ZTQ0ZTc5M2UyNjUyZWU3ODdiYmFlNzg2N2FjZjJjMDg3MDcxMGUzNTcxNSIsInRhZyI6IiJ9",
            "custom_app_login_code": 184014,
            "subscription_status_1": "UNSUBSCRIBED",
            "subscription_status_3": "UNSUBSCRIBED",
            "subscription_status_4": "UNSUBSCRIBED",
            "subscription_status_5": "UNSUBSCRIBED",
            "loyalty_associated_shops": "",
            "previous_loyalty_balance": 0,
            "default_contact_identifier": null,
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/3a685add-1711-425d-9184-fc6db18e0e1d",
            "loyalty_last_transaction_date": null,
            "loyalty_first_transaction_date": null,
            "loyalty_number_of_transactions": 0,
            "loyalty_total_credits_received": 0,
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=3a685add-1711-425d-9184-fc6db18e0e1d&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=3a685add-1711-425d-9184-fc6db18e0e1d&subscription_type=3"
        },
        "current_display_values": {
            "age": "",
            "city": "",
            "dier": "False",
            "tier": "",
            "uuid": "",
            "email": "new_contac6@piggy.eu",
            "avatar": "",
            "locale": "",
            "status": "Active",
            "address": "",
            "channel": "Oauth API",
            "lastname": "",
            "birthdate": "",
            "firstname": "",
            "created_at": "2023-11-01, 15:59",
            "postalcode": "",
            "updated_at": "2023-11-01, 16:05",
            "housenumber": "",
            "phonenumber": "",
            "is_anonymous": "True",
            "add_to_wallet": "https://piggy.eu/add-to-wallet/loyalty?uuid=3a685add-1711-425d-9184-fc6db18e0e1d",
            "a_custom_attribute": "",
            "loyalty_balance": "0",
            "prepaid_balance": "0",
            "custom_app_login_url": "https://piggy.eu/home?token=eyJpdiI6ImhWRlZWek8vUDBFWHpGUFRqaXVuQ1E9PSIsInZhbHVlIjoiRFU4WHF4bitIOFVuMHZvdEdFcjZYSlJOZFFxMHJ6bVpNVW4yZVRCZktna3ZXTExZb3d5RnRYNEVIU2FWM3RrMThOamZlUWRXRTdhdCtxazgvQjRyVEJrREwrNDFsSkhpUERWTTJLTTRkbDMrd1kzM3d5KzdLMXMvbVFZcGVFaHFSR0p4aG5WMUJlRHg0UFB6NllINTdYMURhMzMxY1FuVVZZeGdrcmRGaWIrQytxdXBveVZHampsbUdpUlowWktDbEpzbFlHTzBQSWFyWHo0N2ErbWFzZz09IiwibWFjIjoiOTQ4ZmExZGM2YWU4NTAwYjIxZTY4ZmI5YzhhZDFiYTQzMDQ5ZTdmNWNlNGQyNmU3Yjk3MmZkNDI5ZjJmYzE3NSIsInRhZyI6IiJ9",
            "custom_app_login_code": "184014",
            "subscription_status_1": "Unsubscribed",
            "subscription_status_3": "Unsubscribed",
            "subscription_status_4": "Unsubscribed",
            "subscription_status_5": "Unsubscribed",
            "loyalty_associated_shops": "",
            "previous_loyalty_balance": "0",
            "default_contact_identifier": "",
            "subscription_preferences_url": "http://piggy.eu/subscription-preferences/3a685add-1711-425d-9184-fc6db18e0e1d",
            "loyalty_last_transaction_date": "",
            "loyalty_first_transaction_date": "",
            "loyalty_number_of_transactions": "0",
            "loyalty_total_credits_received": "0",
            "double_opt_in_confirmation_url_1": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=3a685add-1711-425d-9184-fc6db18e0e1d&subscription_type=1",
            "double_opt_in_confirmation_url_3": "http://piggy.eu/double-opt-in-confirmation?contact_uuid=3a685add-1711-425d-9184-fc6db18e0e1d&subscription_type=3"
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

`60016`

Contact not anonymous.

### Merge Contact

WARNING: Use at your own risk and with consent from your clients. Before using, **ensure both email addresses belong to the same person**.

Merges a 'source' Contact into a 'destination' Contact. The 'destination' Contact will be the resulting Contact, whilst the 'source' Contact will be removed. **Note:** This cannot be undone, so use with care. Only use after verifying the person in question has access to both email addresses.

**Note:** This API call schedules the merge of the Contacts, this may take a few minutes.

POST

`https://api.piggy.eu/api/v3/oauth/clients/contacts/merge`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`source_contact_uuid` `string`

`REQUIRED`

The UUID of the Source Contact which will cease to exist.

`destination_contact_uuid` `string`

`REQUIRED`

The UUID of the Destination Contact which remain.

Possible errors

Code

Message

`55031`

Contact not found.

### Get Contact Identifiers

Use this endpoint to retrieve a Contact's Contact Identifiers.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}/contact-identifiers`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID from whom you want to retrieve the Contact Identifiers.

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
` `{
    "data": {
        [\
            "name": "Main Loyalty Card",\
            "value": "ABCDE12356-90",\
            "active": true,\
            "default": true,\
            "contact": {\
                "uuid": "123abc-def789-qwerty-34567",\
            }\
        ],
        [\
            "name": "Fallback Card",\
            "value": "QWERTY-7890",\
            "active": true,\
            "default": false,\
            "contact": {\
                "uuid": "123abc-def789-qwerty-34567",\
            }\
        ],
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

### Get Credit Balance

Retrieve the current Credit Balance of a specific Contact, identified through their UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}/credit-balance`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID

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
        "balance": 49
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

### Get Prepaid Balance

Use this function to retrieve the Prepaid Balance of a particular Contact, identified by their UUID.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}/prepaid-balance`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID

Response Example

Show more

`1
2
3
4
5
6
7
` `{
    "data": {
        "balance_in_cents": 5000,
        "balance": "€50.00"
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

### Get Tier

Use this endpoint to retrieve the current loyalty Tier of a specific Contact. It’s useful for understanding a Contact's loyalty status and offering them tailored Rewards based on their Tier.

GET

`https://api.piggy.eu/api/v3/oauth/clients/contacts/{{contact_uuid}}/tier`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`contact_uuid` `string`

`REQUIRED`

The Contact's UUID from whom you want to retrieve the Tier.

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
` `{
    "data": {
        "name": "Gold",
        "description": "VIP Members only",
        "position": 1,
        "media": null,
        "uuid": "ea77edd4-5a5e-4a6e-aeda-55038c43c839"
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

### Related

[**Contact Identifiers** \\
Contacts can also be found using Contact Identifiers](https://docs.piggy.eu/v3/oauth/contact-identifiers) [**Credit Receptions** \\
The issuing of credits is done using so-called Credit Receptions](https://docs.piggy.eu/v3/oauth/credit-receptions)
