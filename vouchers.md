### List Vouchers

Retrieve a paginated list of all Vouchers for an Account, with the option to filter by Promotion, Contact, Voucher status, and more.

GET

`https://api.piggy.eu/api/v3/oauth/clients/vouchers`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`promotion_uuid` `string`

`OPTIONAL`

UUID of the Promotion for which the Vouchers are to be retrieved.

`contact_uuid` `string`

`OPTIONAL`

UUID of the Contact for which the Vouchers are to be retrieved.

`status` `string`

`OPTIONAL`

Status of the Vouchers, options: ACTIVE, REDEEMED, EXPIRED, DEACTIVATED, INACTIVE and LOCKED.

`limit` `number`

`OPTIONAL`

Number of Vouchers to retrieve (min: 0, max: 500, default: 30).

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
` `{
    "data": [\
        {\
            "uuid": "57d93a44-1c9d-4548-8c0e-e6cd71d6170a",\
            "code": "1",\
            "status": "DEACTIVATED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": null,\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "96ab0da0-f502-4e6d-9b7d-c5b0f5c18795",\
            "code": "2",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "eac16058-d08e-44f3-8bb3-d42afef192cfs",\
                "email": "wymvxnnyyrt_113@ziggy.nl"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "1390b1e6-ac11-413e-8d20-143640fab2f4",\
            "code": "3",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "c4fa414b-b67c-415c-a772-b4a53608754d",\
            "code": "4",\
            "status": "DEACTIVATED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "1d4ad8a5-1b71-4ccd-8834-7891a32dbbf4",\
                "email": "ldzrks000_114@ziggy.nl"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "fcf42c74-00c5-4822-a646-0408ae7cc3a1",\
            "code": "5",\
            "status": "REDEEMED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": "2023-04-04T08:33:49+00:00",\
            "is_redeemed": true,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "6d6b545d-eaf3-430b-b3a0-86c86ba90532",\
                "email": ""\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "0d7d667a-9a8b-4954-a357-82384a5b7268",\
            "code": "6",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "f0da8c2b-cead-42be-b0ae-dc5b08e93824",\
            "code": "7",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "e07200c1-4f1a-4093-a727-293df2446c52",\
            "code": "8",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "b4c8f3a3-5fce-4b7d-9757-8c41f79b615f",\
            "code": "9",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "29a6673e-21d6-4302-b1ae-1d138cec5f03",\
            "code": "10",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-02T14:45:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "f2545dca-7e24-4f3d-9069-5b6bf3e45781",\
            "code": "11",\
            "status": "EXPIRED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": "2023-03-08T11:18:12+00:00",\
            "activation_date": "2023-03-08T09:52:11+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": null,\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "64a72382-cecd-460f-91ab-dadb07a9eae7",\
            "code": "12",\
            "status": "EXPIRED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": "2023-03-08T11:18:17+00:00",\
            "activation_date": "2023-03-08T09:52:11+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": null,\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "4f49eae2-ec29-40d0-9002-d1074fa3cfa4",\
            "code": "13",\
            "status": "REDEEMED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-08T09:52:11+00:00",\
            "redeemed_at": "2023-03-08T11:09:39+00:00",\
            "is_redeemed": true,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": null,\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "ea4dee1b-2025-45d2-a7fc-3a7265bfdcf5",\
            "code": "14",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-08T09:52:11+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "96ab0da0-f502-4e6d-9b7d-c5b0f5c18794",\
            "code": "15",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-08T09:52:11+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "efac7044-9ce9-4c7c-b0a7-52625e23bba3",\
            "code": "16",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:05:30+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "946c45e2-e00c-428c-8590-2e800e1c3c36",\
            "code": "17",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:05:33+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "b5d3179b-1eef-4253-b70a-718fa9b26021",\
            "code": "18",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:05:35+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "0f340d6c-8200-4ddd-b6e1-f4363e9ac197",\
            "code": "19",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:05:37+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "da1b4070-377d-4815-9ea3-dc358a48b069",\
            "code": "20",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:05:38+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "e2d04525-02ea-4d8a-b913-b25d5485e844",\
            "code": "21",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:06:49+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "e57ed94e-317c-41f4-adb4-aa863f5805e9",\
            "code": "22",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:06:51+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "1e567029-3a4d-4b76-b375-400e7d32fd83",\
            "code": "23",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:06:52+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "6bf6c3ae-351b-410a-b2a5-4cb1254eea44",\
            "code": "24",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:06:53+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "389c78b1-4c96-4acc-a45e-0b8bf173d034",\
            "code": "25",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:06:53+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "399ebf3b-d1b5-4c22-b190-e0ccc034a640",\
            "code": "26",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:10:44+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "ad98547d-c537-4346-8f30-296041ad9a8f",\
            "code": "27",\
            "status": "ACTIVE",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:10:45+00:00",\
            "redeemed_at": null,\
            "is_redeemed": false,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "9cb37162-647f-410b-ac4d-a645781bbf34",\
            "code": "28",\
            "status": "REDEEMED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:10:51+00:00",\
            "redeemed_at": "2023-04-03T15:45:56+00:00",\
            "is_redeemed": true,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "2f3d0236-c466-4ab2-97c7-176dafa36edc",\
            "code": "29",\
            "status": "REDEEMED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:10:52+00:00",\
            "redeemed_at": "2023-04-03T15:47:04+00:00",\
            "is_redeemed": true,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        },\
        {\
            "uuid": "86dc3c01-c6ab-47d3-a765-a8569cf02bda",\
            "code": "30",\
            "status": "REDEEMED",\
            "name": "Gratis Pizza",\
            "description": "Lekker gratis pizza",\
            "expiration_date": null,\
            "activation_date": "2023-03-16T16:11:05+00:00",\
            "redeemed_at": "2023-04-03T15:43:42+00:00",\
            "is_redeemed": true,\
            "attributes": {\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null,\
                "rens_123": null,\
                "somename": null\
            },\
            "contact": {\
                "uuid": "9504d4b2-03c5-41d8-8423-4b1821b6aa00",\
                "email": "john@doe.com"\
            },\
            "promotion": {\
                "uuid": "123",\
                "name": "Gratis Pizza",\
                "description": "Lekker gratis pizza",\
                "voucher_limit": 25012,\
                "limit_per_contact": null,\
                "expiration_duration": null,\
                "shipping": null,\
                "shipping_region": null,\
                "some_random_attribute_3": null\
            }\
        }\
    ],
    "links": {
        "first": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=1",
        "last": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=9",
        "prev": null,
        "next": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=2"
    },
    "meta": {
        "current_page": 1,
        "from": 1,
        "last_page": 9,
        "links": [\
            {\
                "url": null,\
                "label": "pagination.previous",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=1",\
                "label": "1",\
                "active": true\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=2",\
                "label": "2",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=3",\
                "label": "3",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=4",\
                "label": "4",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=5",\
                "label": "5",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=6",\
                "label": "6",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=7",\
                "label": "7",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=8",\
                "label": "8",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=9",\
                "label": "9",\
                "active": false\
            },\
            {\
                "url": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers?page=2",\
                "label": "pagination.next",\
                "active": false\
            }\
        ],
        "path": "http://127.0.0.1:8001/api/v3/oauth/clients/vouchers",
        "per_page": "30",
        "to": 30,
        "total": 241
    }
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80002`

Promotion not found.

`55031`

Contact not found.

### Create Voucher

Creates a new Voucher for a Promotion. Only the `promotion_uuid` is required. A specific code can be given to be used, or else a code will be generated by the system. Similarly, if no `activation_date` and/or `expiration_date` is given, the system will calculate those dates by the validity rules set up for the promotion. It is recommended to send in the expiration and activation date **only** if business logic demands its, and the validity logic in the Piggy system is unable to handle that logic. **Note:** If you do send in activation- or expiration dates, keep in mind they need to be in ISO 8601 format and will be stored in UTC time. If the date-time you're sending in is not already in UTC time, please either parse it to UTC beforehand, or pass along the timezone.

POST

`https://api.piggy.eu/api/v3/oauth/clients/vouchers`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`promotion_uuid` `string`

`REQUIRED`

UUID of the Promotion for which the Voucher is to be created.

`code` `string`

`OPTIONAL`

The unique code of the Voucher which will serve as its identifier. If not given, a random code will be automatically generated.

`contact_uuid` `string`

`OPTIONAL`

The Contact's UUID to be linked to this Voucher. If not given, the recipient will need to identify themselves to use the Voucher.

`expiration_date` `string`

`OPTIONAL`

The Vouchers expiration date in ISO 8601 format. Dates are stored in UTC time. If not given, Promotion validity rules apply.

`activation_date` `string`

`OPTIONAL`

The Vouchers activation date in ISO 8601 format. Dates are stored in UTC time. If not given, Promotion validity rules apply.

`total_redemptions_allowed` `integer`

`OPTIONAL`

Number of times this specific Voucher can be redeemed. If not given, Promotion type and redemptions per voucher will apply.

`custom_attributes` `array`

`OPTIONAL`

Additional Custom Attributes for the created Voucher.

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
    "data": {
        "uuid": "62f6dc8c-4ff1-4c42-99ba-713c68ed10a0",
        "code": "EXAMPLE-CODE-03785",
        "status": "ACTIVE",
        "name": "Gratis Pizza",
        "description": "Lekker gratis pizza",
        "expiration_date": "2023-12-12T10:00:00+00:00",
        "activation_date": "2023-12-10T10:00:00+00:00",
        "redeemed_at": null,
        "is_redeemed": false,
        "attributes": {
            "joker": null,
            "joker2": null,
            "jokertje1233": null,
            "someName2": "meer"
        },
        "contact": {
            "uuid": "123",
            "email": "john@doe.com"
        },
        "promotion": {
            "uuid": "123",
            "name": "Gratis Pizza",
            "description": "Lekker gratis pizza",
            "voucher_limit": 25012,
            "limit_per_contact": null,
            "expiration_duration": null,
            "joker": null,
            "joker2": null,
            "jokertje1233": null,
            "someName2": "meer"
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80002`

Promotion not found.

`80003`

Voucher already exists.

`80008`

Voucher limit reached.

`55031`

Contact not found.

### Create Batch of Vouchers

Create multiple Vouchers at once for a specific Promotion. Specify the quantity and, optionally, a Contact UUID, `activation_date` and/or `expiration_date`. The system will generate a batch of Vouchers according to the Promotion's rules.

POST

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/batch`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`promotion_uuid` `string`

`REQUIRED`

UUID of the Promotion for which the Vouchers are to be created.

`quantity` `integer`

`REQUIRED`

The number of Vouchers you want to generate (min: 1, max: 500)."

`contact_uuid` `string`

`OPTIONAL`

The Contact's UUID to be linked to the batch of Vouchers.

`expiration_date` `string`

`OPTIONAL`

The expiration date for the batch of Vouchers in ISO 8601 format. Dates are stored in UTC time. If not given, promotion validity rules apply.

`activation_date` `string`

`OPTIONAL`

The activation date for the batch of Vouchers in ISO 8601 format. Dates are stored in UTC time. If not given, promotion validity rules apply.

`custom_attributes` `array`

`OPTIONAL`

Additional Custom Attributes for the Vouchers.

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
        "uuid": "bfada889-6a69-4fcf-aed6-42933ae9084d",
        "type": "vouchers",
        "quantity": 30,
        "status": "PENDING",
        "created_at": "2023-11-27, 17:59"
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80002`

Promotion not found.

`80008`

Voucher limit reached.

`55031`

Contact not found.

### Find Voucher

Finds a Voucher by its unique code. This call will help you with identifying individual Vouchers and viewing their specifications like whether the Voucher is redeemed or not or when it expires.

GET

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/find`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`code` `string`

`REQUIRED`

The unique code of the Voucher, which serves as its identifier.

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
    "data": {
        "uuid": "62f6dc8c-4ff1-4c42-99ba-713c68ed10a0",
        "code": "EXAMPLE-CODE-03785",
        "status": "REDEEMED",
        "name": "Gratis Pizza",
        "description": "Lekker gratis pizza",
        "expiration_date": "2023-12-12T10:00:00+00:00",
        "activation_date": "2023-12-10T10:00:00+00:00",
        "redeemed_at": "2023-11-13T08:51:00+00:00",
        "is_redeemed": true,
        "attributes": {
            "joker": null,
            "joker2": null,
            "jokertje1233": null,
            "someName2": "meer"
        },
        "contact": {
            "uuid": "123",
            "email": "john@doe.com"
        },
        "promotion": {
            "uuid": "123",
            "name": "Gratis Pizza",
            "description": "Lekker gratis pizza",
            "voucher_limit": 25012,
            "limit_per_contact": null,
            "expiration_duration": null,
            "joker": null,
            "joker2": null,
            "jokertje1233": null,
            "someName2": "meer"
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80004`

Voucher not found.

### Update Voucher

Update the status of a Voucher. This endpoint allows you to activate or deactivate a Voucher, changing its usability status as needed.

PUT

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/update/{{voucher_uuid}}`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`voucher_uuid` `string`

`REQUIRED`

The UUID of the Voucher that you want to update.

Body

`status` `string`

`REQUIRED`

Can be either ACTIVE or DEACTIVATED.

`custom_attributes` `array`

`OPTIONAL`

Additional Custom Attributes as a key-value array.

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
` `{
    "data": {
        "uuid": "efcf5588-0329-4e87-927b-df10100e13a5",
        "code": "VQHJSVSCR",
        "status": "DEACTIVATED",
        "name": "Gratis Pizza",
        "description": "Lekker gratis pizza",
        "expiration_date": null,
        "activation_date": "2023-11-03T08:02:32+00:00",
        "redeemed_at": null,
        "is_redeemed": false,
        "attributes": {
            "joker": null,
            "joker2": null,
            "jokertje1233": null,
            "someName2": "meer"
        },
        "contact": null,
        "promotion": {
            "uuid": "123",
            "name": "Gratis Pizza",
            "description": "Lekker gratis pizza",
            "voucher_limit": 25012,
            "limit_per_contact": null,
            "expiration_duration": null,
            "joker": null,
            "joker2": null,
            "jokertje1233": null,
            "someName2": "meer"
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80004`

Voucher not found.

### Lock Voucher (Optional)

Using the lock functionality, you can make sure that a Voucher cannot be used by another process at the same time as well as return the Voucher to its ACTIVE state when a failure occurs on your side (using the Release call). Using the locking method, you can make sure the Voucher only reaches a REDEEMED state when everything completed successful on your side.

When locking a Voucher, a release key is returned which should then be used to either redeem the Voucher later on, or to restore the Voucher to its ACTIVE state using the release call.

If the lock is not released within an hour, the lock will be released by the system automatically and the Voucher will be restored to its ACTIVE state.

POST

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/{{voucher_uuid}}/lock`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`voucher_uuid` `string`

`REQUIRED`

The UUID of the Voucher that you want to lock.

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
` `{
    "data": {
        "voucher": {
            "uuid": "11fe9574-800a-4832-ae64-e90d0192ca9a",
            "status": "LOCKED"
        },
        "lock": {
            "release_key": "5cc7e94a-7aa2-4f4a-8a8c-c20ab3a086ce",
            "locked_at": "2023-11-13T09:01:46+00:00",
            "unlocked_at": null,
            "system_release_at": "2023-11-13T10:01:46+00:00"
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80004`

Voucher not found.

`80006`

Voucher has expired.

`80005`

Voucher code as already been redeemed.

`80012`

Voucher is deactivated.

`80017`

Voucher is already locked.

### Release Voucher (Optional)

When using Voucher locking, there is also the process of releasing the Voucher. Using the call you can release the Voucher from its lock using the release key that was retrieved during the initial lock.

**Extra information:**

When redeeming a Voucher that was previously locked, the system will release the lock automatically (you need to provide the release key as well). However, if your system wants to release the Voucher manually you can do so using this call. If the release is not done within the hour, the system will release it automatically.

POST

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/{{voucher_uuid}}/release`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`voucher_uuid` `string`

`REQUIRED`

The UUID of the Voucher that you want to release.

Body

`release_key` `string`

`REQUIRED`

The release key of the Voucher that was given upon locking it.

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
` `{
    "data": {
        "voucher": {
            "uuid": "11fe9574-800a-4832-ae64-e90d0192ca9a",
            "status": "ACTIVE"
        },
        "lock": {
            "release_key": "5cc7e94a-7aa2-4f4a-8a8c-c20ab3a086ce",
            "locked_at": "2023-11-13T09:01:46+00:00",
            "unlocked_at": "2023-11-13T09:03:32+00:00",
            "system_release_at": null
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80004`

Voucher not found.

`80006`

Voucher has expired.

`80005`

Voucher code as already been redeemed.

### Redeem Voucher

Redeems a Voucher for a Contact. If the Voucher is already linked to a Contact – because it was sent in an automated email for instance – the call only requires the Voucher's code for redeeming it.

If the Voucher isn't yet linked to a Contact – because a series of Vouchers were printed as flyers to be given away for instance – then the `contact_uuid` is required as well as the Voucher code.

Please note that when using locked Vouchers, you should send in your `release_key` here as well.

POST

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/redeem`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`code` `string`

`REQUIRED`

The unique code of the Voucher which will serve as its identifier

`contact_uuid` `string`

`REQUIRED_IF`

Not required if Contact is already assigned to Voucher, but could serve as identity check if given nevertheless. In case no Contact is assigned to the Voucher, the contact\_uuid must be supplied.

`release_key` `string`

`OPTIONAL`

The release key that was returned when locking the voucher (Only applicable if locking is used).

`shop_uuid` `string`

`OPTIONAL`

UUID of the Shop through which the voucher is redeemed.

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
` `{
    "data": {
        "uuid": "62f6dc8c-4ff1-4c42-99ba-713c68ed10a0",
        "code": "EXAMPLE-CODE-03785",
        "status": "REDEEMED",
        "name": "Gratis Pizza",
        "description": "Lekker gratis pizza",
        "expiration_date": "2023-12-12T10:00:00+00:00",
        "activation_date": "2023-12-10T10:00:00+00:00",
        "redeemed_at": "2023-11-13T08:51:00+00:00",
        "is_redeemed": true,
        "attributes": {
            "plu": "ABC12"
        },
        "contact": {
            "uuid": "123",
            "email": "john@doe.com"
        },
        "promotion": {
            "uuid": "123",
           "name": "Free Pizza",
            "description": "Choice of Margherita or Piccanto",
            "voucher_limit": 25012,
            "limit_per_contact": null,
            "expiration_duration": null,
            "plu": "ABC12"
        }
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80004`

Voucher not found.

`80006`

Voucher has expired.

`80005`

Voucher code as already been redeemed.

`80012`

Voucher is deactivated.

`80014`

Voucher is inactive.

### Reverse Voucher Redemption

Reverses a Voucher Redemption.

POST

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/{{uuid}}/reverse`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Params

`uuid` `string`

`REQUIRED`

The UUID of the Voucher for which the redemption is to be reversed

Possible errors

Code

Message

`1003`

Invalid input.

`80004`

Voucher not found.

### Multi Redeem Voucher

Redeems a Voucher multiple times at once for a Contact.

If the Voucher isn't yet linked to a Contact – because a series of Vouchers were printed as flyers to be given away for instance – then the `contact_uuid` is required as well as the Voucher code.

Please note that when using locked Vouchers, you should send in your `release_key` here as well.

POST

`https://api.piggy.eu/api/v3/oauth/clients/vouchers/multi-redeem`

Headers

Authorization

`Bearer {{ access_token | api_key }}`

Accept

`application/json`

Body

`code` `string`

`REQUIRED`

The unique code of the Voucher which will serve as its identifier

`number_of_times` `integer`

`REQUIRED`

Number of times the Voucher is to be redeemed (min: 1, max: 50). Note: Only multi-use Vouchers can be used here.

`contact_uuid` `string`

`REQUIRED_IF`

Not required if Contact is already assigned to Voucher, but could serve as identity check if given nevertheless. In case no Contact is assigned to the Voucher, the contact\_uuid must be supplied.

`shop_uuid` `string`

`REQUIRED`

The Shop or Business Profile where the Voucher is redeemed

`release_key` `string`

`OPTIONAL`

The release key that was returned when locking the voucher (Only applicable if locking is used).

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
` `{
    "data": {
        "voucher: {
            "uuid": "62f6dc8c-4ff1-4c42-99ba-713c68ed10a0",
            "code": "EXAMPLE-CODE-03785",
            "status": "REDEEMED",
            "name": "Free Pizza",
            "description": "Choice of Margherita or Piccanto",
            "expiration_date": "2023-12-12T10:00:00+00:00",
            "activation_date": "2023-12-10T10:00:00+00:00",
            "redeemed_at": "2023-11-13T08:51:00+00:00",
            "is_redeemed": true,
            "attributes": {
                "plu": "ABC12"
            },
            "contact": {
                "uuid": "123",
                "email": "john@doe.com"
            },
            "promotion": {
                "uuid": "123",
                "name": "Free Pizza",
                "description": "Choice of Margherita or Piccanto",
                "voucher_limit": 1000,
                "limit_per_contact": null,
                "expiration_duration": null,
                "plu": "ABC12"
            }
        },
        "voucher_redemptions": [\
            {\
                "uuid": "abc123-890asd-d8a6s7e",\
                "created_at": "2023-11-13T08:51:00+00:00"\
            },\
            {\
                "uuid": "789bacs-asd1234-d8a6s7e",\
                "created_at": "2023-11-13T08:51:00+00:00"\
            },\
            {\
                "uuid": "fsa12aa-g332as-xv234a",\
                "created_at": "2023-11-13T08:51:00+00:00"\
            }\
        ]
    },
    "meta": []
}`

Possible errors

Code

Message

`1003`

Invalid input.

`80004`

Voucher not found.

`80006`

Voucher has expired.

`80005`

Voucher code as already been redeemed.

`80012`

Voucher is deactivated.

`80014`

Voucher is inactive.

`80019`

Voucher doesn't have enough redemptions left.

`80020`

Promotion is inactive.

### Related

[**Vouchers** \\
Manage the Promotions for which the Vouchers are created](https://docs.piggy.eu/v3/oauth/promotions) [**Promotion Attributes** \\
Customize your Promotions and fully integrate with your software](https://docs.piggy.eu/v3/oauth/promotion-attributes)
