# 前言
fedora算是實驗體\
knoppix不需要安裝

LAMP  →  linux  apache  mysql  php

硬體設備文件\
IDE : /dev/hd[a-d]\
USB : /dev/sd[a-p]\
/dev/cdrom  /dev/sr0\
/dev/fd\
/dev/lp\
…


**安裝**
---
安裝時必須分區\
* 根目錄
* swap分區(記憶體2倍，不超過2G)  →  當快取用

推薦分區
* /boot

在root底下有默認文件\
安裝日誌
<font color=#FF0000>**/root/install.log**</font> \
儲存安裝在系統中的軟體以及其版本訊息\
<font color=#FF0000>**/root/install.log.syslog**</font>  儲存了安裝過程中留下的事件紀錄\
<font color=#FF0000>**/root/anaconda-ks.cfg**</font>   以kickstart配置文件格式紀錄安裝過程中設置的選項  →  用來搭配自動安裝

虛擬機器遠程登入管理工具

橋接  →  利用實體網卡進行網路連接\
NAT  →  通過虛擬網卡\
Host-only  →  只能和本機進行通訊

<font color=#FF00FF>ifconfig</font>  →  查詢ip \
<font color=#FF00FF>ifconfig</font> 硬體清單 'ip'  設置網路位址。\
該操作為臨時性，若需要永久更動ip位置需要更改配置文件

<font color=#FF0000>**linux會因為版本不同會不支援遠程root直接登入，但centos可以**</font>

關於linux的先備知識:
<font color=#FFD700>linux嚴格區分大小寫</font>\
linux所有內容都是以文件儲存，包括硬體。若沒有寫在文件中，僅使用命令來生效\
的話，則視為臨時生效。\
linux不靠副檔名來區分，靠文件權限。\
副檔名是給使用者方便管理使用，即便沒有副檔名依舊能正常使用。\
linux的儲存設備都必須掛載才能使用，包括USB等外接硬體\
windows下的程式不能直接在linux中安裝執行。

root  tmp為練習用時較好的建立文件位置。

<font color=#FFD700>遠程服務器不准關機，只能重啟。</font>\
重啟時應關閉服務。\
不要再服務器高峰期進行高負載指令。\
遠程配置防火牆不要把自已踢出服務器。(定時將防火牆設置清空，等確定沒問題再正式)\
指定合理的密碼規範並定期更新。\
合理分配權限。(需求狀態下的最小權限)\
定期備份重要數據即日誌。

指令格式: 命令 [<font color=#FFAD86>-選項</font>] [<font color=#FFAD86>參數</font>] 大多命令選項可以對調，少數命令不行。

# <font color=#FFD700>命令格式與目錄處理命令</font>
## <font color=#FF0000>ls -lad 路徑</font>
* -a 顯示所有文件，包括隱藏文件
* -l (長格式顯示)詳細訊息顯示
* -d (direct)查看目錄屬性，而非裡面檔案的屬性
* -h (human)將資訊轉換為人類方便閱讀的格式，通用選項
* -i (id)會回傳文件的id

<font color=#FF0000>.</font>開頭的為隱藏文件，設計初衷是為了不讓使用者碰系統文件
權限後的數字代表被調用的計數，後方的參數分別為\
<font color=#FF0000>所有者  所有組  檔案大小 最後修改時間 檔名</font>\
所有者為<font color=#FF0000>檔案擁有者</font>，所有組的使用者能使用該檔案

權限說明:共有10個字符，第一個為文件類型，剩下9個每三個做為一組來看\
分別是  <font color=#FF0000>所有者權限  所屬組權限  其他人權限  (u g o)</font>\
文件類型 :  <font color=#FF0000>-</font> 文件，<font color=#FF0000>d</font> 資料夾，<font color=#FF0000>l</font> 軟連結\
權限類型 : <font color=#FF0000>r</font> 讀取，<font color=#FF0000>w</font> 寫入，<font color=#FF0000>x</font> 執行\
執行權限為程式或腳本才會有

# <font color=#FFD700>目錄處理命令</font> 
創建資料夾\
<font color=#FF0000>mkdir 資料夾名稱</font> : (make directories)建立資料夾，可同時建立多個，以空格作為分隔
* -p 遞迴創建: (parents)如果沒有目標路徑沒有指定資料夾，則會自動創建
  
<font color=#FF0000>cd 目標資料夾</font>  : (change directories) 更換當前資料夾

<font color=#FF0000>pwd</font> : (print working directory) 回傳當前路徑

<font color=#FF0000>rmdir 資料夾名稱</font> : (remove empty directries) 刪除<font color=#FF0000>空資料夾</font> 

<font color=#FF0000>cp -rp [原資料夾或文件, …] 目標目錄</font> : (copy)複製檔案或目錄
* -r 複製資料夾
* -p 保留文件屬性(包含最後修改時間等等屬性)

目標目錄可以設置複製後檔案(或資料夾)的名稱

<font color=#FF0000>mv [原資料夾或文件, …] 目標目錄</font> : (move)移動檔案或目錄支持相對路徑操作，也可以拿來做檔案改名的動作(移動到當前位置)

<font color=#FF0000>rm -rf [文件或目錄]</font> : (remove)刪除目錄
* -r 刪除目錄
* -f 強制刪除

<font color=#FF7575>**!!! linux並沒有資源回收桶的概念，刪除就刪除了!!**</font> 


# <font color=#FFD700>4.1 文件處理命令</font>

<font color=#FF0000>touch [文件名, …]</font> : 建立空文件，若需要空格作為檔案名稱，則需要使用雙引號包起來

<font color=#FF0000>cat [文件名]</font> : 瀏覽文件，不適合瀏覽
* -n 顯示行號

<font color=#FF0000>tac [文件名]</font> : 反向顯示文件內容

<font color=#FF0000>more [文件名]</font> : 分頁顯示文件內容，使用空格或F進行翻頁。enter進行換行，Q退出。

<font color=#FF0000>less [文件名]</font> : 分頁顯示文件內容，pageup往前翻，↑往上翻行，/搜索關鍵字進行查詢，使用n往下找指定關鍵字。

<font color=#FF0000>head [-n num] [文件名]</font> : 瀏覽文件前幾行
* -n 顯示幾行(默認10行)
* 
<font color=#FF0000>tail [-n num] [文件名]</font> : 瀏覽文件最後幾行
* 	-n 顯示幾行(默認10行)
*	-f 動態檢視文件末尾內容

<font color=#FF0000>/var/log/message</font> 是日誌功能，可以拿來作練習用。

<font color=#FFD700>連接命令</font>\
<font color=#FF0000>ln -s 原文件 目標文件</font> : (link)生成連接文件
* -s 建立軟捷徑

軟連接的權限，單純只是路徑指向，所以所有人都能使用

硬連接的特點，<font color=#FF0000>拷貝cp -p+同步更新</font>

<font color=#00CACA>echo 文件 >> 內容</font> → 會將內容寫入文件最後一行

判斷硬連接  →  從文件index來判斷，硬連接的index會與原文件相同(ls -i)\
由於修改的文件都是同一個文件節點，所以能夠做到同步更新。可以拿來做備份。

<font color=#FF0000>**!!硬連接不能跨其他硬碟分區。不能針對目錄使用。**</font>

# <font color=#FFD700>4.2 權限管理命令</font>
<font color=#FF0000>chmod {ugoa} {+-=} {rwx} 文件或目錄 </font> : 
(change the permissions mode of a file) 改變文件或目錄權限
* -R 遞迴更改，經過路徑的所有檔案都更改成該權限

<font color=#FF0000>ugoa</font> 分別是<font color=#FF0000>所有者  所屬組  其他人  全部</font>\
<font color=#FF0000>+</font>為增加權限  <font color=#FF0000>-</font>為刪除權限  <font color=#FF0000>=</font>為強制設置權限

權限設置也能使用數字來表示
<font color=#FF0000>r</font> = <font color=#00FFFF>4</font>,
<font color=#FF0000>w</font> = <font color=#00FFFF>2</font>,
<font color=#FF0000>x</font> = <font color=#00FFFF>1</font>\
例如，5=4+1為設置<font color=#FF0000>r-x</font>權限，7=4+2+1為設置<font color=#FF0000>rwx</font>權限

||權限|對於文件含意|對於資料夾含意|
|:-:|:-:|:-:|:-:|
|r|讀取權限|可查看文件內容|可列出目錄中內容|
|w|寫權限|可修改文件內容|可在目錄中創建、刪除文件|
|x|執行權限|可執行文件|可進入目錄|

<font color=#00CACA>useradd 用戶名稱</font> → 新增帳戶

<font color=#00CACA>passwd 用戶名稱 : 設置密碼</font> → 為帳戶設置密碼

<font color=#FF0000>chown 用戶 文件或目錄 </font> : (change file ownership)改變<font color=#FF0000>文件或目錄</font>所有者為<font color=#FF0000>用戶</font>

<font color=#FF0000>**!! 只有root才能更改檔案所有者**</font>

<font color=#FF0000>chgrp 用戶組 文件或目錄 </font> : (change file group ownership)改變<font color=#FF0000>文件或目錄</font>所屬組為<font color=#FF0000>用戶組</font>

<font color=#00CACA>groupadd 用戶組</font> → 新增用戶組

<font color=#FF0000>umask [-S]</font> : (the ueser file-creation mask)顯示、設置文件的預設權限。

<font color=#FF0000>**touch建立的文件，是沒有可執行權限的**</font>

umask回傳的數字 : 0022
0(特殊權限) 022數於正常權限(ugo) → 解讀方式為777-022=755
而755才是我們所熟悉的ugo權限(rwxr-xr-x)

設置預設權限:
1. 先將權限數字設定好
2. 再拿<font color=#FF0000>777</font>將權限數字做扣除
3. 輸入<font color=#FF0000>umask 扣除後數字</font>即可

總結:
1. **只有檔案所有者及root可以更改文件權限(chmod)**
2. **只有root可以更改文件所有者(chown)**

# <font color=#FFD700>4.3 文件搜索命令</font>

**!!盡量少用搜索，會盡量大量的系統資源**

<font color=#FF0000>find [搜索範圍] [條件]</font> : 文件搜索
* -name 檔名 : 搜尋檔名(完全符合才會被找出來)，如果想要進行包含檔名，則使用<font color=#FF0000>通用字符*及?</font>進行搜索(\*init\*)，大小寫嚴格區分
* -iname 檔名 : 搜尋檔名，不區分大小寫。
* -size (+-)大小(0.5kb) : 
  * +大小:大於  -大小:小於  大小:等於
  * 1格是0.5kb
* -user 用戶名 : 根據所有者做搜尋
* -group 組別 : 根據所有組做搜尋
* -cmin -n : 搜尋最近n分鐘內被修改過屬性的文件
    * -n n分鐘之內 +n 超過n分鐘 n等於5分鐘
    * -amin : 訪問時間(access)
    * -cmin : 文件屬性(change)
    * -mmin : 文件內容(modify)
    * -exec/-ok 命令 {}\; : 搜尋完之後執行命令
        * {}\;對搜尋結果進行操作
        * -ok會逐一詢問
    * -type : 根據文件類型搜尋
        * f文件  d目錄  l連結文件
    * -inum : 根據i節點搜尋
    * 綜合條件
        * -a 兩個條件同時滿足
        * -o 條件任一滿足
ex. find path -size -2048000 -a +1024000

<font color=#FF0000>whereis 指令或配置文件</font> : 搜尋命令的簡短訊息
* -b : 只找可執行檔(二進位)格式的檔案
* -m : 只找在說明檔 manual 路徑下的檔案
* -s : 只找source來源檔案
* -u : 搜尋不再上述三個選項的其他特殊檔案
* **不加選項則會全部列出**

<font color=#FF0000>which 指令</font> : 會根據<font color=#FF0000>"PATH"</font>這個環境變數去搜尋"執行檔"的檔名，而且which後面接的是<font color=#FF0000>"完整檔名"</font>。若加上-a，可以列出所有能找到的同名執行檔，而非第一個
* -a : 將所有由PATH目錄中可以找到得指令都列出來

比較:\
whereis 和locate是利用資料庫來搜尋資料，而非搜尋硬碟。此在找資料的速度相當快速，真的找不到才會動用find。
* which 找<font color=#FF0000>執行檔</font>
* whereis 找<font color=#FF0000>特定檔案</font>
* find以<font color=#FF0000>整個硬碟的資料</font>為主來做搜尋

# <font color=#FFD700>4.4 幫助命令</font>

<font color=#FF0000>man [幫助編號] 指令或配置文件</font> : (manual)搜尋目標的配置訊息。
* / 用來搜尋字元
* 查詢配置文件只要輸入檔名就好，否則會得到文件內容
* 尋找配置格式
* man後面的編號:<font color=#FF0000>1</font>代表命令的幫助，<font color=#FF0000>5</font>代表配置文件的幫助，數字越小優先權越高

<font color=#FF0000>whatis 命令</font> : 查詢指令的簡短訊息

<font color=#FF0000>apropos 配置文件</font> : 查詢配置文件的簡短訊息

如果只是想查看命令選項，則在指令的後方輸入<font color=#FF0000>"--help"</font>，就會跳出指令的所有選項

<font color=#FF0000>date</font> : 查詢及更改時間。

<font color=#FF0000>info</font> : 也是一種幫助命令

<font color=#FF0000>help 命令</font> : 獲得shell內置命令的幫助訊息。(判斷是否為內置命令→找不到命令檔案，無法使用man來獲取訊息)

# <font color=#FFD700>4.5 用戶管理命令</font>

<font color=#FF0000>useradd 用戶名稱</font> : 增加新用戶(路徑:/usr/sbin/useradd)

<font color=#FF0000>passwd 用戶名稱</font> : 為用戶設置密碼(路徑:/usr/bin/passwd)
* __**root不會受限於密碼過於簡單的限制，但一般使用者會。**__
* root帳號可以該改其他用戶的密碼，但一般使用者只能改變自己的。

<font color=#FF0000>who</font> : 查看登陸用戶訊息(路徑:/usr/bin/who)
* 格式 : <font color=#FF0000>用戶名 登陸終端 登陸時間 登陸地址</font>
    * tty : 本機登陸
    * pts/終端 : 遠端登陸

<font color=#FF0000>w</font> : 查看登陸用戶詳細訊息(路徑:/usr/bin/w)
* 格式
    * USR : 帳號名稱
    * TTY : 登入模式
    * FROM : 當路地址
    * LOGIN : 登陸時間
    * IDLE : 閒置時間
    * JCPU : 累積占用CPU時間
    * PCPU : 占用CPU持間
    * WHAT : 當前操作

<font color=#00FFFF>uptime</font> : 查看運行狀態
* 格式: 登陸時間  up  以運行時間  登錄人數 load: 5分鐘  10分鐘  15分鐘的平均負載

# <font color=#FFD700>4.6 壓縮及解壓縮</font>

壓縮格式:
* .gz
* .tar
* .zip
* .bz2

<font color=#FF0000>gzip 文件</font> : (GNU zip)壓縮文件(命令路徑:/bin/gzip)
* __**無法壓縮資料夾**__
* 壓縮完畢後，原檔案會被刪除

<font color=#FF0000>gunzip 壓縮文件</font> : (GNU zip)解壓縮文件(命令路徑:/bin/gunzip)

<font color=#FF0000>tar 選項 打包後文件名 目錄</font> : 打包資料夾(命令路徑:/bin/tar)
* -c/x 打包/解包
* -v 顯示詳細訊息
* -f 指定文件名
* -z 打包同時壓縮(解包同時解壓縮) : 此時打包後文件名變成打包及壓縮後文件名
* -j 打包成bzip2文件(-cjf→壓縮  -xjf→解壓縮)

<font color=#FF0000>zip 選項 壓縮後文件名 文件或目錄</font> : 壓縮文件或目錄(命令路徑:/usr/bin/zip)
* -r 壓縮資料夾
* 能保留原文件

__**!!linux的壓縮檔在windows系統下大多都能解壓縮，但windows壓縮檔不見得能被linux解壓縮**__

<font color=#FF0000>unzip 壓縮文件名</font> : 解壓縮zip文件或目錄(命令路徑:/usr/bin/unzip)

<font color=#FF0000>bzip2 選項 文件</font> : 壓縮文件(命令路徑:/usr/bin/bzip2)
* -k 產生壓縮文件後並保留原文件
* 壓縮檔案比較大的檔案使用效果較佳

<font color=#FF0000>bunzip2 選項 文件</font> : 解壓縮bzip2文件(命令路徑:/usr/bin/bunzip2)
* -k 解壓縮後保留原文件