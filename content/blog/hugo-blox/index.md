---
title: Hugo Bloxを使って個人サイトを作成する
summary: このホームページの作成方法の紹介
date: 2024-03-13
authors:
  - admin
tags:
  - Hugo
  - Github
  - Theme
---

### はじめに
このページではHugoのテーマであるHugo bloxを使った個人サイトの作り方を紹介する。<br>
Hugoで個人サイトを作るときの大きな悩みどころの1つがどのテーマを用いるかである。<br>
テーマの人気ランキングを見るとよく目に入るのがAcademicであり、現在AcademicはHugo bloxに統合されている<br>
Hugo Bloxは（特にAcademicは）人気のテーマである一方で、作者のチュートリアルに従うと他と異なるステップで設定する必要がある。<br>
この記事では、Hugo Bloxの作者に従った方法でサイトを作成する。<br>
そのため、より汎用的なHugoの使い方が知りたいなら偉大な先輩の[Hugoの記事](https://ymat2.github.io/site/hugo.html)やMetalさんの[記事](https://heavywatal.github.io/misc/hugo.html)を参考にしていただきたい。<br><br>


### 事前準備
今回の方法では、事前にGitHubアカウントが必要になる。<br>
また、個人的にGithubアカウントと連携したVScodeの使用をお勧めする。<br><br>


### テーマ選び
Hugo Bloxの[テンプレート一覧](https://hugoblox.com/templates/)から好きなもの選ぶ。Proと付いているものは有料。<br>
[私のサイト](https://irisinch.github.io/ShinOno.github.io/)ではBlogを選んだ。<br>
ここから先は、Hugo Bloxの[tutorial](https://docs.hugoblox.com/tutorial/blog/)に従って進める。<br>
選んだテンプレートのEditを押すと、新しいレポジトリを作成する画面に移行する。![screen reader text](Edit.png)
![screen reader text](new-repository.png)<br>
このとき、レポジトリの名前を**.github.ioにする。<br>
また、作成したレポジトリのSettings欄でPagesの設定を変更する必要がある。
![screen reader text](github-actions.png)<br>
ここで、Build and deploymentのSourceをGitHub Actionsに変更しておく。<br><br>

### トップページの編集
ここから先、tutorialでは、レポジトリのcodeボタンからCreate codespace on mainを選んで、編集していく。<br><br>
今回は、VScodeで進めていく。<br>
VScodeのSource Control欄、Clone Repository → GitHubから複製→ 作成したレポジトリを選択し、レポジトリを開く。
![screen reader text](vscode.png)<br>
あとは、[tutorial](https://docs.hugoblox.com/)に従って編集するだけ。<br>

### 所感
Hugo Bloxテーマはtutorialやdocumentが充実している一方で、一般的なテーマと比べ不自由さも感じる。<br>
特に、、テーマの切り替えが手軽にできないのがHugoの良さを消してしまっている気がする。<br>
また、cssを使うことで構成をさらに変えることができるらしいので、チャレンジしたい。<br>
Hugo Bloxのいじる上で、山本さんのホームページが参考になったので紹介しておく。（[サイト](https://r9y9.github.io/)、[コード](https://github.com/r9y9/website/tree/master)）

