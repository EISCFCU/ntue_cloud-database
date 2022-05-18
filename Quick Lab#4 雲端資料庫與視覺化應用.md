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

