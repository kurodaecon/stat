# �{�T�C�g�̍\�z

�uGitBook�v�ɂ��Markdown�����HTML����GitHub��Ɍ��J

* �����ɁAr-ngtm����́u[GitBook��Web�T�C�g�������GitHub Pages�Ō��J������@](https://r-ngtm.hatenablog.com/entry/2020/06/18/193235)�v�i2020-06-18�j���Q��
* Windows����PowerShell���g�p
 * `npm init` �ł��Ȃ��ꍇ�APowerShell���Ǘ��҂Ƃ��Ď��s�B`Set-ExecutionPolicy Unrestricted` �Ŏ��s�|���V�[��ύX
* `TypeError: cb.apply is not a function` �� [�unode.js�̃o�[�W�W��������������G���[�������v�Ƃ̋L�q](https://teratail.com/questions/279576)���������̂ŁA�ŐV�ł��A���C���X�g�[�����Ă���A[�����Â��o�[�W����](https://nodejs.org/ja/download/releases/)���ăC���X�g�[��
* `Error: ENOENT: no such file or directory, * '* \gitbook\gitbook-plugin-fontsettings\fontsettings.js'` �� [GitBook �� 'no such file or directory' �Ƃ����G���[���\�������](http://kuttsun.blogspot.com/2018/06/gitbook-no-such-file-or-directory.html)
* [Git�̃C���X�g�[��](https://notepad-plus-plus.org/downloads/)
 * �G�f�B�^��[Notepad++](https://notepad-plus-plus.org/downloads/)�Ȃǂɂ��Ă���
 * `git commit` �� `Author identity unknown` �ƌ���ꂽ��A�w�����ꂽ�Ƃ���ɓ���

* [MathJax](https://github.com/GitbookIO/plugin-mathjax)���Y��Ȑ������\���ł���
 * [�u�R�[�h�u���b�N�̌���w��� "math" ���w�肷�邱�Ƃ�TeX�L�@��p���Đ������L�q�v](https://qiita.com/Qiita/items/c686397e4a0f4f11683d#%E6%95%B0%E5%BC%8F%E3%81%AE%E6%8C%BF%E5%85%A5)�Ƃ̏�񂠂�i�ڍז��m�F�j
* Markdown�̍쐬
 * Chrome��Firefox�̊g���@�\�uMarkdown Viewer�v�𗘗p���ăv���r���[

````
cd c://dir

git remote add origin https://github.com/kurodaecon/stat.git
npm install mathjax@2.7.6
gitbook install

gitbook build . docs
git add .
git commit -m "comment"
git push origin master
````