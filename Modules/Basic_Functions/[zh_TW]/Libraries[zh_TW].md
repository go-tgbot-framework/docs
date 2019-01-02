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
      - Username (string): 使用者的……使用者名稱？反正就是你熟悉的「@blablabla」，但注意 Username 回傳的不包含「@」這個字符喔！
        也就是直接就是「blablabla」(無使用者名稱則為空白)
      - LanguageCode (string): 使用者的語言代碼，例如「zh-hant」之類的。
    - Date (int): Unix 時間格式的訊息傳送時間。
    - Chat (Chat): 使用者傳送此訊息時所在的聊天室 (又譯對話)
      - ID (int): 這個聊天室 (或使用者) 的專屬 ID。
      - Type (string): 此聊天室的種類，例如「supergroup」或「private」
      - Title (string): 此聊天室的標題，如果是私訊機器人就會是空白的。
      - Username (string): 使用者的……使用者名稱？反正就是你熟悉的「@blablabla」，但注意 Username 回傳的不包含「@」這個字符喔！
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

<!-- [TODO] 待補，打這一堆好累-->
