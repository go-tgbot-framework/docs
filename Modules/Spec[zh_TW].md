## 基本準則
- package 必須以 "main" 起頭
- 編譯方法：`go build -buildmode=plugin (檔案名稱).go`

## 基本架構
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

## 基本資訊
```
func Info() map[string]string {
    // 只要確保回傳的是 map[string]string 格式
    // 並有 Name, Author, Version, Description（加其他的也行，但我們只抓這四項）
    // 即可。
    var info map[string]string = {
        "Name": "模組名稱",
        "Author": "模組作者",
        "Version": "模組版本",
        "Description": "模組資訊",
    }
    return info
}
```
