# Telegram Bot Libraries - 函式庫完整手冊
這裡會用淺顯易懂的方式告訴您該如何使用函式庫。

因為函式庫會有些許變動，因此若有差異請立即使用
Issues 回報唷～

## 建構體們
### Update (用於 `GetUpdates()`)
```
// 基本建構體：Update
//
// 請參考文件：https://core.telegram.org/bots/api#update
//
// 為讓部份建構體可被導出，因此部份名稱有修改過。請參考 `json:"xxx"`
// 得知原本代表的項目。
```

其實這跟 https://core.telegram.org/bots/api#update 裡面講的東西真的差不多，
你大概可以把他裡面的資料想成把官方的 JSON 首字大寫然後用大駝峰命名風格
搞出來的東西。

而我這裡會用中文來告訴你這建構體到底裝了些什麼～

- Result: 一個裡面包著 struct 的 Slice，例如 Result[len(Result)-1] 就能取得 Result
  切片最後一項的內容、Result[0] 就能取得 Result 切片第一項的內容。
  - UpdateID (int): 此次 Updates 的專屬 ID。
  - Message (Message): 使用者新傳的訊息，是個建構體。
    - MessageID (int): 此訊息的專屬 ID。
    - From (User): 告訴你是哪位使用者傳的訊息。
      - ID (int): 使用者的專屬 ID。
      - IsBot (bool): 這使用者是不是個機器人呢？如果是就回傳 true，反之 false。
      - FirstName (string): 使用者的**名**。歐洲和美國人的取名方式是「名在前姓在後」，也因此 FirstName
        跟我們一般所認知的「姓」是不同的唷！
      - LastName (string): 使用者的**姓**。(若未取姓則為空白)
      - Username (string): 使用者的使用者名稱，也就是你熟悉的「@blablabla」，但注意 Username 回傳的不包含「@」這個字符喔！
        也就是直接就是「blablabla」(無使用者名稱則為空白)
      - LanguageCode (string): 使用者的語言代碼，例如「zh-hant」之類的。
    - Date (int): Unix 時間格式的訊息傳送時間。
    - Chat (Chat): 使用者傳送此訊息時所在的聊天室 (又譯對話)
      - ID (int): 這個聊天室 (或使用者) 的專屬 ID。
      - Type (string): 此聊天室的種類，例如「supergroup」或「private」
      - Title (string): 此聊天室的標題，如果是私訊機器人就會是空白的。
      - Username (string): 使用者的使用者名稱，也就是你熟悉的「@blablabla」，但注意 Username 回傳的不包含「@」這個字符喔！
        也就是直接就是「blablabla」(無使用者名稱則為空白)
      - FirstName (string): 使用者的**名**。歐洲和美國人的取名方式是「名在前姓在後」，也因此 FirstName
        跟我們一般所認知的「姓」是不同的唷！(非私訊時則為空白)
      - LastName (string): 使用者的**姓**。 (非私訊或未取姓時則為空白)
      - AllMembersAreAdmins (bool): 是否所有的成員都是管理員？如果是就回傳 true，反之 false。
    - ForwardFrom (User): 被轉傳使用者的資訊，架構同 From，請參考 From 部份。
    - ForwardFromChat (Chat): 被轉傳聊天室的資訊，架構同 Chat，請參考 Chat 部份。
    - ForwardDate (int): 被轉傳訊息當初傳的時間 (Unix 格式)。
    - Text (string): 傳送之訊息文字
    - (其實還有更多，但還在補……)
  - EditedMessage: 使用者新編輯的訊息，架構同 Message，請參考 Message 部份。
  - ChannelPost: 頻道新發出的訊息，架構同 Message，請參考 Message 部份。
  - EditedChannelPost: 頻道新編輯的訊息，架構同 Message，請參考 Message 部份。

### Update_GetMe (用於 `GetMe()`)
```
// 基本建構體：Update_GetMe
//
// 如文件：https://core.telegram.org/bots/api#getme
// 所說，就是一個 Result 裡面包著 User 的 JSON。
//
// 為讓部份建構體可被導出，因此部份名稱有修改過。請參考 `json:"xxx"`
// 得知原本代表的項目。
```

其實這跟 https://core.telegram.org/bots/api#getme 裡面講的東西真的差不多，
你大概可以把他裡面的資料想成把官方的 JSON 首字大寫然後用大駝峰命名風格
搞出來的東西。

建構體的中文解釋：

- Result (User): 機器人的資訊 (這次沒有包 Slice，他的下層確實就是 User)
  - ID (int): 機器人的專屬 ID。
  - IsBot (bool): 這使用者是不是個機器人呢？大部分情況下應該都會回傳 true。
  - FirstName (string): 機器人的**名**。歐洲和美國人的取名方式是「名在前姓在後」，也因此 FirstName
    跟我們一般所認知的「姓」是不同的唷！
  - LastName (string): 機器人的**姓**。(若未取姓則為空白)
  - Username (string): 機器人的使用者名稱，也就是你熟悉的「@blablabla」，但注意 Username 回傳的不包含「@」這個字符喔！
    也就是直接就是「blablabla」(無使用者名稱則為空白)
  - LanguageCode (string): 機器人所在區域的的語言代碼。

## 函式們
### 基礎版函式
這是官方所沒有的函式，其實照我開發的大原則：「盡量不要多出官方函式以外的功能」，
這很明顯是完全違反了。

但大部分的使用者其實也就只會用到那幾個功能，所以基礎版函式還是有其存在的必要。

#### GetUpdatesBasic()
```
// 基本函式：接收訊息函式 (最基本模式)
//
// 請參考文件：
// https://core.telegram.org/bots/api#getting-updates
//
// token: 機器人 (從 @botFather 取得的) Token
//
// clean_prev_msg: 是否透過設定上一個接收的 offset 來防止已抓取訊息再次出現。
// 可參閱：https://core.telegram.org/bots/faq#long-polling-gives-me-the-same-updates-again-and-again
//
// 回傳內容：伺服器收到回應後傳回訊息。
func GetUpdatesBasic(token string, clean_prev_msg bool) *Update 
```

> 「這也太簡單了吧……？」

就只有兩項，第一項是必要的 token -- 框架會給你 token，所以這點可以忽略。

那 clean_prev_msg 又是什麼呢？其實這是 Telegram Bot API 的一個機制，大概接收到
一定的訊息數之後就不接收了，等到你給他「已讀」。而 clean_prev_msg
的用途就是「已讀」那些你已經接收過的訊息。

至於回傳的 Update 建構體就往上翻到《建構體們》中的《Update (用於 `GetUpdates()`)》
區塊囉。

#### SendMessageBasic()
```
// 基本函式：傳送訊息函式 (最基本模式)
//
// 請參考文件：
// https://core.telegram.org/bots/api#sendmessage
//
// token: 機器人 (從 @botFather 取得的) Token
//
// 回傳內容：伺服器收到回應後傳回訊息。
func SendMessageBasic(token string, chat_id int, text string) string
```

比起原本的 SendMessage() 函式，這真的短了許多。

第一項是必要的 token -- 框架會給你 token，所以這點可以忽略。

第二項的 chat_id 就是要傳到哪個聊天室囉。你可以用 GetMessage() 或
GetMessageBasic() 甚至自己帶入都行。chat_id 是組數字（聊天室專屬 ID），
但你看說明會發現到他還可以帶入使用者名稱 (username)，但因為作者偷懶，
懶著抓型態所以就暫時只能用 chat_id（抱歉）。

第三項的 text 則是要傳送的訊息 -- 這應該十分好理解，你要傳任何訊息都行 --
甚至包括包含換行的文字都行（會經處理），所以不必過於拘束，直接傳送你所想傳
的訊息就好～

最後回傳的內容是未經處理的伺服器傳回內容，大概就是「訊息傳送成功～」的訊息，
但假如訊息並沒如你期望般正常送出，那你可能要看一下訊息寫了什麼錯誤訊息囉。

### 完整版函式
這還是有加一點官方所沒有的功能，但我在文件中都會逐一詳細描述，所以不必太擔心
這些額外增加的功能你看不懂。

#### GetUpdates() 函式
```
// 基本函式：接收訊息函式
//
// 請參考文件：
// https://core.telegram.org/bots/api#getting-updates
//
// token: 機器人 (從 @botFather 取得的) Token
//
// offset: 若不打算設定 offset，請傳入 -1。
//
// limit: 若不打算設定 limit，請傳入 -1。
//
// timeout: 若不打算設定 timeout，請傳入 -1。
//
// clean_prev_msg: 是否透過設定上一個接收的 offset 來防止已抓取訊息再次出現。
// 可參閱：https://core.telegram.org/bots/faq#long-polling-gives-me-the-same-updates-again-and-again
//
// 回傳內容：Update 建構體。
func GetUpdates(token string, offset, limit, timeout int, clean_prev_msg bool) *Update
```

> 這跟 GetUpdatesBasic() 也差太多了吧…… QAQ

這是跟官方 API 大概一致的版本，也因此裡面有許多不是一般開發者所需的參數。
所以除非這裡面有你所需的參數，否則我還是比較建議你使用 `GetUpdatesBasic()` --
簡單化但結果相同的函式。

