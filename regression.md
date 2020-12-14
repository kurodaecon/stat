# 線形回帰モデル

$$ f(x) = \int dx $$ ← ここに数式のようなものが表示されていない場合は、お手数ですがこのページを再読み込み（F5）してください。数式の表示エラーが生じています。


## 参考文献

* 田中隆一『[計量経済学の第一歩](http://www.yuhikaku.co.jp/books/detail/9784641150287)』有斐閣、2015．
    * 第5章 単回帰分析、第6章 重回帰分析の基本、第7章 重回帰分析の応用
* 山本勲『[実証分析のための計量経済学―正しい手法と結果の読み方](https://www.biz-book.jp/isbn/978-4-502-16811-6)』中央経済社、2015．
    * 第5章 最小二乗法の仕組みと適用条件　他
* 末石直也『[計量経済学―ミクロデータ分析へのいざない](https://www.nippyo.co.jp/shop/book/6899.html)』日本評論社、2015．
    * 第1章 線形回帰とOLS
* 浅野皙、中村二朗『[計量経済学 (第2版)](http://www.yuhikaku.co.jp/books/detail/9784641163362)』有斐閣、2009．
    * 第2章　古典的2変数回帰モデル　他
* Jeffrey M. Wooldridge, [Econometric Analysis of Cross Section and Panel Data (Second Edition)](https://mitpress.mit.edu/books/econometric-analysis-cross-section-and-panel-data-second-edition), MIT Press, 2010.
    * Ch. 4 Single-Equation Linear Model and Ordinary Least Squares Estimation
* William H. Greene, [Econometric Analysis (8th Edition)](https://www.pearson.com/us/higher-education/program/Greene-Econometric-Analysis-8th-Edition/PGM334862.html), Pearson, 2018.
    * Part I The Linear Regression Model
* A. Colin Cameron and Pravin K. Trivedi, [Microeconometrics: Methods and Applications](https://www.cambridge.org/core/books/microeconometrics/982158DE989697607C858068ED05C7B1), Cambridge University Press, 2005.
    * Ch. 4 Linear Models


## 基礎

### ノーテーション [Greene, pp. 13ff]

$$ y = x_1 \beta_1 + \cdots + x_K \beta_K + \epsilon = \mathbf{x}' \boldsymbol{\beta} + \epsilon $$

$$ \mathbf{y} = \mathbf{x}_1 \beta_1 + \cdots + \mathbf{x}_K \beta_K + \boldsymbol{\epsilon} = \begin{bmatrix} \mathbf{x}_1 & \mathbf{x}_2 & \cdots & \mathbf{x}_K \end{bmatrix} \begin{bmatrix} \beta_1 \\ \beta_2 \\ \vdots \\ \beta_K \end{bmatrix} + \boldsymbol{\epsilon} = \mathbf{X} \boldsymbol{\beta} + \boldsymbol{\epsilon} $$

* $$ n $$: observations
* $$ K $$: variables (include intercept)
* $$ y $$: 被説明変数, dependent variable, explained variable, regressand, outcome
    * $$ \mathbf{y} = \begin{bmatrix} y_1 \\ y_2 \\ \vdots \\ y_n \end{bmatrix} $$: $$ n \times 1 $$ vector
* $$ x_1, \ldots, x_K $$: 説明変数, independent variable, explanatory variable, regressors
    * $$ \mathbf{x} = \begin{bmatrix} x_1 \\ x_2 \\ \vdots \\ x_n \end{bmatrix}, \quad
 \mathbf{X} = \begin{bmatrix} \mathbf{x}_1 & \mathbf{x}_2 & \cdots & \mathbf{x}_K \end{bmatrix} = \begin{bmatrix}
 x_{11} & x_{12} & \cdots & x_{1K} \\
 x_{21} & x_{22} & \cdots & x_{2K} \\
 \vdots & \vdots & \ddots & \vdots \\
 x_{n1} & x_{n2} & \cdots & x_{nK} \end{bmatrix} $$
* $$ \epsilon $$: random disturbance
    * $$ \boldsymbol{\epsilon} = \begin{bmatrix} \epsilon_1 \\ \epsilon_2 \\ \vdots \\ \epsilon_n \end{bmatrix} $$: $$ n \times 1 $$ vector
* $$ \boldsymbol{\beta} = \begin{bmatrix} \beta_1 \\ \beta_2 \\ \vdots \\ \beta_K \end{bmatrix} $$: $$ K \times 1 $$ vector （回帰係数）

※行列演算

* $$ \{AB\}_{ij} = \sum_{l = 1}^{m} \{A\}_{il} \{B\}_{lj} $$
    * $$ A: n \times m, \quad B: m \times k, \quad AB: n \times k $$
* $$ A' $$ or $$ A^T $$：転置 ... $$ \{A'\}_{ij} = \{A\}_{ji}$$
* $$ I $$ or $$ E $$：単位行列 ... $$ AI = IA = A $$
* $$ A^{-1} $$：逆行列 ... $$ A^{-1} A = A A^{-1} = I $$

### 線形回帰モデルの仮定 [Greene, pp. 17ff, p. 55]

* A1. 線形性：$$ \mathbf{y} = \mathbf{X} \boldsymbol{\beta} + \boldsymbol{\epsilon} $$
* A2. フルランク：$$ E[\mathbf{xx}'] = \mathbf{Q} $$ （$$ K \times K $$ の行列）がフルランク、すなわち $$ \mbox{rank}(\mathbf{Q}) = K $$
* A3. 説明変数の外生性：$$ E[\epsilon | x_1, \ldots, x_K] = E[\epsilon | \mathbf{x}] $$
* A4. 誤差項の均一分散：$$ \mbox{Var}[\epsilon | \mathbf{x}] = \mbox{Var}[\epsilon] = \sigma^2 $$ and $$ E[\epsilon_i \epsilon_j | \mathbf{X}] = 0 $$ → $$ E[\boldsymbol{\epsilon} \boldsymbol{\epsilon}' | \mathbf{X}] = \sigma^2 \mathbf{I} $$
* A5. データ生成過程：非確率的な、$$ (y, \mathbf{x}) $$ の結合分布からの iid (independent, identically distributed) ドローのランダム標本
* A6. 誤差項の正規性：$$ \epsilon \sim N(\mathbf{x}' \boldsymbol{\beta}, \sigma^2) $$

### 最小二乗法 [Greene, pp. 28ff]

* $$ y_i = \mathbf{x}'_i \mathbf{b} + e_i $$
    * $$ y_i = \mathbf{x}'_i \boldsymbol{\beta} + \epsilon_i = \mathbf{x}'_i \mathbf{b} + e_i $$
    * $$ e $$: 残差 residual
    * $$ b $$: 回帰パラメータの推定量 estimator
* OLS: $$ \mbox{Minimize}_{\mathbf{b}} S(\mathbf{b}) $$
    * $$ S(\mathbf{b}) \equiv \mathbf{e}' \mathbf{e} = (\mathbf{y} - \mathbf{Xb})'(\mathbf{y} - \mathbf{Xb}) = \sum e_{i}^2 = \sum (y_{i} - \mathbf{x}'_i \mathbf{b})^2 $$
    * $$ S(\mathbf{b}) = \mathbf{y}' \mathbf{y} - 2 \mathbf{y}' \mathbf{Xb} + \mathbf{b' X' X b} $$
    * 残差二乗和が最小 ⇔ 微分して0：$$ \frac{\partial S(\mathbf{b})}{\partial \mathbf{b}} = -2 \mathbf{X' y} + 2 \mathbf{X' X b} = \mathbf{0} $$
        * 「最大」ではなく「最小」であるためには2階微分が正定値行列である必要があり、これも確認可
    * 正規方程式：$$ \mathbf{X' y} = \mathbf{X' X b} $$
    * OLS推定量：$$ \mathbf{b} = (\mathbf{X' X})^{-1} \mathbf{X' y} = \boldsymbol{\beta} + \mathbf{A} \boldsymbol{\epsilon} $$ ← 線形推定量
        * $$ \mathbf{A} \equiv (\mathbf{X' X})^{-1} \mathbf{X'} $$
    * OLS推定量の期待値：$$ E[\mathbf{b} | \mathbf{X}] = \boldsymbol{\beta} $$ ← 不偏推定量
    * OLS推定量の分散：$$ \mbox{Var}[\mathbf{b} | \mathbf{X}]
 = E[(\mathbf{b} - \boldsymbol{\beta}) (\mathbf{b} - \boldsymbol{\beta})' | \mathbf{X}]
 = E[\mathbf{A} \boldsymbol{\epsilon} \boldsymbol{\epsilon}' \mathbf{A}' | \mathbf{X}]
 = \mathbf{A} E[\boldsymbol{\epsilon} \boldsymbol{\epsilon}' | \mathbf{X}] \mathbf{A}'
 = \sigma^2 (\mathbf{X' X})^{-1} $$
* 決定係数：$$ R^2 = \frac{\mbox{regression sum of squares}}{\mbox{total sum of squares}} = 1 - \frac{\sum e_i^2}{\sum (y_i - \bar{y})^2} $$
    * 自由度調整済み決定係数：$$ \bar{R}^2 = 1 - \frac{n - 1}{n - K} (1 - R^2) $$
* ガウス・マルコフの定理により、OLS推定量はBLUE (best linear unbiased estimator)
    * 線形性と不偏性は上記のとおり
    * 最小分散 (best)：線形で不偏な一般的な推定量 $$ \mathbf{b}_0 = \mathbf{Cy} $$ を考え、その分散がOLS推定量の分散より小さくはならないことを示す
* 回帰係数の検定：$$ b_k \sim N(\beta_K, \sigma^2 S^{kk}), \quad z_k = \frac{b_k - \beta_k}{\sqrt{\sigma^2 S^{kk}}} $$
    * $$ S^{kk} $$: $$ k $$-th diagonal element of $$ (\mathbf{X' X})^{-1} $$
    * 95%信頼区間：$$ \mbox{Prob}[-1.96 \le z_k \le 1.96] = 0.95 $$
        * $$ \mbox{Prob}[b_k -1.96 \sqrt{\sigma^2 S^{kk}} \le \beta_k \le b_k + 1.96 \sqrt{\sigma^2 S^{kk}}] = 0.95 $$
    * $$ t_k = \frac{b_k - \beta_k}{\sqrt{\sigma^2 S^{kk}}} \sim t(n - K) $$
    * $$ \mbox{Prob}[b_k - t_{1-\alpha/2,n-K} \sqrt{\sigma^2 S^{kk}} \le \beta_k \le b_k + t_{1-\alpha/2,n-K} \sqrt{\sigma^2 S^{kk}}] = 1 - \alpha $$
        * $$ \alpha $$：有意水準（5%など）
* Joint検定：$$ H_0: \mathbf{R} \boldsymbol{\beta} - \mathbf{q} = \mathbf{0} $$

## 統計ソフトウェア

### R

```r
lm0 <- lm(y ~ x1 + x2, data = dataset0)
summary(lm0)
car::linearHypothesis(lm0, c("x1 = 0", "x2 = 0")
```

### Stata

```Stata
regress y x1 x2, robust
test x1 x2 // joint test
```
