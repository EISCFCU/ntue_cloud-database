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



