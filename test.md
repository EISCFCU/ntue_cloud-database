

```
CREATE EXTERNAL TABLE `aws-rekognition`(
  `agelow` int, 
  `agehigh` int, 
  `emot1-value` string, 
  `emot1-cofindence` float, 
  `emot2-value` string, 
  `emot2-cofindence` float, 
  `gender` string, 
  `gender-cofindence` float, 
  `smile` boolean, 
  `smile-cofindence` float, 
  `eyeglasses` boolean, 
  `eyeglasses-cofindence` float, 
  `beard` boolean, 
  `beard-cofindence` string, 
  `mustache` boolean, 
  `mustache-cofindence` float, 
  `uploadtime` timestamp)
ROW FORMAT DELIMITED 
  FIELDS TERMINATED BY '\;' 
STORED AS INPUTFORMAT 
  'org.apache.hadoop.mapred.TextInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  's3://fcu-rekognition/txt'
TBLPROPERTIES (
  'has_encrypted_data'='false', 
  'transient_lastDdlTime'='1637836051')
```
