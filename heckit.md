# Heckit

James J. Heckman, the Nobel laureate による2段階推定。通称Heckit（ヘキット）。

## 参考文献

* 山本勲『[実証分析のための計量経済学―正しい手法と結果の読み方](https://www.biz-book.jp/isbn/978-4-502-16811-6)』中央経済社、2015．
 * 第9章 トービットモデルとヘーキットモデル
* 末石直也『[計量経済学―ミクロデータ分析へのいざない](https://www.nippyo.co.jp/shop/book/6899.html)』日本評論社、2015．
 * §6.5 Heckmanの2段階推定量
* 北村行伸『[ミクロ計量経済学入門](https://www.nippyo.co.jp/shop/book/4146.html)』日本評論社、2009．
 * 第10章 トービット・モデルとヘックマンの2段階推定―労働供給問題 (cf. [PDF](http://www.ier.hit-u.ac.jp/~kitamura/lecture/Hit/08Statsys10.pdf))
* Patrick Puhani, [The Heckman Correction for Sample Selection and Its Critique](https://doi.org/10.1111/1467-6419.00104), Journal of Economic Surveys, 14 (1), 53-68, 2000.
* A. Colin Cameron and Pravin K. Trivedi, [Microeconometrics Methods and Applications](https://www.cambridge.org/core/books/microeconometrics/982158DE989697607C858068ED05C7B1), Cambridge University Press, 2005.
 * Section 16.5.4. Heckman Two-Step Estimator

## 概要

ある条件を満たす場合しかデータが観測されない場合がある。

たとえば、賃金関数を推定する場合。就労していない人の賃金は観測されない。「就労しているかどうか」が完全にランダムに（外生的に）決定されるのであれば、観測されるデータ（就労している人の分）だけを使って賃金関数を推計することで何ら問題はないが、現実にはランダムではない。このような場合、サンプルセレクション sample selection の問題が生じる。

$$ Y_i = \begin{cases} \beta_0 + \beta_1 x_i + u_i \quad \mbox{if} \quad M_i = 1 \\ . \quad \mbox{if} \quad M_i = 0 \end{cases} $$

$$ M_i = \begin{cases} 1 \quad \mbox{if} \quad M_i^* > m \\ 0 \quad \mbox{if} \quad M_i^* \le m \end{cases} , \quad M_i^* = \alpha + \beta z_i + v_i $$

$$ a \text{ if } b $$

* $$ Y_i $$: 観測される賃金（観測されない `.` 場合もある）
* $$ x_i^* $$: 個人属性（教育年数など）
* $$ M_i = 1 $$ if in labor force （就労）
