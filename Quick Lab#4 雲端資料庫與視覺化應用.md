# 雲端資料庫與視覺化應用

# 情境

五十藍考量到未來資訊服務的使用彈性，預計第一階段先將公司自建的POS系統資料庫遷移到 Amazon Web Services平台。
由於POS資料庫從SQLite資料庫更換為MYSQL資料庫(Aurora)，資訊主管指示A團隊小組將先將既有POS資料匯出(CSV檔)，上傳雲端儲存桶(S3)做備份，並同時直接存入MYSQL資料庫。
另外，這一系列的上傳備份與資料庫異動的動作(Log)，主管希望記錄在No sql的資料庫(Dynamodb)中，方便後續追蹤備份與轉換，請協助進行POS系統的資料庫雲端搬遷。
近期想將資料庫資料做視覺化儀錶板，方便主管查看目前商品銷售狀況與檢索。

# 實施架構

![image](https://user-images.githubusercontent.com/103306835/168982036-1ab09e1e-9250-4d3f-b5dd-e10d5f30579a.png)


# 資料集

![image](https://user-images.githubusercontent.com/103306835/168982125-97d0062d-f546-4980-8dd6-1ac97b38585a.png)


# 操作步驟

![image](https://user-images.githubusercontent.com/103306835/168982260-d9619d5b-cd87-4ae6-9afd-190fef76d80a.png)

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

![image](https://user-images.githubusercontent.com/103306835/168988284-28e99449-c3fb-4899-95aa-f888d965b1b4.png)

19.選擇[上傳]

![image](https://user-images.githubusercontent.com/103306835/168988328-2712203a-faa0-43f2-973d-30451c5f5ee7.png)

20.選擇[儲存]

![image](https://user-images.githubusercontent.com/103306835/168988390-b6ee5b82-8cac-466e-9ddf-2c21fa2f3746.png)

21.更改6、12、19行資料

RDS Endpoint、 S3名稱、Dynamodb資料表名稱

![image](https://user-images.githubusercontent.com/103306835/168995718-f77ac33f-c241-4b93-8d58-d6a3db080782.png)

22.點選[Deploy]

![image](https://user-images.githubusercontent.com/103306835/168997382-d6fcf12f-e802-49ae-8b12-2cf8fd55a31e.png)

23.點選[Test]

![image](https://user-images.githubusercontent.com/103306835/168997497-85928e51-97a5-4e1d-98fd-15f650d9e652.png)

24.輸入事件名稱：test

![image](https://user-images.githubusercontent.com/103306835/168997541-12a3191c-bfe0-41cf-8f39-9e615b6484c4.png)

25.貼上測試語法

![image](https://user-images.githubusercontent.com/103306835/168997616-5dd24305-5dae-4311-9283-9cc73b71497c.png)

26.修改23、27行的s3 bucket name及30行的object name

![image](https://user-images.githubusercontent.com/103306835/168997696-40e52f98-0660-408a-b4e7-e28756eba9b9.png)

27.點選[儲存]

![image](https://user-images.githubusercontent.com/103306835/168997770-c4c24fe9-11f7-4d4d-8822-7467ffed9fe1.png)

28.成功儲存測試事件

![image](https://user-images.githubusercontent.com/103306835/168997985-fdf6fedb-96e6-4ca0-bb26-04282d687036.png)

# Step5： RDS連線到雲端資料庫 

1.回到RDS

2.點選RDS

3.查看RDS

4.查看Security group

5.

6.開啟sqlectron

![image](https://user-images.githubusercontent.com/103306835/168998388-eb3bae08-edbe-4a1b-a5f4-6bce569377c6.png)

7.點選[Add]

![image](https://user-images.githubusercontent.com/103306835/168998453-4254ab67-3de7-4179-9782-40659a2ca43e.png)

8.輸入資料庫連線資訊

輸入資料庫連線資訊![image](https://user-images.githubusercontent.com/103306835/168998513-4d82cd8c-04fd-46aa-b595-22a0904db9cc.png)

9.點選[Save]

![image](https://user-images.githubusercontent.com/103306835/168998564-072bcd48-a8c5-4675-953f-fbbf62b553e8.png)

10.點選[Connect]

![image](https://user-images.githubusercontent.com/103306835/168998639-f662d6e5-498e-45f4-a0bc-27aa7b37de80.png)

11.輸入SQL語法

![image](https://user-images.githubusercontent.com/103306835/168998687-9c1aaf8f-969a-4d4d-a3ea-e03b93284bc6.png)

```
CREATE TABLE IF NOT EXISTS ordertable(
  id varchar(255),
  order_id varchar(255),
  items_count int,
  order_total double,
  order_status int
  );
```

12.點選[Execute]

![image](https://user-images.githubusercontent.com/103306835/168998779-859eb6ed-195f-45ac-9328-5b3adf9c4988.png)

13.點選上方Reconnect

![image](https://user-images.githubusercontent.com/103306835/168998886-a378c442-c9b1-4548-98b4-34e043305f59.png)

14.更新後 顯示資料表

![image](https://user-images.githubusercontent.com/103306835/168998936-72489dea-5d15-4b8f-a9a8-512e904ffb93.png)

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
![image](https://user-images.githubusercontent.com/103306835/168999885-9dedd433-99d3-464a-a5a6-b82a86942ee0.png)

9.點選資料表

![image](https://user-images.githubusercontent.com/103306835/169000103-859570c5-4aae-489e-a0f6-3ca0abb46723.png)

10.查詢目前資料表的資料欄位

![image](https://user-images.githubusercontent.com/103306835/169000181-08fa2545-a20f-4f48-a9e9-89b44d110485.png)

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

![image](https://user-images.githubusercontent.com/103306835/169000955-31190431-5f5c-4784-8bab-e66cc6ee207b.png)

8.點選[驗證連線]

![image](https://user-images.githubusercontent.com/103306835/169001004-7b9f7176-b176-4057-bb32-e4c0e3bfae57.png)

9.點選[建立資料來源]

![image](https://user-images.githubusercontent.com/103306835/169001088-4b812ea1-58e1-469a-8dfd-f8bf09b73faa.png)

10.點選Sale資料表

![image](https://user-images.githubusercontent.com/103306835/169001152-c8a709fe-39c1-45ad-80f8-3374eb06f731.png)

11.點選[選取]

![image](https://user-images.githubusercontent.com/103306835/169001271-5475ff23-d3c1-4f5c-9bda-71ed39ae1ed0.png)

12.點選[Visualize]

![image](https://user-images.githubusercontent.com/103306835/169001335-893e0a22-e80e-40d9-9d4d-bc3c074c6e9d.png)

13.設計介面

![image](https://user-images.githubusercontent.com/103306835/169001396-afd013e8-8b0c-4de2-8020-1176d5cac9f6.png)

14.選取欄位設計圖表

![image](https://user-images.githubusercontent.com/103306835/169001455-d7478246-9523-4e18-9e27-b875f3b3d744.png)

