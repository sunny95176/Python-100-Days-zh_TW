## NoSQL入門

### NoSQL概述

如今，大多數的計算機系統（包括伺服器、PC、移動裝置等）都會產生龐大的資料量。其實，早在2012年的時候，全世界每天產生的資料量就達到了2.5EB（艾位元組，$$1EB\approx10^{18}B$$）。這些資料有很大一部分是由關係型資料庫來儲存和管理的。 早在1970年，E.F.Codd發表了論述關係型資料庫的著名論文“*A relational model of data for large shared data banks*”，這篇文章奠定了關係型資料庫的基礎並在接下來的數十年時間內產生了深遠的影響。實踐證明，關係型資料庫是實現資料持久化最為重要的方式，它也是大多數應用在選擇持久化方案時的首選技術。

NoSQL是一項全新的資料庫革命性運動，雖然它的歷史可以追溯到1998年，但是NoSQL真正深入人心並得到廣泛的應用是在進入大資料時候以後，業界普遍認為NoSQL是更適合大資料儲存的技術方案，這才使得NoSQL的發展達到了前所未有的高度。2012年《紐約時報》的一篇專欄中寫到，大資料時代已經降臨，在商業、經濟及其他領域中，決策將不再基於經驗和直覺而是基於資料和分析而作出。事實上，在天文學、氣象學、基因組學、生物學、社會學、網際網路搜尋引擎、金融、醫療、社交網路、電子商務等諸多領域，由於資料過於密集和龐大，在資料的分析和處理上也遇到了前所未有的限制和阻礙，這一切都使得對大資料處理技術的研究被提升到了新的高度，也使得各種NoSQL的技術方案進入到了公眾的視野。

NoSQL資料庫按照其儲存型別可以大致分為以下幾類：

| 型別       | 部分代表                            | 特點                                                         |
| ---------- | ----------------------------------- | ------------------------------------------------------------ |
| 列族資料庫 | HBase<br>Cassandra<br>Hypertable    | 顧名思義是按列儲存資料的。最大的特點是方便儲存結構化和半結構化資料，方便做資料壓縮，對針對某一列或者某幾列的查詢有非常大的I/O優勢，適合於批次資料處理和即時查詢。 |
| 文件資料庫 | MongoDB<br>CouchDB<br>ElasticSearch | 文件資料庫一般用類JSON格式儲存資料，儲存的內容是文件型的。這樣也就有機會對某些欄位建立索引，實現關係資料庫的某些功能，但不提供對參照完整性和分佈事務的支援。 |
| KV資料庫   | DynamoDB<br>Redis<br>LevelDB        | 可以透過key快速查詢到其value，有基於記憶體和基於磁碟兩種實現方案。 |
| 圖資料庫   | Neo4J<br>FlockDB<br>JanusGraph      | 使用圖結構進行語義查詢的資料庫，它使用節點、邊和屬性來表示和儲存資料。圖資料庫從設計上，就可以簡單快速的檢索難以在關係系統中建模的複雜層次結構。 |
| 物件資料庫 | db4o<br>Versant                     | 透過類似面嚮物件語言的語法操作資料庫，透過物件的方式存取資料。 |

> **說明**：想了解更多的NoSQL資料庫，可以訪問<http://nosql-database.org/>。

### Redis概述

Redis是一種基於鍵值對的NoSQL資料庫，它提供了對多種資料型別（字串、雜湊、列表、集合、有序集合、點陣圖等）的支援，能夠滿足很多應用場景的需求。Redis將資料放在記憶體中，因此讀寫效能是非常驚人的。與此同時，Redis也提供了持久化機制，能夠將記憶體中的資料儲存到硬碟上，在發生意外狀況時資料也不會丟掉。此外，Redis還支援鍵過期、地理資訊運算、釋出訂閱、事務、管道、Lua指令碼擴充套件等功能，總而言之，Redis的功能和效能都非常強大，如果專案中要實現快取記憶體和訊息佇列這樣的服務，直接交給Redis就可以了。目前，國內外很多著名的企業和商業專案都使用了Redis，包括：Twitter、Github、StackOverflow、新浪微博、百度、優酷土豆、美團、小米、唯品會等。


#### Redis簡介

2008年，一個名為Salvatore Sanfilippo的程式設計師為他開發的LLOOGG專案定製了專屬的資料庫（因為之前他無論怎樣最佳化MySQL，系統性能已經無法再提升了），這項工作的成果就是Redis的初始版本。後來他將Redis的程式碼放到了全球最大的程式碼託管平臺[Github](<https://github.com/antirez/redis>)，從那以後，Redis引發了大量開發者的好評和關注，繼而有數百人參與了Redis的開發和維護，這使得Redis的功能越來越強大和效能越來越好。

Redis是REmote DIctionary Server的縮寫，它是一個用ANSI C編寫的高效能的key-value儲存系統，與其他的key-value儲存系統相比，Redis有以下一些特點（也是優點）：

- Redis的讀寫效能極高，並且有豐富的特性（釋出/訂閱、事務、通知等）。
- Redis支援資料的持久化（RDB和AOF兩種方式），可以將記憶體中的資料儲存在磁碟中，重啟的時候可以再次載入進行使用。
- Redis支援多種資料型別，包括：string、hash、list、set，zset、bitmap、hyperloglog等。
- Redis支援主從複製（實現讀寫分析）以及哨兵模式（監控master是否宕機並自動調整配置）。
- Redis支援分散式叢集，可以很容易的透過水平擴充套件來提升系統的整體效能。
- Redis基於TCP提供的可靠傳輸服務進行通訊，很多程式語言都提供了Redis客戶端支援。

#### Redis的應用場景

1. 快取記憶體  - 將不常變化但又經常被訪問的熱點資料放到Redis資料庫中，可以大大降低關係型資料庫的壓力，從而提升系統的響應效能。
2. 排行榜 - 很多網站都有排行榜功能，利用Redis中的列表和有序集合可以非常方便的構造各種排行榜系統。
3. 商品秒殺/投票點贊 - Redis提供了對計數操作的支援，網站上常見的秒殺、點贊等功能都可以利用Redis的計數器透過+1或-1的操作來實現，從而避免了使用關係型資料的`update`操作。
4. 分散式鎖 - 利用Redis可以跨多臺伺服器實現分散式鎖（類似於執行緒鎖，但是能夠被多臺機器上的多個執行緒或程序共享）的功能，用於實現一個阻塞式操作。
5. 訊息佇列 - 訊息佇列和快取記憶體一樣，是一個大型網站不可缺少的基礎服務，可以實現業務解耦和非實時業務削峰等特性，這些我們都會在後面的專案中為大家展示。

#### Redis的安裝和配置

可以使用Linux系統的包管理工具（如yum）來安裝Redis，也可以透過在Redis的[官方網站](https://redis.io/)下載Redis的原始碼，解壓縮解歸檔之後透過make工具對原始碼進行構建並安裝，在更新這篇文件時，Redis官方提供的最新穩定版本是[Redis 5.0.10](https://download.redis.io/releases/redis-5.0.10.tar.gz)。

下載：

```Bash
wget https://download.redis.io/releases/redis-5.0.10.tar.gz
```

解壓縮和解歸檔：

```Bash
tar -zxf redis-5.0.10.tar.gz
```

進入Redis原始碼目錄：

```Bash
cd redis-5.0.10
```

構建和安裝：

```Bash
make && make install
```

在redis原始碼目錄下有一個名為redis.conf的配置檔案，我們可以先檢視一下該檔案。

```Bash
vim redis.conf
```

下面我們對Redis的配置檔案進行一個扼要的介紹。

配置Redis服務的IP地址和埠：

![](./res/redis-bind-and-port.png)

配置底層有多少個數據庫：

![](./res/redis-databases.png)

配置Redis的持久化機制 - RDB。

![](./res/redis-rdb-1.png)

![](./res/redis-rdb-3.png)

配置Redis的持久化機制 - AOF：

![](./res/redis-aof.png)

配置訪問Redis伺服器的口令：

![](./res/redis-security.png)

配置Redis的主從複製（透過主從複製可以實現讀寫分離）：

![](./res/redis-replication.png)

配置慢查詢：

![](./res/redis-slow-logs.png)

上面這些內容就是Redis的基本配置，如果你對上面的內容感到困惑也沒有關係，先把Redis用起來再回頭去推敲這些內容就行了。如果想找一些參考書，[《Redis開發與運維》](https://item.jd.com/12121730.html)是一本不錯的入門讀物，而[《Redis實戰》](https://item.jd.com/11791607.html)是不錯的進階讀物。

#### Redis的伺服器和客戶端

接下來啟動Redis伺服器，下面的方式將以預設的配置啟動Redis服務。

```Bash
redis-server
```

如果希望修改Redis的配置（如埠、認證口令、持久化方式等），可以透過下面兩種方式。

**方式一**：透過引數指定認證口令和AOF持久化方式。

```Bash
redis-server --requirepass yourpass --appendonly yes
```

**方式二**：透過指定的配置檔案來修改Redis的配置。

```Bash
redis-server /root/redis-5.0.10/redis.conf
```

下面我們使用第一種方式來啟動Redis並將其置於後臺執行，將Redis產生的輸出重定向到名為redis.log的檔案中。

```Bash
redis-server --requirepass yourpass > redis.log &
```

可以透過`ps`或者`netstat`來檢查Redis伺服器是否啟動成功。

```Bash
ps -ef | grep redis-server
netstat -nap | grep redis-server
```

接下來，我們嘗試用Redis命令列工具`redis-cli`去連線伺服器，該工具預設連線本機的`6379`埠，如果需要指定Redis伺服器和埠，可以使用`-h`和`-p`引數分別進行指定。

```Bash
redis-cli
```

進入命令列工具後，就可以透過Redis的命令來操作Redis伺服器，如下所示。

```Bash
127.0.0.1:6379> auth yourpass
OK
127.0.0.1:6379> ping
PONG
127.0.0.1:6379>
```

Redis有著非常豐富的資料型別，也有很多的命令來操作這些資料，具體的內容可以檢視[Redis命令參考](http://redisdoc.com/)，在這個網站上，除了Redis的命令參考，還有Redis的詳細文件，其中包括了通知、事務、主從複製、持久化、哨兵、叢集等內容。

![](./res/redis-data-types.png)

> **說明**：上面的插圖來自付磊和張益軍編著的《Redis開發與運維》一書。

```Bash
127.0.0.1:6379> set username admin
OK
127.0.0.1:6379> get username
"admin"
127.0.0.1:6379> set password "123456" ex 300
OK
127.0.0.1:6379> get password
"123456"
127.0.0.1:6379> ttl username
(integer) -1
127.0.0.1:6379> ttl password
(integer) 286
127.0.0.1:6379> hset stu1 name hao
(integer) 0
127.0.0.1:6379> hset stu1 age 38
(integer) 1
127.0.0.1:6379> hset stu1 gender male
(integer) 1
127.0.0.1:6379> hgetall stu1
1) "name"
2) "hao"
3) "age"
4) "38"
5) "gender"
6) "male"
127.0.0.1:6379> hvals stu1
1) "hao"
2) "38"
3) "male"
127.0.0.1:6379> hmset stu2 name wang age 18 gender female tel 13566778899
OK
127.0.0.1:6379> hgetall stu2
1) "name"
2) "wang"
3) "age"
4) "18"
5) "gender"
6) "female"
7) "tel"
8) "13566778899"
127.0.0.1:6379> lpush nums 1 2 3 4 5
(integer) 5
127.0.0.1:6379> lrange nums 0 -1
1) "5"
2) "4"
3) "3"
4) "2"
5) "1"
127.0.0.1:6379> lpop nums
"5"
127.0.0.1:6379> lpop nums
"4"
127.0.0.1:6379> rpop nums
"1"
127.0.0.1:6379> rpop nums
"2"
127.0.0.1:6379> sadd fruits apple banana orange apple grape grape
(integer) 4
127.0.0.1:6379> scard fruits
(integer) 4
127.0.0.1:6379> smembers fruits
1) "grape"
2) "orange"
3) "banana"
4) "apple"
127.0.0.1:6379> sismember fruits apple
(integer) 1
127.0.0.1:6379> sismember fruits durian
(integer) 0
127.0.0.1:6379> sadd nums1 1 2 3 4 5
(integer) 5
127.0.0.1:6379> sadd nums2 2 4 6 8
(integer) 4
127.0.0.1:6379> sinter nums1 nums2
1) "2"
2) "4"
127.0.0.1:6379> sunion nums1 nums2
1) "1"
2) "2"
3) "3"
4) "4"
5) "5"
6) "6"
7) "8"
127.0.0.1:6379> sdiff nums1 nums2
1) "1"
2) "3"
3) "5"
127.0.0.1:6379> zadd topsinger 5234 zhangxy 1978 chenyx 2235 zhoujl 3520 xuezq
(integer) 4
127.0.0.1:6379> zrange topsinger 0 -1 withscores
1) "chenyx"
2) "1978"
3) "zhoujl"
4) "2235"
5) "xuezq"
6) "3520"
7) "zhangxy"
8) "5234"
127.0.0.1:6379> zrevrange topsinger 0 -1
1) "zhangxy"
2) "xuezq"
3) "zhoujl"
4) "chenyx"
127.0.0.1:6379> zrevrank topsinger zhoujl
(integer) 2
127.0.0.1:6379> geoadd pois 116.39738549206541 39.90862689286386 tiananmen
(integer) 1
127.0.0.1:6379> geoadd pois 116.27172936413572 39.99135172904494 yiheyuan
(integer) 1
127.0.0.1:6379> geoadd pois 117.27766503308104 40.65332064313784 gubeishuizhen
(integer) 1
127.0.0.1:6379> geodist pois tiananmen gubeishuizhen km
"111.5333"
127.0.0.1:6379> geodist pois tiananmen yiheyuan km
"14.1230"
127.0.0.1:6379> georadius pois 116.86499108288572 40.40149669363615 50 km withdist
1) 1) "gubeishuizhen"
   2) "44.7408"
```

#### 在Python程式中使用Redis

可以使用pip安裝名為`redis`的三方庫，該三方庫的核心是一個名為`Redis`的類，`Redis`物件代表一個Redis客戶端，透過該客戶端可以向Redis伺服器傳送命令並獲取執行的結果。上面我們在Redis客戶端中使用的命令基本上就是`Redis`物件可以接收的訊息，所以如果瞭解了Redis的命令就可以在Python中玩轉Redis。

```Bash
pip3 install redis
```

進入Python互動式環境，使用`redis`三方庫來操作Redis。

```Bash
>>> import redis
>>>
>>> client = redis.Redis(host='127.0.0.1', port=6379, password='yourpass')
>>>
>>> client.set('username', 'admin')
True
>>> client.hset('student', 'name', 'luohao')
1
>>> client.hset('student', 'age', 40)
1
>>> client.keys('*')
[b'username', b'student']
>>> client.get('username')
b'admin'
>>> client.hgetall('student')
{b'name': b'luohao', b'age': b'40'}
```

### MongoDB概述

#### MongoDB簡介

MongoDB是2009年問世的一個面向文件的資料庫管理系統，由C++語言編寫，旨在為Web應用提供可擴充套件的高效能資料儲存解決方案。雖然在劃分類別的時候後，MongoDB被認為是NoSQL的產品，但是它更像一個介於關係資料庫和非關係資料庫之間的產品，在非關係資料庫中它功能最豐富，最像關係資料庫。

MongoDB將資料儲存為一個文件，一個文件由一系列的“鍵值對”組成，其文件類似於JSON物件，但是MongoDB對JSON進行了二進位制處理（能夠更快的定位key和value），因此其文件的儲存格式稱為BSON。關於JSON和BSON的差別大家可以看看MongoDB官方網站的文章[《JSON and BSON》](https://www.mongodb.com/json-and-bson)。

目前，MongoDB已經提供了對Windows、macOS、Linux、Solaris等多個平臺的支援，而且也提供了多種開發語言的驅動程式，Python當然是其中之一。

#### MongoDB的安裝和啟動

可以從MongoDB的[官方下載連結](https://www.mongodb.com/try/download/community)下載MongoDB，官方提供了Windows、macOS和多種Linux版本的安裝包。下面以CentOS為例，簡單說一下如何安裝和啟動MongoDB。

下載伺服器和命令列的RPM安裝包。

```Bash
wget https://repo.mongodb.org/yum/redhat/7/mongodb-org/4.4/x86_64/RPMS/mongodb-org-server-4.4.2-1.el7.x86_64.rpm
rpm -ivh mongodb-org-server-4.4.2-1.el7.x86_64.rpm
wget https://repo.mongodb.org/yum/redhat/7/mongodb-org/4.4/x86_64/RPMS/mongodb-org-shell-4.4.2-1.el7.x86_64.rpm
rpm -ivh mongodb-org-shell-4.4.2-1.el7.x86_64.rpm
```

啟動MongoDB伺服器，需要先建立儲存資料的資料夾。

```Bash
mkdir -p /data/db
```

修改MongoDB的配置檔案，將其中`bindIp`選項的值修改為本機IP地址而不是預設的`127.0.0.1`，本機IP地址可以透過`ifconfig`命令進行檢視。

```Bash
vim /etc/mongod.conf
```

使用`systemctl`命令啟動服務。

```Bash
systemctl start mongod
```

#### MongoDB基本概念

我們透過與關係型資料庫的比較來說明MongoDB中的一些概念。

| SQL                   | MongoDB            |
| --------------------- | ------------------ |
| database              | database           |
| table（表）           | collection（集合） |
| row（行）             | document（文件）   |
| column（列）          | field（欄位）      |
| index                 | index              |
| table joins（表連線） | （巢狀文件）       |
| primary key           | primary key        |

#### 透過Shell操作MongoDB

0. 啟動命令列工具，進入互動式環境。

    ```Bash
    mongo
    ```

    > **說明**：

1. 檢視、建立和刪除資料庫。

   ```JavaScript
   > // 顯示所有資料庫
   > show dbs
   admin   0.000GB
   config  0.000GB
   local   0.000GB
   > // 建立並切換到school資料庫
   > use school
   switched to db school
   > // 刪除當前資料庫
   > db.dropDatabase()
   { "ok" : 1 }
   ```

2. 建立、刪除和檢視集合。

   ```JavaScript
   > // 建立並切換到school資料庫
   > use school
   switched to db school
   > // 建立colleges集合
   > db.createCollection('colleges')
   { "ok" : 1 }
   > // 建立students集合
   > db.createCollection('students')
   { "ok" : 1 }
   > // 檢視所有集合
   > show collections
   colleges
   students
   > // 刪除colleges集合
   > db.colleges.drop()
   true
   ```

   > **說明**：在MongoDB中插入文件時如果集合不存在會自動建立集合，所以也可以按照下面的方式透過插入文件來建立集合。

3. 文件的CRUD操作。

   ```JavaScript
   > // 向students集合插入文件
   > db.students.insert({stuid: 1001, name: '駱昊', age: 40})
   WriteResult({ "nInserted" : 1 })
   > // 向students集合插入文件
   > db.students.save({stuid: 1002, name: '王大錘', tel: '13012345678', gender: '男'})
   WriteResult({ "nInserted" : 1 })
   > // 檢視所有文件
   > db.students.find()
   { "_id" : ObjectId("5b13c72e006ad854460ee70b"), "stuid" : 1001, "name" : "駱昊", "age" : 38 }
   { "_id" : ObjectId("5b13c790006ad854460ee70c"), "stuid" : 1002, "name" : "王大錘", "tel" : "13012345678", "gender" : "男" }
   > // 更新stuid為1001的文件
   > db.students.update({stuid: 1001}, {'$set': {tel: '13566778899', gender: '男'}})
   WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
   > // 插入或更新stuid為1003的文件
   > db.students.update({stuid: 1003}, {'$set': {name: '白元芳', tel: '13022223333', gender: '男'}},  upsert=true)
   WriteResult({
           "nMatched" : 0,
           "nUpserted" : 1,
           "nModified" : 0,
           "_id" : ObjectId("5b13c92dd185894d7283efab")
   })
   > // 查詢所有文件
   > db.students.find().pretty()
   {
           "_id" : ObjectId("5b13c72e006ad854460ee70b"),
           "stuid" : 1001,
           "name" : "駱昊",
           "age" : 38,
           "gender" : "男",
           "tel" : "13566778899"
   }
   {
           "_id" : ObjectId("5b13c790006ad854460ee70c"),
           "stuid" : 1002,
           "name" : "王大錘",
           "tel" : "13012345678",
           "gender" : "男"
   }
   {
           "_id" : ObjectId("5b13c92dd185894d7283efab"),
           "stuid" : 1003,
           "gender" : "男",
           "name" : "白元芳",
           "tel" : "13022223333"
   }
   > // 查詢stuid大於1001的文件
   > db.students.find({stuid: {'$gt': 1001}}).pretty()
   {
           "_id" : ObjectId("5b13c790006ad854460ee70c"),
           "stuid" : 1002,
           "name" : "王大錘",
           "tel" : "13012345678",
           "gender" : "男"
   }
   {
           "_id" : ObjectId("5b13c92dd185894d7283efab"),
           "stuid" : 1003,
           "gender" : "男",
           "name" : "白元芳",
           "tel" : "13022223333"
   }
   > // 查詢stuid大於1001的文件只顯示name和tel欄位
   > db.students.find({stuid: {'$gt': 1001}}, {_id: 0, name: 1, tel: 1}).pretty()
   { "name" : "王大錘", "tel" : "13012345678" }
   { "name" : "白元芳", "tel" : "13022223333" }
   > // 查詢name為“駱昊”或者tel為“13022223333”的文件
   > db.students.find({'$or': [{name: '駱昊'}, {tel: '13022223333'}]}, {_id: 0, name: 1, tel: 1}).pretty()
   { "name" : "駱昊", "tel" : "13566778899" }
   { "name" : "白元芳", "tel" : "13022223333" }
   > // 查詢學生文件跳過第1條文件只查1條文件
   > db.students.find().skip(1).limit(1).pretty()
   {
           "_id" : ObjectId("5b13c790006ad854460ee70c"),
           "stuid" : 1002,
           "name" : "王大錘",
           "tel" : "13012345678",
           "gender" : "男"
   }
   > // 對查詢結果進行排序(1表示升序，-1表示降序)
   > db.students.find({}, {_id: 0, stuid: 1, name: 1}).sort({stuid: -1})
   { "stuid" : 1003, "name" : "白元芳" }
   { "stuid" : 1002, "name" : "王大錘" }
   { "stuid" : 1001, "name" : "駱昊" }
   > // 在指定的一個或多個欄位上建立索引
   > db.students.ensureIndex({name: 1})
   {
           "createdCollectionAutomatically" : false,
           "numIndexesBefore" : 1,
           "numIndexesAfter" : 2,
           "ok" : 1
   }
   ```

使用MongoDB可以非常方便的配置資料複製，透過冗餘資料來實現資料的高可用以及災難恢復，也可以透過資料分片來應對資料量迅速增長的需求。關於MongoDB更多的操作可以查閱[官方文件](https://mongodb-documentation.readthedocs.io/en/latest/) ，同時推薦大家閱讀Kristina Chodorow寫的[《MongoDB權威指南》](http://www.ituring.com.cn/book/1172)。

#### 在Python程式中操作MongoDB

可以透過pip安裝`pymongo`來實現對MongoDB的操作。

```Shell
pip install pymongo
```

進入Python互動式環境，就可以執行以下的操作。

```Python
>>> from pymongo import MongoClient
>>>
>>> client = MongoClient('mongodb://127.0.0.1:27017') 
>>> db = client.school
>>> for student in db.students.find():
...     print('學號:', student['stuid'])
...     print('姓名:', student['name'])
...     print('電話:', student['tel'])
... 
學號: 1001.0
姓名: 駱昊
電話: 13566778899
學號: 1002.0
姓名: 王大錘
電話: 13012345678
學號: 1003.0
姓名: 白元芳
電話: 13022223333
>>> db.students.find().count()
3
>>> db.students.remove()
{'n': 3, 'ok': 1.0}
>>> db.students.find().count()
0
>>> from pymongo import ASCENDING
>>>
>>> coll = db.students
>>> coll.create_index([('name', ASCENDING)], unique=True)
'name_1'
>>> coll.insert_one({'stuid': int(1001), 'name': '駱昊', 'gender': True})
<pymongo.results.InsertOneResult object at 0x1050cc6c8>
>>> coll.insert_many([{'stuid': int(1002), 'name': '王大錘', 'gender': False}, {'stuid': int(1003), 'name': '白元芳', 'gender': True}])
<pymongo.results.InsertManyResult object at 0x1050cc8c8>
>>> for student in coll.find({'gender': True}):
...     print('學號:', student['stuid'])
...     print('姓名:', student['name'])
...     print('性別:', '男' if student['gender'] else '女')
... 
學號: 1001
姓名: 駱昊
性別: 男
學號: 1003
姓名: 白元芳
性別: 男
```

關於[`pymongo`](https://api.mongodb.com/python/current/tutorial.html)更多的知識可以透過它的官方文件進行了解，也可以使用[`MongoEngine`](<https://pypi.org/project/mongoengine/>)這樣的庫來簡化Python程式對MongoDB的操作，除此之外，還有以非同步I/O方式訪問MongoDB的三方庫[`motor`](<https://pypi.org/project/motor/>)都是不錯的選擇。