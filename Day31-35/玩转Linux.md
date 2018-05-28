## 玩轉Linux作業系統

### 作業系統發展史

![](./res/history-of-os.png)

### Linux概述

Linux是一個通用作業系統。一個作業系統要負責任務排程、記憶體分配、處理外圍裝置I/O等操作。作業系統通常由核心和系統程式（裝置驅動、底層庫、shell、服務程式等）兩部分組成。

Linux核心是芬蘭人Linus Torvalds開發的，於1991年9月釋出。而Linux作業系統作為Internet時代的產物，它是由全世界許多開發者共同合作開發的，是一個自由的作業系統（注意是自由不是免費）。

### Linux系統優點

1. 通用作業系統，不跟特定的硬體繫結。
2. 用C語言編寫，有可移植性，有核心程式設計介面。
3. 支援多使用者和多工，支援安全的分層檔案系統。
4. 大量的實用程式，完善的網路功能以及強大的支援文件。
5. 可靠的安全性和良好的穩定性，對開發者更友好。

### 基礎命令

Linux系統的命令通常都是如下所示的格式：

```Shell
命令名稱 [命名引數] [命令物件]
```

1. 獲取登入資訊 - **w** / **who** / **last**。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# w
    23:31:16 up 12:16,  2 users,  load average: 0.00, 0.01, 0.05
   USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
   root     pts/0    182.139.66.250   23:03    4.00s  0.02s  0.00s w
   jackfrue pts/1    182.139.66.250   23:26    3:56   0.00s  0.00s -bash
   [root@izwz97tbgo9lkabnat2lo8z ~]# who
   root     pts/0        2018-04-12 23:03 (182.139.66.250)
   jackfrued pts/1        2018-04-12 23:26 (182.139.66.250)
   [root@izwz97tbgo9lkabnat2lo8z ~]# who am i
   root     pts/0        2018-04-12 23:03 (182.139.66.250)
   ```

2. 檢視自己使用的Shell - **ps**。

   Shell也被稱為“殼”，它是使用者與核心交流的翻譯官，簡單的說就是人與計算機互動的介面。目前很多Linux系統預設的Shell都是bash（<u>B</u>ourne <u>A</u>gain <u>SH</u>ell），因為它可以使用Tab鍵進行命令補全、可以儲存歷史命令、可以方便的配置環境變數以及執行批處理操作等。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# ps
     PID TTY          TIME CMD
    3531 pts/0    00:00:00 bash
    3553 pts/0    00:00:00 ps
   ```

3. 檢視命令的說明 - **whatis**。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# whatis ps
   ps (1)        - report a snapshot of the current processes.
   [root@izwz97tbgo9lkabnat2lo8z ~]# whatis python
   python (1)    - an interpreted, interactive, object-oriented programming language
   ```

4. 檢視命令的位置 - **which** / **whereis**。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# whereis ps
   ps: /usr/bin/ps /usr/share/man/man1/ps.1.gz
   [root@izwz97tbgo9lkabnat2lo8z ~]# whereis python
   python: /usr/bin/python /usr/bin/python2.7 /usr/lib/python2.7 /usr/lib64/python2.7 /etc/python /usr/include/python2.7 /usr/share/man/man1/python.1.gz
   [root@izwz97tbgo9lkabnat2lo8z ~]# which ps
   /usr/bin/ps
   [root@izwz97tbgo9lkabnat2lo8z ~]# which python
   /usr/bin/python
   ```

5. 檢視幫助文件 - **man** / **info** / **apropos**。
   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# ps --help
   Usage:
    ps [options]
    Try 'ps --help <simple|list|output|threads|misc|all>'
     or 'ps --help <s|l|o|t|m|a>'
    for additional help text.
   For more details see ps(1).
   [root@izwz97tbgo9lkabnat2lo8z ~]# man ps
   PS(1)                                User Commands                                PS(1)
   NAME
          ps - report a snapshot of the current processes.
   SYNOPSIS
          ps [options]
   DESCRIPTION
   ...
   [root@izwz97tbgo9lkabnat2lo8z ~]# info ps
   ...
   ```

6. 切換使用者 - **su**。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# su hellokitty
   [hellokitty@izwz97tbgo9lkabnat2lo8z root]$
   ```

7. 以管理員身份執行命令 - **sudo**。

   ```Shell
   [jackfrued@izwz97tbgo9lkabnat2lo8z ~]$ ls /root
   ls: cannot open directory /root: Permission denied
   [jackfrued@izwz97tbgo9lkabnat2lo8z ~]$ sudo ls /root
   [sudo] password for jackfrued:
   calendar.py  code  error.txt  hehe  hello.c  index.html  myconf  result.txt
   ```

   > **說明**：如果希望使用者能夠以管理員身份執行命令，使用者必須在sudoers（/etc/sudoers）名單中。

8. 登入登出相關 - **logout** / **exit** / **adduser** / **userdel** / **passwd** / **ssh**。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# adduser jackfrued
   [root@izwz97tbgo9lkabnat2lo8z ~]# passwd jackfrued
   Changing password for user jackfrued.
   New password:
   Retype new password:
   passwd: all authentication tokens updated successfully.
   [root@izwz97tbgo9lkabnat2lo8z ~]# ssh hellokitty@1.2.3.4
   hellokitty@1.2.3.4's password:
   Last login: Thu Apr 12 23:05:32 2018 from 10.12.14.16
   [hellokitty@izwz97tbgo9lkabnat2lo8z ~]$ logout
   Connection to 1.2.3.4 closed.
   [root@izwz97tbgo9lkabnat2lo8z ~]#
   ```

9. 檢視系統和主機名 - **uname** / **hostname**。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# uname
   Linux
   [root@izwz97tbgo9lkabnat2lo8z ~]# hostname
   izwz97tbgo9lkabnat2lo8z
   [root@iZwz97tbgo9lkabnat2lo8Z ~]# cat /etc/centos-release
   CentOS Linux release 7.4.1708 (Core) 
   ```

10. 重啟和關機 - **reboot** / **init 6** / **shutdown** / **init 0**。

11. 檢視歷史命令 - **history**。

### 實用程式

#### 檔案和資料夾操作

1. 建立/刪除目錄 - **mkdir** / **rmdir**。

2. 建立/刪除檔案 - **touch** / **rm**。

   - touch命令用於建立空白檔案或修改檔案時間。在Linux系統中一個檔案有三種時間：
     - 更改內容的時間（mtime）
     - 更改許可權的時間（ctime）
     - 最後訪問時間（atime）

3. 切換和檢視當前工作目錄 - **cd** / **pwd**。

4. 檢視目錄內容 - **ls**。

5. 檢視檔案內容 - **cat** / **head** / **tail** / **more** / **less**。

6. 拷貝/移動檔案 - **cp** / **mv**。

7. 檢視檔案及內容 - **find** / **grep**。

   ```Shell
   [root@izwz97tbgo9lkabnat2lo8z ~]# find -name *.html
   ./index.html
   ./code/index.html
   [root@izwz97tbgo9lkabnat2lo8z ~]# grep "<script>" . -R -n
   ./index.html:15:                <script>
   ./code/index.html:2884: <script>
   ./code/foo.html:2:<!--STATUS OK--><html> <head><meta ...
   ```

8. 符號連結 - **ln**。

9. 壓縮和歸檔 - **gzip** / **gunzip** / **xz** / **tar**。

10. 其他工具 - **sort** / **uniq** / **diff** / **file** / **wc**。

#### 管道和重定向

1. 管道的使用 - **\|**。
2. 輸出重定向和錯誤重定向 - **\>** / **2\>**。
3. 輸入重定向 - **\<**。

#### 別名

1. **alias**
2. **unalias**

#### 其他程式

1. 時間和日期 - **date** / **cal**。
2. 錄製操作指令碼 - **script**。
3. 給使用者傳送訊息 - **mesg** / **write** / **wall** / **mail**。

### 檔案系統

#### 檔案和路徑

1. 命名規則
2. 副檔名
3. 隱藏檔案
4. 工作目錄和主目錄
5. 絕對路徑和相對路徑

#### 目錄結構

1. /bin - 基本命令的二進位制檔案
2. /boot - 引導載入程式的靜態檔案
3. /dev - 裝置檔案
4. /etc - 配置檔案
5. /home - 使用者主目錄的父目錄
6. /lib - 共享庫檔案
7. /lib64 - 共享64位庫檔案
8. /lost+found - 存放未連結檔案
9. /media - 自動識別裝置的掛載目錄
10. /mnt - 臨時掛載檔案系統的掛載點
11. /opt - 可選外掛軟體包安裝位置
12. /proc -  核心和程序資訊
13. /root - root賬戶主目錄
14. /run - 存放系統執行時需要的東西
15. /sbin - 超級使用者的二進位制檔案
16. /sys - 裝置的偽檔案系統
17. /tmp - 臨時資料夾
18. /usr - 使用者應用目錄
19. /var - 變數資料目錄

#### 訪問許可權

1. **chmod**。
2. **chown**。

#### 磁碟管理

1. 列出檔案系統的磁碟使用狀況 - **df**。
2. 磁碟分割槽表操作 - **fdisk**。
3. 格式化檔案系統 - **mkfs**。
4. 檔案系統檢查 - **fsck**。
5. 掛載/解除安裝 - **mount** / **umount**。

### 編輯器vim

1. 啟動和退出

2. 命令模式和編輯模式

3. 游標操作

4. 文字操作

5. 查詢和替換

   /正則表示式

   :1,$s/正則表示式/替換後的內容/gice

   g - global

   i - ignore case

   c - confirm

   e - error

6. 引數設定

   .vimrc

   set ts=4

   set nu

7. 高階技巧

   - 對映快捷鍵
     - inoremap key:...
   - 錄製巨集
     - 在命令模式下輸入qa開始錄製巨集（qa/qb/qc/qd）
     - 執行你的操作，這些操作都會被錄製下來
     - 如果要錄製的操作完成了，按q結束錄製
     - @a播放巨集（1000@a - 將巨集播放1000次）

### 環境變數

1. HOME
2. SHELL
3. HISTSIZE
4. RANDOM
5. PATH

### 軟體安裝和配置

#### yum

- yum update
- yum install / yum remove
- yum list / yum search
- yum makecache

#### rpm

- rpm -ivh \-\-force \-\-nodeps
- rpm -e 
- rpm -qa | grep

#### 原始碼構建安裝

- ...
- make && make install

#### 例項

1. 安裝MySQL。
2. 安裝Redis。
3. 安裝NginX。

### 配置服務

1. systemctl start / stop / restart / status
2. systemctl enable / disable
3. 計劃任務 - **crontab**。
4. 開機自啟。

### 網路訪問和管理

1. 通過網路獲取資源 - **wget**。
   - -b 後臺下載模式
   - -O 下載到指定的目錄
   - -r 遞迴下載
2. 顯示/操作網路配置（舊） - **ipconfig**。
3. 顯示/操作網路配置（新） - **ip**。
4. 網路可達性檢查 - **ping**。
5. 檢視網路服務和埠 - **netstat**。
6. 安全檔案拷貝 - **scp**。
7. 安全檔案傳輸 - **sftp**。

### Shell和Shell程式設計

1. 萬用字元。
2. 後臺執行。

### 其他內容

1. awk
2. sed
3. xargs