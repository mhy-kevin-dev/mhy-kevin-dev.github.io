### 問題描述：程式執行時，GPU無法全速執行，而且會佔用16~32個process

### 原因：

GPU不支援64位元運算時，只要牽涉64位元的運算就會使用CPU來計算，導致GPU的memory雖然有被使用，但是執行速度不如印象中的GPU那麼快。（例如：兩個宣告為float64的matrix運算）

### 解決方法：使用numpy宣告float32的最小正數：

```py
eps = np.finfo('float32').eps
```

或者直接宣告一個很小的數字：

`eps = 1e-16`

也可透過Keras的函數：

```py
from keras import backend as K
K.set_epsilon(np.finfo(np.float32).eps)
```
