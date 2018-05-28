## 玩轉PyCharm(上)

PyCharm是由JetBrains公司開發的提供給Python專業的開發者的一個整合開發環境，它最大的優點是能夠大大提升Python開發者的工作效率，為開發者集成了很多用起來非常順手的功能，包括程式碼除錯、高亮語法、程式碼跳轉、智慧提示、自動補全、單元測試、版本控制等等。此外，PyCharm還提供了對一些高階功能的支援，包括支援基於Django框架的Web開發、。

### PyCharm的安裝

可以在[JetBrains公司的官方網站]()找到PyCharm的[下載連結](https://www.jetbrains.com/pycharm/download/)，有兩個可供下載的版本一個是社群版一個是專業版，社群版在[Apache許可證](https://zh.wikipedia.org/wiki/Apache%E8%AE%B8%E5%8F%AF%E8%AF%81)下發布，專業版在專用許可證下發布（需要購買授權下載後可試用30天），其擁有許多額外功能。安裝PyCharm需要有JRE（Java執行時環境）的支援，如果沒有可以在安裝過程中選擇線上下載安裝。

### 首次使用的設定

第一次使用PyCharm時，會有一個匯入設定的嚮導，如果之前沒有使用PyCharm或者沒有儲存過設定的就直接選擇“Do not import settings”進入下一步即可。

![](./res/pycharm-import-settings.png)

專業版的PyCharm是需要啟用的，強烈建議為優秀的軟體支付費用，如果不用做商業用途，我們可以暫時選擇試用30天或者使用社群版的PyCharm。

![](./res/pycharm-activate.png)

 接下來是選擇UI主題，這個可以根據個人喜好進行選擇。

![](./res/pycharm-set-ui-theme.png)

 再接下來是建立可以在終端（命令列）中使用PyCharm專案的啟動指令碼，當然也可以直接跳過這一步。

![](./res/pycharm-create-launcher-script.png)

然後可以選擇需要安裝哪些外掛，我們可以暫時什麼都不安裝等需要的時候再來決定。

![](./res/pycharm-plugins.png)

### 用PyCharm建立專案

點選上圖中的“Start using PyCharm”按鈕就可以開始使用PyCharm啦，首先來到的是一個歡迎頁，在歡迎頁上我們可以選擇“建立新專案”、“開啟已有專案”和“從版本控制系統中檢出專案”。

![](./res/pycharm-welcome.png)

如果選擇了“Create New Project”來建立新專案就會打一個建立專案的嚮導頁。

![](./res/pycharm-new-project.png)

在如上圖所示的介面中，我們可以選擇建立專案的模板，包括了純Python專案、基於各種不同框架的Web專案、Web前端專案、跨平臺專案等各種不同的專案模板。如果選擇Python的專案，那麼有一個非常重要的設定是選擇“New environment…”（建立新的虛擬環境）還是使用“Existing Interpreter”（已經存在的直譯器）。前者肯定是更好的選擇，因為新的虛擬環境不會對系統環境變數中配置的Python環境造成影響，簡單舉個例子就是你在虛擬環境下安裝或者更新了任何三方庫，它並不會對系統原有的Python直譯器造成任何的影響，但代價是需要額外的儲存空間來建立這個虛擬環境。

專案建立完成後就可以開始新建各種檔案來書寫Python程式碼了。

![](./res/pycharm-workspace.png)

在工作視窗的右鍵選單中可以找到“Run ...”和“Debug ...”選單項，通過這兩個選單項我們就可以執行和除錯我們的程式碼啦。建議關注一下選單欄中的“Code”、“Refactor”和“Tools”選單，這裡面為編寫Python程式碼提供了很多有用的幫助，我們在後面也會陸續為大家介紹這些功能。