## Django 2.x實戰(02) - 深入模型

在上一個章節中，我們提到了Django是一個基於MVC架構的Web框架，MVC架構要追求的是模型和檢視的解耦合，而其中的模型說得更直白一些就是資料，所以通常也被稱作資料模型。在實際的專案中，資料模型通常通過資料庫實現持久化操作，而關係型資料庫在很長一段時間都是持久化的首選方案，在我們的OA專案中，我們選擇使用MySQL來實現資料持久化。

### 配置關係型資料庫MySQL 

1. 進入oa資料夾，修改專案的settings.py檔案，首先將我們之前建立的應用hrs新增已安裝的專案中，然後配置MySQL作為持久化方案。

   ```Shell
   (venv)$ cd oa
   (venv)$ vim settings.py
   ```

   ```Python
   # 此處省略上面的程式碼
   
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'hrs',
   ]
   
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'oa',
           'HOST': 'localhost',
           'PORT': 3306,
           'USER': 'root',
           'PASSWORD': '123456',
       }
   }
   
   # 此處省略下面的程式碼
   ```

   在配置ENGINE屬性時，常用的可選值包括：

   - `'django.db.backends.sqlite3'`：SQLite嵌入式資料庫
   - `'django.db.backends.postgresql'`：BSD許可證下發行的開源關係型資料庫產品
   - `'django.db.backends.mysql'`：轉手多次目前屬於甲骨文公司的經濟高效的資料庫產品
   - `'django.db.backends.oracle'`：甲骨文公司的旗艦關係型資料庫產品

   其他的配置可以參考官方文件中[資料庫配置](https://docs.djangoproject.com/zh-hans/2.0/ref/databases/#third-party-notes)的部分。

   NAME屬性代表資料庫的名稱，如果使用SQLite它對應著一個檔案，在這種情況下NAME的屬性值應該是一個絕對路徑。如果使用其他關係型資料庫，還要配置對應的HOST（主機）、PORT（埠）、USER（使用者名稱）、PASSWORD（口令）等屬性。

2. 安裝MySQL客戶端工具，Python 3中使用PyMySQL，Python 2中用MySQLdb。

   ```Shell
   (venv)$ pip install pymysql
   ```

   如果使用Python 3需要修改**專案**的`__init__.py`檔案並加入如下所示的程式碼，這段程式碼的作用是將PyMySQL視為MySQLdb來使用，從而避免Django找不到連線MySQL的客戶端工具而詢問你：“Did you install mysqlclient? ”（你安裝了mysqlclient嗎？）。

   ```Python
   import pymysql
   
   pymysql.install_as_MySQLdb()
   ```

3. 執行manage.py並指定migrate引數實現資料庫遷移，為應用程式建立對應的資料表，當然在此之前需要**先啟動MySQL資料庫伺服器並建立名為oa的資料庫**，在MySQL中建立資料庫的語句如下所示。

   ```SQL
   drop database if exists oa;
   create database oa default charset utf8;
   ```

   ```Shell
   (venv)$ cd ..
   (venv)$ python manage.py migrate
   Operations to perform:
     Apply all migrations: admin, auth, contenttypes, sessions
   Running migrations:
     Applying contenttypes.0001_initial... OK
     Applying auth.0001_initial... OK
     Applying admin.0001_initial... OK
     Applying admin.0002_logentry_remove_auto_add... OK
     Applying contenttypes.0002_remove_content_type_name... OK
     Applying auth.0002_alter_permission_name_max_length... OK
     Applying auth.0003_alter_user_email_max_length... OK
     Applying auth.0004_alter_user_username_opts... OK
     Applying auth.0005_alter_user_last_login_null... OK
     Applying auth.0006_require_contenttypes_0002... OK
     Applying auth.0007_alter_validators_add_error_messages... OK
     Applying auth.0008_alter_user_username_max_length... OK
     Applying auth.0009_alter_user_last_name_max_length... OK
     Applying sessions.0001_initial... OK
   ```

4. 可以看到，Django幫助我們建立了10張表，這些都是使用Django框架需要的東西，稍後我們就會用到這些表。除此之外，我們還應該為我們自己的應用建立資料模型。如果要在hrs應用中實現對部門和員工的管理，我們可以建立如下所示的資料模型。

   ```Shell
   (venv)$ cd hrs
   (venv)$ vim models.py
   ```

   ```Python
   from django.db import models
   
   
   class Dept(models.Model):
       """部門類"""
       
       no = models.IntegerField(primary_key=True, db_column='dno', verbose_name='部門編號')
       name = models.CharField(max_length=20, db_column='dname', verbose_name='部門名稱')
       location = models.CharField(max_length=10, db_column='dloc', verbose_name='部門所在地')
   
       class Meta:
           db_table = 'tb_dept'
   
   
   class Emp(models.Model):
       """員工類"""
       
       no = models.IntegerField(primary_key=True, db_column='eno', verbose_name='員工編號')
       name = models.CharField(max_length=20, db_column='ename', verbose_name='員工姓名')
       job = models.CharField(max_length=10, verbose_name='職位')
       # 自參照完整性多對一外來鍵關聯
       mgr = models.ForeignKey('self', on_delete=models.SET_NULL, null=True, blank=True, verbose_name='主管編號')
       sal = models.DecimalField(max_digits=7, decimal_places=2, verbose_name='月薪')
       comm = models.DecimalField(max_digits=7, decimal_places=2, null=True, blank=True, verbose_name='補貼')
       dept = models.ForeignKey(Dept, db_column='dno', on_delete=models.PROTECT, verbose_name='所在部門')
   
       class Meta:
           db_table = 'tb_emp'
   
   
   ```
   > 說明：上面定義模型時使用了欄位類及其屬性，其中IntegerField對應資料庫中的integer型別，CharField對應資料庫的varchar型別，DecimalField對應資料庫的decimal型別，ForeignKey用來建立多對一外來鍵關聯。欄位屬性primary_key用於設定主鍵，max_length用來設定欄位的最大長度，db_column用來設定資料庫中與欄位對應的列，verbose_name則設定了Django後臺管理系統中該欄位顯示的名稱。如果對這些東西感到很困惑也不要緊，文末提供了欄位類、欄位屬性、元資料選項等設定的相關說明，不清楚的讀者可以稍後檢視對應的參考指南。

5. 通過模型建立資料表。

   ```Shell
   (venv)$ cd ..
   (venv)$ python manage.py makemigrations hrs
   Migrations for 'hrs':
     hrs/migrations/0001_initial.py
       - Create model Dept
       - Create model Emp
   (venv)$ python manage.py migrate
   Operations to perform:
     Apply all migrations: admin, auth, contenttypes, hrs, sessions
   Running migrations:
     Applying hrs.0001_initial... OK
   ```

   執行完資料遷移操作之後，可以在通過圖形化的MySQL客戶端工具檢視到E-R圖（實體關係圖）。

   ![](./res/er-graph.png)

### 在後臺管理模型

1. 建立超級管理員賬號。

   ```Shell
   (venv)$ python manage.py createsuperuser
   Username (leave blank to use 'hao'): jackfrued
   Email address: jackfrued@126.com
   Password: 
   Password (again): 
   Superuser created successfully.
   ```

2. 啟動Web伺服器，登入後臺管理系統。

   ```Shell
   (venv)$ python manage.py runserver
   ```

   訪問<http://127.0.0.1:8000/admin>，會來到如下圖所示的登入介面。

   ![](./res/admin-login.png)

   登入後進入管理員操作平臺。

   ![](./res/admin-welcome.png)

   至此我們還沒有看到之前建立的模型類，需要在應用的admin.py檔案中模型進行註冊。

3. 註冊模型類。

   ```Shell
   (venv)$ cd hrs
   (venv)$ vim admin.py
   ```

   ```Python
   from django.contrib import admin
   
   from hrs.models import Emp, Dept
   
   admin.site.register(Dept)
   admin.site.register(Emp)
   
   ```

   註冊模型類後，就可以在後臺管理系統中看到它們。

   ![](./res/admin-model.png)

4. 對模型進行CRUD操作。

   可以在管理員平臺對模型進行C（新增）R（檢視）U（更新）D（刪除）操作，如下圖所示。

   新增新的部門。

   ![](./res/admin-model-create.png)

   檢視所有部門。

   ![](./res/admin-model-read.png)

   更新和刪除部門。

   ![](./res/admin-model-delete-and-update.png)

5. 註冊模型管理類。

   再次修改admin.py檔案，通過註冊模型管理類，可以在後臺管理系統中更好的管理模型。

   ```Python
   from django.contrib import admin
   
   from hrs.models import Emp, Dept
   
   
   class DeptAdmin(admin.ModelAdmin):
   
       list_display = ('no', 'name', 'location')
       ordering = ('no', )
   
   
   class EmpAdmin(admin.ModelAdmin):
   
       list_display = ('no', 'name', 'job', 'mgr', 'sal', 'comm', 'dept')
       search_fields = ('name', 'job')
   
   
   admin.site.register(Dept, DeptAdmin)
   admin.site.register(Emp, EmpAdmin)
   
   ```

   ![](./res/admin-model-depts.png)

   ![](./res/admin-model-emps.png)

   為了更好的檢視模型資料，可以為Dept和Emp兩個模型類新增`__str__`魔法方法。

   ```Python
   from django.db import models
   
   
   class Dept(models.Model):
       """部門類"""
       
       # 此處省略上面的程式碼
       
       def __str__(self):
           return self.name
   
       # 此處省略下面的程式碼
   
   
   class Emp(models.Model):
       """員工類"""
       
       # 此處省略上面的程式碼
       
       mgr = models.ForeignKey('self', on_delete=models.SET_NULL, null=True, blank=True, verbose_name='直接主管')
       
       # 此處省略下面的程式碼
       
       # 此處省略上面的程式碼
   
       def __str__(self):
           return self.name
   
       # 此處省略下面的程式碼
   
   ```

   修改程式碼後重新整理檢視Emp模型的頁面，效果如下圖所示。

   ![](./res/admin-model-emps-modified.png)

### 使用ORM完成模型的CRUD操作

在瞭解了Django提供的模型管理平臺之後，我們來看看如何從程式碼層面完成對模型的CRUD（Create / Read / Update / Delete）操作。我們可以通過manage.py開啟Shell互動式環境，然後使用Django內建的ORM框架對模型進行CRUD操作。

```Shell
(venv)$ cd ..
(venv)$ python manage.py shell
Python 3.6.4 (v3.6.4:d48ecebad5, Dec 18 2017, 21:07:28) 
[GCC 4.2.1 (Apple Inc. build 5666) (dot 3)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
(InteractiveConsole)
>>> 
```

#### 新增

```Shell
>>>
>>> from hrs.models import Dept, Emp
>>> dept = Dept(40, '研發2部', '深圳')
>>> dept.save()
```

#### 更新

```Shell
>>>
>>> dept.name = '研發3部'
>>> dept.save()
```

#### 查詢

查詢所有物件。

```Shell
>>>
>>> Dept.objects.all()
<QuerySet [<Dept: 研發1部>, <Dept: 銷售1部>, <Dept: 運維1部>, <Dept: 研發3部>]>
```

過濾資料。

```Shell
>>> 
>>> Dept.objects.filter(name='研發3部') # 查詢部門名稱為“研發3部”的部門
<QuerySet [<Dept: 研發3部>]>
>>>
>>> Dept.objects.filter(name__contains='研發') # 查詢部門名稱包含“研發”的部門(模糊查詢)
<QuerySet [<Dept: 研發1部>, <Dept: 研發3部>]>
>>>
>>> Dept.objects.filter(no__gt=10).filter(no__lt=40) # 查詢部門編號大於10小於40的部門
<QuerySet [<Dept: 銷售1部>, <Dept: 運維1部>]>
```

查詢單個物件。

```Shell
>>> 
>>> Dept.objects.get(pk=10)
<Dept: 研發1部>
>>> Dept.objects.get(no=20)
<Dept: 銷售1部>
>>> Dept.objects.get(no__exact=30)
<Dept: 運維1部>
```

排序資料。

```Shell
>>>
>>> Dept.objects.order_by('no') # 查詢所有部門按部門編號升序排列
<QuerySet [<Dept: 研發1部>, <Dept: 銷售1部>, <Dept: 運維1部>, <Dept: 研發3部>]>
>>> Dept.objects.order_by('-no') # 查詢所有部門按部門編號降序排列
<QuerySet [<Dept: 研發3部>, <Dept: 運維1部>, <Dept: 銷售1部>, <Dept: 研發1部>]>
```

切片資料。

```Shell
>>>
>>> Dept.objects.order_by('no')[0:2] # 按部門編號排序查詢1~2部門
<QuerySet [<Dept: 研發1部>, <Dept: 銷售1部>]>
>>> Dept.objects.order_by('no')[2:4] # 按部門編號排序查詢3~4部門
<QuerySet [<Dept: 運維1部>, <Dept: 研發3部>]>
```

高階查詢。

```Shell
>>>
>>> Emp.objects.filter(dept__no=10) # 根據部門編號查詢該部門的員工
<QuerySet [<Emp: 喬峰>, <Emp: 張無忌>, <Emp: 張三丰>]>
>>> Emp.objects.filter(dept__name__contains='銷售') # 查詢名字包含“銷售”的部門的員工
<QuerySet [<Emp: 黃蓉>]>
>>> Dept.objects.get(pk=10).emp_set.all() # 通過部門反查部門所有的員工
<QuerySet [<Emp: 喬峰>, <Emp: 張無忌>, <Emp: 張三丰>]>
```

> 說明：由於員工與部門之間存在外來鍵關聯，所以也能通過部門反向查詢該部門的員工（從一對多關係中“一”的一方查詢“多”的一方），預設情況下反查屬性名是`類名小寫_set`（例子中的`emp_set`），當然也可以在建立模型時通過`related_name`指定反查屬性的名字。

#### 刪除

```Shell

```

最後，我們通過上面掌握的知識來實現部門展示以及根據部門獲取部門對應員工資訊的功能，效果如下圖所示，對應的程式碼可以訪問<>。

### Django模型最佳實踐

1. 正確的模型命名和關係欄位命名。
2. 設定適當的related_name屬性。
3. 用OneToOneField代替ForeignKeyField(unique=True)。
4. 通過遷移操作來新增模型。
5. 用NoSQL來應對需要降低正規化級別的場景。
6. 如果布林型別可以為空要使用NullBooleanField。
7. 在模型中放置業務邏輯。
8. 用ModelName.DoesNotExists取代ObjectDoesNotExists。
9. 在資料庫中不要出現無效資料。
10. 不要對QuerySet呼叫len函式。
11. 將QuerySet的exists()方法的返回值用於if條件。
12. 用DecimalField來儲存貨幣相關資料而不是FloatField。
13. 定義\_\_str\_\_方法。
14. 不要將資料檔案放在同一個目錄中。

> 說明：以上內容來自於STEELKIWI網站的[*Best Practice working with Django models in Python*](https://steelkiwi.com/blog/best-practices-working-django-models-python/)，有興趣的小夥伴可以閱讀原文。

### 模型定義參考

#### 欄位

對欄位名稱的限制

- 欄位名不能是Python的保留字，否則會導致語法錯誤
- 欄位名不能有多個連續下劃線，否則影響ORM查詢操作

Django模型欄位類

| 欄位類                |  說明                                                         |
| --------------------- | ------------------------------------------------------------ |
| AutoField             |自增ID欄位                                                   |
| BigIntegerField       |64位有符號整數                                               |
| BinaryField           | 儲存二進位制資料的欄位，對應Python的bytes型別                  |
| BooleanField          | 儲存True或False                                              |
| CharField             | 長度較小的字串                                             |
| DateField             | 儲存日期，有auto_now和auto_now_add屬性                       |
| DateTimeField         | 儲存日期和日期，兩個附加屬性同上                             |
| DecimalField          |儲存固定精度小數，有max_digits（有效位數）和decimal_places（小數點後面）兩個必要的引數 |
| DurationField         |儲存時間跨度                                                 |
| EmailField            | 與CharField相同，可以用EmailValidator驗證                    |
| FileField             | 檔案上傳欄位                                                 |
| FloatField            | 儲存浮點數                                                   |
| ImageField            | 其他同FileFiled，要驗證上傳的是不是有效影象                  |
| IntegerField          | 儲存32位有符號整數。                                         |
| GenericIPAddressField | 儲存IPv4或IPv6地址                                           |
| NullBooleanField      | 儲存True、False或null值                                      |
| PositiveIntegerField  | 儲存無符號整數（只能儲存正數）                               |
| SlugField             | 儲存slug（簡短標註）                                         |
| SmallIntegerField     | 儲存16位有符號整數                                           |
| TextField             | 儲存資料量較大的文字                                         |
| TimeField             | 儲存時間                                                     |
| URLField              | 儲存URL的CharField                                           |
| UUIDField             | 儲存全域性唯一識別符號                                           |

#### 欄位屬性

通用欄位屬性

| 選項           | 說明                                                         |
| -------------- | ------------------------------------------------------------ |
| null           | 資料庫中對應的欄位是否允許為NULL，預設為False                |
| blank          | 後臺模型管理驗證資料時，是否允許為NULL，預設為False          |
| choices        | 設定欄位的選項，各元組中的第一個值是設定在模型上的值，第二值是人類可讀的值 |
| db_column      | 欄位對應到資料庫表中的列名，未指定時直接使用欄位的名稱       |
| db_index       | 設定為True時將在該欄位建立索引                               |
| db_tablespace  | 為有索引的欄位設定使用的表空間，預設為DEFAULT_INDEX_TABLESPACE |
| default        | 欄位的預設值                                                 |
| editable       | 欄位在後臺模型管理或ModelForm中是否顯示，預設為True          |
| error_messages | 設定欄位丟擲異常時的預設訊息的字典，其中的鍵包括null、blank、invalid、invalid_choice、unique和unique_for_date |
| help_text      | 表單小元件旁邊顯示的額外的幫助文字。                         |
| primary_key    | 將欄位指定為模型的主鍵，未指定時會自動新增AutoField用於主鍵，只讀。 |
| unique         | 設定為True時，表中欄位的值必須是唯一的                       |
| verbose_name   | 欄位在後臺模型管理顯示的名稱，未指定時使用欄位的名稱         |

ForeignKey屬性

1. limit_choices_to：值是一個Q物件或返回一個Q物件，用於限制後臺顯示哪些物件。
2. related_name：用於獲取關聯物件的關聯管理器物件（反向查詢），如果不允許反向，該屬性應該被設定為`'+'`，或者以`'+'`結尾。
3. to_field：指定關聯的欄位，預設關聯物件的主鍵欄位。
4. db_constraint：是否為外來鍵建立約束，預設值為True。
5. on_delete：外來鍵關聯的物件被刪除時對應的動作，可取的值包括django.db.models中定義的：
   - CASCADE：級聯刪除。
   - PROTECT：丟擲ProtectedError異常，阻止刪除引用的物件。
   - SET_NULL：把外來鍵設定為null，當null屬性被設定為True時才能這麼做。
   - SET_DEFAULT：把外來鍵設定為預設值，提供了預設值才能這麼做。

ManyToManyField屬性

1. symmetrical：是否建立對稱的多對多關係。
2. through：指定維持多對多關係的中間表的Django模型。
3. throughfields：定義了中間模型時可以指定建立多對多關係的欄位。
4. db_table：指定維持多對多關係的中間表的表名。

#### 模型元資料選項

| 選項                  | 說明                                                         |
| --------------------- | ------------------------------------------------------------ |
| abstract              | 設定為True時模型是抽象父類                                   |
| app_label             | 如果定義模型的應用不在INSTALLED_APPS中可以用該屬性指定       |
| db_table              | 模型使用的資料表名稱                                         |
| db_tablespace         | 模型使用的資料表空間                                         |
| default_related_name  | 關聯物件回指這個模型時預設使用的名稱，預設為<model_name>_set |
| get_latest_by         | 模型中可排序欄位的名稱。                                     |
| managed               | 設定為True時，Django在遷移中建立資料表並在執行flush管理命令時把表移除 |
| order_with_respect_to | 標記物件為可排序的                                           |
| ordering              | 物件的預設排序                                               |
| permissions           | 建立物件時寫入許可權表的額外許可權                               |
| default_permissions   | 預設為`('add', 'change', 'delete')`                          |
| unique_together       | 設定組合在一起時必須獨一無二的欄位名                         |
| index_together        | 設定一起建立索引的多個欄位名                                 |
| verbose_name          | 為物件設定人類可讀的名稱                                     |
| verbose_name_plural   | 設定物件的複數名稱                                           |

### 資料庫API參考



按欄位查詢可以用的條件：

1. exact / iexact
2. contains / icontains
3. in
4. gt / gte / lt / lte
5. startswith / istartswith / endswith / iendswith
6. range
7. year / month / day / week_day / hour / minute / second
8. isnull
9. search
10. regex / iregex

跨關係查詢

