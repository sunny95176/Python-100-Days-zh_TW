## Django 2.x實戰(01) - 快速上手

Web開發的早期階段，開發者需要手動編寫每個頁面，例如一個新聞入口網站，每天都要修改它的HTML頁面，這樣隨著網站規模和體量的增大，這種方式就變得極度糟糕。為了解決這個問題，開發人員想到了用外部程式來為Web伺服器生成動態內容，也就是說HTML頁面以及頁面中的動態內容不再通過手動編寫而是通過程式自動生成。最早的時候，這項技術被稱為CGI（公共閘道器介面），當然隨著時間的推移，CGI暴露出的問題也越來越多，例如大量重複的樣板程式碼，總體效能較為低下等，因此在呼喚新的英雄的時代，PHP、ASP、JSP這類Web應用開發技術在上世紀90年代中後期如雨後春筍般湧現。通常我們說的Web應用是指通過瀏覽器來訪問網路資源的應用程式，因為瀏覽器的普及性以及易用性，Web應用使用起來方便簡單，而且在應用更新時使用者通常不需要做任何的處理就能使用更新後的應用，而且也不用關心使用者到底用的是什麼作業系統，甚至不用區分是PC端還是移動端。

### Web應用機制和術語

下圖向我們展示了Web應用的工作流程，其中涉及到的術語如下表所示。

![](./res/web-application.png)

> 說明：相信有經驗的讀者會發現，這張圖中其實還少了很多東西，例如反向代理伺服器、資料庫伺服器、防火牆等，而且圖中的每個節點在實際專案部署時可能是一組節點組成的叢集。當然，如果你對這些沒有什麼概念也不要緊，後續的課程中我們會為大家進行講解。

| 術語          | 解釋                                                         |
| ------------- | ------------------------------------------------------------ |
| **URL/URI**   | 統一資源定位符/統一資源識別符號，網路資源的唯一標識            |
| **域名**      | 與Web伺服器地址對應的一個易於記憶的字串名字                |
| **DNS**       | 域名解析服務，可以將域名轉換成對應的IP地址                   |
| **IP地址**    | 網路上的主機的身份標識，通過IP地址可以區分不同的主機         |
| **HTTP**      | 超文字傳輸協議，應用層協議，全球資訊網資料通訊的基礎             |
| **反向代理**  | 代理客戶端向伺服器發出請求，然後將伺服器返回的資源返回給客戶端 |
| **Web伺服器** | 接受HTTP請求，然後返回HTML檔案、純文字檔案、影象等資源給請求者 |
| **Nginx**     | 高效能的Web伺服器，也可以用作[反向代理](https://zh.wikipedia.org/wiki/%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86)，[負載均衡](https://zh.wikipedia.org/wiki/%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1) 和 [HTTP快取](https://zh.wikipedia.org/wiki/HTTP%E7%BC%93%E5%AD%98) |

#### HTTP協議

這裡我們稍微費一些筆墨來談談上面提到的HTTP。HTTP（超文字傳輸協議）是構建於TCP（傳輸控制協議）之上應用級協議，它利用了TCP提供的可靠的傳輸服務實現了Web應用中的資料交換。按照維基百科上的介紹，設計HTTP最初的目的是為了提供一種釋出和接收[HTML](https://zh.wikipedia.org/wiki/HTML)頁面的方法，也就是說這個協議是瀏覽器和Web伺服器之間傳輸的資料的載體。關於這個協議的詳細資訊以及目前的發展狀況，大家可以閱讀阮一峰老師的[《HTTP 協議入門》](http://www.ruanyifeng.com/blog/2016/08/http.html)、[《網際網路協議入門》](http://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)系列以及[《圖解HTTPS協議》](http://www.ruanyifeng.com/blog/2014/09/illustration-ssl.html)進行了解，下圖是我於2009年9月10日凌晨4點在四川省網路通訊技術重點實驗室用開源協議分析工具Ethereal（抓包工具WireShark的前身）擷取的訪問百度首頁時的HTTP請求和響應的報文（協議資料），由於Ethereal擷取的是經過網路介面卡的資料，因此可以清晰的看到從物理鏈路層到應用層的協議資料。

HTTP請求（請求行+請求頭+空行+[訊息體]）：

![](./res/http-request.png)

HTTP響應（響應行+響應頭+空行+訊息體）：

![](./res/http-response.png)

>  說明：但願這兩張如同泛黃的照片般的截圖幫助你大概的瞭解到HTTP是一個怎樣的協議。

### Django概述

Python的Web框架有上百個，比它的關鍵字還要多。所謂Web框架，就是用於開發Web伺服器端應用的基礎設施（通常指封裝好的模組和一系列的工具）。事實上，即便沒有Web框架，我們仍然可以通過socket或[CGI](https://zh.wikipedia.org/wiki/%E9%80%9A%E7%94%A8%E7%BD%91%E5%85%B3%E6%8E%A5%E5%8F%A3)來開發Web伺服器端應用，但是這樣做的成本和代價在實際開發中通常是不能接受的。通過Web框架，我們可以化繁為簡，同時降低建立、更新、擴充套件應用程式的工作量。Python的Web框架中比較有名的有：Flask、Django、Tornado、Pyramid、Bottle、Web2py、web.py等。

在基於Python的Web框架中，Django是所有重量級選手中最有代表性的一位，開發者可以基於Django快速的開發可靠的Web應用程式，因為它減少了Web開發中不必要的開銷，對常用的設計和開發模式進行了封裝，並對MVC架構提供了支援（MTV）。許多成功的網站和App都是基於Django框架構建的，國內比較有代表性的網站包括：知乎、豆瓣網、果殼網、搜狐閃電郵箱、101圍棋網、海報時尚網、背書吧、堆糖、手機搜狐網、咕咚、愛福窩、果庫等。

![](./res/mvc.png)

Django誕生於2003年，它是一個在真正的應用中成長起來的專案，由勞倫斯出版集團旗下線上新聞網站的內容管理系統（CMS）研發團隊編寫（主要是Adrian Holovaty和Simon Willison），以比利時的吉普賽爵士吉他手Django Reinhardt來命名，在2005年夏天作為開源框架釋出。使用Django能用很短的時間構建出功能完備的網站，因為它代替程式設計師完成了所有乏味和重複的勞動，剩下真正有意義的核心業務給程式設計師，這一點就是對DRY（Don't Repeat Yourself）理念的最好踐行。

### 快速上手

#### 準備工作

1. 檢查Python環境：Django 1.11需要Python 2.7或Python 3.4以上的版本；Django 2.0需要Python 3.4以上的版本。

   ```Shell
   $ python3 --version
   ```

   ```Shell
   $ python3
   >>> import sys
   >>> sys.version
   >>> sys.version_info
   ```

2. 建立專案資料夾並切換到該目錄，例如我們要例項一個OA（辦公自動化）專案。

   ```Shell
   $ mkdir oa
   $ cd oa
   ```

3. 建立並激活虛擬環境。

   ```Shell
   $ python3 -m venv venv
   $ source venv/bin/activate
   ```
   > 注意：Windows系統下是執行`venv/Scripts/activate.bat`批處理檔案。

4. 更新包管理工具pip。

   ```Shell
   (venv)$ python -m pip install --upgrade pip
   ```
   > 注意：請注意終端提示符發生的變化，前面的`(venv)`說明我們已經進入虛擬環境，而虛擬環境下的python和pip已經是Python 3的直譯器和包管理工具了。

5. 安裝Django。

   ```Shell
   (venv)$ pip install django
   ```

   或指定版本號來安裝對應的Django的版本。

   ```Shell
   (venv)$ pip install django==1.11
   ```

6. 檢查Django的版本。

   ```Shell
   (venv)$ python -m django --version
   (venv)$ django-admin --version
   ```

   ```Shell
   (venv)$ python
   >>> import django
   >>> django.get_version()
   ```
   下圖展示了Django版本和Python版本的對應關係，在我們的專案中我們選擇了最新的Django 2.0的版本。

   | Django版本 | Python版本              |
   | ---------- | ----------------------- |
   | 1.8        | 2.7、3.2、3.3、3.4、3.5 |
   | 1.9、1.10  | 2.7、3.4、3.5           |
   | 1.11       | 2.7、3.4、3.5、3.6      |
   | 2.0        | 3.4、3.5、3.6           |

   > 說明：在建立這篇文章時Django 2.1版本尚未正式釋出，因此我們的教程使用了2.0.5版本。

7. 使用`django-admin`建立專案，專案命名為oa。

   ```Shell
   (venv)$ django-admin startproject oa .
   ```

   > 注意：上面的命令最後的那個點，它表示在當前路徑下建立專案。

   執行上面的命令後看看生成的檔案和資料夾，它們的作用如下所示：

   - `manage.py`： 一個讓你用各種方式管理 Django 專案的命令列工具。
   - `oa/__init__.py`：一個空檔案，告訴 Python 這個目錄應該被認為是一個 Python 包。
   - `oa/settings.py`：Django 專案的配置檔案。
   - `oa/urls.py`：Django 專案的 URL 宣告，就像你網站的“目錄”。
   - `oa/wsgi.py`：作為你的專案的執行在 WSGI 相容的Web伺服器上的入口。

8. 啟動伺服器執行專案。

   ```Shell
   (venv)$ python manage.py runserver
   ```

   在瀏覽器中輸入<http://127.0.0.1:8000>訪問我們的伺服器，效果如下圖所示。

   ![](./res/django-index-1.png)


   > 說明1：剛剛啟動的是Django自帶的用於開發和測試的伺服器，它是一個用純Python編寫的輕量級Web伺服器，但它並不是真正意義上的生產級別的伺服器，千萬不要將這個伺服器用於和生產環境相關的任何地方。

   > 說明2：用於開發的伺服器在需要的情況下會對每一次的訪問請求重新載入一遍Python程式碼。所以你不需要為了讓修改的程式碼生效而頻繁的重新啟動伺服器。然而，一些動作，比如新增新檔案，將不會觸發自動重新載入，這時你得自己手動重啟伺服器。

   > 說明3：可以通過`python manage.py help`命令檢視可用命令列表；在啟動伺服器時，也可以通過`python manage.py runserver 1.2.3.4:56789`來指定繫結的IP地址和埠。

   > 說明4：可以通過Ctrl+C來終止伺服器的執行。

9. 接下來我們進入專案目錄oa並修改配置檔案settings.py，Django是一個支援國際化和本地化的框架，因此剛才我們看到的預設首頁也是支援國際化的，我們將預設語言修改為中文，時區設定為東八區。

   ```Shell
   (venv)$ cd oa
   (venv)$ vim settings.py
   ```

   ```Python
   # 此處省略上面的內容
   
   # 設定語言程式碼
   LANGUAGE_CODE = 'zh-hans'
   # 設定時區
   TIME_ZONE = 'Asia/Chongqing'
   
   # 此處省略下面的內容
   ```

10. 回到manage.py所在的目錄，重新整理剛才的頁面。

  ```Shell
  (venv)$ cd ..
  (venv)$ python manage.py runserver
  ```

  ![](./res/django-index-2.png)

#### 動態頁面

1. 建立名為hrs（人力資源系統）的應用（注：一個專案可以包含多個應用）。

   ```Shell
   (venv)$ python manage.py startapp hrs
   ```

   執行上面的命令會在當前路徑下建立hrs目錄，其目錄結構如下所示：

   - `__init__.py`：一個空檔案，告訴 Python 這個目錄應該被認為是一個 Python 包。
   - `admin.py`：可以用來註冊模型，讓Django自動建立管理介面。
   -  `apps.py`：當前應用的配置。
   - `migrations`：存放與模型有關的資料庫遷移資訊。
     - `__init__.py`：一個空檔案，告訴 Python 這個目錄應該被認為是一個 Python 包。
   - `models.py`：存放應用的資料模型，即實體類及其之間的關係（MVC/MVT中的M）。
   - `tests.py`：包含測試應用各項功能的測試類和測試函式。
   - `views.py`：處理請求並返回響應的函式（MVC中的C，MVT中的V）。
2. 進入應用目錄修改檢視檔案views.py。

   ```Shell
   (venv)$ cd hrs
   (venv)$ vim views.py
   ```

   ```Python
   from django.http import HttpResponse
   
   
   def index(request):
       return HttpResponse('<h1>Hello, Django!</h1>')
   
   ```

3. 在應用目錄建立一個urls.py檔案並對映URL。

   ```Shell
   (venv)$ touch urls.py
   (venv)$ vim urls.py
   ```

   ```Python
   from django.urls import path
   
   from hrs import views
   
   urlpatterns = [
       path('', views.index, name='index'),
   ]
   ```
   > 說明：上面使用的path函式是Django 2.x中新新增的函式，除此之外還有re_path是支援正則表示式的URL對映函式；Django 1.x中是用url函式來設定URL對映。

4. 切換到專案目錄，修改該目錄下的urls.py檔案，對應用中設定的URL進行合併。

   ```Shell
   (venv) $ cd ..
   (venv) $ cd oa
   (venv) $ vim urls.py
   ```

   ```Python
   from django.contrib import admin
   from django.urls import path, include
   
   urlpatterns = [
       path('admin/', admin.site.urls),
       path('hrs/', include('hrs.urls')),
   ]
   ```

5. 啟動專案並訪問應用。

   ```Shell
   (venv)$ cd ..
   (venv)$ python manage.py runserver
   ```

   在瀏覽器中訪問<http://localhost:8000/hrs>。

   > 說明：如果想實現遠端訪問，需要先確認防火牆是否已經打開了8000埠，而且需要在配置檔案settings.py中修改ALLOWED_HOSTS的設定，新增一個'*'表示允許所有的客戶端訪問Web應用。

6. 修改views.py生成動態內容。

   ```Shell
   (venv)$ cd hrs
   (venv)$ vim views.py
   ```

   ```Python
   from django.http import HttpResponse
   
   from io import StringIO
   from random import randrange
   
   fruits = ['蘋果', '草莓', '榴蓮', '香蕉', '葡萄', '山竹', '藍莓', '西瓜']
   
   
   def index(request):
       output = StringIO()
       output.write('<html>\n')
       output.write('<head>\n')
       output.write('\t<meta charset="utf-8">\n')
       output.write('\t<title>首頁</title>')
       output.write('</head>\n')
       output.write('<body>\n')
       output.write('\t<h1>Hello, world!</h1>\n')
       output.write('\t<hr>\n')
       output.write('\t<ol>\n')
       for _ in range(3):
           rindex = randrange(0, len(fruits))
           output.write('\t\t<li>' + fruits[rindex]  + '</li>\n')
       output.write('\t</ol>\n')
       output.write('</body>\n')
       output.write('</html>\n')
       return HttpResponse(output.getvalue())
   
   ```

#### 使用檢視模板

上面通過拼接HTML程式碼的方式生成動態檢視的做法在實際開發中是無能接受的，這一點我相信是不言而喻的。為了解決這個問題，我們可以提前準備一個模板頁，所謂模板頁就是一個帶佔位符的HTML頁面，當我們將程式中獲得的資料替換掉頁面中的佔位符時，一個動態頁面就產生了。

我們可以用Django框架中template模組的Template類建立模板物件，通過模板物件的render方法實現對模板的渲染。所謂的渲染就是用資料替換掉模板頁中的佔位符，Django框架通過shortcuts模組的快捷函式render簡化了渲染模板的操作，具體的用法如下所示。

1. 先回到manage.py檔案所在的目錄建立一個templates資料夾。

   ```Shell
   (venv)$ cd ..
   (venv)$ mkdir templates
   (venv)$ cd templates
   ```

2. 建立模板頁index.html。

   ```Shell
   (venv)$ touch index.html
   (venv)$ vim index.html
   ```
   ```HTML
   <!DOCTYPE html>
   <html lang="en">
   <head>
   	<meta charset="UTF-8">
   	<title>首頁</title>
   </head>
   <body>
   	<h1>{{ greeting }}</h1>
   	<hr>
   	<h3>今天推薦{{ num }}種水果是:</h3>
   	<ul>
   		{% for fruit in fruits %}
   		<li>{{ fruit }}</li>
   		{% endfor %}
   	</ul>
   </body>
   </html>
   ```
   注意在模板頁中我們使用了`{{ greeting }}`這樣的模板佔位符語法，也使用了`{% for %}`這樣的模板指令，如果對此不熟悉並不要緊，我們會在後續的內容中進一步的講解模板的用法。

3. 回到應用目錄，修改views.py檔案。

   ```Shell
   (venv)$ cd ..
   (venv)$ cd hrs
   (venv)$ vim views.py
   ```

   ```Python
   from django.shortcuts import render
   from random import randrange
   
   
   def index(request):
       fruits = ['蘋果', '香蕉', '草莓', '葡萄', '山竹', '楊梅', '西瓜', '榴蓮']
       start, end = 0, randrange(len(fruits))
       ctx = {
           'greeting': 'Hello, Django!',
           'num': end + 1,
           'fruits': fruits[start:end + 1]
       }
       return render(request, 'index.html', ctx)
   
   ```

   到此為止，我們還沒有辦法讓views.py中的render函式找到模板檔案index.html，為此我們需要修改settings.py檔案，配置模板檔案所在的路徑。

4. 切換到專案目錄修改settings.py檔案。

   ```Shell
   (venv)$ cd ..
   (venv)$ cd oa
   (venv)$ vim settings.py
   ```

   ```Python
   # 此處省略上面的內容
   
   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': [os.path.join(BASE_DIR, 'templates')],
           'APP_DIRS': True,
           'OPTIONS': {
               'context_processors': [
                   'django.template.context_processors.debug',
                   'django.template.context_processors.request',
                   'django.contrib.auth.context_processors.auth',
                   'django.contrib.messages.context_processors.messages',
               ],
           },
       },
   ]
   
   # 此處省略下面的內容
   ```

5. 重新執行專案並檢視結果。

   ```Shell
   (venv)$ cd ..
   (venv)$ python manage.py runserver
   ```

   ![](./res/runserver.png)

### 總結

至此，我們已經利用Django框架完成了一個非常小的Web應用，雖然它並沒有任何的實際價值，但是我們需要通過這個專案瞭解到Django框架的使用方式。當然，如果使用PyCharm的Professional版本，也可以通過PyCharm的建立專案的選項直接建立Django專案，使用PyCharm的好處在於編寫程式碼時可以獲得程式碼提示、錯誤修復、自動匯入等功能，從而提升開發效率，但是代價是需要支付對應的費用才能使用專業版的PyCharm，社群版的PyCharm中並未包含對Web框架的支援。

此外，學習Django最好的資料肯定是它的[官方文件](https://docs.djangoproject.com/zh-hans/2.0/)，除此之外圖靈社群最近出版的[《Django基礎教程》](http://www.ituring.com.cn/book/2630)也是非常適合初學者的讀物。 