---
layout:     post
title:      CongJyu 嘅 Linux 使用配置入門筆記
subtitle:   Linux 配置等等
date:       2023-05-06
author:     Rain Chen
catalog: true
tags:
    - 編程思考
---

# CongJyu 嘅 Linux 使用配置入門筆記

Written by [Rain CongJyu Chen](https://github.com/congjyu), not for commercial use.

2023-05-02

繁體粵語版

## 0. 基本介紹，揀一個啱你用嘅 Linux Distro

### 0.1. Unix/Linux 哲學：世界觀與方法論

Linux 哲學直接來源於 Unix 哲學。一種比較精簡嘅講法係以下三條理念：

1. 一個應用盡可能只關注一個目標
2. 盡可能使多個應用相互組合
3. 一切皆文件

Unix 分支，如下面嘅圖片所示。值得注意嘅係圖中紫色嘅部分，較為舊嘅 Mac OS X 同埋而家 macOS 嘅 Darwin 內核都係標準嘅 Unix 系統。所以，如果你係 macOS 用戶，咁你已經可以算係一個 Unix 用戶，對於 Unix-like 系統已經有少少理解。

上面圖片中嘅系統，如 Linux、macOS 都可以算係 Unix-like 系統。Unix 誕生同發展嘅時期係上個世紀六十到八十年代，所以「崇尚自由」嘅理念一直貫穿喺整個 Unix 哲學入面。

#### 0.1.1. 一個應用盡可能只關注一個目標

如果一個 App 咩都做，咁佢大概率乜都做唔好。理解自己嘅目標，知道自己要做啲乜非常之重要。需要注意嘅係，明確自己嘅目標唔係指呢個目標一直唔變，而係需要一個短期嘅目標、長期嘅目標咁。

#### 0.1.2. 盡可能使多個應用相互組合

「面向現實」，承認應用都係有缺點嘅，所以需要組合。唔好使用一個「超級應用」完成你需要嘅任務或者工作。雖然「超級應用」有可能省時省力，但係我哋唔可以假裝睇唔見佢巨大嘅「集中性風險」，包括但唔限於歷史數據遷移困難等等⋯⋯

> 我相信，上个世纪的 Unix 大佬，对于系统的「集中性风险」显然有过认真的考虑，可能和「越战创伤」有关，他们可能害怕集中且无所顾忌的……我觉得不能说更多……
> 
> —— 火箭君CC（Matrix 作者）

#### 0.1.3. 一切皆文件

文件（File）係一組相關訊息嘅組合。我哋把一個網頁、一個雲上文檔、一封 email、一張相片、一個傾偈訊息都叫做「文件」。喺而家呢個手機互聯網時代，係唔係感覺對呢個概念好陌生？

從某種程度來講，文件係各種信息統一嘅終點，係一種 ultimate solution 一般嘅存在。通過電子文件，你可以保存、存檔、備份各種信息，存儲喺自己可控嘅存儲介質入面，唔使承受雲盤關停之類嘅風險。

#### 0.1.4. 本節結語

無論你係唔係認同「Unix 哲學」，又或者對佢冇興趣，都唔係咁重要。緊要嘅係我哋知道 Unix 嘅嗰啲了不起嘅先驅，佢哋搵到咗心中嘅「獨角獸」，並喺 Unix 系統中用一種驚世駭俗嘅方式體現出來。咁我覺得話 Unix 係一個有靈魂嘅系統其實不為過。

一齊感受一下「Unix 哲學」帶來嘅思考同震撼！

### 0.2. Linux 發行版

#### 0.2.1. 簡介

Linux 發行版（Linux Distro）係各個 Linux 版本嘅一種統稱，包括但唔限於 Debian、Ubuntu、Fedora、Arch、RedHat、Manjaro 等等等等。對於各種發行版，我哋可以基本分為幾大派系，依據佢哋嘅內核來歸類。

- Debian 系：Debian 系統以及佢嘅各分支，包括 Ubuntu 及其分支 Kubuntu、Xubuntu、Kylin OS 等等。
- Red Hat 系：RHEL 系統同埋 Fedora 系統及佢嘅各分支，包括但唔限於 CentOS 等等。
- Arch 系：Arch Linux 系統同埋佢嘅各分支，包括但唔限於 Manjaro、Asahi Linux、Black Arch 等等。

完整詳細嘅 Linux 版本分支可以去搵網上嘅圖片。

每個發行版都有各自嘅特點，你只需要揀一個適合自己嘅系統來用就得。上網絡上嘅 Linux 社區睇下大家對各個 Linux Distro 嘅評價同使用情況，或者問下其他嘅 Linux 使用者嘅建議。

#### 0.2.2. 點樣安裝

首先，你已決定咗你想用嘅 Linux Distro，咁就去相應嘅 Linux Distro 嘅官方網站上面下載鏡像（比如，`.iso` 文件），下載完之後，準備安裝。如果你係第一次使用 Linux 系統，又或者你之前冇安裝過操作系統，咁我推介喺虛擬機器入面安裝試下；如果你有熟練嘅使用經驗，咁直接安裝到物理機器上面，甚至安裝雙系統都係冇問題嘅。

掛載鏡像文件，啟動電腦。按照提示操作，揀你需要嘅選項，設置用戶名（username）同密碼（password），等待完成安裝。

注意，請一定要記住自己嘅用戶名同密碼，千祈唔好忘記。

## 1. 配置環境

而家假定你已經安裝好咗一個 Linux 系統。點樣算安裝好？如果係有圖形介面嘅系統（比如 Debian），你睇到 Desktop 嘅畫面嘅時候就算安裝好；如果係冇圖形介面嘅系統（比如 Arch），睇到「`你嘅用戶名@你嘅電腦名 ~ $`」就算安裝好。比如：

```shell
[rainchen@rains-arch ~]#
```

就係一個 Arch 系統安裝好咗嘅表現。

### 1.1. 終端與 Shell

#### 1.1.1. 基本介紹

終端、Shell、tty 同埋 console 有咩區別？佢哋分別係咩意思？

喺上古時期，呢啲嘢都係有實體嘅。

- Terminal，即係終端，唔係主機上面，遠端控制。
- Console，即係控制台，係主機上面，本機控制。
- TTY，係「電傳打字機（TeleTypewriter）」嘅縮寫，長得好似一台打字機咁。

但係而家，Console 同 Terminal 大部分時候被人理解成同一個概念，叫做「終端」或者「命令行」。睇下邊呢個 screenshot，可以睇到寫住 ttys000。

咁話又講返，咩係 Shell 咧？我哋講到命令嗰陣時，我哋一般係講緊 shell。shell 係一個接收鍵盤輸入嘅命令並且把呢啲命令冚唪唥傳俾操作系統嘅程式。可以話，幾乎所有嘅 Linux 發行版都提供 shell 程式。呢個程式來自於一個叫做 bash 嘅 GNU Project，bash 係 Bourne Again Shell 嘅縮寫，Bourne 係一個人嘅名。

如果我哋需要訪問 shell 並且同電腦系統交互，就需要一個而家叫做「終端模擬器（Terminal Emulator）」嘅嘢。喺一啲帶有 GNOME 環境嘅 Linux 發行版（比如 Debian、Ubuntu）上，可以搵到一個「GNOME Terminal」；喺一啲帶有 KDE 環境嘅 Linux 發行版（比如 Manjaro）上面，可以搵到一個「Konsole」；喺 macOS 上面，可以搵到一個「Terminal」，或者好似我一樣把佢換成「iTerm2」。

值得注意嘅係，終端開頭左邊嗰段文字叫做「提示符（prompt）」，終於提示當前狀態同標記輸入命令嘅位置。提示符右邊嗰個「`#`」、「`$`」或者「`%`」代表權限提示符，其中「`%`」同「`$`」表示普通用戶權限，「`#`」表示超級用戶（root）權限。**一般情況下請唔好使用超級用戶權限來做嘢。**

下面使用 `$` 表示喺終端輸入命令，喺實際輸入命令嘅時候唔需要輸入 `$` 及類似嘅嘢。

#### 1.1.2 常用嘅命令

***注意，請時時刻刻留心空格（space），防止命令出錯。***

##### 1.1.2.1. 列出文件

使用以下命令。

```shell
$ ls
```

`ls` 命令係 list 嘅縮寫，表示列出當前目錄下面嘅嘢。呢個命令後面可以帶上參數。比如：

- `-l` 表示除咗文件名之外，仲要列出所有信息。
- `-a` 表示列出包括 `.` 開頭嘅隱藏文件嘅所有文件。

等等。其他嘅命令可以在需要用嘅時候上網搜尋。

睇下我喺 `~` 目錄下輸入 `ls -l` 命令之後嘅結果。

```shell
$ ls -l
```

喺我嘅電腦上面輸出下面內容：

```shell
total 32
drwxr-xr-x@ 176 rainchen  staff   5632 May  1 15:37 Calibre Library
drwxr-xr-x@  20 rainchen  staff    640 Apr 28 21:46 CodingArea
drwx------+   7 rainchen  staff    224 May  1 21:20 Desktop
drwx------+  13 rainchen  staff    416 Apr 25 12:26 Documents
drwx------+   6 rainchen  staff    192 May  1 15:24 Downloads
drwxr-xr-x    8 rainchen  staff    256 Mar 31 23:34 HMCL
drwx------+ 100 rainchen  staff   3200 Apr  9 21:04 Library
drwx------    5 rainchen  staff    160 May  1 11:48 Movies
drwx------+   5 rainchen  staff    160 Apr  3 16:38 Music
lrwxr-xr-x    1 rainchen  staff     54 Mar 26 20:45 OneDrive - CCDXUNVS -> /Users/rainchen/Library/CloudStorage/OneDrive-CCDXUNVS
drwx------@   5 rainchen  staff    160 Apr 11 09:47 Parallels
drwx------+  10 rainchen  staff    320 Apr 21 23:54 Pictures
drwxr-xr-x+   5 rainchen  staff    160 Apr 11 23:43 Public
-rw-r--r--@   1 rainchen  staff  12564 Apr 25 01:16 Rain's Profile.json
drwxr-xr-x@  11 rainchen  staff    352 Mar 26 23:39 jetbra
drwxr-xr-x   11 rainchen  staff    352 Apr 29 22:48 vmShare
```

##### 1.1.2.2. 轉換目錄

使用以下命令。

```shell
$ cd [目錄名]
```

`cd` 命令係 change directory 嘅縮寫，表示切換到後面呢個目錄。方形括號 `[]` 表示呢部分內容可以省略。當 `cd` 命令後面冇任何嘢嘅時候，表示切換到使用者嘅 `home` 目錄，即係用戶一開始登錄系統嘅時候嘅目錄，經常用一個波浪線 `~` 表示。另外，一粒點 `.` 表示當前目錄，兩粒點 `..` 表示當前目錄嘅上一層目錄。

切到自己嘅 `home` 目錄：

```shell
$ cd ~
```

或者，咁寫：

```shell
$ cd
```

從提示符可以睇到而家嘅目錄喺邊度，比如呢個：

```shell
[rainchen@Rains-Macbook-Pro ~]%
```

表示當前目錄係 `home` 目錄；

```shell
[rainchen@Rains-Macbook-Pro CodingArea]%
```

表示當前喺 `/Users/rainchen/CodingArea` 目錄下。

##### 1.1.2.3. 安裝軟件包命令

大部分 Unix/Linux 系統使用「包管理工具（package manager）」來安裝同卸載軟件包，但係唔同嘅 Linux Distros 可能使用唔同嘅包管理工具。

- Debian 系嘅系統：一般使用 `apt` 作為軟件包管理工具。
- Red Hat 系嘅系統：一般使用 `dnf` 或者 `yum` 作為軟件包管理工具。
- Arch 系嘅系統：一般使用 `pacman` 作為軟件包管理工具。
- Darwin (macOS) 系統：一般使用 `homebrew` 作為軟件包管理工具。

當然，軟件包管理工具並唔係唯一唔變嘅，你可以換咗佢，只要佢唔 crash。

###### 1.1.2.3.1. apt

對於 Debian 系嘅系統，包括 Debian、Ubuntu、Xubuntu、Kylin 之類，使用 `apt` 命令，`apt` 係 Advanced Package Tool 嘅縮寫，佢嘅語法係咁嘅：

```shell
$ apt [選項] [命令] [軟件包]
```

搵嘢：

```shell
$ apt search <你想搵嘅嘢>
```

安裝軟件：

```shell
$ apt install <軟件包名>
```

刪除軟件包：

```shell
$ apt remove <軟件包名>
```

刪除軟件包同埋佢嘅配置：

```shell
$ apt purge <軟件包名>
```

仲有好多命令，可以喺需要使用嗰陣時去網絡上面搵，有非常之多嘅參考資料。需要注意嘅係，上面呢啲命令之中，有啲係需要超級權限來運行嘅，咁只需要喺嗰個命令前面加埋一個 `sudo` 就得，例如安裝一個 VLC 播放器（VLC Media Player）：

```shell
$ sudo apt install vlc
```

###### 1.1.2.3.2. dnf

對於 Rad Hat 系嘅 Linux Distros，例如 Fedora、RHEL 等等，適用呢一個。`dnf` 係 Dandified YUM 嘅縮寫，作為 Red Hat 系嘅 Linux Distros 嘅 `yum` 包管理器嘅下一代版本。佢嘅語法係咁嘅：

```shell
$ dnf [參數] [軟件名]
```

詳細信息可以去到網絡上面搵到，暫時先寫咁多。

###### 1.1.2.3.3. pacman

對於 Arch 系嘅 Linux Distros，使用 `pacman` 作為包管理，詳細請參考著名嘅 Arch Wiki：<https://wiki.archlinux.org/title/Pacman>，此處冇必要再介紹。

##### 1.1.2.4. 嗌超級用戶嚟做

使用以下命令：

```shell
$ sudo <你要超級用戶權限做嘅命令>
```

呢個 `sudo` 命令嘅全稱係 Super User Do，意思係超級用戶來做，可以用於你需要提升權限執行命令嗰陣時。`sudo` 命令係一個 Linux 系統管理指令，佢允許系統管理員讓普通用戶執行一啲 `root` 命令（比如一部分系統嘅安裝軟件操作）嘅工具。

### 1.2. 文本編輯器與 IDE

***請注意，以下內容有可能引起「編輯器聖戰」，小心。***

而家主流嘅編輯器同埋 IDE 有咩咧？

- Vim
- Emacs
- VSCode
- Sublime
- JetBrains 嘅 IDEs
- 等等等等

***請注意，為咗你嘅人身安全考慮，唔好喺 Vim 重度用戶面前講 Emacs 好用或者 Vim 難用⋯⋯又或者唔好喺 Unix/Linux 用戶面前嗌佢哋去使用乜 Dev-C++ 之類嘅嘢。***

#### 1.2.1. Vim

Vim the editor.

> 幫助可憐嘅烏干達兒童！
>
> —— Vim 編輯器

> Vim is a highly configurable text editor built to make creating and changing any kind of text very efficient. It is included as "vi" with most UNIX systems and with Apple OS X.

根據 Vim 官方網站嘅講法，佢係一個非常可定製嘅文本編輯器，同時非常之高效。佢已經包含於大部分嘅 Unix 系統入面，包括 macOS。下面係我電腦嘅 Vim 初始界面嘅 screenshot，可以清楚嘅睇見「Help poor children in Uganda!」呢句話。

需要注意嘅係，Vim 會使你擺脫鼠標⋯⋯所以請唔好嘗試使用鼠標來操作呢個軟件，因為完全冇用。

使用之前請先認真配置，寫好 `.vimrc` 嘅配置文件，放喺 `~` 目錄下面；同時，你需要足夠了嘅呢個軟件嘅操作模式，多加練習。又或者，你會走投無路，去網絡上面搵「點樣退出 Vim 編輯器」⋯⋯下面係一個 meme，非常之貼合 Vim 嘅特點，你有冇 get 到呢個 point 咧？

#### 1.2.2. Emacs

唔好意思啊，我唔識用，咁就唔寫先。

#### 1.2.3. VSCode

呢個嘢全稱係 Visual Studio Code，注意，佢唔係 Visual Studio，佢係 Visual Studio Code。呢兩隻嘢區別非常大，此處唔討論 Visual Studio，我哋只係介紹下 VSCode。

誒算啦，冇乜嘢好介紹，自己去 VSCode 文檔嗰度睇：<https://code.visualstudio.com> 同埋 <https://code.visualstudio.com/docs>。

#### 1.2.4. Sublime Text

係同 VSCode 差唔多嘅一個編輯器，我亦都唔多識用，咁唔寫先。

### 1.3. 編程開發環境配置

未完待續。

## 2. 安裝輸入法

Linux 系統喺啱安裝完嘅時候係只單有一個英語嘅輸入嘅，如果你需要使用其他輸入法（例如粵語、漢語拼音、泰語、日語之類嘅語言輸入），咁就需要自己安裝，有時候你可能需要去睇睇輸入法嘅安裝說明，或者 wiki。一般呢啲嘢會被軟件作者或者文檔作者放喺 GitHub 上面，入面一般有非常詳細嘅操作步驟。

### 2.1. 粵語拼音（JyutJyu PingJam）

使用香港語言學學會粵語拼音方案（LSHK Jyutping）。

如果你之前使用過 Rime，請直接使用以下口令：

```shell
$ bash rime-install cantonese
```

如果你係全新開始安裝，請按照以下步驟，**呢啲嘢適用於 Debian 系嘅 Linux**：

準備安裝環境，更新 `apt` 管理器：

```shell
$ sudo apt-get update && sudo apt-get upgrade
```

然後安裝「中州韻」算法引擎：

```shell
$ sudo apt-get install git ibus-rime -y
```

跟住安裝粵語輸入法同埋相關嘅程式：

```shell
$ curl -fsSL https://git.io/rime-install | bash -s -- cantonese emoji CanCLID/rime-loengfan custom:set:config=default key=installed_from,value=rime-cantonese custom:clear_schema_list custom:add:schema=jyut6ping3 custom:add:schema=cangjie5 custom:add:schema=stroke custom:add:schema=luna_pinyin lotem/rime-octagram-data lotem/rime-octagram-data@hant lotem rime-octagram-data:customize:schema=jyut6ping3,model=hant
```

呢個命令好長，請一次性輸入完成，唔好中間撳「回車」掣；上面為咗寫得落所以換咗行，所以請係每一個換行嘅位置用一個空格（space）來代替。

如果你後續想要更新碼表檔案同埋詞庫資料，使用以下口令就得：

```shell
$ cd plum
$ bash rime-install cantonese
```

### 2.2. 漢語拼音（Han Yu Pin Yin）

使用中國大陸嘅漢語拼音方案。

使用 fcitx5 輸入法，輸入以下命令安裝，對於 Debian 係嘅 Linux 發行版：

```shell
$ sudo apt install fcitx5
```

跟住安裝 Google 拼音：

```shell
$ sudo apt install fcitx-googlepinyin
```

然後進行設置就可以使用啦，有其他需求可以參考網絡上面嘅其他文檔資料。

### 2.3. 維吾爾語（ئۇيغۇر تىلى）

如果你係使用緊帶有 GUI （圖形用戶界面）嘅 Linux 系統，咁打開設置（Settings 或者 Preference），揀語言，撳「添加語言輸入」掣，輸入 Uyghur 搵輸入法。搵到之後撳「添加（Add）」掣，然後就可以使用啦。需要切換輸入嘅話就喺系統嘅菜單欄嗰度撳當前輸入語言嘅標籤，即可切換。

### 2.4. 哈薩克語（Қазақ тілі）

操作方式同上，但係非常遺憾嘅係只搵到西里爾字母嘅哈薩克語輸入，仲未搵到完善嘅阿拉伯字母嘅哈薩克語輸入方式⋯⋯唯一有一個唔係好成熟嘅輸入方式係呢個：

「Rime Hapin Input Schema رايم حاپين ەنگيزۋ سقەماسى」

佢嘅介紹同源代碼喺呢度：<https://github.com/ha-pin/hapin-arabic>，就唔知好唔好用，放入嚟做一個參考。

### 2.5. 本節結語

實話講，如果你睇得明 English 嘅話，***請使用英語作為系統嘅顯示語言***，然後添加自己需要用嘅輸入法。問就係萬一路徑（path）或者目錄入面就其他文字嘅名，然後又冇輸入法⋯⋯諗下就高血壓。

## 3. 安裝實用軟件

自己睇住嚟搞，放住喺度先。

## 4. 根據自己嘅偏好來設定界面

前面用熟咗先，就咁啦。
