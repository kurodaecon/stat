# 線形回帰モデル



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
* A. Colin Cameron and Pravin K. Trivedi, [Microeconometrics Methods and Applications](https://www.cambridge.org/core/books/microeconometrics/982158DE989697607C858068ED05C7B1), Cambridge University Press, 2005.
 * Ch. 4 Linear Models


## 基礎

Greene [pp. 13ff]

$$ y = x_1 \beta_1 + \cdots + x_K \beta_K + \epsilon = \mathbf{x}' \boldsymbol{\beta} + \epsilon $$
* $$ y $$: dependent variable, explained variable, regressand
* $$ x_1, \ldots, x_K $$: independent variable, explanatory variable, regressors
 * $$ \mathbf{x} = (x_1, x_2, \ldots, x_K) $$
* $$ \epsilon $$: random disturbance

$$ \mathbf{y} = \mathbf{x}_1 \beta_1 + \cdots + \mathbf{x}_K \beta_K + \boldstyle{\epsilon} = \mathbf{X} \boldsymbol{\beta} + \boldsymbol{\epsilon} $$
* $$ \mathbf{y} = \begin{bmatrix} y_1 \\ y_2 \\ \\ \vdots \\ y_n \end{bmatrix} $$
* $$ \mathbf{X} = \begin{bmatrix}
 x_{11} & x_{12} & \cdots & x_{1K} \\
 x_{21} & x_{22} & \cdots & x_{2K} \\
 \vdots & \vdots & \ddots & \vdots \\
 x_{n1} & x_{n2} & \cdots & x_{nK} \end{bmatrix} $$
* $$ \boldstyle{\epsilon} = \begin{bmatrix} \epsilon_1 \\ \epsilon_2 \\ \\ \vdots \\ \epsilon_n \end{bmatrix} $$


Greene [pp. 17ff]

線形回帰モデルの仮定
* A1. 線形性：$$ y = x_1 \beta_1 + \cdots + x_K \beta_K + \epsilon = \mathbf{x}' \boldsymbol{\beta} + \epsilon $$
* A2. フルランク：$$ E[\mathbf{xx}'] = \mathbf{Q} $$ （$$ K \times K $$ の行列）がフルランク、すなわち $$ \mbox{rank}(\mathbf{Q}) = K $$
