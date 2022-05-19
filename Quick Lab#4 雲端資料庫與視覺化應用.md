# 雲端資料庫與視覺化應用

# 情境

60藍考量到未來資訊服務的使用彈性，預計第一階段先將公司自建的POS系統資料庫遷移到 Amazon Web Services平台。
由於POS資料庫從SQLite資料庫更換為MYSQL資料庫(Aurora)，資訊主管指示A團隊小組將先將既有POS資料匯出(CSV檔)，上傳雲端儲存桶(S3)做備份，並同時直接存入MYSQL資料庫。
另外，這一系列的上傳備份與資料庫異動的動作(Log)，主管希望記錄在No sql的資料庫(Dynamodb)中，方便後續追蹤備份與轉換，請協助進行POS系統的資料庫雲端搬遷。


近期想將資料庫資料做視覺化儀錶板，方便主管查看目前商品銷售狀況與檢索。

# 使用環境(請先Start Lab)
AWS Academy Learner Lab-Foundation Services(課程代碼：19091)

AWS Academy登入連結：https://awsacademy.instructure.com/login/canvas

如何進入Learner Lab，請參考：https://github.com/EISCFCU/ntue_cloud-database/blob/main/Quick%20Lab%230%E9%80%B2%E5%85%A5AWS%20Academy.md

 使用資源(請先下載)

1.sqlectron：https://sqlectron.github.io/

1-1.下載sqlectron，連結：https://sqlectron.github.io/

1-2.點選GUI

![image](https://user-images.githubusercontent.com/103306835/167972790-e833bc5d-f8e2-4208-8ffc-98ed2127361c.png)

1-3.選擇對應的GUI版本

![image](https://user-images.githubusercontent.com/103306835/167972992-370792de-f0c7-4cdd-ac17-d7da570a00ef.png)


1-4.下載後解壓縮

1-5.開啟sqlectron

![image](https://user-images.githubusercontent.com/103306835/167973456-485b1160-8c1f-49a7-96b2-e061cd966c9c.png)


2.csv檔：https://github.com/EISCFCU/ntue_cloud-database/blob/main/order.csv


3.lambda.zip：https://github.com/EISCFCU/ntue_cloud-database/blob/main/s3-csv-trans-fe42e873-5146-447d-ab7b-b186cfbd1caa.zip


![image](https://user-images.githubusercontent.com/103306835/167975240-880be2da-44b8-4036-af1b-f4e56ccf6e28.png)


# 實施架構

![image](https://user-images.githubusercontent.com/103306835/168982036-1ab09e1e-9250-4d3f-b5dd-e10d5f30579a.png)


# 資料表綱要

![image](https://user-images.githubusercontent.com/103306835/169140143-5260d6af-0b03-478e-bd19-f13abb49ef29.png)


# 操作步驟

![image](https://user-images.githubusercontent.com/103306835/169141865-33189436-596a-41c9-9807-ffc323494d2a.png)

# Step1：建立雲端資料庫

1.點選[RDS]

![image](https://user-images.githubusercontent.com/103306835/168982387-40220f0f-70e0-463f-9684-e84970eac055.png)

2.點擊[創建資料庫]

![image](https://user-images.githubusercontent.com/103306835/168982431-d4346e48-6007-4b51-94a1-a8d0fd0b1159.png)

3.選擇[標準建立]

![image](https://user-images.githubusercontent.com/103306835/168982497-9f701649-0c58-4882-b3a8-d8242826f557.png)

4.選擇 Amazon Aurora

![image](https://user-images.githubusercontent.com/103306835/168982548-a7c15d5d-d681-4dcb-9cbb-5b0484096ca3.png)

5.選擇開發/測試

![image](https://user-images.githubusercontent.com/103306835/168982598-5ece305e-1b50-4d15-8ab6-837495c1180a.png)

6.資料庫設定

資料庫實例標識符 ：database-1

主要使用者名稱：admin

主要密碼：pass1234

確認密碼：pass1234

![image](https://user-images.githubusercontent.com/103306835/168982657-f752bd71-e2b2-49fc-b43d-82f10feb7ff4.png)

7.資料庫執行個體類別選擇爆量類別 (包括 t 類別)

![image](https://user-images.githubusercontent.com/103306835/168982848-66b43d39-c4d8-4902-ac32-7587a056a62a.png)

8.Virtual Private Cloud (VPC)：Defalt VPC

![image](https://user-images.githubusercontent.com/103306835/168983013-e29e1d3a-245b-4a55-ac90-1fb033d059ba.png)

9.公開存取：是

![image](https://user-images.githubusercontent.com/103306835/168983065-7d98838e-c22e-4300-847f-43b2426b680b.png)

10.選擇現有的VPC安全群組：default

![image](https://user-images.githubusercontent.com/103306835/168983106-26cf254f-c5fa-4b5e-b58a-9b128d5b922f.png)

11.展開其他組態

![image](https://user-images.githubusercontent.com/103306835/168983154-a6a44941-b845-4408-a2b1-2c9af8c7e58f.png)

12.初始資料庫名稱：lab

![image](https://user-images.githubusercontent.com/103306835/168983195-d0d44ee1-8dc0-4b72-8a71-9dd2246cd8cc.png)

13.取消勾選[啟用增強監控]與[啟用刪除保護]

![image](https://user-images.githubusercontent.com/103306835/168983801-229fa6f7-99a6-4bd4-9a32-c6108860e909.png)

14.點選[建立資料庫]

![image](https://user-images.githubusercontent.com/103306835/168983945-9a047e22-5ede-442b-9144-735d194dab3d.png)

15.等待建立完成(狀態變成可用)

![image](https://user-images.githubusercontent.com/103306835/168983990-b5c3522e-83a3-4a89-be15-6f3297813932.png)

# 步驟2：建立儲存桶(S3) 

1.搜尋S3

![image](https://user-images.githubusercontent.com/103306835/168984395-d5901bab-669e-411b-b847-53e7e9ad7280.png)

2.點選[建立儲存貯體]

![image](https://user-images.githubusercontent.com/103306835/168984555-993ed219-5da2-42b4-9724-d14da630de49.png)

3.輸入儲存貯體名稱(自訂)

![image](https://user-images.githubusercontent.com/103306835/168984597-8cbe9515-ca7e-45ad-92f7-c09aba546bb8.png)

4.取消勾選封鎖 並勾選我確認…

![image](https://user-images.githubusercontent.com/103306835/168984810-baa19676-b92e-4f0a-bcc8-566ea3760fc2.png)

5.點選[建立儲存貯體]

![image](https://user-images.githubusercontent.com/103306835/168984891-4b213417-dce2-4257-9619-5f034da0412c.png)

6.成功建立儲存貯體

![image](https://user-images.githubusercontent.com/103306835/168984934-6d297abc-5633-4f61-a5b0-203f8105f3c3.png)

# Step3：建立No sql 資料庫(Dynamodb)

1.搜尋Dynamodb

![image](https://user-images.githubusercontent.com/103306835/168985191-e8fc2713-6eb6-48ad-a42a-8086f11c6652.png)

2.點選Dynamodb

![image](https://user-images.githubusercontent.com/103306835/168985247-5dc14560-536d-4088-addc-d0501f6f4151.png)

3.點選[建立資料表]

![image](https://user-images.githubusercontent.com/103306835/168985372-ce4f13ea-dfdb-44ce-aaae-997a194410db.png)

4.輸入資料表名稱：S3-log-20220512 

分區索引：StartTime(字串) 
排序索引：FileName(字串)

![image](https://user-images.githubusercontent.com/103306835/168985466-666c64d1-6b22-472e-a008-af6ed75673a0.png)

5.點選[建立資料表]

![image](https://user-images.githubusercontent.com/103306835/168985532-5e1fdbf3-223a-4d2c-bfe0-3a02d806f6e1.png)

6.成功建立S3-log-20220512資料表(狀態：作用中)

![image](https://user-images.githubusercontent.com/103306835/168985585-4a601511-7cb8-44b0-a3bb-c6000b26e411.png)

# Step4：建立 Lambda

1.搜尋Lambda 並點選

![image](https://user-images.githubusercontent.com/103306835/168985666-fd818eb2-4d77-411a-8608-eac5c35329af.png)

2.點選[建立函式]

![image](https://user-images.githubusercontent.com/103306835/168985726-67452be2-6e3a-4b81-ab4d-4c116aa137f0.png)

3.點選從頭開始撰寫

![image](https://user-images.githubusercontent.com/103306835/168985816-499ab68b-955b-47d4-b893-63e21a04aac1.png)

4.函數名稱自訂

![image](https://user-images.githubusercontent.com/103306835/168985869-2fafd01e-2468-4845-93ba-e3a69f350e11.png)

5.執行時間：Python3.8

![image](https://user-images.githubusercontent.com/103306835/168985926-c3d12f72-1c95-4c7f-8bbb-d680737fb4b1.png)

6.選擇使用現有的角色：LabRole

![image](https://user-images.githubusercontent.com/103306835/168985974-4de0381c-8464-41b2-aab2-5d08783223ea.png)

7.點選[建立函式]

![image](https://user-images.githubusercontent.com/103306835/168986022-0f120b2d-934b-4c45-af64-0a294eb878a6.png)

8.成功建立函式

![image](https://user-images.githubusercontent.com/103306835/168986067-df6fc79f-b61d-416e-a010-4ab8fef11ff3.png)

9.點選[新增觸發]

![image](https://user-images.githubusercontent.com/103306835/168986104-57c990bc-6cec-4bdf-987f-f4b1fee4e172.png)

10.選擇S3

![image](https://user-images.githubusercontent.com/103306835/168986152-5b3f116d-cfab-4d7e-a5bc-70b8379291fe.png)

11.選擇步驟2建立的S3

![image](https://user-images.githubusercontent.com/103306835/168986188-9daaf2e9-3e87-42df-a487-0e8eef8a3cf9.png)

12.尾碼為.csv

![image](https://user-images.githubusercontent.com/103306835/168986223-14efa58f-db0c-4557-91b7-c9db183cb482.png)

13.勾選我確認

![image](https://user-images.githubusercontent.com/103306835/168987677-bc7ed6ee-5abf-4f31-8778-50566e2090b6.png)

14.點選[新增]

![image](https://user-images.githubusercontent.com/103306835/168987811-36ea9ab6-662c-4ee7-8484-02aecd83095f.png)

15.完成觸發設定

![image](https://user-images.githubusercontent.com/103306835/168987880-4f117cd4-8d22-4f54-a578-bf7c2750800e.png)

16.往下滑動視窗

![image](https://user-images.githubusercontent.com/103306835/168988152-4d3d92e0-ea23-4d0b-bd4e-e5ba16075673.png)

17.找到上傳按鈕 並點選

![image](https://user-images.githubusercontent.com/103306835/168988192-5708df45-38e0-4e5f-8019-debba9192d88.png)

18.選擇.zip 檔案

lambda.zip：https://github.com/EISCFCU/ntue_cloud-database/blob/main/s3-csv-trans-fe42e873-5146-447d-ab7b-b186cfbd1caa.zip

![image](https://user-images.githubusercontent.com/103306835/168988284-28e99449-c3fb-4899-95aa-f888d965b1b4.png)

19.選擇[上傳] 

![image](https://user-images.githubusercontent.com/103306835/168988328-2712203a-faa0-43f2-973d-30451c5f5ee7.png)

20.選擇[儲存]

![image](https://user-images.githubusercontent.com/103306835/168988390-b6ee5b82-8cac-466e-9ddf-2c21fa2f3746.png)

21.更改6、12、19行資料

RDS Endpoint、 S3名稱、Dynamodb資料表名稱

![image](https://user-images.githubusercontent.com/103306835/169138766-85a6389b-f728-4643-8371-38a00d70fa0c.png)

22.點選[Deploy]

![image](https://user-images.githubusercontent.com/103306835/169138799-bb4de053-1097-4b11-aa70-0da9b0082dc1.png)

23.點選[Test]

![image](https://user-images.githubusercontent.com/103306835/168997497-85928e51-97a5-4e1d-98fd-15f650d9e652.png)

24.輸入事件名稱：test

![image](https://user-images.githubusercontent.com/103306835/168997541-12a3191c-bfe0-41cf-8f39-9e615b6484c4.png)

25.貼上測試語法

![image](https://user-images.githubusercontent.com/103306835/168997616-5dd24305-5dae-4311-9283-9cc73b71497c.png)

```
{
  "Records": [
    {
      "eventVersion": "2.0",
      "eventSource": "aws:s3",
      "awsRegion": "us-east-1",
      "eventTime": "1970-01-01T00:00:00.000Z",
      "eventName": "ObjectCreated:Put",
      "userIdentity": {
        "principalId": "EXAMPLE"
      },
      "requestParameters": {
        "sourceIPAddress": "127.0.0.1"
      },
      "responseElements": {
        "x-amz-request-id": "EXAMPLE123456789",
        "x-amz-id-2": "EXAMPLE123/5678abcdefghijklambdaisawesome/mnopqrstuvwxyzABCDEFGH"
      },
      "s3": {
        "s3SchemaVersion": "1.0",
        "configurationId": "testConfigRule",
        "bucket": {
          "name": "my-s3-bucket",
          "ownerIdentity": {
            "principalId": "EXAMPLE"
          },
          "arn": "arn:aws:s3:::example-bucket"
        },
        "object": {
          "key": "product_sale.csv",
          "size": 1024,
          "eTag": "0123456789abcdef0123456789abcdef",
          "sequencer": "0A1B2C3D4E5F678901"
        }
      }
    }
  ]
}

```
26.修改23、27行的s3 bucket name及30行的object name

![image](https://user-images.githubusercontent.com/103306835/169138850-599413e9-5324-4c07-9e6b-fa77b0593446.png)

27.點選[儲存]

![image](https://user-images.githubusercontent.com/103306835/169138873-9469f7a1-737a-49c2-97e2-823d84814f5d.png)

28.成功儲存測試事件

![image](https://user-images.githubusercontent.com/103306835/168997985-fdf6fedb-96e6-4ca0-bb26-04282d687036.png)

# Step5： RDS連線到雲端資料庫 

1.點選RDS

![image](https://user-images.githubusercontent.com/103306835/169137749-55787a61-66e9-4b67-bd8a-e77e4c817467.png)

3.查看RDS

![image](https://user-images.githubusercontent.com/103306835/169137796-ce65d433-74ae-4354-9681-758b81faede5.png)


4.查看Security group

![image](https://user-images.githubusercontent.com/103306835/169137894-3a81b66f-dd49-4002-8f52-594fe10c91fe.png)

5.點選傳入規則

![image](https://user-images.githubusercontent.com/103306835/169138074-2719bb6e-3726-46d1-ac17-f394f38f96a8.png)

6.來源為0.0.0.0/0 若不是，請修改輸入規則來源為0.0.0.0/0(指任何一台電腦都可以登入RDS)

![image](https://user-images.githubusercontent.com/103306835/169138196-8b514b0d-2ad5-45ca-8568-630776b2d343.png)

7.下載sqlectron(已經下載sqlectron請忽略此步驟)

https://sqlectron.github.io/

8.點選GUI

![image](https://user-images.githubusercontent.com/103306835/167972790-e833bc5d-f8e2-4208-8ffc-98ed2127361c.png)

9.選擇對應的GUI版本

![image](https://user-images.githubusercontent.com/103306835/167972992-370792de-f0c7-4cdd-ac17-d7da570a00ef.png)


10.下載後解壓縮

11.開啟sqlectron

![image](https://user-images.githubusercontent.com/103306835/168998388-eb3bae08-edbe-4a1b-a5f4-6bce569377c6.png)

12.點選[Add]

![image](https://user-images.githubusercontent.com/103306835/168998453-4254ab67-3de7-4179-9782-40659a2ca43e.png)

13.輸入資料庫連線資訊

![image](https://user-images.githubusercontent.com/103306835/169138329-0ba9f26b-0108-4c5c-9694-e71457dca921.png)

14.點選[Save]

![image](https://user-images.githubusercontent.com/103306835/169138361-6db0d925-32ff-4258-830c-4a0c21838e85.png)

15.點選[Connect]

![image](https://user-images.githubusercontent.com/103306835/169138390-1c1c9f17-5170-464d-bfa4-2a3e7c04e064.png)

16.輸入SQL語法

![image](https://user-images.githubusercontent.com/103306835/169138423-0cd379e7-df4a-429d-86f7-9a96c3843d3b.png)

```
CREATE TABLE IF NOT EXISTS ordertable(
  id varchar(255),
  order_id varchar(255),
  items_count int,
  order_total double,
  order_status int,
  file_name varchar(255)
  );
```

17.點選[Execute]

![image](https://user-images.githubusercontent.com/103306835/169138461-78f037b2-ca1c-44f8-b3f1-0b7c2dacf76e.png)

18.點選上方Reconnect

![image](https://user-images.githubusercontent.com/103306835/169138495-0385ffc6-3dce-4a96-b2e0-2229ed7f7a3b.png)

19.更新後 顯示資料表

![image](https://user-images.githubusercontent.com/103306835/169138521-1d8d4a03-0cf2-4127-9e60-227c8f8612fc.png)

#Step6： S3 上傳CSV

1.搜尋S3 並點選

![image](https://user-images.githubusercontent.com/103306835/168999025-6095ee34-525a-4064-85d1-db0dab0763d5.png)

2.點選步驟2新增的S3

![image](https://user-images.githubusercontent.com/103306835/168999080-8fa969ed-65a8-4a44-8828-9ce318cb63a1.png)

3.點選[上傳]

![image](https://user-images.githubusercontent.com/103306835/168999150-4ab9d0a8-c842-4326-a8dc-2858b68c26de.png)

4.點選[新增檔案]

![image](https://user-images.githubusercontent.com/103306835/168999201-e96ec2b8-b1ab-4789-9571-1a2fdb8e2adc.png)

5.點選[上傳]

![image](https://user-images.githubusercontent.com/103306835/168999461-47e57a87-07cc-465e-806e-6ccb1100e30a.png)

6.成功上傳

![image](https://user-images.githubusercontent.com/103306835/168999600-d135ef4f-e8ca-47e9-b778-18e369bb5566.png)

7.查看資料庫(RDS)

8.點選Reconnect圖示

![image](https://user-images.githubusercontent.com/103306835/169138575-fb8c4187-5cef-4a22-a94f-aea68d9b2080.png)

9.點選資料表

![image](https://user-images.githubusercontent.com/103306835/169138605-66275171-9008-4697-bc23-0bc41eaa3721.png)

10.查詢目前資料表的資料欄位

![image](https://user-images.githubusercontent.com/103306835/169138636-597ebbde-e0d5-4fdb-b83e-ebb0840ecfba.png)

11.點選步驟3建立的Dynamodb資料表

![image](https://user-images.githubusercontent.com/103306835/169000249-23f6ac5c-2a65-403b-9fb6-42a6e769df12.png)

12.點選[探索資料表項目]

![image](https://user-images.githubusercontent.com/103306835/169000300-34a6b680-2dc5-4a4d-b4c4-c7231ac0ea2b.png)

13.點選[執行]

![image](https://user-images.githubusercontent.com/103306835/169000373-7fc0fb32-ef79-4481-947f-5d187e406c14.png)

14.查看結果

![image](https://user-images.githubusercontent.com/103306835/169000471-ea25f728-773b-46f5-84d0-b490d1867176.png)

# Step7：視覺化儀錶板(Quicksight) 

1.收信 並點選接受邀請

![image](https://user-images.githubusercontent.com/103306835/169000617-889c617e-4b13-477a-9d96-e3acdff99382.png)

2.輸入密碼並點選[Create account and sign in]

![image](https://user-images.githubusercontent.com/103306835/169000679-d002f08d-804e-4d8e-8bb5-4adfcba33073.png)

3.點選[Continue]

![image](https://user-images.githubusercontent.com/103306835/169000740-11700a01-2ed9-4d36-9c37-02e29601be97.png)

4.點選[資料集]

![image](https://user-images.githubusercontent.com/103306835/169000789-c7a73f2d-9b52-44d2-82ba-25f28f9dd327.png)

5.點選[新資料集]

![image](https://user-images.githubusercontent.com/103306835/169000834-c40af04f-333b-4242-abc0-5bbd3599d278.png)

6.點選[RDS]

![image](https://user-images.githubusercontent.com/103306835/169000883-3d6084d0-84b9-419f-979b-563b3c17e680.png)

7.輸入資料庫連線資訊

![image](https://user-images.githubusercontent.com/103306835/169141444-dd461515-8fbd-4d89-abe7-5eaaaf3f00cf.png)

8.點選[驗證連線]

![image](https://user-images.githubusercontent.com/103306835/169141477-b195cea5-1495-4cdb-8ec7-879b47f504e0.png)

9.點選[建立資料來源]

![image](https://user-images.githubusercontent.com/103306835/169141503-f85d53cf-4e25-402d-aeb3-4d0ce20d2c52.png)

10.點選ordertable資料表

![image](https://user-images.githubusercontent.com/103306835/169141530-3854dc64-579a-42a9-8fe9-4f12f8c19f97.png)

11.點選[選取]

![image](https://user-images.githubusercontent.com/103306835/169141567-9db3dffd-8d74-40d7-99e8-3a737e9c0a27.png)

12.點選直接查詢資料

![image](https://user-images.githubusercontent.com/103306835/169141599-8221b8f6-191e-408b-85f8-49d880424c95.png)


13.點選[Visualize]

![image](https://user-images.githubusercontent.com/103306835/169141656-2b3330f2-b7ef-42a0-9213-5820d7bea671.png)

14.設計介面

![image](https://user-images.githubusercontent.com/103306835/169001396-afd013e8-8b0c-4de2-8020-1176d5cac9f6.png)

14.選取欄位設計圖表

![image](https://user-images.githubusercontent.com/103306835/169141678-632766c5-e61c-405e-ab16-b3372d597720.png)

