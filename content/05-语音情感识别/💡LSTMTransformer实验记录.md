> `macro avg` 就是 `UA` ，计算方法，对应的上面直接平均即可，这就是未加权精度。
> `weighted avg` 就是 `WA` ，计算方法，上面对应的各个值乘上该类别占总样本数量的比例，这就是加权平均。

---
<center>📌📌📌09-13📌📌📌</center>

| batch | epoch | lr   | 类别平衡 | Loss  |     |
| ----- | ----- | ---- | ---- | ----- | --- |
| 16    | 500   | 1e-4 | True | Cross |     |
 + epoch-332
                  precision    recall  f1-score   support
          正向       0.65      0.52      0.58      1943
          正常       0.64      0.49      0.55      2145
          负向       0.28      0.89      0.42       406

    accuracy                           0.54      4494
   macro avg       0.52      0.63      0.52      4494
weighted avg       0.61      0.54      0.55      4494

+ 🌟epoch-232
                  precision    recall  f1-score   support
          正向       0.65      0.53      0.59      1943
          正常       0.63      0.53      0.58      2145
          负向       0.32      0.87      0.47       406

    accuracy                           0.56      4494
   macro avg       0.53      0.64      0.54      4494
weighted avg       0.61      0.56      0.57      4494

|  正向  |  正常  | 负向 |
|:------:|:------:|:----:|
| `1032` |  632   | 279  |
|  529   | `1134` | 482  |
|   23   |  29  | `354`  |

---
<center>📌📌📌09-14📌📌📌</center>

| batch | epoch | lr   | 类别平衡 | Loss  |
| ----- | ----- | ---- | -------- | ----- |
| 16    | 500   | 🚩1e-3 | True     | Cross | 
极其抖动，不测了

---

| batch | epoch | lr   | 类别平衡 | Loss  |
| ----- | ----- | ---- | -------- | ----- |
| 16    | 500   | 🚩1e-5 | True     | Cross | 
+ epoch-483
                  precision    recall  f1-score   support
          正向       0.58      0.76      0.66      1943
          正常       0.72      0.33      0.45      2145
          负向       0.34      0.81      0.47       406

    accuracy                           0.56      4494
   macro avg       0.55      0.63      0.53      4494
weighted avg       0.63      0.56      0.54      4494

|  正向  | 正常 | 负向  |
|:------:|:----:|:-----:|
| `1472` | 253  |  218  |
| `1001` | 710  |  434  |
|   55   |  22  | `329` |

> 💡💡💡 以上是类别平衡下的结果，出结论，最好的是 1e-4 的训练结果，ACC 56%

---

| batch | epoch | lr   | 类别平衡    | Loss  |     |
| ----- | ----- | ---- | ------- | ----- | --- |
| 16    | 500   | 1e-4 | 🚩False | Cross |     |
+ epoch-495
                   precision    recall  f1-score   support
          正向       0.90      0.84      0.87      1943
          正常       0.79      0.92      0.85      2145
          负向       0.77      0.38      0.51       406

    accuracy                                  0.84      4494
   macro avg       0.82      0.71      0.74      4494
weighted avg       0.84      0.84      0.83      4494

+ 🌟epoch-395
                  precision    recall  f1-score   support
          正向       0.89      0.86      0.87      1943
          正常       0.80      0.91      0.85      2145
          负向       0.85      0.35      0.49       406

    accuracy                                  0.84      4494
   macro avg       0.84      0.70      0.74      4494
weighted avg       **0.84**      0.84      0.83      4494

| 正向 | 正常 | 负向 |
|:----:|:----:|:----:|
| `1666` | 262  |  15  |
| 183  | `1952` |  10  |
|  28  | `237`  | 141  |

---
<center>📌📌📌09-15📌📌📌</center>

| batch | epoch | lr   | 类别平衡 | Loss  |
| ----- | ----- | ---- | -------- | ----- |
| 16    | 500   | 1e-3 | 🚩False     | Cross | 
奇怪曲线，放弃

---

| batch | epoch | lr   | 类别平衡    | Loss  |     |
| ----- | ----- | ---- | ------- | ----- | --- |
| 16    | 500   | 1e-5 | 🚩False | Cross |     |

已经没有类别能够预测为 **负向情绪了**

> 💡💡💡 以上是类别不平衡下使用交叉熵损失的结果，最好 ACC 84%

---

| batch | epoch | lr   | 类别平衡  | Loss    |     |
| ----- | ----- | ---- | ----- | ------- | --- |
| 16    | 500   | 1e-4 | False | 🚩Focal |     |
+ epoch-499
                     precision    recall  f1-score   support
          正向       0.89      0.92      0.90      1943
          正常       0.88      0.90      0.89      2145
          负向       0.89      0.69      0.78       406

    accuracy                           0.89      4494
   macro avg       0.89      0.84      0.86      4494
weighted avg       **0.89**      0.89      0.89      4494

|  正向  |  正常  | 负向 |
|:------:|:------:|:----:|
| `1781` |  151   |  11  |
|  1944  | `1926` |  25  |
|   20   |  105   | `281`  |

---
<center>📌📌📌09-16📌📌📌</center>

| batch | epoch | lr   | 类别平衡 | Loss  |
| ----- | ----- | ---- | -------- | ----- |
| 16    | 500   | 1e-5 | False     | 🚩Focal | 
+ epoch-499
                  precision    recall  f1-score   support
          正向       0.81      0.80      0.80      1943
          正常       0.75      0.86      0.80      2145
          负向       0.89      0.30      0.44       406

    accuracy                                  0.78      4494
   macro avg       0.82      0.65      0.68      4494
weighted avg       0.79      0.78      0.77      4494

---

| batch | epoch | lr   | 类别平衡 | Loss  |
| ----- | ----- | ---- | -------- | ----- |
| 16    | 500   | 1e-3 | False     | 🚩Focal | 
不行，该学习率太大了

> 以上在全部样本上，使用 Focal Loss 最好的识别结果 ACC 89%
