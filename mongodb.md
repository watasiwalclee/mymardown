# MongoDB筆記

MongoDB有三個要點，與常見的關聯式資料庫比較


| 關聯式資料庫 | NoSql |
| -------- | -------- |
|資料庫(Database)|資料庫|
|資料表(table)|集合(table)|
|紀錄(record)|文檔(document)|

* <font color=#FF0000>**與一般關聯式資料庫最大的不同為每筆紀錄的長度可以不同**</font>
* 在MongoDB操作資料庫使用javascript來操作

# 簡單操作


| 指令 | 說明 |
| -------- | -------- |
|mongo|進入mongodb介面|
|exit|退出介面|
|help| 幫助|
|show dbs|顯示目前資料庫清單|
|show collection|顯示<font color=#FF0000>當前資料庫</font>集合|
|use "db名稱"|切換至指定資料庫**若無資料庫，則會新增並切換**|
|db.createCollection("集合名稱")|建立集合|
|db.status()|檢視目前資料庫狀態|
|db.dropDatabase()|刪除當前資料庫|

# 操作集合
| 指令 | 說明 |
| -------- | -------- |
|db.集合名稱.renameCollection("新集合名稱")|重新命名集合|
|db.集合名稱.drop()|刪除集合|

# 操作文檔
| 指令 | 說明 |
| -------- | -------- |
|db.集合名稱.insert(json對象)|寫入資料|
|db.集合名稱.find()|搜尋文檔|
|db.集合名稱.count()|查詢紀錄筆數|
|db.集合名稱.remove({條件})|刪除紀錄，**若無條件，則為全部刪除**|
|db.集合名稱.distinct("欄位")|尋找欄位的唯一值|

## 搜尋條件
* 







