## 玩轉PyCharm

PyCharm是由JetBrains公司開發的提供給Python專業的開發者的一個整合開發環境，它最大的優點是能夠大大提升Python開發者的工作效率，為開發者集成了很多用起來非常順手的功能，包括程式碼除錯、高亮語法、程式碼跳轉、智慧提示、自動補全、單元測試、版本控制等等。此外，PyCharm還提供了對一些高階功能的支援，包括支援基於Django框架的Web開發。

### PyCharm的下載和安裝

可以在[JetBrains公司的官方網站](<https://www.jetbrains.com/>)找到PyCharm的[下載連結](https://www.jetbrains.com/pycharm/download/)，有兩個可供下載的版本，一個是社群版（PyCharm CE），一個是專業版（PyCharm Professional）。社群版在Apache許可證下發布，可以免費使用；專業版在專用許可證下發布，需要購買授權後才能使用，但新使用者可以試用30天。很顯然，專業版提供了更為強大的功能和對企業級開發的各種支援，但是對於初學者來說，社群版已經足夠強大和好用了。安裝PyCharm只需要直接執行下載的安裝程式，然後持續的點選“Next”（下一步）按鈕就可以啦。下面是我在Windows系統下安裝PyCharm的截圖，安裝完成後點選“Finish”（結束）按鈕關閉安裝嚮導，然後可以透過雙擊桌面的快捷方式來執行PyCharm。

![](res/pycharm-installation.png)

### 首次使用的設定

第一次使用PyCharm時，會有一個匯入設定的嚮導，如果之前沒有使用PyCharm或者沒有儲存過設定的就直接選擇“Do not import settings”進入下一步即可，下面是我在macOS系統下第一次使用PyCharm時的截圖。

![](res/pycharm-import-settings.png)

專業版的PyCharm是需要啟用的，**強烈建議大家在條件允許的情況下支付費用來支援優秀的產品**，如果不用做商業用途或者不需要使用PyCharm的高階功能，我們可以暫時選擇試用30天或者使用社群版的PyCharm。如果你是一名學生，希望購買PyCharm來使用，可以看看[教育優惠官方申請指南](https://sales.jetbrains.com/hc/zh-cn/articles/207154369)。如下圖所示，我們需要點選“Evaluate”按鈕來試用專業版PyCharm。

![](res/pycharm-activation.png)

接下來是選擇UI主題，可以根據個人喜好進行選擇，深色的主題比較護眼而淺色的主題對比度更好。

![](res/pycharm-ui-themes.png)

再接下來是建立可以在“終端”或“命令列提示符”中執行PyCharm的啟動指令碼，當然也可以不做任何勾選，直接點選“Next: Featured plugins”按鈕進入下一環節。

![](res/pycharm-create-launcher.png)

然後可以選擇需要安裝哪些外掛，我們可以暫時什麼都不安裝，等需要的時候再來決定。

![](res/pycharm-install-plugins.png)

最後點選上圖右下角的“Start using PyCharm”（開始使用PyCharm）就可以開啟你的PyCharm之旅了。

### 用PyCharm建立專案

啟動PyCharm之後會來到一個歡迎頁，在歡迎頁上我們可以選擇“建立新專案”（Create New Project）、“開啟已有專案”（Open）和“從版本控制系統中檢出專案”（Get from Version Control）。

![](res/pycharm-welcome.png)

如果選擇了“Create New Project”來建立新專案就會打一個建立專案的嚮導頁。下圖所示是PyCharm專業版建立新專案的嚮導頁，可以看出專業版支援的專案型別非常的多，而社群版只能建立純Python專案（Pure Python），沒有這一系列的選項。

![](res/pycharm-project-wizard.png)

接下來，我們要為專案建立專屬的虛擬環境，每個Python專案最好都在自己專屬的虛擬環境中執行，因為每個專案對Python直譯器和三方庫的需求並不相同，虛擬環境對不同的專案進行了隔離。在上圖所示的介面在，我們可以選擇新建虛擬環境（New environment using Virtualenv），這裡的“Virtualenv”是PyCharm預設選擇的建立虛擬環境的工具，我們就保留這個預設的選項就可以了。

專案建立完成後就可以開始新建各種檔案來書寫Python程式碼了，如下圖所示。左側是專案瀏覽器，可以看到剛才建立的專案資料夾以及虛擬環境資料夾。我們可以在專案上點選滑鼠右鍵，選擇“New”，在選擇“Python File”來建立Python程式碼檔案，下圖中我們建立了兩個Python檔案，分別是`poker_game.py`和`salary_system.py`。當然，如果願意，也可以使用複製貼上的方式把其他地方的Python程式碼檔案複製到專案資料夾下。

![](res/pycharm-workspace.png)

在工作視窗點選滑鼠右鍵可以在上下文選單中找到“Run”選項，例如要執行`salary_system.py`檔案，右鍵選單會顯示“Run 'salary_system'”選項，點選這個選項我們就可以執行Python程式碼啦，執行結果在螢幕下方的視窗可以看到，如下圖所示。

![](res/pycharm-run-result.png)

### 常用操作和快捷鍵

PyCharm為寫Python程式碼提供了自動補全和高亮語法功能，這也是PyCharm作為整合開發環境（IDE）的基本功能。PyCharm的“File”選單有一個“Settings”選單項（macOS上是在“PyCharm”選單的“Preferences…”選單項），這個選單項會開啟設定視窗，可以在此處對PyCharm進行設定，如下圖所示。

![](/Users/Hao/Desktop/Python-Core-50-Courses/res/pycharm-settings.png)

PyCharm的選單項中有一個非常有用的“Code”選單，選單中提供了自動生成程式碼、自動補全程式碼、格式化程式碼、移動程式碼等選項，這些功能對開發者來說是非常有用的，大家可以嘗試使用這些選單項或者記住它們對應的快捷鍵，例如在macOS上，格式化程式碼這個選單項對應的快捷鍵是`alt+command+L`。除此之外，“Refactor”選單也非常有用，它提供了一些重構程式碼的選項。所謂重構是在不改變程式碼執行結果的前提下調整程式碼的結構，這也是資深程式設計師的一項重要技能。還有一個值得一提的選單是“VCS”，VCS是“Version Control System”（版本控制系統）的縮寫，這個選單提供了對程式碼版本管理的支援。版本控制的知識會在其他的課程中為大家講解。

下表列出了一些PyCharm中特別常用的快捷鍵，當然如果願意，也可以透過設定視窗中“Keymap”選單項自定義快捷鍵，PyCharm本身也針對不同的作業系統和使用習慣對快捷鍵進行了分組。

| 快捷鍵                                        | 作用                                   |
| --------------------------------------------- | -------------------------------------- |
| `command + j`                                 | 顯示可用的程式碼模板                     |
| `command + b`                                 | 檢視函式、類、方法的定義               |
| `ctrl + space`                                | 萬能程式碼提示快捷鍵，一下不行按兩下     |
| `command + alt + l`                           | 格式化程式碼                             |
| `alt + enter`                                 | 萬能程式碼修復快捷鍵                     |
| `ctrl + /`                                    | 註釋/反註釋程式碼                        |
| `shift + shift`                               | 萬能搜尋快捷鍵                         |
| `command + d` / `command + y`                 | 複製/刪除一行程式碼                      |
| `command + shift + -` / `command + shift + +` | 摺疊/展開所有程式碼                      |
| `F2`                                          | 快速定位到錯誤程式碼                     |
| `command+ alt + F7`                           | 檢視哪些地方用到了指定的函式、類、方法 |

> **說明**：Windows系統下如果使用PyCharm的預設設定，可以將上面的`command`鍵換成`ctrl`鍵即可，唯一的例外是`ctrl + space`那個快捷鍵，因為它跟Windows系統切換輸入法的快捷鍵是衝突的，所以在Windows系統下預設沒有與之對應的快捷鍵。