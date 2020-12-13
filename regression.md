# 線形回帰モデル



## 参考文献

* 田中隆一『[計量経済学の第一歩](http://www.yuhikaku.co.jp/books/detail/9784641150287)』有斐閣、2015．
 * 第5章 単回帰分析、第6章 重回帰分析の基本、第7章 重回帰分析の応用
* 山本勲『[実証分析のための計量経済学―正しい手法と結果の読み方](https://www.biz-book.jp/isbn/978-4-502-16811-6)』中央経済社、2015．
 * 第5章 最小二乗法の仕組みと適用条件　他
* 末石直也『[計量経済学―ミクロデータ分析へのいざない](https://www.nippyo.co.jp/shop/book/6899.html)』日本評論社、2015．
 * 第1章 線形回帰とOLS
* Jeffrey M. Wooldridge, [Econometric Analysis of Cross Section and Panel Data (Second Edition)](https://mitpress.mit.edu/books/econometric-analysis-cross-section-and-panel-data-second-edition), MIT Press, 2010.
 * Ch. 4 Single-Equation Linear Model and Ordinary Least Squares Estimation
* William H. Greene, [Econometric Analysis (8th Edition)](https://www.pearson.com/us/higher-education/program/Greene-Econometric-Analysis-8th-Edition/PGM334862.html), Pearson, 2018.
 * Part I The Linear Regression Model
* A. Colin Cameron and Pravin K. Trivedi, [Microeconometrics Methods and Applications](https://www.cambridge.org/core/books/microeconometrics/982158DE989697607C858068ED05C7B1), Cambridge University Press, 2005.
 * Ch. 4 Linear Models

## 概要

Greene pp. 17ff

線形回帰モデルの仮定
* A1. 線形性：$$ y = x_1 \beta_1 + \cdots + x_K \beta_K + \epsilon = \mathbf{x}' \mathbf{\beta} + \epsilon $$
* A2. フルランク：$$ K \times K $$ の行列 $$ E[\mathbf{xx}'] = \mathbf{Q} $$ がフルランク、すなわち $$ \mbox{rank}(\mathbf{Q}) = K $$
