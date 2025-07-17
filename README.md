# Telegraph-Image

免費圖片托管解決方案，Flickr/imgur 替代品。使用 Cloudflare Pages 和 Telegraph。

[English](README-EN.md)|中文

> [!IMPORTANT]
>
> 由於原有的Telegraph API接口被官方關閉，需要將上傳渠道切換至Telegram Channel，請按照文檔中的部署要求設置`TG_Bot_Token`和`TG_Chat_ID`，否則將無法正常使用上傳功能。

## 如何獲取Telegram的`Bot_Token`和`Chat_ID`

如果您還沒有Telegram賬戶，請先創建一個。接著，按照以下步驟操作以獲取`BOT_TOKEN`和`CHAT_ID`：

1. **獲取`Bot_Token`**
   - 在Telegram中，向[@BotFather](https://t.me/BotFather)發送命令`/newbot`，根據提示依次輸入您的機器人名稱和用戶名。成功創建機器人後，您將會收到一個`BOT_TOKEN`，用於與Telegram API進行交互。
   
![202409071744569](https://github.com/user-attachments/assets/04f01289-205c-43e0-ba03-d9ab3465e349)

2. **設置機器人為頻道管理員**
   - 創建一個新的頻道（Channel），進入該頻道後，選擇頻道設置。將剛剛創建的機器人添加為頻道管理員，這樣機器人才能發送消息。

![202409071758534](https://github.com/user-attachments/assets/cedea4c7-8b31-42e0-98a1-8a72ff69528f)
   
![202409071758796](https://github.com/user-attachments/assets/16393802-17eb-4ae4-a758-f0fdb7aaebc4)


3. **獲取`Chat_ID`**
   - 通過[@VersaToolsBot](https://t.me/VersaToolsBot)獲取您的頻道ID。向該機器人發送消息，按照指示操作，最後您將得到`CHAT_ID`（即頻道的ID）。
   - 或者通過[@GetTheirIDBot](https://t.me/GetTheirIDBot)獲取您的頻道ID。向該機器人發送消息，按照指示操作，最後您將得到`CHAT_ID`（即頻道的ID）。

   ![202409071751619](https://github.com/user-attachments/assets/59fe8b20-c969-4d13-8d46-e58c0e8b9e79)

最後去Cloudflare Pages後台設置相關的環境變量（注：修改環境變量後，需要重新部署才能生效）
| 環境變量        | 示例值                    | 說明                                                                                   |
|-----------------|---------------------------|----------------------------------------------------------------------------------------|
| `TG_Bot_Token`   | `123468:AAxxxGKrn5`        | 從[@BotFather](https://t.me/BotFather)獲取的Telegram Bot Token。                        |
| `TG_Chat_ID`     | `-1234567`                 | 頻道的ID，確保TG Bot是該頻道或群組的管理員。 |

## 如何部署

### 提前準備

你唯一需要提前準備的就是一個 Cloudflare 賬戶 （如果需要在自己的服務器上部署，不依賴 Cloudflare，可參考[#46](https://github.com/cf-pages/Telegraph-Image/issues/46) ）

### 手把手教程

簡單 3 步，即可部署本項目，擁有自己的圖床

1.Fork 本倉庫 (注意：必須使用 Git 或者 Wrangler 命令行工具部署後才能正常使用，[文檔](https://developers.cloudflare.com/pages/functions/get-started/#deploy-your-function))

2.打開 Cloudflare Dashboard，進入 Pages 管理頁面，選擇創建項目，選擇`連接到 Git 提供程序`

![1](https://telegraph-image.pages.dev/file/8d4ef9b7761a25821d9c2.png)

3. 按照頁面提示輸入項目名稱，選擇需要連接的 git 倉庫，點擊`部署站點`即可完成部署

## 特性

1.無限圖片儲存數量，你可以上傳不限數量的圖片

2.無需購買服務器，托管於 Cloudflare 的網絡上，當使用量不超過 Cloudflare 的免費額度時，完全免費

3.無需購買域名，可以使用 Cloudflare Pages 提供的`*.pages.dev`的免費二級域名，同時也支持綁定自定義域名

4.支持圖片審查 API，可根據需要開啟，開啟後不良圖片將自動屏蔽，不再加載

5.支持後台圖片管理，可以對上傳的圖片進行在線預覽，添加白名單，黑名單等操作

### 綁定自定義域名

在 pages 的自定義域里面，綁定 cloudflare 中存在的域名，在 cloudflare 托管的域名，自動會修改 dns 記錄
![2](https://telegraph-image.pages.dev/file/29546e3a7465a01281ee2.png)

### 開啟圖片審查

1.請前往https://moderatecontent.com/ 注冊並獲得一個免費的用於審查圖像內容的 API key

2.打開 Cloudflare Pages 的管理頁面，依次點擊`設置`，`環境變量`，`添加環境變量`

3.添加一個`變量名稱`為`ModerateContentApiKey`，`值`為你剛剛第一步獲得的`API key`，點擊`保存`即可

注意：由於所做的更改將在下次部署時生效，你或許還需要進入`部署`頁面，重新部署一下該本項目

開啟圖片審查後，因為審查需要時間，首次的圖片加載將會變得緩慢，之後的圖片加載由於存在緩存，並不會受到影響
![3](https://telegraph-image.pages.dev/file/bae511fb116b034ef9c14.png)

### 限制

1.由於圖片文件實際存儲於 Telegraph，Telegraph 限制上傳的圖片大小最大為 5MB

2.由於使用 Cloudflare 的網絡，圖片的加載速度在某些地區可能得不到保證

3.Cloudflare Function 免費版每日限制 100,000 個請求（即上傳或是加載圖片的總次數不能超過 100,000 次）如超過可能需要選擇購買 Cloudflare Function 的付費套餐，如開啟圖片管理功能還會存在 KV 操作數量的限制，如超過需購買付費套餐

### 感謝

Hostloc @feixiang 和@烏拉擦 提供的思路和代碼

## 更新日志
2024 年 7 月 6 日--後台管理頁面更新

- 支持兩個新的管理頁面視圖（網格視圖和瀑布流）

    1、網格視圖，感謝@DJChanahCJD 提交的代碼
        支持批量刪除/覆制鏈接
        支持按時間倒序排序
        支持分頁功能
        ![](https://camo.githubusercontent.com/a0551aa92f39517f0b30d86883882c1af4c9b3486e540c7750af4dbe707371fa/68747470733a2f2f696d6774632d3369312e70616765732e6465762f66696c652f6262616438336561616630356635333731363237322e706e67)
    2、瀑布流視圖，感謝@panther125 提交的代碼
        ![](https://camo.githubusercontent.com/63d64491afc5654186248141bd343c363808bf8a77d3b879ffc1b8e57e5ac85d/68747470733a2f2f696d6167652e67696e636f64652e6963752f66696c652f3930346435373737613363306530613936623963642e706e67)

- 添加自動更新支持

    現在fork的項目能夠自動和上遊倉庫同步最新的更改，自動實裝最新的項目功能，感謝 @bian2022

    打開自動更新步驟：
        當你 fork 項目之後，由於 Github 的限制，需要手動去你 fork 後的項目的 Actions 頁面啟用 Workflows，並啟用 Upstream Sync Action，啟用之後即可開啟每小時定時自動更新：
        ![](https://im.gurl.eu.org/file/f27ff07538de656844923.png)
        ![](https://im.gurl.eu.org/file/063b360119211c9b984c0.png)
    `如果你遇到了 Upstream Sync 執行錯誤，請手動 Sync Fork 一次！`

    手動更新代碼

    如果你想讓手動立即更新，可以查看 [Github 的文檔](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork) 了解如何讓 fork 的項目與上遊代碼同步。

    你可以 star/watch 本項目或者 follow 作者來及時獲得新功能更新通知。
- 添加遠端遙測

    可通過添加`disable_telemetry`環境變量退出遙測

2023 年 1 月 18 日--圖片管理功能更新

1、支持圖片管理功能，默認是關閉的，如需開啟請部署完成後前往後台依次點擊`設置`->`函數`->`KV 命名空間綁定`->`編輯綁定`->`變量名稱`填寫：`img_url` `KV 命名空間` 選擇你提前創建好的 KV 儲存空間，開啟後訪問 http(s)://你的域名/admin 即可打開後台管理頁面
| 變量名稱 | KV 命名空間 |
| ----------- | ----------- |
| img_url | 選擇提前創建好的 KV 儲存空間 |

![](https://im.gurl.eu.org/file/a0c212d5dfb61f3652d07.png)
![](https://im.gurl.eu.org/file/48b9316ed018b2cb67cf4.png)

2、後台管理頁面新增登錄驗證功能，默認也是關閉的，如需開啟請部署完成後前往後台依次點擊`設置`->`環境變量`->`為生產環境定義變量`->`編輯變量` 添加如下表格所示的變量即可開啟登錄驗證
| 變量名稱 | 值 |
| ----------- | ----------- |
|BASIC_USER = | <後台管理頁面登錄用戶名稱>|
|BASIC_PASS = | <後台管理頁面登錄用戶密碼>|

![](https://im.gurl.eu.org/file/dff376498ac87cdb78071.png)

當然你也可以不設置這兩個值，這樣訪問後台管理頁面時將無需驗證，直接跳過登錄步驟，這一設計使得你可以結合 Cloudflare Access 進行使用，實現支持郵件驗證碼登錄，Microsoft 賬戶登錄，Github 賬戶登錄等功能，能夠與你域名上原有的登錄方式所集成，無需再次記憶多一組後台的賬號密碼，添加 Cloudflare Access 的方式請參考官方文檔，注意需要保護路徑包括/admin 以及 /api/manage/\*

3、新增圖片總數量統計
當開啟圖片管理功能後，可在後台頂部查看記錄中的圖片數量

![](https://im.gurl.eu.org/file/b7a37c08dc2c504199824.png)

4、新增圖片文件名搜索
當開啟圖片管理功能後，可在後台搜索框使用圖片文件名稱，快速搜索定位需要管理的圖片

![](https://im.gurl.eu.org/file/faf6d59a7d4a48a555491.png)

5、新增圖片狀態顯示
當開啟圖片管理功能後，可在後台查看圖片當前的狀態{ "ListType": "None", "TimeStamp": 1673984678274 }
ListType 代表圖片當前是否在黑白名單當中，None 則表示既不在黑名單中也不在白名單中，White 表示在在白名單中，Block 表示在黑名單中，TimeStamp 為圖片首次加載的時間戳，如開啟的圖片審查 API，則這里還會顯示圖片審查的結果用 Label 標識

![](https://im.gurl.eu.org/file/6aab78b83bbd8c249ee29.png)

6、新增黑名單功能
當開啟圖片管理功能後，可在後台手動為圖片加入黑名單，加入黑名單的圖片將無法正常加載

![](https://im.gurl.eu.org/file/fb18ef006a23677a52dfe.png)

7、新增白名單功能
當開啟圖片管理功能後，可在後台手動為圖片加入白名單，加入白名單的圖片無論如何都會正常加載，可繞過圖片審查 API 的結果

![](https://im.gurl.eu.org/file/2193409107d4f2bcd00ee.png)

8、新增記錄刪除功能
當開啟圖片管理功能後，可在後台手動刪除圖片記錄，即不再後台顯示該圖片，除非有人再次上傳並加載該圖片，注意由於圖片儲存在 telegraph 的服務器上，我們無法刪除上傳的原始圖片，只能通過上述第 6 點的黑名單功能屏蔽圖片的加載

9、新增程序運行模式：白名單模式
當開啟圖片管理功能後，除了默認模式外，這次更新還新增了一項新的運行模式，在該模式下，只有被添加進白名單的圖片才會被加載，上傳的圖片需要審核通過後才能展示，最大程度的防止不良圖片的加載，如需開啟請設置環境變量：WhiteList_Mode=="true"

10、新增後台圖片預覽功能
當開啟圖片管理功能後，可在後台預覽通過你的域名加載過的圖片，點擊圖片可以進行放大，縮小，旋轉等操作

![](https://im.gurl.eu.org/file/740cd5a69672de36133a2.png)

## 已經部署了的，如何更新？

其實更新非常簡單，只需要參照上面的更新內容，先進入到 Cloudflare Pages 後台，把需要使用的環境變量提前設置好並綁定上 KV 命名空間，然後去到 Github 你之前 fork 過的倉庫依次選擇`Sync fork`->`Update branch`即可，稍等一會，Cloudflare Pages 那邊檢測到你的倉庫更新了之後就會自動部署最新的代碼了

## 一些限制：

Cloudflare KV 每天只有 1000 次的免費寫入額度，每有一張新的圖片加載都會占用該寫入額度，如果超過該額度，圖片管理後台將無法記錄新加載的圖片

每天最多 100,000 次免費讀取操作，圖片每加載一次都會占用該額度（在沒有緩存的情況下，如果你的域名在 Cloudflare 開啟了緩存，當緩存未命中時才會占用該額度），超過黑白名單等功能可能會失效

每天最多 1,000 次免費刪除操作，每有一條圖片記錄都會占用該額度，超過將無法刪除圖片記錄

每天最多 1,000 次免費列出操作，每打開或刷新一次後台/admin 都會占用該額度，超過將進行後台圖片管理

絕大多數情況下，該免費額度都基本夠用，並且可以稍微超出一點，不是已超出就立馬停用，且每項額度單獨計算，某項操作超出免費額度後只會停用該項操作，不影響其他的功能，即即便我的免費寫入額度用完了，我的讀寫功能不受影響，圖片能夠正常加載，只是不能在圖片管理後台看到新的圖片了。

如果你的免費額度不夠用，可以自行向 Cloudflare 購買 Cloudflare Workers 的付費版本，每月$5 起步，按量收費，沒有上述額度限制

另外針對環境變量所做的更改將在下次部署時生效，如更改了`環境變量`，針對某項功能進行了開啟或關閉，請記得重新部署。

![](https://im.gurl.eu.org/file/b514467a4b3be0567a76f.png)

### Sponsorship

This project is tested with BrowserStack.

This project is support by [Cloudflare](https://www.cloudflare.com/).
