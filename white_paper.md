# 無窮鏈白皮書 Infinitechain White Paper
## 大綱 Outline
- 摘要 Abstract
    - 技術上
    - 共識上的進步
    - 經濟上
- 簡介 Introduction
    - 主鏈信任機器, 側鏈做商業邏輯 (分散式稽核簡述)
    - 利他點數(Kudos 或許會修改)良性循環
    - 經濟模型
- 主鏈 Mainchain
    - 帳戶 Account
    - 交易 Transaction
    - 節點 Node
        - Master Node
        - Previledge
    - 共識 Consensus
    - 區塊 Block
    - 經濟激勵 Incentive
    - 治理 Governance
- 側鏈 Sidechain
    - 角色 Roles
        - 中央式服務
        - 客戶端
        - 稽核員
        - 節點
    - 資料模型 Data Model
        - 區段 Stage
        - 側帳 Payment
        - 收據 Receipt
        - 收據樹 Receipt Tree
            - 索引莫克樹 Indexed Merkle Tree
        - 餘額樹 Balance Tree
    - 側鏈協定 Protocol
        - 金流側鏈協定（放時序圖）
            - 存款 Deposit
            - 提款 Withdraw
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
    - 安全性分析(側鏈節點的底限為接收Payment時會如實驗證簽章，否則側鏈節點是毀損的，以下討論皆在此前提下進行)
        - Deposit階段攻擊分析
            - No response(使用 deposit timeout解決)
            - Wrong value or balance(GSN paymentHash balanceRootHash objection)
        - Withdraw階段攻擊分析
            - No response(使用 withdraw timeout解決)
            - Wrong value or balance(GSN paymentHash balanceRootHash objection)
        - Remittance階段攻擊分析
            - No response(等同於raw payment不合法拿不到簽名，無解)
            - Wrong value or balance(GSN paymentHash balanceRootHash objection)
    - 競品分析
        - Payment Channel / Raiden Network
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

## 摘要
### 技術上
### 共識上的進步
### 經濟上
## 簡介 Introduction
### 主鏈信任機器, 側鏈做商業邏輯 (分散式稽核簡述)
### 利他點數(Kudos 或許會修改)良性循環
### 經濟模型
## 主鏈
### 帳戶 Account
### 交易 Transaction
### 節點 Node
#### Master Node
#### Previledge
### 共識 Consensus
### 區塊 Block
### 經濟激勵 Incentive
### 治理 Governance
## 側鏈
### 角色 Roles
#### 中央式服務 Central service
中央式服務是指結合了無窮鏈之伺服器端軟體開發套件(Infinitechain server SDK)，並提供特定服務給客戶端的應用平台。
較常見的中央式服務包括了線上直播視訊平台(Live video streaming platform)、電子商務網站(E-commerce website)等。
#### 客戶端 Client
客戶端是指結合了無窮鏈之客戶端軟體開發套件(Infinitechain client SDK)，能為客戶提供與中央式服務界接的應用程式。
較常見的客戶端包括了網頁瀏覽器(Web browser)、手機應用程式(Moblie app)等。
#### 稽核員 Auditer
稽核員是指在無窮鏈網路中負責維護側鏈安全的執法者，稽核員可以是無窮鏈網路上的任意一個節點，任意稽核員可以對任意一條無窮鏈側鏈進行稽核，確保該側鏈不會因中央式服務之共謀或錯帳，導致客戶端實質上的損失。
#### 節點 Node (節點名詞定義應該是在主鏈-節點說明)
節點負責的工作如下：
- 接收來自中央式服務更新帳本的請求，並回傳收據給中央式服務
- 提供稽核員查詢帳本進行稽核
### 資料模型 Data Model
#### 索引莫克樹 Indexed Merkle Tree
![](https://i.imgur.com/54x0izT.png)
索引莫克樹是用來儲存資料雜湊值之資料結構，特性是可以快速索引某筆特定資料在索引莫克樹的位置以便對該筆資料做存在性證明。
#### 區段 Stage
區段是指側鏈特定期間內智能合約上的狀態，其內容包含索引莫克樹的根雜湊值roothash，根雜湊值代表一個密碼學證據，是依照儲存在索引莫克樹中的一批交易而產生出根節點的雜湊值，可用來對索引莫克樹內的任何一筆交易做完整性驗證。
#### 側帳 Payment
側帳為不包含買賣雙方交易餘額之側鏈的交易內容，其資料格式可分為三種:
- Deposit payment
```
payment = {
    paymentData: {
        type: deposit
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb'    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a'
        value: 100
        fee: 5
        LSN: 2
        DSN: 8
    }
    paymentHash:
    signature
}
```
- Remittance payment
```
payment = {
    paymentHash
    paymentData: {
        type
        from
        to
        value
        fee
        LSN
        DSN/WSN
    }
    signature
}
```
- Withdraw payment
```
payment = {
    paymentHash
    paymentData: {
        type
        from
        to
        value
        fee
        LSN
        DSN/WSN
    }
    signature
}
```
#### 收據 Receipt
收據為側鏈在某個區段內儲存於索引莫克樹的交易內容，其資料格式為:
```
receipt = {
    paymentHash
    paymentData
    receiptData: {
        GSN
        paymentHash
        fromBalance
        toBalance
    }
    signature
}
```
#### 收據樹 Receipt Tree
#### 餘額樹 Balance Tree
### 側鏈協定 Protocol
#### 金流側鏈協定（放時序圖）
#### 存款 Deposit
#### 提款 Withdraw
#### 轉帳 Remittance
#### 分散式稽核與挑戰期（放時序圖）
#### Off-chain
##### 稽核 Audit
#### On-chain
##### 抗議 Take Objection
##### 自清 Exonerate
##### 罰款 Pay Penalty
##### 確認 Finalize
#### 合約實作
### 側鏈的經濟威脅與經濟激勵
#### 經濟威脅：如安全性分析的幾種抗議
#### 經濟激勵：若如實維護側鏈，則可得到利他點數，有利於更高機會取得區塊獎勵
### 安全性分析(側鏈節點的底限為接收Payment時會如實驗證簽章，否則側鏈節點是毀損的，以下討論皆在此前提下進行)
#### Deposit階段攻擊分析

![](https://i.imgur.com/HWEQxRi.png)

如上圖，若Bob要提交一個存幣申請，試圖進到某一條側鏈進行多次的微支付，他將執行以下的動作，Bob在鏈下產生一個隨機數，稱**Local Sequence Number**，此後稱**LSN**，執行存幣，並向智能合約存入100個代幣，等到礦工打包此交易後，側鏈智能合約就會產生一個持續遞增不重複的存幣編號，稱**Deposit Sequence Number**，此後用代號**DSN**表示，接著，智能合約會在合約狀態中登記一筆存幣紀錄，具體內容為(type=deposit, address=Bob地址, value=存幣數量, LSN=防止重放攻擊之亂數, DSN=存幣編號, depositTimeout=存幣逾時時間, deposited=是否成功存入側鏈)，其中，存幣逾時時間將會大於單一挑戰期的時間週期，而deposited表示中央式服務是否有誠實地將存幣正確的移轉至側鏈中，最後待存幣申請成功後，智能合約會廣播**存幣申請事件**。

##### 存幣無回應攻擊

![](https://i.imgur.com/Vyvqo17.png)

中央式服務在監聽到**存幣申請事件**後，若沒有後續幫助Bob在側鏈新增一筆存幣側帳，並增加Bob在側鏈對應地址的餘額，則等到智能合約中depositTimeout時限一到，相同存幣紀錄中的deposited仍為false時，表示中央式服務沒有盡到該盡的責任，則此時Bob可以透過執行智能合約之**depositTimeout**方法，告知合約DSN，待智能合約驗證地址與該DSN所指的存幣紀錄中，deposited確實為false，則取出該筆存幣紀錄的value，並轉移value個資產回Bob在主鏈的地址，因此，整個過程Bob並不會因為中央式服務關閉或惡意不處理等問題，導致存幣無法取回。

##### 錯誤存幣金額攻擊

若Bob透過智能合約提出存幣申請，數量為100個，但中央式服務在監聽到存幣申請事件後，在側鏈替Bob寫入的存幣金額為錯誤的90個，試圖造成Bob損失，則當中央式服務試圖提交存幣收據給智能合約驗證時，會向合約提出以下資訊，(paymentHash=存幣哈希值, paymentSig=中央式服務針對paymentHash的簽章, type=deposit, from=此處送方地址為null, to=Bob地址, value=側鏈存幣金額, LSN=Bob產生之亂數, DSN=存幣編號, receiptHash=收據哈希值, receiptSig=中央式服務針對receiptHash的簽章, GSN=單一stage中側帳編號, fromBalance=此處送方帳戶餘額為null, toBalance=Bob側鏈存幣後餘額)，此時，智能合約只需要根據存幣收據中的DSN，查詢出Bob在智能合約上的存幣申請，並比較兩者value數值是否相同，以及驗證paymentSig與receiptSig是否為中央式的簽章，成功後將更改deposited為true，接著才能廣播出**提交收據事件**，故在此處兩者value並不相同，無法通過智能合約驗證，deposited依然為false，除非中央式服務再提出正確金額的收據，否則待depositTimeout時限一到，Bob就能將此次存幣申請，視同無回應，可直接從合約移轉存幣回自己的地址。

#### Withdraw階段攻擊分析

![](https://i.imgur.com/CAg4FJp.png)

如上圖，若Bob試圖要從側鏈轉移資產回自己的主鏈幣地址，他將執行以下的動作，Bob在鏈下隨機產生LSN，並向智能合約提出提幣申請，等到礦工打包此交易後，側鏈智能合約就會產生一個持續遞增不重複的提幣編號，稱**Withdraw Sequence Number**，此後用代號**WSN**表示，接著，智能合約會在合約狀態中登記一筆提幣紀錄，具體內容為(type=withdraw, address=Bob地址, value=提幣數量, LSN=防止重放攻擊之亂數, WSN=提幣編號, withdrawTimeout=提幣逾時時間, withdrawed=是否成功提幣)，其中，提幣逾時時間將會大於單一挑戰期的時間週期，而withdrawed表示中央式服務是否有誠實移轉側鏈資產回主鏈Bob地址中，接著智能合約將廣播**提幣申請事件**，並在側鏈更新側帳，回傳收據等工作，待中央式服務打包側帳為一個新的階段後，開始挑戰期的流程，最後待挑戰期過後，若沒有人針對此筆提幣有異議，中央式服務即可執行**finalize**方法，使包含此提幣申請的階段帳本達成最終性，同時更改withdrawed為true，接著智能合約會廣播**提幣成功事件**，此後Bob可立即執行 **withdraw(WSN)** 方法從智能合約轉出資產回自己主鏈地址。

##### 提幣無回應攻擊

![](https://i.imgur.com/iPzdojU.png)

中央式服務在監聽到**提幣申請事件**後，若沒有後續幫助Bob在側鏈新增一筆提幣側帳，並增加Bob在側鏈對應地址的餘額，則等到智能合約中withdrawTimeout時限一到，相同提幣紀錄中的withdrawed仍為false時，表示中央式服務沒有盡到該盡的責任，則此時Bob可以透過執行智能合約之**withdrawTimeout**方法，告知合約WSN，待智能合約驗證地址該WSN所指的提幣紀錄中，withdrawed確實為false，則取出該筆提幣紀錄的value，並轉移value個資產回Bob在主鏈的地址，因此，若提幣金額合理，整個過程Bob並不會因為中央式服務關閉或惡意不處理等問題，導致提幣無法執行。

##### 錯誤提幣金額攻擊

若Bob透過智能合約提出提幣申請，數量為100個，但中央式服務在監聽到提幣申請事件後，在側鏈替Bob寫入的提幣金額為錯誤的90個，試圖造成Bob損失，則當中央式服務試圖提交提幣收據給智能合約驗證時，會向合約提出以下資訊，(paymentHash=提幣哈希值, paymentSig=中央式服務針對paymentHash的簽章, type=withdraw, from=Bob地址, to=此處收方為null, value=側鏈提幣金額, LSN=Bob產生之亂數, WSN=提幣編號, receiptHash=收據哈希值, receiptSig=中央式服務針對receiptHash的簽章, GSN=單一stage中側帳編號, fromBalance=Bob側鏈提幣後餘額, toBalance=此處收方帳戶餘額為null)，此時，智能合約只需要根據提幣收據中的WSN，查詢出Bob在智能合約上的提幣申請，並比較兩者value數值是否相同，以及驗證paymentSig與receiptSig是否為中央式的簽章，接著才能廣播出**提交收據事件**，故在此處兩者value並不相同，無法通過智能合約驗證，提幣紀錄中的withdrawed依然為false，除非中央式服務再提出正確金額的收據，否則待withdrawTimeout時限一到，Bob就能直接將此次提幣申請，視同無回應，可直接從合約移轉value個資產回自己的主鏈地址。

#### Remittance階段攻擊分析
### 競品分析
#### Payment Channel / Raiden Network
- 更少的開關channel產生的on-chain交易，造成潛在網路壅塞問題
- 沒有找不到hub的問題
- 沒有hub存款不夠，導致無法根據路由完成微支付的問題
- 由整合中央式系統的思考出發，才能方便建立web/app等落地應用
#### Plasma
- 當Plasma child chain惡意攻擊時，用戶只能選擇mass-exit，金流側鏈保有修正並持續工作的能力
- 沒有原生與中央式系統整合來設計，企業依然需要聘雇了解區塊鏈運作的工程師
#### Cardano
- 側鏈採用PoW，但側鏈若是只由中央式服務出帳，則沒有需要用到耗能且只適用於互不信任網路的公式演算法
### 應用場景
#### 金融服務
#### Streaming Service
#### E-Commerce
