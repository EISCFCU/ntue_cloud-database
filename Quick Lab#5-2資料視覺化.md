# 資料視覺化

# 使用平台：AWS Academy Learner Lab-Foundation Services / Standard Account

課程代號：18579(只要是Learner Lab皆可學習本實作內容)

# 實施架構

![image](https://user-images.githubusercontent.com/103306835/170396281-af850cc4-ced2-4ced-858d-349ed16fbead.png)


# 操作步驟

![image](https://user-images.githubusercontent.com/103306835/170396339-14d4fb8a-87c3-44db-b18e-7adcbc5db57f.png)

# 步驟1：新增S3儲存桶

1.下載Github資料並解壓縮

![image](https://user-images.githubusercontent.com/103306835/170396687-242b52e8-fe09-439c-a28c-0cf59273ac60.png)

2.登入AWS Management Console

![image](https://user-images.githubusercontent.com/103306835/170396717-d0bd5438-29fc-4434-ad2b-a78324922817.png)

3.點選[S3]

![image](https://user-images.githubusercontent.com/103306835/170396744-85e97460-19ac-4ffe-bbb0-99a28a6d5f04.png)

4.點選[Create bucket]

![image](https://user-images.githubusercontent.com/103306835/170396776-77348739-1586-4c76-a0d4-8e2e74114d6e.png)

5.輸入Bucket Name(唯一；名稱自訂)

![image](https://user-images.githubusercontent.com/103306835/170396802-24d1058d-37e1-4181-8764-fd55af324368.png)

6.輸入AWS Region(預設：us-east-1)

![image](https://user-images.githubusercontent.com/103306835/170396836-f0585a65-f8b6-4663-8248-40355a73d237.png)

7.點選[Create Bucket]

![image](https://user-images.githubusercontent.com/103306835/170396863-f75e5030-f5af-4ceb-83eb-827d6d6cd163.png)

8.完成名為fcu-20210302(名稱自訂)的Bucket新增

![image](https://user-images.githubusercontent.com/103306835/170396892-298a0855-2521-4070-9d08-4239a5390082.png)

# 步驟2：上傳資料到S3

1.點選fcu-20210505(名稱自訂)的Bucket name

![image](https://user-images.githubusercontent.com/103306835/170396946-3ce1f81a-6b95-4bc9-8842-aa5e6b11bcfe.png)

2.點選[Upload]

![image](https://user-images.githubusercontent.com/103306835/170396972-4748f130-273c-489d-9ef5-b951d40f0c2d.png)

3.點選[Add folder]

![image](https://user-images.githubusercontent.com/103306835/170397010-ea03044d-e4a4-4960-8c5d-bca98e9f0290.png)

4.點選[上傳]

![image](https://user-images.githubusercontent.com/103306835/170397061-f4768472-5591-41c6-8b76-e289a6fd479c.png)

5.點選上傳

![image](https://user-images.githubusercontent.com/103306835/170397091-0575d53d-a51c-4e17-a138-7395e0d7b1cb.png)

6.點選[Upload]上傳

![image](https://user-images.githubusercontent.com/103306835/170397119-8a9970d6-5ca4-4524-96d6-03fee9f849b8.png)

7.等待資料上傳

![image](https://user-images.githubusercontent.com/103306835/170397173-46099ab9-9e17-43be-9dd7-72b2e4d4d85f.png)

8.資料上傳成功

![image](https://user-images.githubusercontent.com/103306835/170397197-c2f126d5-a698-4407-a796-edc0a1d89e85.png)

9.記錄S3 Destination(步驟3會用到)

![image](https://user-images.githubusercontent.com/103306835/170397228-932a65e2-2bb6-45a8-a626-8c9c2acc7895.png)

10.點選[Exit]

![image](https://user-images.githubusercontent.com/103306835/170397265-2be79836-807b-4f6b-a93c-a90d120fadbd.png)

# 步驟3：建立Athena資料定義

1.搜尋Athena 並點選

![image](https://user-images.githubusercontent.com/103306835/170397362-ad0fd05d-fec2-47b9-9bbd-35c4765466c1.png)

2.點選[Setting] 設定產出路徑與資料夾

![image](https://user-images.githubusercontent.com/103306835/170397383-21133ded-4ad5-412c-bb82-b4167ae6ef27.png)

3.點選[Save]

![image](https://user-images.githubusercontent.com/103306835/170397413-57d3cef2-fbdb-4dc7-8b9f-2c440f6ef997.png)

4.選擇Database:sampledb

![image](https://user-images.githubusercontent.com/103306835/170397429-b51e9ae2-40ab-4f7d-ae67-a33f989c6be9.png)

5.複製 SQL語法 貼在New query 1 此動作在做新增資料表與欄位
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
![image](https://user-images.githubusercontent.com/103306835/170397678-cbb618e9-e2f9-4ac3-80e6-0e7cb294cb91.png)

6.第23行改為自己的s3 URL 紅框指令表示為將S3資料匯入切割

![image](https://user-images.githubusercontent.com/103306835/170397782-44d82a60-dad0-4948-b99e-9d656c20d84a.png)

7.忘記S3 URL可以回到S3 Copy S3 URL

![image](https://user-images.githubusercontent.com/103306835/170397828-e91c1abb-81d3-4684-9f0f-5721aac844c1.png)

8.點選[Run query]

![image](https://user-images.githubusercontent.com/103306835/170397843-a48f584e-31d3-47fd-97ae-d42bd0624216.png)

9.即新增一個資料表 名為aws-rekognition

![image](https://user-images.githubusercontent.com/103306835/170397882-76ebbe61-9087-40bc-90e1-7aa6b6e5c103.png)

# 步驟4：查詢資料

1.點選[Preview table]

![image](https://user-images.githubusercontent.com/103306835/170397936-8b94e0db-43ca-46a5-834e-a8890ed9cb1f.png)

2.查詢資料顯示

![image](https://user-images.githubusercontent.com/103306835/170397958-e74627ac-8f4f-458f-8df9-2c4b6e0f76dc.png)

3.S3產生資料集

![image](https://user-images.githubusercontent.com/103306835/170397977-48d3cc8b-6390-4235-86fa-d32d7396bd31.png)

# 步驟5：建立視覺化儀表板

1.收信 並點選接受邀請

![image](https://user-images.githubusercontent.com/103306835/170398046-c5f525f6-8408-40df-a058-2e8c59b7b496.png)

2.輸入密碼並點選[Create account and sign in]

![image](https://user-images.githubusercontent.com/103306835/170398075-d48af882-dc61-46fa-a067-f487c7317d5b.png)

3.點選[Continue]

![image](https://user-images.githubusercontent.com/103306835/170398100-44fe8cb8-f533-4510-abe0-0d6fd58e73ed.png)

4.成功登入

![image](https://user-images.githubusercontent.com/103306835/170398131-1521cf03-36ae-4570-b367-0209a653953b.png)

5.點選[資料集]

![image](https://user-images.githubusercontent.com/103306835/170398253-f52ecdcd-fc52-4b29-b8fc-b545d53d8cd4.png)

6.點選[新資料集]

![image](https://user-images.githubusercontent.com/103306835/170398350-6d0e28d9-9fea-4ae3-86d8-4a40468e6e71.png)

7.點選[Athena]

![image](https://user-images.githubusercontent.com/103306835/170398562-27e483f5-e625-4c1f-8cb5-e4d8a5720983.png)

8.資料來源名稱輸入AWSDataCatalog

![image](https://user-images.githubusercontent.com/103306835/170398603-e60c3532-7493-481d-b802-484d989f44ae.png)

9.點選[建立資料來源]

![image](https://user-images.githubusercontent.com/103306835/170398639-5a68c0e6-f30d-470c-8595-4a22a3af1a09.png)

10.資料庫選擇fcu

![image](https://user-images.githubusercontent.com/103306835/170398665-8524826c-4477-4c8a-b5aa-0eeb031da484.png)

11.選擇aws-rekognition

![image](https://user-images.githubusercontent.com/103306835/170398732-8e3d8f05-9101-4745-8c3f-c069f4b329be.png)

12.選擇[直接查詢資料]

![image](https://user-images.githubusercontent.com/103306835/170398774-1c1e5933-f771-44e4-be03-c4f9ac4d5a07.png)

13.設計介面

![image](https://user-images.githubusercontent.com/103306835/170398794-e7191824-93a5-4185-a16d-591c9fef8d93.png)

14.自動判別合適圖表

![image](https://user-images.githubusercontent.com/103306835/170398843-f43983e9-0c4e-432b-89c0-c0b10d5b798b.png)

15.以圓餅圖顯示性別比例

![image](https://user-images.githubusercontent.com/103306835/170398866-abd5ac66-1fe8-419c-bf2a-c74d6a9f6417.png)

# 政府公開資料集-NYTaxi


# 教材編制：逢甲大學企業資訊系統研究中心
# 更新日期：2022/05/14(Peggy)
