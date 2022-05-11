

# 情境

森森購物考量到未來資訊服務的使用彈性，預計第一階段先將公司自建的POS系統資料庫遷移到 Amazon Web Services平台。
由於POS資料庫從SQLite資料庫更換為MYSQL資料庫，資訊主管指示A團隊小組將先將既有POS資料匯出(CSV檔)，上傳雲端儲存桶(S3)做備份，並同時直接存入MYSQL資料庫。
另外，這一系列的上傳備份與資料庫異動的動作(Log)，主管希望記錄在No sql的資料庫(Dynamodb)中，方便後續追蹤備份與轉換，請協助進行POS系統的資料庫雲端搬遷。


# Lab Scenario

![image](https://user-images.githubusercontent.com/103306835/167766069-215b877d-fb67-447d-8b91-e377b066bc69.png)

# 操作步驟

![image](https://user-images.githubusercontent.com/103306835/167766102-f3d54bac-f440-480f-a0fc-1d053e320537.png)


# Step1：建立雲端資料庫

1.點選[RDS]

![image](https://user-images.githubusercontent.com/103306835/167766127-cae8ab04-b83c-4f5d-a791-3f059dadcf85.png)

2.點擊[創建資料庫]

![image](https://user-images.githubusercontent.com/103306835/167766236-ae9ba7e5-fa3b-4c57-ab36-c644b9d9489f.png)

3.選擇[標準建立]

![image](https://user-images.githubusercontent.com/103306835/167766288-839d96b3-6185-45e1-af76-a413e9ad8e85.png)

4.選擇 MySQL

![image](https://user-images.githubusercontent.com/103306835/167766328-da406717-53bf-411f-9bf1-cc7ab553c738.png)

5.選擇Free Tier

![image](https://user-images.githubusercontent.com/103306835/167766349-95792490-a82f-47df-894a-c56089a15933.png)

6.資料庫帳密設定

資料庫實例標識符 ：lab3-db
主要使用者名稱：admin
主要密碼：pass1234
確認密碼：pass1234

![image](https://user-images.githubusercontent.com/103306835/167766434-0131cdc5-b98e-4661-90bb-396f7e95ba04.png)

7.資料庫執行個體類別->選擇 db.t3.micro

![image](https://user-images.githubusercontent.com/103306835/167766461-60aa2d0f-4599-49f3-b5bb-7bed15197297.png)

8.儲存體類型：General Purpose (SSD)  配置儲存：20

![image](https://user-images.githubusercontent.com/103306835/167766490-fd724508-e8bc-4f97-83b7-6f23a6897e60.png)

9.Virtual Private Cloud (VPC)：Defalt VPC

![image](https://user-images.githubusercontent.com/103306835/167766517-57553bc5-d19a-4419-823b-97c70a9ab943.png)

10.公開存取：是(方便後續本機連線資料庫)

![image](https://user-images.githubusercontent.com/103306835/167766566-0839807e-e34f-48f5-85e1-53fd180f35cb.png)

11.選擇現有的VPC安全群組：default

![image](https://user-images.githubusercontent.com/103306835/167766581-808d6a88-a1fc-425e-8624-7f9ecddb1897.png)

12.展開其他組態

![image](https://user-images.githubusercontent.com/103306835/167766611-15a78b1f-23f2-4386-864f-77cd9843dbac.png)

13.初始資料庫名稱：lab

![image](https://user-images.githubusercontent.com/103306835/167766656-79427aef-0124-4a53-8286-a33dbbd1db66.png)

14.取消勾選[啟用自動備份]與[啟用增強監控]

![image](https://user-images.githubusercontent.com/103306835/167766694-8d49547d-79a8-470d-9907-117f4952f289.png)

15.點選[建立資料庫]

![image](https://user-images.githubusercontent.com/103306835/167766717-a2cb1fbe-46bf-4352-80bf-f392b3b30337.png)


16.等待建立完成

![image](https://user-images.githubusercontent.com/103306835/167766745-8e681435-d121-4c79-8327-c5d1175bf730.png)

#  Step2：建立儲存桶(S3) 
