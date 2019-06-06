## Oracle-Machine-Learning-workshop
## Why Query ML?

用SQL就可以建立和執行ML Model,並且做出預測,讓SQL使用者可以用現有工具加速開發，不用搬移資料，不用費時建立TensorFlow及學習新語言如R、Python etc..，讓Machine Learning普及化。

![image](https://github.com/Lhanber/Oracle-Machine-Learning-workshop/blob/master/image/Image1.png)

**特徵與標籤**

假設要讓機器分辨橘子或是蘋果該怎麼區分？
你可能會覺得用顏色分辨！
這是一個非常好的答案，但假設我們得到的照片是黑白照片我們要如何分辨？

範例
來看下面這個例子：

|  重量	 |  表面 | 標籤  |
| ----- | ----- | ----- |
| 150g  | 皺 |  橘子 |
|  170g | 皺 |  橘子 |
| 130g  | 平滑 | 蘋果  |
| 140g  |  平滑 |  蘋果 |
| ...   |  ...  | ...  |


利用重量跟表面來當我們訓練的資料，而重量跟表面的資料就是我們所謂的特徵，
而標籤就是定義這些特徵的結果是哪一種水果，也就是我們透過機器學習想讓電腦告訴我們的答案。

**Traning step**

將需要被預測的歷史資料篩選出Label、Feature當作Traning data，並且選擇演算法訓練模型。

**Predict step**

將新資料丟進model跑出model預測結果。

參考資料:

[1]https://ithelp.ithome.com.tw/articles/10197029
