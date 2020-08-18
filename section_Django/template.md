# Template 簡介

模板是能幫助開發者快速生成頁面給用戶觀看的工具

* shortcuts.render本質上也是HttpResponse，他也是把模板跟context資料渲染成字串。

* 模板主要有兩個部分
    * HTML靜態碼
    * 動態插入代碼 → 變數，template language
* 模板中的變數
    * 遵守標示符規則
    * 語法 {{ var }}
    * 如果變數不存在，則插入空字串
* 點語法
    * grade.g_name
    * grade.0.g_name  0為索引
    * 若被傳遞的對象是字典，則調用值時不應使用 **[]** 進行調用，而使用點 **.** 來呼叫  (student['name']→student.name)
* 傳遞參數比較複雜，不建議

* {% empty %} → 空值時顯示區塊

# forloop
* {{ forloop.counter }} 計數器
* {{ forloop.counter0 }} 計數器，從0開始
* {{ forloop.revcounter }} 倒數計數器
* {{ forloop.revcounter0 }} 倒數計數器，倒數至0
* {{ forloop.first }} 回傳對象是否為第一個
* {{ forloop.last }} 回傳對象是否為最後一個

# 註解
1. 單行註解
    * {# 註解內容 #}
2. 多行註解
    * {% comment %} ... {% endcomment %}
3. 乘除
    * {% widthratio 數 分母 分子 %}
4. 整除
    * {% if num|divisbleby:分母 %}
    * ex. {% if forloop.counter|divisbleby:2 %}

* 模板語言的註解被傳遞給使用者時不會被顯示。

# 邏輯控制
* {% ifequal value1 value2 %} ... {% endifequal %}
* {% ifnotequal value1 value2 %} ... {% endifnotequal %}

# 過濾器
* {{ var\|過濾器 }}
* {{ var\|add:2}}...

## HTML轉譯
* {{ var\|safe }} 將字串轉譯成html。請謹慎使用，坑很多
* {% autoescape off(on) %} ... {% endautoescape %}
    * off 開啟渲染
    * on 關閉渲染

# 模板繼承

* block
    * {% block 名稱 %} ... {% endblock %}
    * 首次出現為規劃，第二次出現代表填空規劃，第三次出現代表覆蓋之前的規劃
        * 若不想覆蓋，可以加上{{ block.super }}
* extends
    * {% extends 模板名稱 %}
    * 繼承

* block + extends → 化整為零
* include 包含
    * 可以將頁面作為頁面的一部分，嵌入到其他頁面中
* include + block → 由零聚一
* 能用block + extends解決的就盡量不要使用include
* 如果繼承自父模板，子模板自己重寫頁面結構是不會生效的，只能在既有的區塊中進行擴充。

# 靜態資源

使用css等靜態資源必須在setting中設定 **STATICFILES_DIRS=[路徑]**

* 在Django中，動態資源與靜態資源是分開的
* 一般放在static資料夾中
* 在模板中使用
    1. 先加載靜態資源{% load static %}
    2. 使用 {% static 'css檔案相對路徑' %}
    * 僅在debug模式才能使用
    * 以後需要自己單獨處理

