# 無窮鏈白皮書 Infinitechain White Paper
## 大綱 Outline
- 摘要 Abstract
    - 技術上
    - 共識上
    - 經濟上
- 簡介 Introduction
    - 主鏈做信任機器, 側鏈做商業邏輯 
    - 利他點數
    - 經濟模型
- 主鏈 Mainchain
    - 帳戶 Account
    - 交易 Transaction
    - 區塊 Block
    - 節點 Node
    - 共識 Consensus
    - 經濟激勵 Incentive
    - 治理 Governance
- 側鏈 Sidechain
    - 角色 Roles
        - 中心化服務 Central service
        - 客戶端 Client
        - 稽核員 Auditer
        - 去中心化儲存媒體
    - 資料模型 Data Model
        - 區段 Stage
        - 側帳 Light Transaction
        - 收據 Receipt
        - 索引莫克樹 Indexed Merkle Tree
        - 收據樹 Receipt Tree
        - 餘額樹 Balance Tree
    - 側鏈協定 Protocol
        - 金流側鏈協定（放時序圖）
            - 存款 Deposit
            - 提款 Withdraw
            - 即時提款 Instant Withdraw
            - 轉帳 Remittance
        - 分散式稽核與挑戰期（放時序圖）
            - Off-chain
                - 稽核 Audit
            - On-chain
                - 抗議 Take Objection
                - 自清 Exonerate
                - 罰款 Pay Penalty
                - 確認 Finalize
        - 合約實作
    - 側鏈的經濟威脅與經濟激勵
        - 經濟威脅：如安全性分析的幾種抗議
        - 經濟激勵：若如實維護側鏈，則可得到利他點數，有利於更高機會取得區塊獎勵
    - 安全性分析(側鏈節點的底限為接收Light Transaction時會如實驗證簽章，否則側鏈節點是毀損的，以下討論皆在此前提下進行)(詳見投影片之安全分析，先簡述側鏈之安全性分析，接下來講每一種攻擊的解法)
        - Light Transaction Data
            - Attack
            - Solution
        - GSN
            - Attack
            - Solution
        - Balance
            - Attack
            - Solution
    - 競品分析
        - Raiden Network
        - Plasma
        - Cardano
- 應用 Application
    - 應用場景
        - 金融服務
        - Streaming Service
        - E-Commerce

- 可能攻擊 Possible Attack
- 鏈中鏈 Chain on Chain
- 如何解決規模問題？ How to Solve Scalability?

## 摘要 Abstract
### 技術上
### 共識上的進步
### 經濟上
## 簡介 Introduction
### 主鏈信任機器, 側鏈做商業邏輯 
### 利他點數
### 經濟模型
## 主鏈 Mainchain
### 帳戶 Account
### 交易 Transaction
### 區塊 Block
### 節點 Node
### 共識 Consensus
### 經濟激勵 Incentive
### 治理 Governance
## 側鏈 Sidechain
### 角色 Roles
#### 中心化服務 Central service
中心化服務是指結合了無窮鏈之伺服器端軟體開發套件(Infinitechain server SDK)，並提供特定服務給客戶端的應用平台。
可能的中心化服務包括了線上直播視訊平台(Live video streaming platform)、電子商務網站(E-commerce website)等。
#### 客戶端 Client
客戶端是指結合了無窮鏈之客戶端軟體開發套件(Infinitechain client SDK)，能為客戶提供與中心化服務介接的應用程式。
可能的客戶端包括了網頁瀏覽器(Web browser)、手機應用程式(Moblie app)等。
#### 稽核員 Auditer
稽核員是無窮鏈網路中負責維護側鏈安全與穩定的執法者，確保任何側鏈不會因中心化服務之共謀或錯帳，導致客戶端實質上的損失。
稽核員可以是無窮鏈網路上的任意一個節點、用戶甚至是其他中心化服務，意味著任何人都可以對任意一條無窮鏈側鏈進行稽核。
#### 去中心化儲存媒體
去中心化儲存媒體是利用無窮鏈節點構成的一種分散式檔案儲存系統，該系統擁有資料不可串改與單點失效不影響資料完整性的特點。
主要用於儲存中心化服務產生的索引莫克樹，並產生對應的訪問地址(Address)，以利稽核員對相應區段(Stage)的索引莫克樹進行稽核。
### 資料模型 Data Model
#### 區段 Stage
區段是指側鏈在特定期間內智能合約上的狀態，其內容包含索引莫克樹的根雜湊值roothash，根雜湊值代表一個密碼學證據，是依照儲存在索引莫克樹中一批交易所產生出的雜湊值，可用來對索引莫克樹內的任何一筆交易做稽核。
#### 側帳 Light Transaction
側帳為不包含買賣雙方交易餘額之側鏈的交易內容，其資料格式可分為三種:
- Deposit light transaction
```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166cca'
    lightTxData: {
        type: deposit
        from: 'null'    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a'
        value: 100
        fee: 5
        LSN: 2
        stageHeight: 1
    }
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }

}
```
- Remittance transaction
```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166cca'
    lightTxData: {
        type: remittance
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb'    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a'
        value: 100
        fee: 5
        LSN: 2
        stageHeight: 1
    }
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
- Withdraw transaction
```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a'
    lightTxData: {
        type: withdraw
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb'    
        to: 'null'
        value: 100
        fee: 5
        LSN: 2
        stageHeight: 1
    }
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
#### 收據 Receipt
收據為側鏈在某個區段內儲存於索引莫克樹的交易內容，其資料格式可分為三種:
- deposit receipt
```
receipt = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a'
    receiptHash:'73f83ec398e8a4cd2354d1a622426003eeda9b0d0b4999368468dacd08848638'
    lightTxData: {
        type: deposit
        from: 'null'    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a'
        value: 100
        fee: 5
        LSN: 2
        stageHeight: 1
    }
    receiptData: {
        GSN: 20
        lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a'
        fromBalance: 0
        toBalance: 500
    }
    
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverReceiptHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
- remittance receipt
```
receipt = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a'
    receiptHash:'73f83ec398e8a4cd2354d1a622426003eeda9b0d0b4999368468dacd08848638'
    lightTxData: {
        type: remittance
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb'    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a'
        value: 100
        fee: 5
        LSN: 2
        stageHeight: 1
    }
    receiptData: {
        GSN: 21
        lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a'
        fromBalance: 50
        toBalance: 500
    }
    
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverReceiptHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
- withdraw receipt
```
receipt = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a'
    receiptHash:'73f83ec398e8a4cd2354d1a622426003eeda9b0d0b4999368468dacd08848638'
    lightTxData: {
        type: withdraw
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb'    
        to: 'null'
        value: 100
        fee: 5
        LSN: 2
        stageHeight: 1
    }
    receiptData: {
        GSN: 22
        lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a'
        fromBalance: 100
        toBalance: 0
    }
    
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
        serverReceiptHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
#### 索引莫克樹 Indexed Merkle Tree
![](https://i.imgur.com/54x0izT.png)
索引莫克樹是用來儲存資料雜湊值之資料結構，特性是可以快速索引某筆特定資料在索引莫克樹的位置以便對該筆資料做稽核。
#### 收據樹 Receipt Tree
收據樹為索引莫克樹之資料結構，節點會將每筆建立好的收據儲存起來，並在側鏈觸發CommitStage時產生收據樹，將其roothash放到主鏈上，以利後續稽核員與客戶端可以和節點取得相關之密碼學證據對收據進行驗證。
#### 餘額樹 Balance Tree
餘額樹同樣為索引莫克樹之資料結構，為節點紀錄在側鏈中所有用戶之餘額，節點會在收到側帳時產生對應之收據，並同時動態的更新餘額樹中收據內相關用戶之餘額。
### 側鏈協定 Protocol
#### 金流側鏈協定（放時序圖）
##### 存款 Deposit
##### 提款 Withdraw
##### 即時提款 Instant Withdraw
##### 轉帳 Remittance
#### 分散式稽核與挑戰期（放時序圖）
##### Off-chain
###### 稽核 Audit
##### On-chain
###### 抗議 Take Objection
###### 自清 Exonerate
###### 罰款 Pay Penalty
###### 確認 Finalize
#### 合約實作
### 側鏈的經濟威脅與經濟激勵
#### 經濟威脅：如安全性分析的幾種抗議
#### 經濟激勵：若如實維護側鏈，則可得到利他點數，有利於更高機會取得區塊獎勵
### 安全性分析(側鏈節點的底限為接收Light Transaction時會如實驗證簽章，否則側鏈節點是毀損的，以下討論皆在此前提下進行)(詳見投影片之安全分析，先簡述側鏈之安全性分析，接下來講每一種攻擊的解法)
#### Light Transaction Data
##### Attack
##### Solution
#### GSN
##### Attack
##### Solution
#### Balance
##### Attack
##### Solution
-- 以下不適用現有目錄架構 --
#### Deposit階段攻擊分析

![](https://i.imgur.com/HWEQxRi.png)

如上圖，若Bob要提交一個存幣申請，試圖進到某一條側鏈進行多次的微支付，他將執行以下的動作，Bob在鏈下產生一個隨機數，稱**Local Sequence Number**，此後稱**LSN**，執行存幣，並向智能合約存入100個代幣，等到礦工打包此交易後，側鏈智能合約就會產生一個持續遞增不重複的存幣編號，稱**Deposit Sequence Number**，此後用代號**DSN**表示，接著，智能合約會在合約狀態中登記一筆存幣紀錄，具體內容為(type=deposit, address=Bob地址, value=存幣數量, stageHeight=預計此筆側帳將被放入側鏈哪個階段, LSN=防止重放攻擊之亂數, DSN=存幣編號, depositTimeout=存幣逾時時間, deposited=是否成功存入側鏈)，其中，存幣逾時時間將會大於單一挑戰期的時間週期，而deposited表示中央式服務是否有誠實地將存幣正確的移轉至側鏈中，最後待存幣申請成功後，智能合約會廣播**存幣申請事件**。

##### 存幣無回應攻擊

![](https://i.imgur.com/Vyvqo17.png)

中央式服務在監聽到**存幣申請事件**後，若沒有後續幫助Bob在側鏈新增一筆存幣側帳，並增加Bob在側鏈對應地址的餘額，則等到智能合約中depositTimeout時限一到，相同存幣紀錄中的deposited仍為false時，表示中央式服務沒有盡到該盡的責任，則此時Bob可以透過執行智能合約之**depositTimeout**方法，告知合約DSN，待智能合約驗證地址與該DSN所指的存幣紀錄中，deposited確實為false，則取出該筆存幣紀錄的value，並轉移value個資產回Bob在主鏈的地址，因此，整個過程Bob並不會因為中央式服務關閉或惡意不處理等問題，導致存幣無法取回。

##### 錯誤存幣金額攻擊

若Bob透過智能合約提出存幣申請，數量為100個，但中央式服務在監聽到存幣申請事件後，在側鏈替Bob寫入的存幣金額為錯誤的90個，試圖造成Bob損失，則當中央式服務試圖提交存幣收據給智能合約驗證時，會向合約提出以下資訊，(paymentSig=中央式服務針對paymentHash的簽章, type=deposit, from=此處送方地址為null, to=Bob地址, value=側鏈存幣金額, stageHeight=此側帳將被放置於哪個側鏈階段, LSN=Bob產生之亂數, DSN=存幣編號, receiptSig=中央式服務針對receiptHash的簽章, GSN=單一stage中側帳編號, fromBalance=此處送方帳戶餘額為null, toBalance=Bob側鏈存幣後餘額)，此時，智能合約只需要根據存幣收據中的DSN，查詢出Bob在智能合約上的存幣申請，並比較兩者value數值是否相同，接著根據以下公式從收據資訊中計算paymentHash=keccak256(type||from||to||value||stageHeight||LSN||DSN)，以及receiptHash=keccak256(GSN||fromBalance||toBalance)，以及檢查paymentHash和paymentSig是否通過簽名驗證與receiptHash和receiptSig是否通過簽名驗證，且兩者皆為服務端地址發送，成功後將更改deposited為true，接著才能廣播出**提交收據事件**，故在此處兩者value並不相同，無法通過智能合約驗證，deposited依然為false，除非中央式服務再提出正確金額的收據，否則待depositTimeout時限一到，Bob就能將此次存幣申請，視同無回應，可直接從合約移轉存幣回自己的地址。

#### Withdraw階段攻擊分析

![](https://i.imgur.com/CAg4FJp.png)

如上圖，若Bob試圖要從側鏈轉移資產回自己的主鏈幣地址，他將執行以下的動作，Bob在鏈下隨機產生LSN，並向智能合約提出提幣申請，等到礦工打包此交易後，側鏈智能合約就會產生一個持續遞增不重複的提幣編號，稱**Withdraw Sequence Number**，此後用代號**WSN**表示，接著，智能合約會在合約狀態中登記一筆提幣紀錄，具體內容為(type=withdraw, address=Bob地址, value=提幣數量, stageHeight=此側帳將被放置於哪個側鏈階段, LSN=防止重放攻擊之亂數, WSN=提幣編號, withdrawTimeout=提幣逾時時間, withdrawed=是否成功提幣)，其中，提幣逾時時間將會大於單一挑戰期的時間週期，而withdrawed表示中央式服務是否有誠實移轉側鏈資產回主鏈Bob地址中，接著智能合約將廣播**提幣申請事件**，並在側鏈更新側帳，回傳收據等工作，待中央式服務打包側帳為一個新的階段後，開始挑戰期的流程，最後待挑戰期過後，若沒有人針對此筆提幣有異議，中央式服務即可執行**finalize**方法，使包含此提幣申請的階段帳本達成最終性，同時更改withdrawed為true，接著智能合約會廣播**提幣成功事件**，此後Bob可立即執行 **withdraw(WSN)** 方法從智能合約轉出資產回自己主鏈地址。

##### 提幣無回應攻擊

![](https://i.imgur.com/iPzdojU.png)

中央式服務在監聽到**提幣申請事件**後，若沒有後續幫助Bob在側鏈新增一筆提幣側帳，並增加Bob在側鏈對應地址的餘額，則等到智能合約中withdrawTimeout時限一到，相同提幣紀錄中的withdrawed仍為false時，表示中央式服務沒有盡到該盡的責任，則此時Bob可以透過執行智能合約之**withdrawTimeout**方法，告知合約WSN，待智能合約驗證地址該WSN所指的提幣紀錄中，withdrawed確實為false，則取出該筆提幣紀錄的value，並轉移value個資產回Bob在主鏈的地址，因此，若提幣金額合理，整個過程Bob並不會因為中央式服務關閉或惡意不處理等問題，導致提幣無法執行。

##### 錯誤提幣金額攻擊

若Bob透過智能合約提出提幣申請，數量為100個，但中央式服務在監聽到提幣申請事件後，在側鏈替Bob寫入的提幣金額為錯誤的90個，試圖造成Bob損失，則當中央式服務試圖提交提幣收據給智能合約驗證時，會向合約提出以下資訊，(paymentSig=中央式服務針對paymentHash的簽章, type=withdraw, from=Bob地址, to=此處收方為null, value=側鏈提幣金額, stageHeight=此側帳將被放置於哪個側鏈階段, LSN=Bob產生之亂數, WSN=提幣編號, receiptSig=中央式服務針對receiptHash的簽章, GSN=單一stage中側帳編號, fromBalance=Bob側鏈提幣後餘額, toBalance=此處收方帳戶餘額為null)，此時，智能合約只需要根據提幣收據中的WSN，查詢出Bob在智能合約上的提幣申請，並比較兩者value數值是否相同，接著根據以下公式從收據資訊中計算paymentHash=keccak256(type||from||to||value||stageHeight||LSN||WSN)，以及receiptHash=keccak256(GSN||fromBalance||toBalance)，以及檢查paymentHash和paymentSig是否通過簽名驗證與receiptHash和receiptSig是否通過簽名驗證，且兩者皆為服務端地址發送，接著才能廣播出**提交收據事件**，故在此處兩者value並不相同，無法通過智能合約驗證，提幣紀錄中的withdrawed依然為false，除非中央式服務再提出正確金額的收據，否則待withdrawTimeout時限一到，Bob就能直接將此次提幣申請，視同無回應，可直接從合約移轉value個資產回自己的主鏈地址。

#### Remittance階段攻擊分析
-- 以上不適用現有目錄架構 --
### 競品分析
#### Payment Channel / Raiden Network
- hub要一直在線，需要高度專業化
- hub若存款不夠，導致無法根據路由完成微支付的問題
- 開關通道產生的主鏈交易，造成潛在網路壅塞問題
- 沒有整合中央式系統，無法方便建立web/app等落地應用
#### Plasma
- 當Plasma child chain遭受惡意攻擊時，用戶只能選擇離開側鏈
- 沒有原生與中央式系統整合來設計，企業依然需要聘雇了解區塊鏈運作的工程師
- 只適用於Ethereum
#### Cardano
- 側鏈採用PoW，共識耗能，速度慢
- 中心化的側鏈卻使用PoW此類互不信任節點之共識演算法，造成矛盾
## 應用 Application
### 應用場景
#### 金融服務
#### Streaming Service
#### E-Commerce
## 可能攻擊 Possible Attack
## 鏈中鏈 Chain on Chain
## 如何解決規模問題？ How to Solve Scalability?
