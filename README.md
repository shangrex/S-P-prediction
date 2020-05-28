---
tags: homework
---

# FDA S&P500 prediction
## 目標
藉由今日以及過去的資料去預測明日的漲跌
## data 分析
train data: 02-Jan-2009 to 29-Dec-2017
test data: 02-Jan-2018 to 31-Dec-2018
* [可能有貿易戰的影響](https://zh.wikipedia.org/wiki/2018%EF%BC%8D2020%E5%B9%B4%E4%B8%AD%E7%BE%8E%E8%B4%B8%E6%98%93%E6%88%98)，貿易戰從2018年3月開始，所以準確度低了許多，可能老川發一個twitter股價就嚴重撥動。
* 因為有做Close Price和Volume、Date的相關程度分析，結果發現和兩者的關係其實並沒有太大的關係，所以在多數情況下要謹慎使用
Volume(呈現負相慣-0.68)
* 成交量代表著當日市場的熱度，一般來說，成交量愈高代表股票市場中愈不穩定古可能是股票急漲，或是急跌
* 股票的帳跌為當日收盤價與前一天收盤價的比較
* 股票容易有馬太效應，在沒有外力的影響下，很容易有單方向成長的發生，所以我主要以此為發想，做feature的操作。
## featrue & preprocess


Open Price:開盤價
Close Price:收盤價
Highe Price:高點
Low Price:低點
Volume: 成交
slope_result: 明日的漲跌差值（所求)-> 因為有用到明日的資料，所以沒有使用，否則準確率會為1
slope_d: 前一天與今日的收盤價的方向
slope_m: 前1月與今日的收盤價的方向（30天）
slope_w: 前一周（5日）與今日的收盤價的方向
open_w: 前一周（5日）與今日的開盤價的方向
read_d: 前一天與今日的收盤價的差值
Hight - Low Prices: 高點與低點的差值
season : 由Date加入四季的因素

## 模型

* logistic regression
使用GridSearchCV，測試準確率最高的參數
feature經過測試發現使用['slope_d', 'open_w', 'slope_w', 'Volume', 'real_w', 'High Price', 'Open Price', 'Low Price']會有較高的準確率
* neural network
使用GridSearchCV，測試準確率最高的參數
feature經過測試發現使用['slope_d', 'Volume', 'season', 'Open Price', 'Low Price', 'High Price', 'real_w']會有較高的準確率
* random forest
feature經過測試發現使用['slope_d', 'slope_m', 'slope_w', 'real_w']會有較高的準確率
* SVM classifier
feature經過測試發現使用['slope_w', 'slope_d']會有較高的準確率
* ensemble
利用許多classifier做ensemble，但準確率並沒有帶來太大的提升。

我認為在不同的dataset下，這些準確率不太會改變，反而有可能上升，原因是我認為在有貿易戰的情況下，本身就比較難預測，若沒有，理論上我的準確率還可以在提升。

## 結論：
我認為這次作業模型本身不是重點，重點是要找到適合模型本身的feature。

我認為這次作業本身的預測非常難做，因為在股票市場中，有太多太多的應邊變因，雖然S&P500已經是相對來說比較穩定，但剛好2018年是中美貿易戰，可以發現隨著衝突愈來愈嚴俊，股票收盤價愈來愈低，而且交易量也相對來說低了非常多，市場反應非常不樂觀。

基本上，我這次使用Logistic Regressoin和neural network的classifier會有最高的準確度，我覺得很大的程度是加入了Volume這個fearture，又因為我沒有對Volume去做任何的分類，所以導致那些單純做分類的會感受不到數字變化的敏感度。我想如果換一個時間段的dataset，結果應該還是會一樣的。
