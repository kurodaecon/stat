# 本サイトの構築

「GitBook」によるMarkdownからのHTML化→GitHub上に公開

* おもに、r-ngtmさんの「[GitBookでWebサイトを作ってGitHub Pagesで公開する方法](https://r-ngtm.hatenablog.com/entry/2020/06/18/193235)」（2020-06-18）を参照
* Windows環境でPowerShellを使用
  * `npm init` できない場合、PowerShellを管理者として実行。`Set-ExecutionPolicy Unrestricted` で実行ポリシーを変更
* `TypeError: cb.apply is not a function` → [「node.jsのバージジョンを下げたらエラーが解消」との記述](https://teratail.com/questions/279576)を見つけたので、最新版をアンインストールしてから、[少し古いバージョン](https://nodejs.org/ja/download/releases/)を再インストール
* `Error: ENOENT: no such file or directory, * '* \gitbook\gitbook-plugin-fontsettings\fontsettings.js'` → [GitBook で 'no such file or directory' というエラーが表示される](http://kuttsun.blogspot.com/2018/06/gitbook-no-such-file-or-directory.html)
* [Gitのインストール](https://notepad-plus-plus.org/downloads/)
  * エディタは[Notepad++](https://notepad-plus-plus.org/downloads/)などにしておく
  * `git commit` で `Author identity unknown` と言われたら、指示されたとおりに入力
* [MathJax](https://github.com/GitbookIO/plugin-mathjax)で綺麗な数式が表示できる
  * book.json にプラグインとして追加する：
````
{
    "language": "ja",
    "plugins":["mathjax"]
}
````
  * メニューバーから移動すると数式が表示されない（再読み込みしないと表示されない）という不具合あり。原因不明。
* Markdownの作成
  * ChromeやFirefoxの拡張機能「Markdown Viewer」を利用してプレビュー
  * 数式を使う場合はVisual Studio Codeで「Markdown Preview Enhanced」を利用するほうが便利。ただし、(1) inlineのつもりの数式がdisplayed（別行立て）になってしまう、(2) KaTeXを利用しているため一部コマンドでparse errorが出る。
  * .md ファイルは（sjisではなく）UTF-8N (BOMなし) で保存
  * 目次はVSCodeの「Markdown TOC」で生成。謎の「auto」が出現する場合は[【VSCode】Markdownで書いたブログに目次を付ける！（Markdown TOC）](https://dev.classmethod.jp/articles/vscode-markdown-toc-for-blog/)にあるとおり改行コードを変更。
* build, commit, push
  * PowerShellに直接打ち込んで実行してもよいが、[「PowerShell のスクリプトファイルを作成してバッチ実行する方法
」](https://www.projectgroup.info/tips/Windows/PowerShell_0001.html)のように .ps1 を実行するバッチファイルを作成すると便利

````PowerShell
cd c://dir

git remote add origin https://github.com/kurodaecon/stat.git
npm install mathjax@2.7.6
gitbook install

gitbook build . docs
git add .
git commit -m "m"
git push origin master
````

## TeX

```TeX
\mathbf{X} \boldsymbol{\beta}
 = \begin{bmatrix}
 a_{11} & \cdots & a_{1m} \\
 \vdots & \ddots & \vdots \\
 a_{n1} & \cdots & a_{nm}
\end{bmatrix}
```

$$ \mathbf{X} \boldsymbol{\beta} = \begin{bmatrix} a_{11} & \cdots & a_{1m} \\ \vdots & \ddots & \vdots \\ a_{n1} & \cdots & a_{nm} \end{bmatrix} $$

```TeX
\begin{align}  % KaTeX parse error が出るのでVSCodeではうまく表示されないが、、
 a & = b \\
 cde & = f
\end{align}
```

$$ \begin{align} a & = b \\ cde & = f \end{align} $$

```TeX
y = \begin{cases}
 1 \quad \text{if} \ z > 0 \\
 0 \quad \text{if} \ z \le 0
\end{cases}
```

$$ y = \begin{cases} 1 \quad \text{if} \ z > 0 \\ 0 \quad \text{if} \ z \le 0 \end{cases} $$
