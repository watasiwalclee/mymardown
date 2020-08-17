# Model簡介

除了使用<font color=F75000>python manage.py startapp</font>之外，也能使用<font color=F75000>django-admin startapp</font>達到一樣的效果

# Model
在企業開發中，通常都是從數據開始開發。

model開發流程
1. 配置資料庫
2. 定義model class
3. 生成遷移文件 **python manage.py makemigrations**
4. 執行遷移生成資料表 **python manage.py migrate**
5. 使用model進行crud操作

# ORM (Object Relational Mapping) 對象關係映射
* 可以理解成翻譯機
* 核心思想，解耦合
    * 將業務邏輯和SQL進行解耦

# Django定義模型
* django的查詢方式，不允許使用連續下劃線
* 邏輯刪除:對於重要數據都做邏輯刪除→將資料標記為不可使用，並非將資料從資料庫刪除

# 資料類型
## 字串類型
* AutoField 根據實際ID自動增長的IntegerField
* CharField 字串
* TextField 文本字串，一般超過4000字使用
* IntegerField 整數
* DecimalField 高精度浮點數
* FloatField 浮點數
* BooleanField 布林值
* NullBooleanField 支援True False null
## 時間類型
* DateField
* TimeField
* DateTimeField
## 其他類型
* FileField 上傳文件的字段
* ImageField 繼承FileField，但對上傳的對象進行檢驗，確定是有效的image

# 字段選項
* null 若為true，Django將空值已null存到資料庫中，默認False
* blank 若為True，則字段允許空白，默認False
* db_column 字段名稱，若沒指定，則使用屬性名稱
* db_index 若為True，則在表中建立索引欄位。通常針對FK加索引或經常查詢的字段
* default 默認值
* primary_key 若為True，則該字段為主鍵字段
* unique 若為True，則該字段在表中為唯一值

# 元訊息

```
class Meta:
    ＃ db_table為指定資料表名稱
    db_table=...
    # 做查詢時排序
    ordering=[]
```

# 模型成員Objects

Django默認通過模型的objects對象實現模型的資料查詢

Django有幾種篩選器
1. filter : 回傳<font color=F75000>符合</font>篩選條件的資料
2. exclude : 回傳<font color=F75000>不符合</font>篩選條件的資料
3. order_by() : 排序
* 在欄位前面加上"-"代表反向排序→"id" 從小排到大，"-id" 從大排到小
5. values() : 將結果轉換成list

* 多個filter和exclude可以連在一起使用
* 篩選出來的物件為QuetySet

條件建立方式 [連結](https://docs.djangoproject.com/en/3.0/ref/models/querysets/#id4): **欄位__判斷=值**

## 插入數據
* models.objects.create(欄位=值, ...)
    * !! create為類別方法，不需要建立實體即可進行
* model(欄位=值, ...)
    * 缺點，不能重寫__init__

## 建立對象
* 對象方法
    * 可以調用對象屬性，也可以調用類別屬性
* 類別方法(@classmethod):不需要建立實體，即可調用類別屬性
    * 不能調用對象屬性，只能調用類別屬性
* 靜態方法(@staticmethod)
    * 不能調用對象屬性，也不能調用類別屬性

```
@classmethod
def ...
```

## 取得單一數據
1. get() : 回傳一個滿足條件的對象
    * 查詢條件若沒有符合條件對象，則會觸發錯誤
    * 若符合條件為多個對象，也會觸發錯誤(MultipleObjectReturn)
    * 建議使用get時，使用try except保險一下
3. first() : 回傳滿足條件的第一個對象
4. last() : 回傳滿足條件的最後一個對象
5. count() : 回傳滿足查詢條件的數量
6. exists() : 回傳查詢中是否有資料，有資料則回傳True，反之為False

## HTTP狀態碼
* 2XX
    * 請求成功
* 3XX
    * 轉發或重新導向
* 4XX
    * 客戶端錯誤
* 5XX
    * 伺服器錯誤
    * 後端開發最不想面對

## first和last
* 默認情況可以正常從QuerySet中獲取
* 隱藏BUG
    * 可能會出現first跟last相同的對象
        * 使用寫排序規則來避免

## QuerySet切片限制
切片等同於sql的limit與offset
* 切片規則與list相同，除了下標不能使用負數
* 下標從0開始，[5:15]為編號第5列至編號第14列

該階段應能透過以上功能製作會員登陸邏輯判斷

# 緩存集
* filter
* exclude
* all

以上三種方法觸發時都不會真的去查資料庫

* 只有再迭代結果或獲取對象屬性時，才會真的去查資料庫。這稱為懶查詢
    * 為了優化結構和查詢

# 查詢條件
* 屬性__運算符=值

# 運算符列表
* gt
* lt
* gte
* lte
* in 在集合中
* contains 類似like '%value%'
* startswith 類似like 'value%'
* endswith 類似like '%value'
* 前面同時增加i (ignore)
    * iexact
    * ...
* isnull
* isnotnull

## 時間運算符
moedls.DateTimeField(auto_now_add=True)
* Django查詢條件有時區問題
    * 關閉Django自定義時區(setting.py USE_TZ改為False)
    * 在資料庫中建立對應的時區表(較麻煩)
* datetime的查詢時間單位只會針對該單位進行查詢
* pk 代表主鍵，為默認參數

## 跨關係查詢
* model名稱__屬性__比較運算符，同於join

* models.objects.aggregate() 
    * Max(欄位) 取得結果最大值
    * Avg(欄位) 取得結果平均值
    * Count
    * Min

# F對象
* 用於獲取屬性值，可以實現一個模型不同屬性的運算操作，同時也支援數學運算

# Q對象
* 可以對查詢條件進行封裝
* 封裝後能夠支援邏輯運算
    * 與 & and
    * 或 | or
    * 非 ~ not

# 模型成員
* 顯性屬性
    * 開發者手動書寫的屬性
* 隱性屬性
    * 開發者沒有書寫，ORM自動生成屬性
    * **如果把隱性屬性手動聲明，系統就不會自動生成隱性屬性**

# model manager()


## 作業
* 班級學生列表
    * 班級列表
    * 班級列表可點擊
    * 點擊時顯示所有班級學生

* 整理資料時，要以**只能傳送一次資料**至template做為考量去設計資料格式

# 總結

## 流程
* 客戶端→urls→views→models→views→template→客戶端

## 定義模型
1. 繼承models.Model
    * class Meta
        * db_table
2. 定義欄位
    * 欄位類型
        * 數字
        * 字串
        * 時間
    * 欄位約束
        * max_length
        * default
        * unique
        * index
        * primary_key
        * db_column

## 映射到資料庫
1. 生成遷移文件 makemigrations
2. 執行遷移 migrate
3. 前提
    * 默認設置SQLite
    * 配置MySQL
        * setting需要設置DataBASE相關
        * 需要進行資pymysql偽裝

## ORM (Object Relational Mapping)
* 將業務邏輯和SQL進行解耦

## 建立對象
1. model.objects.create()
2. 自行封裝類別方法建立(於model中搭配@classmethod建立create)
3. 在Manager中的封裝方法建立

## 查詢對象
* 隱性屬性
* objects是Manager的實力
* 操作對象時都在這裡操作
    * all
    * get
    * filter
    * exclude
    * values
    * ...
    * 切片
        * 不支持負數
        * 相當於limit offset
        * 懶查詢
            * 觀察者模型
            * 發布者訂閱者
            * 廣播
    * 獲取單一對象
        * get
            * 不存在拋出DoesNotExist
        * last
            * 存在多個拋出MultipleObjectReturn
        * first
            * 可能出現第一個和最後一個是同一個
            * 需要主動調整順序
    * 內置函數
        * count
        * exists
    * 條件
        * 屬性__操作符=值
    * 條件升級
        * F對象 : 用在模型自我屬性比較，支持算數運算
        * Q對象 : 條件封裝，支持邏輯運算

## 重要資料處理
* 邏輯字段
* is_delete
* 自定義Manager實現統一封裝
    * 重寫get_queryset