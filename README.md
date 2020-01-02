# Home Credit Default Risk 數據資料集分析
![](https://i.imgur.com/K4XCxhv.jpg)
[toc]
## 專題報告要點
1. 研究動機
2. 研究目的
3. 研究方法&流程
4. 預計分工狀況
5. 預計達成結果
6. 預計面臨的困難&可能的解決方法
7. 使用工具
8. 參考資料

### 研究動機
由於投資、資產配置、自身居住，房屋購買的需求越來越高，但對買賣雙方而言，價格是一般人難以直接負擔、出售的，多數人會透過貸款的方式交易。
對於信貸公司而言，房屋貸款業務具有高額貸款的金額、長時間償還期限的特質，這些都是信貸公司需要承擔的風險。


### 研究目的
如何讓信貸公司有效運用客戶過往的信用數據，預測客戶未來的還款能力，接受有還款能力的客戶、拒絕無還款能力的客戶，讓信貸公司避免不必要的呆帳，達到信貸公司利益最大化。


### 研究方法&流程
資料來源: Kaggle Outbrain Click Prediction 

環境建置: hadoop叢集系統、Linux作業系統

資料清洗: R 或是 Python刪減欄位、調整缺失值，或使用爬蟲新增資料

模型建置: 機器學習各種方法(XGboost,隨機森林)

視覺化: D3.js ,tableau(暫定)

### 預計分工狀況
- 環境建置: 文軍皓(Lead) + rest of the team

- 資料準備: 嚴浩祖(Lead) + rest of the team

- 模型建置: 林冠穎(Lead) + rest of the team
 
- 視覺化: TBD 

### 預計達成結果
利用各個顧客資料預測未來顧客違約的風險

### 預計面臨的困難&可能的解決方法
1. 由於是開源資料，對於資料的熟悉度不太夠(多花點時間了解資料)

2. 資料不太足夠，分析有限(尋找能增加欄位的方式、爬蟲等等)

3. 欄位說明不足。(參考外部相關產業的資料)
   
4. 專業知識不足。(請教相關專業人員或有經驗人士)

5. pyspark等使用不熟悉(自己精進)

6. 視覺化呈現方式未定(討論過後應該能有答案)

### 使用工具
語言:R ,python ,MySQL

環境:Linux ,hadoop ,Hive ,Zeppelin 

視覺化:D3.js ,tableau

## 檔案說明
### application_test.csv 測試集 / application_train.csv 訓練集
主要的培訓和測試數據以及關於Home Credit每個貸款申請的信息。每筆貸款都有自己的行，並由功能SK_ID_CURR標識。培訓申請數據附帶TARGET表示0：貸款已還清或1：貸款未還清。
:::info
- Days_employed, 有極端值
- trian 表的是否提供各種電話的欄位可以考慮加總
- region rating 可以收入判斷高低(居住分級和其他資料比對)
- Days_Registration 跟貸款日是否有關
- 是否有房產是否可解釋房子特徵為空值
- 房產特徵可看位於人多區塊還是人少區塊
- Years_B* (房產相關,正規化)
- 大量正規化的資料可以分析是否有錢
- 相關性高的:EXT 3人組、DAYS_BIRTH
:::
### bureau.csv 額外檔案
有關客戶之前來自其他金融機構的信貸的數據。以前的每一筆信貸都有自己的分行，但申請數據中的一筆貸款可能有多筆先前信貸。
:::info
- Credit_Active 是 close 但 Credit_day_overdue不為0
- Credit_Active 是 Active 但 Days_EndDate_FACT 還有值
:::
:::danger
用ID 去Bureau_balance 看情況
第二點問題出在Bureau_balance 對上的ID都是空值，建議設立新欄位紀錄
:::
### bureau_balance.csv 額外檔案
關於主管局以前信用的月度數據。每一行都是上一個信用的一個月，並且一個先前的信用可以有多個行，每個信用額度的每個月有一個行。
:::info
- X ：不知道狀況
- 1 ~ 5：嚴重度 (5已經到被債務賣掉或是房產抵押)
- c：還完
- 0：期限前，準時還款
- 可依ID，將狀態次數加入bureau
:::
:::danger
C 和 0 差別標準未知，有些資料C後面還有其它的資料 
:::
### credit_card_balance.csv 額外檔案
有關之前的信用卡客戶與Home Credit有關的每月數據。每行都是信用卡餘額的一個月，一張信用卡可以有多行。
:::info
- 待增加
:::

### installments_payments.csv 額外檔案
Home Credit以前貸款的付款記錄。每筆付款都有一行，每筆未付款都有一行。
:::info
- 待增加
:::
### POS_CASH_balance.csv 額外檔案
關於客戶以前的銷售點或現金貸款與住房貸款有關的每月數據。每一行都是前一個銷售點或現金貸款的一個月，以前的一筆貸款可以有多行。
:::info
- 待增加
:::

### previous_application.csv 額外檔案
之前在申請數據中擁有貸款的客戶的Home Credit貸款申請。申請數據中的每個當前貸款都可以有多個以前的貸款。每個以前的應用程序都有一行，並由功能SK_ID_PREV標識。
:::info
- 待增加
:::
<span></span>
<span></span>
![](https://i.imgur.com/Wz7I756.png)

## 進度流程
### 資料探索與分析
[Github](https://github.com/stansuo/BDSE12-Group3/tree/master/notebooks/homecdt_eda)

### 資料清洗
資料清洗部份我想請詠茹跟裕鈞提供主要協助，首先要做的事包含：
1. 將類別型資料空值與Na，改成一樣的空格，並新增一欄標注它是空白
2. 將數值型資料補0，並新增一欄標注它是空白；如適合補其它非0的值也可以，但要紀錄原因
3. 異常資料也要改成與空值一樣，像是365243的時間
4. 找出不合理的資料，像是剛剛裕鈞建議bureau 中不合理的active貸款狀態，到bureau balance 表中發現狀態都是X的那些。這些就要經由驗證欄位資料合理性來發覺，也要把它們改成跟空值一樣，且新增一欄標注，如何清洗每一欄的資料都請紀錄下來
5. 其他人則請在我們不知道怎麼code時提供==溫暖的協助==，謝謝～

### 特徵工程

### 建模驗證

### 視覺化

## 參考資料
[Kaggle](https://www.kaggle.com/)
[Home Credit Default Risk](https://www.kaggle.com/c/home-credit-default-risk)
[Kaggle：Home Credit Default Risk 數據探索及可視化](https://www.twblogs.net/a/5b8127e82b71772165ab4d2e)
[Kaggle：Home Credit Default Risk 數據探索及可視化](https://www.cnblogs.com/mtcnn/p/9411598.html)
[印度阿三詳細講解](https://www.kaggle.com/codename007/home-credit-complete-eda-feature-importance)
