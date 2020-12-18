# Heckit

「$$ f(x) = \int dx $$」 ← ここに数式のようなものが表示されていない場合は、お手数ですがこのページを再読み込み（F5）してください。数式の表示エラーが生じています（困）。

James J. Heckman, the Nobel laureate による2段階推定。通称Heckit（ヘキット）。

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

標準正規分布の密度関数 $$\phi(z) = \frac{1}{\sqrt{2\pi}} \exp(-\frac{z^2}{2})$$ について、$$z \ge c$$ の場合を考える（すなわち、$$c$$ 未満の $$z$$ では $$\phi(z)=0$$ となる）。

このような確率分布（「切断正規分布」）の密度関数を考える。$$ \int_{c}^{\infty} \phi(z) dz = \Phi(\infty) - \Phi(c) = 1 - \Phi(c) < 1 $$ なので、切断正規分布の密度関数を積分して $$1$$ になるためには $$1 - \Phi(c)$$ の分だけ膨らませる必要がある（定義上、密度関数を定義域で積分すると $$1$$ にならなければならない）。すなわち、切断（標準）正規分布の密度関数は $$ \frac{\phi(z)}{1 - \Phi(c)} $$ となる。

$$ E[z | z > c]
 = \int_{c}^{\infty} z \frac{\Phi(z)}{1 - \Phi(c)} dz
 = \int_{c}^{\infty} z \frac{1}{1 - \Phi(c)} \frac{1}{\sqrt{2\pi}} \exp(-\frac{z^2}{2}) dz $$

ここで、$$ \frac{d}{dz} \exp(- \frac{z^2}{2}) = - z \exp(- \frac{z^2}{2}) $$ より、

$$ \int_{c}^{\infty} z \exp(-\frac{z^2}{2}) dz
 = [- \exp(-\frac{z^2}{2})]_{c}^{\infty}
 = 0 + \exp(- \frac{c^2}{2}) $$ が得られる。

よって、期待値は以下となる。

$$ E[z | z > c]
 = \frac{1}{1 - \Phi(c)} \frac{1}{\sqrt{2\pi}} \int_{c}^{\infty} z \exp(-\frac{z^2}{2}) dz
 = \frac{1}{1 - \Phi(c)} \frac{1}{\sqrt{2\pi}} \exp(- \frac{c^2}{2})
 = \frac{\phi(c)}{1 - \Phi(c)} $$

### サンプルセレクションモデル

Heckitはサンプルセレクションモデルの一つとして説明されることが多い。以下、Cameron and Trivedi (2005, §16.5) に沿ってサンプルセレクションモデルの概要を示す。

## 本題の「Heckit」

ある条件を満たす場合しかデータが観測されない場合がある。

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
