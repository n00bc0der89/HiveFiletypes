# HiveFiletypes

Welcome to the HiveFiletypes wiki!

- How and when to use Lateral View Explode in Hive?

In case the Hive table is storing array of elements mapped to a single record, we can use lateral view explode to break this single record into each seperate records.

Eg:
Sample data in HDFS :
-----------------------------------
Amar|BE,SSC,HSC,MBA
Akbar|BSC,MSC,SSC,HSC,DIP
Anthony|SSC
Sonal|SSC,HSC,BCOM
--------------------------------------
Create External Hive table  :
--------------------------------------
CREATE EXTERNAL TABLE test_degree(name string,degrees ARRAY<string>) ROW FORMAT DELIMITED FIELDS TERMINATED BY '|' COLLECTION ITEMS TERMINATED 
BY ',' LOCATION '/tmp/hive/root/testtable/'
-------------------------------------

Normal query of hive table :
-------------------------------
select * from test_degree;

Result -
hive> select * from test_degree;                                                                                                                     
OK
Amar    ["BE","SSC","HSC","MBA"]
Akbar   ["BSC","MSC","SSC","HSC","DIP"]
Anthony ["SSC"]
Sonal   ["SSC","HSC","BCOM"]
Time taken: 0.546 seconds, Fetched: 4 row(s)

--------------------------------------------
Using Lateral view explode -

select name,degree from test_degree lateral view explode(degrees) de as degree;

It runs mapper job to produce the result -

OK
Amar    BE
Amar    SSC
Amar    HSC
Amar    MBA
Akbar   BSC
Akbar   MSC
Akbar   SSC
Akbar   HSC
Akbar   DIP
Anthony SSC
Sonal   SSC
Sonal   HSC
Sonal   BCOM
Time taken: 13.673 seconds, Fetched: 13 row(s)

------------------------------------------------------------------------------

Work with json raw files using Hive -
http://thornydev.blogspot.in/2013/07/querying-json-records-via-hive.html

Create hive schema for json input using this tool - https://github.com/quux00/hive-json-schema

Use this serde - https://github.com/rcongiu/Hive-JSON-Serde
