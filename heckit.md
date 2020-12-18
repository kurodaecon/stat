# Heckit

「$$ f(x) = \int dx $$」 ← ここに数式のようなものが表示されていない場合は、お手数ですがこのページを再読み込み（F5）してください。数式の表示エラーが生じています（困）。

James J. Heckman, the Nobel laureate による2段階推定。通称Heckit（ヘキット）。

### 目次

- [Heckit](#heckit)
        - [目次](#目次)
    - [参考文献](#参考文献)
    - [前提知識](#前提知識)
        - [切断正規分布](#切断正規分布)
        - [サンプルセレクションモデルの概要](#サンプルセレクションモデルの概要)
        - [Tobitモデル [CT05, § 16.3]](#tobitモデル-ct05-§-163)
    - [ようやく本題](#ようやく本題)
    - [統計ソフトウェア](#統計ソフトウェア)
        - [R](#r)
        - [Stata](#stata)

## 参考文献

* 山本勲『[実証分析のための計量経済学―正しい手法と結果の読み方](https://www.biz-book.jp/isbn/978-4-502-16811-6)』中央経済社、2015．
  * 第9章 トービットモデルとヘーキットモデル
* 末石直也『[計量経済学―ミクロデータ分析へのいざない](https://www.nippyo.co.jp/shop/book/6899.html)』日本評論社、2015．
  * §6.5 Heckmanの2段階推定量
* 北村行伸『[ミクロ計量経済学入門](https://www.nippyo.co.jp/shop/book/4146.html)』日本評論社、2009．
  * 第10章 トービット・モデルとヘックマンの2段階推定―労働供給問題 (cf. [PDF](http://www.ier.hit-u.ac.jp/~kitamura/lecture/Hit/08Statsys10.pdf))
* Patrick Puhani, [The Heckman Correction for Sample Selection and Its Critique](https://doi.org/10.1111/1467-6419.00104), Journal of Economic Surveys, 14 (1), 53-68, 2000.
* Jeffrey M. Wooldridge, [Econometric Analysis of Cross Section and Panel Data (Second Edition)](https://mitpress.mit.edu/books/econometric-analysis-cross-section-and-panel-data-second-edition), MIT Press, 2010.
  * Ch. 19 Censored Data, Sample Selection, and Attrition
* A. Colin Cameron and Pravin K. Trivedi, [Microeconometrics Methods and Applications](https://www.cambridge.org/core/books/microeconometrics/982158DE989697607C858068ED05C7B1), Cambridge University Press, 2005.
  * Section 16.5.4. Heckman Two-Step Estimator

## 前提知識

### 切断正規分布

標準正規分布の密度関数 $$\phi(z) = \frac{1}{\sqrt{2\pi}} \exp(-\frac{z^2}{2})$$ について、$$z \ge c$$ の場合（「切断正規分布」）を考える（すなわち、$$c$$ 未満の $$z$$ では $$\phi(z)=0$$ となる）。

$$ \int_{c}^{\infty} \phi(z) dz = \Phi(\infty) - \Phi(c) = 1 - \Phi(c) < 1 $$ となる。定義上、密度関数を定義域で積分すると1にならなければならないので、密度関数を $$1 - \Phi(c)$$ の分だけ膨らませて $$ \frac{\phi(z)}{1 - \Phi(c)} $$ となる。

期待値を考える。

$$ E[z | z > c]
 = \int_{c}^{\infty} z \frac{\phi(z)}{1 - \Phi(c)} dz
 = \int_{c}^{\infty} z \frac{1}{1 - \Phi(c)} \frac{1}{\sqrt{2\pi}} \exp(-\frac{z^2}{2}) dz $$

ここで $$ \frac{d}{dz} \exp(- \frac{z^2}{2}) = - z \exp(- \frac{z^2}{2}) $$ より、

$$ \int_{c}^{\infty} z \exp(-\frac{z^2}{2}) dz
 = [- \exp(-\frac{z^2}{2})]_{c}^{\infty}
 = 0 + \exp(- \frac{c^2}{2}) $$

となるので、以下が得られる。

$$ E[z | z > c]
 = \frac{1}{1 - \Phi(c)} \frac{1}{\sqrt{2\pi}} \int_{c}^{\infty} z \exp(-\frac{z^2}{2}) dz
 = \frac{1}{1 - \Phi(c)} \frac{1}{\sqrt{2\pi}} \exp(- \frac{c^2}{2})
 = \frac{\phi(c)}{1 - \Phi(c)} $$

$$ E[z | z > -c]
 = \frac{\phi(-c)}{1 - \Phi(-c)}
 = \frac{\phi(c)}{1 - (1 - \Phi(c))}
 = \frac{\phi(c)}{\Phi(c)} $$

### サンプルセレクションモデルの概要

Heckitはサンプルセレクションモデルの一つとして説明されることが多い。以下、Cameron and Trivedi (2005, Ch. 16) に沿ってサンプルセレクションモデルの概要を示す。

※和文かつ必要最小限の説明がなされているという条件の下では、末石（2015）が分かりやすそう。

被説明変数の値によってサンプリングされる場合、回帰モデル（などの統計モデル）のパラメータは一致推定量ではなくなる場合がある。このとき、一致推定するためには何らかの補正を与える必要が生じる。このようなサンプルは広く selected sample として定義されている。どのように select されるかはケースバイケースなので（例：self-selection）、それらに対応した数多くの selection model が存在する。

* Tobit [type 1 Tobit] ... Section 16.3
* Bivariate sample selection model [type 2 Tobit] ... Section 16.5
* Roy model [type 5 Tobit] ... Section 16.7

### Tobitモデル [CT05, § 16.3]

Tobitモデル (censored normal regression model) は、潜在変数 (latent variable) が0以下で censoring （打ち切り）されるケースの一つ。

$$ y^{*} = \mathbf{x}' \beta + \epsilon, \quad \epsilon \sim N(0, \sigma^2), \quad y =
\begin{cases} y^{*} \quad \text{if } y^{*} > 0 \\
. \text{ (missing)} \quad \text{if } y^{*} \le 0 \end{cases} $$

ここでしれっと $$ \epsilon \sim N $$ を仮定しているが、これが満たされない場合に以下の議論を適用することは適当でない。それはまた別の話 (control function approach by Lung-Fei Lee; Gordon Dahl)。

なお、打ち切り (censoring) と切断 (truncation) は異なる点に注意。切断された分はデータが観測されないが、打ち切りはデータが閾値に張り付く (spike)。

0より左側が切断され (left truncation at zero)、$$ y^{*} > 0 $$ の場合に $$y$$ が観測される場合を考える。

$$ \begin{align} E[y]
 & = E[y^{*} | y^{*} > 0]
 = E[\mathbf{x}' \beta + \epsilon | \mathbf{x}' \beta + \epsilon > 0] \\
 & = E[\mathbf{x}' \beta | \mathbf{x}' \beta + \epsilon > 0] + E[\epsilon | \mathbf{x}' \beta + \epsilon > 0]
 = \mathbf{x}' \beta + E[\epsilon | \epsilon > - \mathbf{x}' \beta]
 \end{align} $$

ここで、$$ E[\mathbf{x}' \beta | \mathbf{x}' \beta + \epsilon > 0] = \mathbf{x}' \beta $$ は $$ \mathbf{x} $$ と $$ \epsilon $$ の独立性より（$$ \mathbf{x}' \beta $$ 部分を定数のように考えればよい）。

$$ E[\epsilon | \epsilon > - \mathbf{x}' \beta]
 = \sigma E[\frac{\epsilon}{\sigma} | \frac{\epsilon}{\sigma} > - \frac{\mathbf{x}' \beta}{\sigma}]
 = \sigma \frac{\phi(\frac{\mathbf{x}' \beta}{\sigma})}{\Phi(\frac{\mathbf{x}' \beta}{\sigma})}
 = \sigma \lambda (\frac{\mathbf{x}' \beta}{\sigma})
$$

この $$ \lambda (z) = \frac{\phi(z)}{\Phi(z)} $$ は逆ミルズ比 (inverse Mills ratio) と呼ばれる。大元の提案者である [John Mills (Biometrika 1926)](https://doi.org/10.1093/biomet/18.3-4.395) はこの逆数である $$ \frac{1 - \Phi(z)}{\phi(z)} = \frac{\Phi(-z)}{\phi(z)} $$ を用いていたため、「逆」と付くとの由。この理由から、一部のテキストでは $$ \lambda^{*}(z) = \frac{\Phi(-z)}{\phi(z)} $$ を逆ミルズ比と呼ぶらしい。

以上より $$ E[y | \mathbf{x}, y > 0] = \mathbf{x}' \beta + \sigma \phi(\frac{\mathbf{x}' \beta}{\sigma}) $$ が得られる。

導出は追わないが、限界効果は以下のとおり。

$$ \frac{\partial}{\partial \mathbf{x}} E[y | \mathbf{x}, y > 0 ] = [1 - \frac{\mathbf{x}' \beta}{\sigma} \lambda (\frac{\mathbf{x}' \beta}{\sigma}) - \lambda (\frac{\mathbf{x}' \beta}{\sigma})^2] \beta $$

推定方法には、最尤法、非線形最小二乗法、Heckmanの2段階推定、などがある。Heckmanの2段階推定について、詳細は後述するが、概要は次の通り。

* 第1段階：Full sample で $$d (=1 \ \text{if} \ y>0)$$ を $$ \mathbf{x} $$ に回帰し、$$ \alpha = \beta/\sigma $$ の一致推定量 $$ \hat{\alpha} $$ を得る
* 第2段階：Truncated sample で $$y$$ を $$ \mathbf{x}, \lambda(\mathbf{x}' \hat{\alpha}) $$ に回帰し、$$ \beta, \sigma $$ の一致推定量を得る

## ようやく本題

たとえば、賃金関数の推定を考える。就労していない人の賃金は観測されない。もし、「就労しているかどうか」が完全にランダムに（外生的に）決定されるのであれば、観測されるデータ（就労している人の分）だけを使って賃金関数を推計することで何ら問題はない。

しかし、現実にはランダムではない。このような場合、サンプルセレクション sample selection の問題が生じる。

$$ Y_i = \begin{cases} a + b x_i + u_i \quad \text{if} \quad M_i = 1 \\ . \text{ (unobservable)} \quad \text{if} \quad M_i = 0 \end{cases} $$

$$ M_i = \begin{cases} 1 \quad \text{if} \quad M_i^{*} > m \\ 0 \quad \text{if} \quad M_i^{*} \le m \end{cases} , \quad M_i^* = \alpha + \beta z_i + v_i $$

* $$ Y $$: 観測される賃金
* $$ x $$: 個人属性（教育年数など）
* $$ M $$: 就労しているかどうか (in labor force --> 1, or not --> 0)
* $$ z $$: IV

cf. [Tobitと線形モデルを比較した分かりやすい図](https://www.researchgate.net/figure/Tobit-model-vs-OLS-Consistent-estimates-of-the-parameters-of-the-Tobit-model-may-be_fig2_255606959)

## 統計ソフトウェア

### R

```R
# install.packages("sampleSelection")
library(sampleSelection)
data(Mroz87)
h0 <- heckit(selection = lfp ~ age + I(age^2) + faminc + I(kids5 + kids618) + educ,
  outcome = wage ~ exper + I(exper^2) + educ + city, Mroz87)
summary(h0)
```

### Stata

```Stata
use "http://www.stata.com/data/jwooldridge/eacsap/mroz", clear
replace wage = . if wage == 0
generate exper2 = exper^2
generate age2 = age^2
generate kids = kidslt6 + kidsge6
heckman wage exper exper2 educ city, select(age age2 faminc kids educ)
```

cf. Wooldridge "Econometric Analysis of Cross Section and Panel Data" の [.dta ファイル](https://www.stata.com/texts/eacsap/)
