# 無窮鏈白皮書 Infinitechain White Paper
## 大綱 Outline
- 摘要 Abstract
- 簡介 Introduction
    - 區塊鏈的挑戰
    - 主側鏈架構
    - 出塊協定
    - 經濟模型
- 主鏈 Mainchain
    - 帳戶 Account
    - 交易 Transaction
    - 交易類型 Transaction Type
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
            - 提幣 Withdraw
            - 即時提幣 Instant Withdraw
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
    - 安全性分析
        可能遭竄改的資料與攻擊
        - Light Transaction Data
        - GSN
        - Balance
    - 競品分析
        - Raiden Network
        - Plasma
        - Cardano
- 應用 Application
    - 金融服務
    - Streaming Service
    - E-Commerce

- 可能攻擊 Possible Attack
- 鏈中鏈 Chain on Chain
- 如何解決規模問題？ How to Solve Scalability?

## 摘要 Abstract
區塊鏈技術的應用越來越普及，然而現行的公有鏈存在一些限制，包含速度、隱私等等的挑戰，使得很多應用皆難以落地發展．無窮鏈在技術應用上、共識機制上以及經濟模型上都提出了創新的方式來處理這些問題，在技術上採取主側鏈架構來增加每秒交易數量同時做到全網共識，而共識上，無窮鏈提出了輕量級挖礦的方式，讓全球用戶都可以公平的取得出塊權而不被礦池壟斷，最後在經濟模型上，我們提出了一個利他點數，讓區塊鏈的用戶們願意共同維護這個區塊鏈社會，並且在這個區塊鏈網路中可以藉由利他點數、來累積信用．
## 簡介 Introduction
### 區塊鏈的挑戰
目前區塊鏈的發展遇到相當多的挑戰，包含技術應用、共識機制以及經濟模型上的挑戰．
* 技術應用
    * 交易頻寬過低
      
      現行的公有鏈中(比特幣、以太坊)，TPS約為7-25，一般的應用難以使用，而現在有許多的解決方案像是私有鏈或是中心化的技術，這些做法都犧牲了區塊鏈原來最大的特色:去中心化
    * 手續費太高
      
      由於每一筆上鏈的交易都需要付費給礦工，意即生活上的微支付像是買飲料等等，都需要付出不少的手續費，這對於使用上來說是相當不方便的．
    * 資料膨脹
      
      就算使用私有鏈的做法，強行增加了TPS，整個網路上的每個節點都需要維護這麼大量的數據將會消耗太多儲存資源，會造成節點的高門檻而讓許多人難以成為區塊鏈的一部分
    * 難以跟中心化系統結合
      
      現在的許多的商業應用都是中央式服務，而難以跟區塊鏈做結合，在無窮鏈中，代理人就是在處理中央式服務的角色，非常容易與這些服務介接，同時無窮鏈的分散式稽核機制也讓用戶跟代理人達到資訊對等，讓中心化系統更公平．
* 共識機制
    * 算力集中
      
      現在的礦池過於集中，全球50%的算力集中在不超過5個礦池，造成出塊權利的壟斷，也同時讓這些礦池在區塊鏈的影響力過大．
    * 成本過高
      
      工作量證明的共識方式，需要大量的競爭算力，造成了這些資源的無謂浪費，包含電力以及建構礦機的成本．
* 經濟模型
    * 治理受少數族群壟斷

      許多專案都是由開發團隊制定方向，而無窮鏈提供參數共識的交易，讓用戶不僅可以自訂側鏈參數，在需要大家取得共識時，也可以發起投票．
      
    * ICO詐騙頻繁
      
      在無窮鏈發行ICO，用戶會參考發行者的利他點數來作為了解發行者信用的一種參考，同時在每一期發行者提預算的時候，用戶可以投票決定是否要繼續撥款給這個發行者，或是因為發行者做得不好，便投票把所有剩餘的錢退回給用戶．
### 主側鏈架構
在無窮鏈的主側鏈架構中，主鏈是用做信任機器來維持全球共識,而側鏈則是用做商業邏輯的使用．用戶送出的交易不需每筆都記錄在主鏈上，用戶會送一筆交易給一個代理人並授權讓代理人協助他做紀錄上鏈，代理人會將一段固定時間內的所有授權他的交易產生一個索引莫克樹並將其根雜湊值記錄在主鏈上．
同時，為了確保主側鏈資料一致，無窮鏈使用了分散式稽核機制，允許每個用戶在結算帳本時稽核自己的交易，達到資訊對等，防止代理人出錯；而索引莫克樹的資料結構，也可以讓每個用戶可以快速地取得跟自己相關的資料切片．

### 出塊協定
無窮鏈的目標不只在效能提升，而是希望可以達到社會公平，在現行的工作量證明中，出塊權(寫帳權)容易被大型的礦池所壟斷，一般的用戶不容易搶到出塊的權利．
無窮鏈提出一個輕量挖礦的機制-參與權益證明(Proof of Participation)，每一次的出塊權都是由一個用戶的地址與前一個出塊者的地址加上亂數所產生；另外，會利用此用戶的利他點數及持有的幣做加權計算產生最後的權益，而用戶會委託全節點幫忙出塊，並且共同分享出塊的利潤．
在無權鏈的出塊協定中，概率函式的設計使得每個人的出塊概率不會相差超過五倍，也不容易讓特定的族群壟斷出塊權．

### 經濟模型
無窮鏈希望建立一個去中心化的信任網路，每個用戶只要良善的送交易，正常的營運側鏈，貢獻每一次的稽核，認真的維護區塊鏈網路的共識，就可以增加自身的利他點數．
利他點數不能轉移，隨著時間跟用戶的貢獻持續增加，並且在權益證明上也會參考近期累積的利他點數來做加權，在無窮鏈的生態系中，經濟模型鼓勵用戶參與活動，有錢出錢，有力出力，共同維護鏈上經濟．
## 主鏈 Mainchain
### 帳戶 Account
一個用戶的帳戶應該包含以下的資訊
* 地址 Address
地址由用戶所持有的私鑰產生

* 帳戶類型 Account Type
用於說明此帳號是用戶或是哪一類型的合約

* 帳戶餘額 Balance
代表此帳戶所擁有的貨幣餘額

* 本地序列號 LSN
對於每次交易都要提交新的LSN來避免雙花

* 利他點數 Litar
信任點數

### 交易 Transaction
一筆交易應該包含以下資訊
* 送方地址 From
* 收方地址 To
* 貨幣數量 Value
* 資料欄位 DataField
    * 交易類型 Type
    * 側鏈側帳根雜湊值 RootHash

### 交易類型 Transaction Type
無窮鏈的定義中，有許多不同的交易類型來讓用戶使用，包含智能合約部署或智能合約的呼叫，未來也會隨著社群成長，變得越來越多樣
* 轉帳交易
  
  一般的轉帳交易，由傳送方轉帳至收錢方
* 部署智能合約交易
  
  節點會判斷交易為哪一種合約樣板再執行礦工程式碼，使用者可以定義輸入參數但不能改動核心合約，目前有以下幾種合約可以部署
    * 記帳型側鏈合約
    * 金流型側鏈合約
    * ICO合約
    * 共識合約
* 執行函式交易
  
  當合約部署完畢，即可呼叫合約函式來執行智能合約
  
  

### 區塊 Block
一個區塊應該包含以下資訊
* 區塊高度 Block Height
* 交易 Transactions (array)
* 區塊雜湊值 Hash
* 父區塊雜湊值 Parent Hash
* 礦工 Miner

### 節點 Node
節點會維護完整的區塊鏈資料，同時提供用戶登記他們的地址來共同爭取出塊權利，而節點可以從中獲取一些利潤來激勵他們協助用戶爭取出塊．
節點也提供不同的API讓一般用戶可以跟區塊鏈互動(包含部署合約、操作合約)
### 經濟激勵 Incentive
經濟激勵上，分為兩個層面:礦工費跟利他點數，活躍用戶在無窮鏈上執行轉帳、稽核等等的動作，會增加區塊鏈網路的安全性同時也增加自身的利他點數，而利他點數在計算出塊權益時，也會增加出塊機率進而易於取得出塊權．
而礦工費用(取得出塊權利的手續費)是用來激勵節點維護帳本以及鼓勵用戶在鏈上活動的方法，每個用戶都有機會取得出塊權利，所以大家就會希望整個無窮鏈網路越來越健康來維護自己的利益．
### 治理 Governance
在無窮鏈上，每個用戶都可以部署合約(包含側鏈合約、ICO合約等)，而用戶可以依據合約擁有者的利他點數來判斷是否值得相信，當側鏈出現問題時，也會立即的扣除擁有者的利他點數，讓合約擁有者願意認真的營運側鏈，累積一定的信用．
用戶也可以透過投票交易決定ICO的金額是否在下一期繼續撥款給擁有者去操作，讓整個網路的不同方可以互相制衡．
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
稽核員可以是無窮鏈網路上的任意一個節點、客戶甚至是其他中心化服務，意味著任何人都可以對任意一條無窮鏈側鏈進行稽核。
#### 去中心化儲存媒體
去中心化儲存媒體是利用無窮鏈節點構成的一種分散式檔案儲存系統，該系統擁有資料不可串改與單點失效不影響資料完整性的特點。
主要用於儲存中心化服務產生的索引莫克樹，並產生對應的訪問地址(Address)，以利稽核員對相應區段(Stage)的索引莫克樹進行稽核。
### 資料模型 Data Model
#### 區段 Stage
區段是指側鏈於特定期間內智能合約上的狀態，其內容包含索引莫克樹的根雜湊值roothash，根雜湊值代表一個密碼學證據，是依照儲存在索引莫克樹中一批側帳所產生出的雜湊值，可用來對索引莫克樹中任何一筆側帳做稽核。
#### 側帳 Light Transaction
側帳是側鏈不包含買賣雙方交易餘額之交易內容，其中：

    1. lightTxHash : lightTxData 運算產生之雜湊值
    2. type : 側鏈交易型態，可分為deposit, withdraw, remittance
    3. from : 交易送方之地址
    4. to : 交易收方之地址
    5. value : 交易金額
    6. fee : 手續費
    7. LSN : 全名為 Local Sequence Number，是客戶端每筆交易之順序編號
    8. stageHeight : 側鏈區段高度
    9. clientLtxHash : 客戶端對 lightTxHash 簽名之結果
    10. serverLtxHash : 中心化服務對 lightTxHash 簽名之結果

完整資料格式可分為以下三種:
- Deposit 
```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166cca',
    lightTxData: {
        type: 'deposit',
        from: 'null',    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
        value: 100,
        fee: 5,
        LSN: 2,
        stageHeight: 1
    },
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }

}
```
- Remittance
```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166cca',
    lightTxData: {
        type: 'remittance',
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb',    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
        value: 100,
        fee: 5,
        LSN: 2,
        stageHeight: 1
    },
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
- Withdraw
```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
    lightTxData: {
        type: 'withdraw',
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb',    
        to: 'null',
        value: 100,
        fee: 5,
        LSN: 2,
        stageHeight: 1
    },
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
#### 收據 Receipt
收據為側鏈包含買賣雙方交易餘額之交易內容，其中:

    1. receiptHash : receiptData 運算產生之雜湊值
    2. GSN : 全名為 Global Sequence Number，是側鏈中某階段所有客戶端交易之順序編號
    3. fromBalance : 交易送方之餘額
    4. toBalance : 交易收方之餘額
    5. serverReceiptHash : 中心化服務對 receiptHash 簽名之結果

完整資料格式可分為以下三種:
- Deposit
```
receipt = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
    receiptHash:'73f83ec398e8a4cd2354d1a622426003eeda9b0d0b4999368468dacd08848638',
    lightTxData: {
        type: 'deposit',
        from: 'null',    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
        value: 100,
        fee: 5,
        LSN: 2,
        stageHeight: 1
    },
    receiptData: {
        GSN: 20,
        lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
        fromBalance: 0,
        toBalance: 500
    },
    
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverReceiptHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
- Remittance
```
receipt = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
    receiptHash:'73f83ec398e8a4cd2354d1a622426003eeda9b0d0b4999368468dacd08848638',
    lightTxData: {
        type: 'remittance',
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb',    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
        value: 100,
        fee: 5,
        LSN: 2,
        stageHeight: 1
    },
    receiptData: {
        GSN: 21,
        lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
        fromBalance: 50,
        toBalance: 500
    },
    
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverReceiptHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```
- Withdraw
```
receipt = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
    receiptHash:'73f83ec398e8a4cd2354d1a622426003eeda9b0d0b4999368468dacd08848638',
    lightTxData: {
        type: 'withdraw',
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb',    
        to: 'null',
        value: 100,
        fee: 5,
        LSN: 2,
        stageHeight: 1
    },
    receiptData: {
        GSN: 22,
        lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
        fromBalance: 100,
        toBalance: 0
    },
    
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
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

為儲存資料雜湊值的資料結構，特性是可以將大量側帳 建構成一個32bytes的雜湊值，另外可以即時定位索引莫克樹中單一資料雜湊值的位置，以做到即時稽核特定資料的目的。
#### 收據樹 Receipt Tree
為索引莫克樹之資料結構，在側鏈協定裡，中心化服務會持續接收客戶端送來的側帳產生對應的收據，將每筆收據儲存起來，並在側鏈新增區段時產生收據樹，把根雜湊值放到智能合約上，以利後續稽核員與客戶端可以和去中心化儲存媒體取得相關之密碼學證據對收據進行驗證。
#### 餘額樹 Balance Tree
同樣為索引莫克樹之資料結構，在側鏈協定裡，是中心化服務用來紀錄側鏈中所有客戶端之餘額，當中心化服務收到側帳產生對應之收據同時，會根據每筆收據動態更新餘額樹中客戶端之餘額。

# 協定 Protocol
## 存幣
![](https://imgur.com/zx0Cp7a.jpg)

### 客戶端向主鏈提出存幣申請 (1~2)
客戶端呼叫無窮鏈合約`proposeDeposit`方法
合約新增一筆存幣紀錄 deposit record
[stageHeight, DSN, value, address, timeout, deposited]
例如 Bob(0x123) 存 2 eth
stageHeight: 3
DSN: 1
value: 2
address: 0x123
timeout: 600s
deposited: false

```
[3, 1, 2, 0x123, 600, false]
```

發出`ProposeDeposit`事件
服務端監聽
### 服務端產生存幣側帳並送至節點 (3~5)
服務端產生存幣側帳並對其簽章
將側帳送至節點
側帳

```
payment: {

}
```

### 節點驗證側帳並更新收據樹及餘額樹 (6~8)
節點驗證側帳

### 服務端向主鏈執行提幣 (9~13)
### 客戶端由主鏈取得收據 (14~15)

## 提幣 Withdraw
![](https://imgur.com/OsZvc6b.jpg)
### 客戶端向主鏈提出提幣申請 (1~2)
[stageHeight, WSN, value, address, timeout, withdrawed]

### 服務端產生存幣側帳並送至節點 (3~5)
### 節點驗證側帳並更新收據樹及餘額樹 (6~8)
### 服務端向主鏈提交收據 (9~13)
### 客戶端由主鏈取得收據 (14~15)
### 服務端新增側鏈區段並達成最終性 (16~21)
### 客戶端向主鏈執行提幣 (22~23)

## 轉帳 Remittance
![](https://imgur.com/7wN6EE8.jpg)
### 客戶端產生轉帳側帳並送至服務端 (1~2)
### 服務端驗證側帳並送至節點 (3~5)
### 節點驗證側帳並更新收據樹及餘額樹 (6~7)
### 服務端取得收據並送至客戶端 (8~9)
### 客戶端取得收據 (10~12)

## 稽核與抗議 Auditing and objection
![](https://imgur.com/LFl5FLh.jpg)
![](https://i.imgur.com/XTsdHAF.jpg)

### 服務端新增側鏈區段 (1~4)
### 客戶端及稽核者進行分散式稽核，必要時可提出抗議 (5~15)
### 服務端收到抗議後可進行自清 (16)
### 服務端於自清失敗後須支付罰款 (17)
### 服務端於所有抗議皆被處理後可達成側鏈之最終性 (18)

## 合約實作
### 抗議
### 自清
### 支付罰金
### 最終化

### 安全性分析

金流側鏈，實質上是一個針對中心化環境設計的協定，可由以下幾點得知

1. 任何用戶在側鏈產生的轉帳，都要經過中心化服務的驗證，才會進一步更改側鏈節點中的收據樹與餘額樹，在此過程，中心化服務都可能透過竄改側鏈節點程式驗證簽章演算法或直接竄改這些轉帳資料
2. 雖然任何側鏈用戶皆可產生交易，但此時此刻決定是否出帳的記帳權，始終在中心化服務身上
3. 正常的存幣、提幣等行為，中心化服務會需要回傳一筆收據給用戶，但中心化服務有控制權決定是否真的要傳送收據
4. 稽核過程所需的Indexed Merkle Tree, Balance Tree等數據結構，中心化服務還是可以控制是否傳送給稽核員核查

因此在整個側鏈的運作中，中心化服務有以上幾個時間點有機會操弄側鏈用戶的交易資料，進一步圖利自身或與中心化服務共謀的用戶。

其中，可能遭竄改的資料內容如以下三處：

1. Light Transaction Data
2. GSN
3. Balance

因此，此節將針對以上幾個時間點和中心化服務可能竄改的資料內容，所產生的攻擊和影響，來分析金流側鏈的安全性。

但在進入正題之前，需要先探討一個重要的側鏈議題，即側鏈相對於主鏈為較中心化的系統，因此會存在**Data Availability**的議題，在前述協定章節中提到，稽核員需要收據樹及餘額樹檢驗各項潛在的攻擊並採取相應的措施，保障側鏈安全，但如果中心化服務不提供收據樹及餘額樹時，將會導致稽核員無法確實稽核，使得側鏈處於不安全的環境下，因此在我們的協定中可以利用去中心化儲存媒體，如IPFS，與智能合約訂下規則，若中心化服務在打包側鏈階段回主鏈時，沒有提供收據樹及餘額樹之IPFS地址，或透過IPFS地址取回之收據樹與餘額樹和登記在智能合約上的完整性資訊不符，則側鏈之參與方可輕易透過智能合約狀態得知此側鏈目前違反**Data Availability**規則，側鏈參與方可直接執行提幣，離開不安全的側鏈，解決了較中心化的環境中**Data Availability**的議題。因此，接下來的安全性分析，將在中心化服務皆能提供收據樹及餘額樹給所有側鏈參與方驗證的前提下進行討論。

#### Light Transaction Data

一個完整的側帳包含許多資訊，其中`lightTxData`內含了一個交易送方自願承諾的交易資訊，其中最容易被竄改的資料為交易金額`value`，以下舉例保有竄改機會的中心化服務可能在未經交易送方同意的狀況下，做出以下的行為，試圖以此竄改的結果圖利自身或特定用戶。

假設Bob的地址`0x49aabbbe9141fe7a80804bdf01473e250a3414cb`試圖傳送給Alice，其地址為`0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a`，100個單位的代幣，則Bob可以透過Infinitechain SDK產生如下`lightTxData`資料，計算出`lightTxHash`，並對`lightTxHash`簽名，附加簽名回側帳資訊上。隨後傳送此側帳給中心化服務，等待出帳。

在側鏈節點沒有被中心化服務竄改驗證簽章邏輯的情況下，可能產生以下兩種攻擊：

##### 竄改lightTxData

```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166cca',
    lightTxData: {
        type: 'remittance',
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb',    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
        value: 100,
        fee: 5,
        LSN: 2,
        stageHeight: 1
    },
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```

但今天中心化服務若是懷抱著惡意，試圖竄改交易資料圖利特定用戶，可能會產生以下的錯誤交易：

```
lightTx = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166cca'
    lightTxData: {
        type: 'remittance',
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb', 
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
        value: 500,
        fee: 10,
        LSN: 2,
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

如以上的結果，`lightTxData`的內容中，`value`從100被改成500，`fee`從5被竄改成10之後，但中心化服務**不重新計算**`lightTxHash`，再將此錯誤的交易資訊傳送給側鏈節點，使側鏈節點更新錯誤的側帳，再此情況中，交易的送方`0x49aabbbe9141fe7a80804bdf01473e250a3414cb`餘額將被多扣除400，而交易的收方`0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a`餘額將多增加400，因此，**側鏈節點在未被竄改**的情況之下，側鏈節點將會重新透過`lightTxData`計算`lightTxHash`後，即可得知遭竄改的`lightTxData`與`lightTxHash`並不吻合，因此將會拒絕此側帳記載在側鏈節點中。

##### 竄改lightTxData與lightTxHash

同上，**側鏈節點在未被竄改**的情況之下，側鏈節點將會重新透過`lightTxData`計算`lightTxHash`後，即可得知遭竄改的`lightTxData`與`lightTxHash`雖吻合，但中心化服務因無法得知交易送方之私鑰，無法偽造Bob對錯誤`lightTxHash`之合法簽名，故交易送方產生的`clientLtxHash`簽名與遭竄改的`lightTxHash`不吻合，因此側鏈節點將會拒絕此側帳。

但側鏈節點本身可能依然是由中心化服務所保持，因此需要考慮中心化服務竄改側鏈節點驗證簽章的可能，使其跳過重新計算`lightTxData`與檢驗`lightTxHash`是否相同等程序，達成將竄改的側帳寫入側鏈節點中。

若發生此情形，可能造成以下幾種攻擊。

##### 竄改lightTxData

錯誤的側帳寫入側鏈節點後，中心化服務需要對收據的`receiptHash`簽名，用戶則會取得錯誤的收據，其結構可能如下：

```
receipt = {
    lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
    receiptHash:'73f83ec398e8a4cd2354d1a622426003eeda9b0d0b4999368468dacd08848638',
    lightTxData: {
        type: 'remittance',
        from: '0x49aabbbe9141fe7a80804bdf01473e250a3414cb',    
        to: '0x5b9688b5719f608f1cb20fdc59626e717fbeaa9a',
        value: 500,
        fee: 10,
        LSN: 2,
        stageHeight: 1
    },
    receiptData: {
        GSN: 21,
        lightTxHash:'6e7f1007bfb89f5af93fb9498fda2e9ca727166ccabd3a7109fa83e9d46d3f1a',
        fromBalance: 50,
        toBalance: 500
    },
    
    sig:{
        clientLtxHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverLtxHash:{
            v: 28,
            r:'0x384234cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        },
        serverReceiptHash:{
            v: 28,
            r:'0x384f9cb16fe9333e44b4ea8bba8cb4cb7cf910252e32014397c73aff5f94480c',
            s:'0x55305fc94b234c21d0025a8bce1fc20dbc7a83b48a66abc3cfbfdbc0a28c5709'
        }
    }
}
```

用戶收到此錯誤的收據後，會執行前述之稽核演算法流程，檢驗此收據資料的各項完整性與不可否認性。其中，在檢驗`lightTxData`之完整性時，就會發現與`lightTxHash`不同。此後，Bob就可以使用此錯誤之收據向主鏈上管理側鏈的智能合約執行抗議，經過挑戰期後即可取得押金。

##### 竄改lightTxData與lightTxHash

同理，用戶透過稽核演算法，將發現遭竄改的`lightTxData`與重新填入的`lightTxHash`雖吻合，但中心化服務因無法得知交易送方之私鑰，故無法偽造Bob對錯誤`lightTxHash`之合法簽名，故Bob在檢驗自己的簽名是否符合遭竄改的`lightTxHash`時，即會發現簽名不符，因此此收據不會通過稽核演算法之不可否認性檢查，此後，Bob就可以使用此錯誤之收據向主鏈上管理側鏈的智能合約執行抗議，經過挑戰期後即可取得押金。

#### GSN

GSN作為管理側帳在某個階段各客戶端交易之順序編號，對側帳合理性的影響為之重大，故也可能遭中心化服務竄改與圖利他人，本節討論各個針對GSN攻擊的可能，與其造成的影響，並說明Infinitechain金流側鏈如何應對此類攻擊。


##### 重複GSN

在側鏈正常運作環境中，一個合理的側帳順序，應與下圖邏輯相同，其中側鏈參與方A, B, C, D, E之間的側帳穿插在側鏈某個階段中，每一筆的側帳都會根據金額，更新相對應的交易雙方餘額，且根據正確GSN的排序每一個側帳的結果都是合理的。

![](https://i.imgur.com/suzeCqi.png)

每當中心化服務打包側帳階段回主鏈後，側鏈參與方可於去中心化儲存媒體上取得該側帳階段的收據樹及餘額樹做完整性稽核，但今天某側鏈參與方可能與中心化服務共謀，試圖竄改收據之順序編號導致某個階段有多筆收據含重複的`GSN`，可能會產生以下的有爭議的順序：

![](https://i.imgur.com/CeERx41.png)

如上圖，其中在同一個階段中，GSN為3的側帳被稽核者發現有兩筆，則此情形無法根據GSN為4的側帳推斷出哪個GSN為3的側帳才是合理的，因此此情形被發現後，將會被定位成側鏈的重大違規，稽核員可將此兩筆重複GSN的收據提交給智能合約執行抗議，待智能合約驗證相關中心化服務之簽章、階段與GSN後，待挑戰期結束後，合約將通知側鏈參與方此側鏈已有重大違規發生，其他側鏈參與方觀察到此事件後，即可向智能合約證明上個階段之餘額，接著立即提領上個階段參與方自己的餘額並離開此側鏈。

##### 缺漏GSN

除了重複GSN的可能外，也可能有缺漏GSN的問題，詳細可能的狀況如下圖：

![](https://i.imgur.com/67Ho2TR.png)

其中，在同個階段中，收據樹中缺漏了GSN為3的收據，可能造成GSN為4之後的餘額不合理，因此，稽核員在透過稽核演算法，依照GSN順序檢驗過程就會發現餘額與轉帳金額不相符，稽核員可以提交給智能合約GSN為2以及GSN為4的收據資料，智能合約根據金額驗算餘額不相符後，就算抗議成功，只要中心化服務無法提出GSN為3的正確收據，使所有收據合理化，等到階段挑戰期過後，中心化服務會因為此次錯帳，將來側鏈參與方B從側鏈轉移資產回主鏈時，由中心化服務放置在智能合約中的押金轉移給參與方B，以示負責，並且，當初提出抗議的稽核員，也能取得稽核獎金，因此，此種錯帳情形下，金流側鏈也能保持錯帳後金流側鏈正常運作，並且有足夠的經濟威脅，防止中心化服務蓄意產生此種不合理的側帳。

#### Balance

Balance記載著每位側鏈參與方在同個階段中帳戶的餘額，所有參與方的Balance會記載在餘額樹裡，而同個階段的側鏈參與人不論產生多少次`Remittance`皆不會造成側鏈金額總量的變動，如果中心化服務與參與方共謀，可能會產生以下兩種攻擊:


##### 共謀提幣

假設參與方與中心化服務企圖共謀提領側鏈中的金額，例如ERC20 token或ETH，可能會發生以下攻擊案例。

![](https://i.imgur.com/rlA1ksY.png)

中心化服務將GSN為3的收據裡參與方E原先的餘額120改為2020並放入收據樹中，接下來參與方E成功提幣，這將導致側鏈金額總量少了2000，在側鏈金額總量變少的情形下，會導致少數側鏈參與方沒辦法正確提幣，如果稽核員在這個階段確實稽核，就會發現這筆Balance被修改的收據金額不對，進而將GSN為2與3的收據提交給智能合約執行抗議，待智能合約驗證相關中心化服務之簽章、階段、GSN與Balance後，讓GSN為3的錯帳中，多給參與方E金額為1900的側帳能透過智能合約的押金3000彌補，使得側鏈能夠正常運行。

##### 超額共謀提幣

![](https://i.imgur.com/J7Ia2Qo.png)

與共謀提幣相同，中心化服務將GSN為3的收據裡參與方E原先的餘額120改為200020，此時參與方E的餘額遠大於中心化服務在智能合約中的押金，假設參與方E提幣成功，將會導致側鏈金額總量不足以讓其他側鏈參與方提幣，如果稽核員在這個階段確實稽核，就會發現這筆Balance被修改的收據裡金額不對，進而將GSN為2與3的收據提交給智能合約執行抗議，待智能合約驗證相關中心化服務之簽章、階段、GSN與Balance後，待挑戰期結束後，合約將通知側鏈參與方此側鏈已有重大違規發生，其他側鏈參與方觀察到此事件後，即可向智能合約證明上個階段之餘額，接著立即提領上個階段參與方自己的餘額並離開此側鏈。

$$
\sum_{a}^{b}
$$

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
### 金融服務
在傳統金融服務上，存款戶往往不知道銀行(或是任何金融平台)是否會沒經過授權就動用自己的資產，在無窮鏈的分散式稽核底下，用戶跟平台不會有資訊不對等的問題，而這些帳也都記錄在區塊鏈上，金融平台不可任意篡改紀錄．
所有的交易會在側鏈上產生，繼續維持高速交易的需求．每個金融機構可以根據自己的商業邏輯來建立側鏈，將交易留在自己的資料庫中，同時達到高速以及隱私．
### Streaming Service
在影音服務中，內容提供商往往難以了解終端用戶的收視情形，只能仰賴代理商平台提供的數字，所以最後內容提供商只願意以買斷的方式讓代理商播映．
無窮鏈可以讓每個收視戶稽核自己的付款紀錄，同時讓內容提供商了解結款的情形，並且不需要了解所有人的個資就可以確保交易的完整性，達到資訊對等．
### E-Commerce
電商平台已經是相當完整並成熟的產業之一，現在很多人已經相當習慣使用電商平台來做購買產品；使用無窮鏈的金流側鏈服務即可以快速地使用虛擬貨幣微支付來購買產品，同時也可以讓各個用戶確認自己的帳，這個支付系統即可達到快速、安全、不可篡改．
## 可能攻擊 Possible Attack
## 鏈中鏈 Chain on Chain
## 如何解決規模問題？ How to Solve Scalability?
