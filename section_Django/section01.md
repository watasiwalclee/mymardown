# Hello Django

部屬平台建議使用Ubuntu

虛擬化技術
* 虛擬機
* 虛擬容器
    * Docker
* 虛擬環境
    * python虛擬環境


採用**MVC**或**MTV**架構
唯一種軟體設計典範，
![MTV](https://2.bp.blogspot.com/-PsIlp7IzG70/Vdgg051LuZI/AAAAAAAA6BE/DLn4eEFnMe0/s1600/mvc.png)

優點:降低每個模型的耦合姓，方便變更，更容易變更程式

MVC
* model : 用於封裝與應用程式的業務邏輯相關處理方法
* View : 負責資料顯示及呈現，是對用戶的直接輸出
* Controller : 負責從用戶端收集用戶輸入，可以看成提供view的反向功能，主要處理用戶交互。

MTV
* Model : 負責業務對象和資料庫的對象
* View : 負責業務邏輯，並在適當的時候調用template跟model
* template : 負責把葉面展示給用戶

!! Django有url路由器，主要用來將每一個url頁面請求

虛擬環境啟動
* windows → Scripts\activate
* linux → source 虛擬環境\bin\activate
    * 離開環境 → deactivate

Django操作:
* 建立專案 : **django-admin startproject 專案名稱**
* 建立應用程式 : **python manage.py startapp 應用程式名稱**
* 啟動專案 : **python manage.py runserver**
* 遷移設置 : **python manage.py migrate**

Django專案結構:

djangoproj\
├───manage.py\
└───專案名稱\
  settings.py\
  urls.py\
  wsgi.py\
  \_\_init\_\_.py

* manage.py 為管理網站的腳本，可以透過他建立一個web server
* setting.py包含網站配置
* url.py 包含路由配置

## setting.py
* BASE_DIR : 專案所在目錄
* SECRET_KEY : 
    * 開發環境
    * 測試
    * 演示
    * 正式上線(生產環境)
* debug : 
* allowed_host : 允許連線IP地址， **["*\"]** 為全允許
* installed_apps : Django內建應用程式
* middleware : 中間件
* root_url_conf : 根網址
* templates : 模板
* wsgi_application : 
* database : 資料庫相關設置
* AUTH_PASSWORD_VALIDATORS : 密碼驗證器相關配置
* LANGUAGE_CODE : 語言編碼(繁中: zh-Hant)
* time_zone : 時區
* use_I18N : 
* use_L18N : 

