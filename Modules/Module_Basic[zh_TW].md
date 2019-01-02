# Telegram Bot Framework - 模組編寫入門
## 概述
Telegram 機器人框架的模組其實也只是讓你不用再重寫
Telegram 機器人的 UI。

包括取得訊息、傳送訊息等等都是自己處理，但機器人框架會
傳給您使用者機器人的 Token，供您在 getUpdates 和
sendMessage 上使用。

## 前提
<!-- [2] 見「常數列表」部份 -->
- 不支援 Windows 和 BSD
  - 目前的 Go plugin 功能還不支援以上兩個系統 QAQ，所以以上兩個
    作業系統的使用者就先暫時開 Bash 子系統或是直接開台虛擬機囉……
    十分抱歉！
- 先安裝 golang 開發套件
  - 可以從 [Golang 官方][2] 下載。

## 架構
<!-- 譯註：[zh_TW] 請改成您所使用的語言。 -->
可參考 Spec[zh_TW].md。

```
// 資訊函式。
// 回傳一組 map 映射資訊，請參閱底下《基本資訊》
func Info() map[string]string {}

// 處理訊息的函式。
// token 為機器人的 Token。
func Handler(token string) {}

// 設定函式。
// 若不想要提供設定介面，則直接回傳空值 (return) 即可。
// 請自行處理設定資料，等到設定完成後只需回傳空值 (return) 即可，
// 此程式會自動導回原介面。
func Settings() {}
```

- Info() 會在「模組資訊」區塊出現，可參考 Spec 的 **基本資訊**
  部份。

- Settings() 則是會在「設定模組」區塊出現，設定檔的管理跟其他
  東東都是你自己管，框架僅會呼叫這個函式，而要回到模組管理頁面
  只需要回傳個 `return`。

- Handler() 就是「開啟機器人」之後會不斷 `for` 呼叫的函式，也因此
  **您不需要在 Handler() 裡面搞個 `for` 函式**，否則會本末倒置，導致
  關閉不了機器人。<br></br> <!--換行-->
  
  那又該怎麼寫呢？每次 Handler() 一被呼叫就呼叫 GetUpdates() 取得訊息，並
  處理目前得到的訊息之後回傳個 `return`，就會進入下一圈 `for` 循環了，所以
  這也是上述為何說不應該在 Handler() 多寫一個 `for` 迴圈的關係。

## 函式庫
> 「聽起來我好像什麼都要自己寫？好麻煩啊！」

> 「其實我們有 Telegram Bot 的現成函式庫 XDD」

### 匯入
<!-- [1] 見「常數列表」部份 -->
從 [GitHub 的 TGBotLib 版本庫][1] clone 回來之後直接
在 clone 目錄的上一層開始寫模組，記得在 `import` 區塊
加上 `./TGBotLib` _(註 1)_ 唷！

_(註 1)_ 這是相對匯入，與全路徑匯入 (也就是 `go get` 得到的模組)
不同的地方在於一個存放在 `$GOPATH`，一個直接存在您的工作目錄。

官方建議您使用全路徑匯入，但這裡為了方便是告訴你相對匯入的方法。
但你要全路徑匯入其實也可以，反正目標就只是要載入這個模組 XDD

### 基本使用
首先我們建議你對你的 import 程式加個別名 (例如 `tb`, `tl`……
什麼名稱隨便你，反正目標也就只是取個別名 XDDD），而不要直接輸
`TGBotLib.GetUpdates()` 之類的，否則程式只會很冗長，且你手可能會
提早報廢 (假如你不是用 IDE)，或者是鍵盤先離你而去 Orz。

接著參考 `Basic_Functions` 資料夾內的內容吧，這裡面有很詳細的各項
函式庫使用教學。

<!-- 譯註：[zh_TW] 請改成您所使用的語言。 -->
BTW，因為作者國文能力極爛，所以如果有任何看不懂的地方，請直接開
個 Issues 或是參考 Menu[zh_TW].md 的「聯絡作者」直接問我 XD

## 編譯與使用
最重要的部份，但假如你用 Windows 作業系統，請準備一台 Linux 虛擬機或是
Bash 子系統，因為 go 現在還不支援 Windows 的模組檔編譯 QAQ

### 編譯
1. 首先切換到您模組的所在目錄。
2. 輸入 `go build -buildmode=plugin -o (輸出檔名).so (模組檔名稱).go` _(註 2)_
3. 然後把得到的 so 檔複製到模組框架的 modules 資料夾
  （假如第一次使用，先開啟程式讓程式建立 modules 資料夾）
4. 完成！

_(註 2)_ 目前我測試過能編譯模組的只有 Darwin (macOS) 和 Linux，所以包括 BSD、
Windows 的使用者我只能說聲抱歉 orz

但連使用都要開個 Linux QwQ…… Go 什麼時候才要支援 Windows 的模組檔編譯啦！Orz

## 接下來？
開發更多模組，讓這框架更加完整吧！假如你在開發時碰到了 Bug，立刻開 Issues 給我
（語言隨便，但建議中文 or 英文），我會在我所能的時間趕快幫你解決問題的！:)

<!-- 常數列表 -->
<!-- [1] Telegram Bot 函式庫位址 -->
[1]: https://github.com/go-tgbot-framework/TGBotLib
<!-- [2] Golang 的官方網站 -->
[2]: https://www.golang.org
