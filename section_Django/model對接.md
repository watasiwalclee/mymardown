# 和Model對接

預設連接sqlite

model 使用了ORM技術(Object Relational Mapping)對象關係映射

將業務邏輯進行了一個解耦合，例如
* object.save()
* object.delete() ...

關聯式資料庫
* DDL
* 通過model定義實現資料庫中資料表的定義

django.db.models.Model 為預設django默認model類別。

當中每一個變數都是對應資料表中的欄位，例如\
first_name = models.CharField()\
first_name為資料表中的欄位，其欄位型態為Char

欄位有各種型態 [連結](https://docs.djangoproject.com/en/3.1/ref/models/fields/#model-field-types) 常用的有
* models.CharField() 字元
* models.IntegerField() 整數
* models.FloatField()
* models.JSONField()
* models.TextField()
* models.BooleanField()
* models.DateTimeField()
* models.TimeField()

model建立結束後在cmd上建立資料遷移文件
**python manage.py makemigrations**
在migreations資料夾中建立model初始化文件(用途為建立資料表)

**python manage.py migrate**
執行應用遷移文件，此時django會透過model的初始文件去建立資料表

<font color=#FF0000>!!若init檔案或model進行欄位變更，則必須使用 **makemigrations 服務名稱** 重新生成資料表建置文件再使用 **migrate** 更動資料表配置。</font>

添加資料置資料庫
1. 在view中建立model物件
2. 將資料逐一放置在model物件中
3. 最後modelobject.save()即可

render(request, html, args)
* context 以字典形式儲存，為連接程式與html變數
* html變數已兩個大括弧作為辨識{{var}}
* 

資料庫crud
* Model.objects.all() 回傳所有資料
* modelobject.save() 添加資料 
* Model.objects.[get](https://docs.djangoproject.com/en/3.0/topics/db/queries/#retrieving-a-single-object-with-get)(條件) 查詢符合條件資料。
* modelobject.delete() 刪除資料(基於查詢結果)

注意事項
1. update的方式為將查詢後的結果更改值後再save()即可
2. 由於進行migrate時，預設設定為每個欄位皆為not null，所以在save()時，需確認每個欄位都有值，也可以在Field中設定default。

## template 語言 [連結](https://docs.djangoproject.com/en/3.0/ref/templates/language/)
若在html中使用模板語言，需用 **{% 內容 %}** 呈現
* {與%之間不能有空格
* 在{%%}區塊中，變數都是參照從程式端傳遞後的變數
* for 使用方式
```
<ul>
{% for athlete in athlete_list %}
    <li>{{ athlete.name }}</li>
{% endfor %}
</ul>
```
* if-elif-else
```
{% if athlete_list %}
    Number of athletes: {{ athlete_list|length }}
{% elif athlete_in_locker_room_list %}
    Athletes should be out of the locker room soon!
{% else %}
    No athletes.
{% endif %}
```

## 更換資料庫設置sqlite3 → MySQL

1. 更改setting → DATABASES
    * ENGINE → django.db.backends.mysql
    * NAME → 資料庫名稱
    * USER → 帳號名稱
    * PASSWORD → 密碼
    * HOST → 位址
    * PORT → 連接埠
2. 在專案位置下的**__init__**設定
    ```
    import pymysql
    # 將pymysql偽裝成mysqlDB
    pymysql.install_as_MySQLdb()
    ```

設定完成後，需確認資料庫名稱是否存在，若不存在，**需手動進入mysql建立**在執行migrate

提醒 → migrate的作用為在設定資料庫上建立資料表。

常見連接mysql方式
* mysqlclient
    * python 2 3都能用
    * 缺點對mysql安裝有要求，必須從指定位置存在配置文件
* python-mysql
    * python3不支持
* pymysql 最為推薦
    * python 2 3都支援
    * 還能偽裝上述兩種模組

服務下的apps.py紀錄該app的名稱，seeting引用方式為**服務名稱.apps.服務名稱Config**

## 回傳response
render屬於簡易型回傳response方式(django.shortcut)，底下為使用django.template.loader方式回傳
```
from django.template import loader
from django.http import HttpResponse

# 獲取html目標
index_page = loader.get_template('index.html')
# 渲染資訊，result為字串，內容為html碼
context = {
    ...
}
result = index_page.render(context=context)
# 回傳資訊
return HttpResponse(result)
```

## python shell
django也有python shell → python manage.py shell

啟動後，介面跟使用python一樣。但不同的是，環境中已包含django的環境配置。可以在shell作一些測試工作。

## Django中解決Bug
* 看日誌
    * 先看第一條
    * 在看最後一條
* 檢查程式邏輯
    * 程式在哪個位置和預期出現偏差

## 資料庫關聯

外來鍵可視為一種約束，作為與其他表的主鍵之連接

* 1對1 
* 1對多
* 多對多

建立外來鍵 [其他說明資料](https://www.itread01.com/content/1550240648.html)
models.ForeignKey(要連結的model, on_delete, related_name)
* **Django 2.0之後，on_delete必須要手填(models.CASCADE)**
* 不需要為了主鍵那張表建立一個欄位，Django會自動將外來鍵與主鍵欄位做關聯。
* 每進行一次get，就會回傳符合外來鍵條件資料表結果，並以<font color=#FF0000>外來鍵model_set</font>的方式儲存在物件中。
* 挑資料方式從最上游的條件開始挑選起
* related_name為做資料關聯時dataset名稱


選取關聯資料
* 1對1
* 1對多
    1. 先從設置主鍵的表挑選出第一階段的資料
    2. 再從有設定外來鍵的表挑資料(表_set)
    ```
    # 先挑班級
    grade = Grade.objects.get(條件)
    # 再挑班級裡的學生
    students = grade.student_set.all()
    ```
MTV
* model
* template 可視為MVC的view，用來做數據呈現
* Views 相當於MVC的Controller