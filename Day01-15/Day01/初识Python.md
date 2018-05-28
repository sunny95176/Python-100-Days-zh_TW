## Day01 - 初識Python

### Python簡介

#### Python的歷史

1. 1989年聖誕節：Guido von Rossum開始寫Python語言的編譯器。
2. 1991年2月：第一個Python編譯器（同時也是直譯器）誕生，它是用C語言實現的（後面又出現了Java和C#實現的版本Jython和IronPython，以及PyPy、Brython、Pyston等其他實現），可以呼叫C語言的庫函式。在最早的版本中，Python已經提供了對“類”，“函式”，“異常處理”等構造塊的支援，同時提供了“列表”和“字典”等核心資料型別，同時支援以模組為基礎的拓展系統。
3. 1994年1月：Python 1.0正式釋出。
4. 2000年10月16日：Python 2.0釋出，增加了實現完整的[垃圾回收](https://zh.wikipedia.org/wiki/%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6_(%E8%A8%88%E7%AE%97%E6%A9%9F%E7%A7%91%E5%AD%B8))，並且支援[Unicode](https://zh.wikipedia.org/wiki/Unicode)。與此同時，Python的整個開發過程更加透明，社群對開發進度的影響逐漸擴大，生態圈開始慢慢形成。
5. 2008年12月3日：Python 3.0釋出，此版不完全相容之前的Python程式碼，不過很多新特性後來也被移植到舊的Python 2.6/2.7版本，因為目前還有公司在專案和運維中使用Python 2.x版本的程式碼。

目前我們使用的Python 3.6.x的版本是在2016年的12月23日釋出的，Python的版本號分為三段，形如A.B.C。其中A表示大版本號，一般當整體重寫，或出現不向後相容的改變時，增加A；B表示功能更新，出現新功能時增加B；C表示小的改動（如修復了某個Bug），只要有修改就增加C。如果對Python的歷史感興趣，可以檢視一篇名為[《Python簡史》](http://www.cnblogs.com/vamei/archive/2013/02/06/2892628.html)的博文。

#### Python的優缺點

Python的優點很多，簡單的可以總結為以下幾點。

1. 簡單和明確，做一件事只有一種方法。
2. 學習曲線低，與其他很多語言比上手更容易。
3. 開放原始碼，擁有強大的社群和生態圈。
4. 解釋型語言，完美的平臺可移植性。
5. 支援兩種主流的程式設計正規化，可以使用面向物件和函數語言程式設計。
6. 可擴充套件性和可嵌入性，可以呼叫C/C++程式碼也可以在C/C++中呼叫。
7. 程式碼規範程度高，可讀性強，適合有程式碼潔癖和強迫症的人群。

Python的缺點主要集中在以下幾點。

1. 執行效率低下，因此計算密集型任務可以由C/C++編寫。
2. 程式碼無法加密，但是現在的公司很多都不是賣軟體而是賣服務，這個問題慢慢會淡化。
3. 在開發時可以選擇的框架太多，有選擇的地方就有錯誤。

#### Python的應用領域

目前Python在雲基礎設施、DevOps、網路爬蟲開發、資料分析挖掘、機器學習等領域都有著廣泛的應用，因此也產生了伺服器開發、資料介面開發、自動化運維、科學計算和資料視覺化、聊天機器人開發、影象識別和處理等一系列的職位。

### 搭建程式設計環境

#### Windows環境

可以在[Python的官方網站](https://www.python.org)下載到Python的Windows安裝程式（exe檔案），需要注意的是如果在Windows 7環境下安裝需要先安裝Service Pack 1補丁包（可以通過一些工具軟體自動安裝系統補丁的功能來安裝），安裝過程建議勾選“Add Python 3.6 to PATH”（將Python 3.6新增到PATH環境變數）並選擇自定義安裝，在設定“Optional Features”介面最好將“pip”、“tcl/tk”、“Python test suite”等項全部勾選上。強烈建議使用自定義的安裝路徑並保證路徑中沒有中文。安裝完成會看到“Setup was successful”的提示，但是在啟動Python環境時可能會因為缺失一些動態連結庫檔案而導致Python直譯器無法執行，常見的問題主要是api-ms-win-crt\*.dll缺失以及更新DirectX之後導致某些動態連結庫檔案缺失，前者可以參照[《api-ms-win-crt\*.dll缺失原因分析和解決方法》]()一文講解的方法進行處理或者直接在[微軟官網](https://www.microsoft.com/zh-cn/download/details.aspx?id=48145)下載Visual C++ Redistributable for Visual Studio 2015檔案進行修復，後者可以下載一個DirectX修復工具進行修復。

#### Linux環境

Linux環境自帶了Python 2.x版本，但是如果要更新到3.x的版本，可以在[Python的官方網站](https://www.python.org)下載Python的原始碼並通過原始碼構建安裝的方式進行安裝，具體的步驟如下所示。

安裝依賴庫（因為沒有這些依賴庫可能在原始碼構件安裝時因為缺失底層依賴庫而失敗）。

```Shell
yum -y install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel
```

下載Python原始碼並解壓縮到指定目錄。

```Shell
wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tar.xz
xz -d Python-3.6.1.tar.xz
tar -xvf Python-3.6.1.tar
```

切換至Python原始碼目錄並執行下面的命令進行配置和安裝。

```Shell
cd Python-3.6.1
./configure --prefix=/usr/local/python3.6 --enable-optimizations
make && make install
```

建立軟連結，這樣就可以直接通過python3直接啟動Python直譯器。

```Shell
ln -s /usr/local/python3.6/bin/python3 /usr/bin/python3
```


#### MacOS環境

MacOS也是自帶了Python 2.x版本的，可以通過[Python的官方網站](https://www.python.org)提供的安裝檔案（pkg檔案）安裝3.x的版本。預設安裝完成後，可以通過在終端執行python命令來啟動2.x版本的Python直譯器，可以通過執行python3命令來啟動3.x版本的Python直譯器，當然也可以通過重新設定軟連結來修改啟動Python直譯器的命令。

### 從終端執行Python程式

#### 確認Python的版本

在終端或命令列提示符中鍵入下面的命令。

```Shell
python --version
```
當然也可以先輸入python進入互動式環境，再執行以下的程式碼檢查Python的版本。

```Python
import sys

print(sys.version_info)
print(sys.version)
```

#### 編寫Python原始碼

可以用文字編輯工具（推薦使用Sublime、Atom、TextMate、VSCode等高階文字編輯工具）編寫Python原始碼並將其命名為hello.py儲存起來，程式碼內容如下所示。

```Python
print('hello, world!')
```

#### 執行程式

切換到原始碼所在的目錄並執行下面的命令，看看螢幕上是否輸出了"hello, world!"。

```Shell
python hello.py
```

### 程式碼中的註釋

註釋是程式語言的一個重要組成部分，用於在原始碼中解釋程式碼的作用從而增強程式的可讀性和可維護性，當然也可以將原始碼中不需要參與執行的程式碼段通過註釋來去掉，這一點在除錯程式的時候經常用到。註釋在隨原始碼進入前處理器或編譯時會被移除，不會在目的碼中保留也不會影響程式的執行結果。

1. 單行註釋 - 以#和空格開頭的部分
2. 多行註釋 - 三個引號開頭，三個引號結尾

```Python
"""

第一個Python程式 - hello, world!
向偉大的Dennis M. Ritchie先生致敬

Version: 0.1
Author: 駱昊
Date: 2018-02-26

"""

print('hello, world!')
# print("你好,世界！")
print('你好', '世界')
print('hello', 'world', sep=', ', end='!')
print('goodbye, world', end='!\n')
```

### 其他工具介紹

#### IDLE - 自帶的整合開發工具

IDLE是安裝Python環境時自帶的整合開發工具，如下圖所示。但是由於IDLE的使用者體驗並不是那麼好所以很少在實際開發中被採用。

![](./res/python-idle.png)

#### IPython - 更好的互動式程式設計工具

IPython是一種基於Python的互動式直譯器。相較於原生的Python Shell，IPython提供了更為強大的編輯和互動功能。可以通過Python的包管理工具pip安裝IPython和Jupyter，具體的操作如下所示。

```Shell
pip install ipython jupyter
```

或者

```Shell
python -m pip install ipython jupyter
```

安裝成功後，可以通過下面的ipython命令啟動IPython，如下圖所示。

![](./res/python-ipython.png)

當然我們也可以通過Jupyter執行名為notebook的專案在瀏覽器視窗中進行互動式操作。

```Shell
jupyter notebook
```

![](./res/python-jupyter-1.png)

![](./res/python-jupyter-2.png)

#### Sublime - 文字編輯神器

![](./res/python-sublime.png)

- 首先可以通過[官方網站](https://www.sublimetext.com/)下載安裝程式安裝Sublime 3或Sublime 2。

- 安裝包管理工具。通過快捷鍵Ctrl+`或者在View選單中選擇Show Console開啟控制檯，輸入下面的程式碼。

  - Sublime 3

  ```Python
  import  urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
  ```

  - Sublime 2

  ```Python
  import  urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp)ifnotos.path.exists(ipp)elseNone;urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read());print('Please restart Sublime Text to finish installation')
  ```

- 安裝外掛。通過Preference選單的Package Control或快捷鍵Ctrl+Shift+P開啟命令面板，在面板中輸入Install Package就可以找到安裝外掛的工具，然後再查詢需要的外掛。我們推薦大家安裝以下幾個外掛。

  - SublimeCodeIntel - 程式碼自動補全工具外掛
  - Emmet - 前端開發程式碼模板外掛
  - Git - 版本控制工具外掛
  - Python PEP8 Autoformat - PEP8規範自動格式化外掛
  - ConvertToUTF8 - 將本地編碼轉換為UTF-8

#### PyCharm - Python開發神器

PyCharm的安裝、配置和使用我們在後面會進行介紹。

![](./res/python-pycharm.png)

### 練習

1. 在Python互動環境中下面的程式碼檢視結果並將內容翻譯成中文。

    ```Python
    import this

    Beautiful is better than ugly.
    Explicit is better than implicit.
    Simple is better than complex.
    Complex is better than complicated.
    Flat is better than nested.
    Sparse is better than dense.
    Readability counts.
    Special cases aren't special enough to break the rules.
    Although practicality beats purity.
    Errors should never pass silently.
    Unless explicitly silenced.
    In the face of ambiguity, refuse the temptation to guess.
    There should be one-- and preferably only one --obvious way to do it.
    Although that way may not be obvious at first unless you're Dutch.
    Now is better than never.
    Although never is often better than *right* now.
    If the implementation is hard to explain, it's a bad idea.
    If the implementation is easy to explain, it may be a good idea.
    Namespaces are one honking great idea -- let's do more of those!
    ```

2. 學習使用turtle在螢幕上繪製圖形。

    ```Python
    import turtle

    turtle.pensize(4)
    turtle.pencolor('red')
    turtle.forward(100)
    turtle.right(90)
    turtle.forward(100)
    turtle.right(90)
    turtle.forward(100)
    turtle.right(90)
    turtle.forward(100)
    turtle.mainloop()
    ```
