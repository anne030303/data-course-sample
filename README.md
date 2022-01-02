# data-course-sample

## Week2. 實作「Content-based Filtering」的推薦系統

### 清理資料

* 將rank中的排名及類別分開，並只保留有較多資訊可使用的欄位，方便後續作業
* 由於上週分析資料後得知
  * 新用戶比例遠高於曾購買並評論的舊用戶
  * 近期的商品被購買的機會較高
 
因此採用方法如下

### 方法1
* 以商品資訊中的title, brand, description三項作為特徵萃取的目標
* 嘗試給予三項資訊不同的權重，重要性依次為title > brand > description
* 若為舊用戶，先計算購買過的商品各自最相似的10種商品，加總後再排序找到相似分數最高的10樣進行推薦
* 若為新用戶，則提供近兩月評論數排名前10的商品
推薦分數：0.15084745762711865

### 方法2
* 以商品資訊中的title, brand, description三項作為特徵萃取的目標
* 嘗試給予三項資訊不同的權重，重要性依次為title > brand > description
* 若為舊用戶，先計算購買過的商品各自最相似的10種商品，加總後再排序找到相似分數最高的5樣商品，並加入近兩月評論數排名前5名的商品進行推薦
* 若為新用戶，則提供近兩月評論數排名前10的商品
推薦分數：0.15084745762711865

### 小結
* 舊用戶的推薦效果不彰，並沒有成功打到用戶實際購買的商品
* 目前的分數主要來源自rule-based的熱銷商品排行

## Week1. 實作「rule-based」的推薦系統

### 資料集：採用2014 年 Amazon 發布的資料集中「美妝（All Beauty）」類別的商品資料
### 資料內容：
* 評論資料：2000-01-10 - 2018-10-02 (共 371345)
  * 訓練資料：2000-01-10 - 2018-09-01 (共 370752)
  * 測試資料：2018-09-01 - 2018-09-30 (共 590)
* 商品數量：共 32892 個商品

### 採用規則：先抓取指定時間內的評論資料，再透過計算商品評論數
* 第一組：只抓近60天的資料
  * 考量美妝商品推陳出新，消費者可能會選擇近期熱銷商品，因此僅抓取近60天的評論資料
* 第二組：只抓去年同月的資料
  * 有些美妝商品可能是有季節性的，可能適合冬季或夏季使用，因此也嘗試推薦前一年度同時間段購買的商品
* 第三組：近60天的資料與去年同月資料各半
  * 折衷兩種方法做評估

另針對評論分數，分別針對未篩選評論分數及篩選評論分數3以上做評估

### 結果
* 未對評論分數做篩選
  * 第一組：0.15254237288135594
  * 第二組：0.1
  * 第三組：0.09661016949152543
* 篩選評論分數3以上
  * 第一組：0.13728813559322034
  * 第二組：0.09830508474576272
  * 第三組：0.11016949152542373

### 小結
* 前一年度的資料對推薦商品的效益較低
* 評論分數篩選僅在第三組有些微的提升，可能評論分數較主觀，或許對推薦有幫助但幫助不大
