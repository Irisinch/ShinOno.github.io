---
title: edgerまとめ
summary: edgerについて細かな解説がなかったので。。。
date: 2024-03-13
authors:
  - admin
tags:
  - bioinfomatics
  - DEG
---

edgeRはRNA-seq, SAGE-seq, ChiP-seq, shRNA-seq, CRISPR-Cas9スクリーニングにも使えるRパッケージである。
今回は、RNA-seqでDEG解析を行うことを前提に解説する。
なお、内容はほぼチュートリアルの日本語訳である。

### インプットファイル
はじめに、edgeRのインプットファイルとして、マッピング後のリードカウントファイル(csv, tsvなど)が必要である。
このとき、マッピングはエキソンレベルでも、遺伝子レベルでも大丈夫。
マッピングツールとして、RsubreadのfeatureCountsが推奨されていた。（理由は不明.STARより高性能という論文は見たことがある。）
遺伝研利用者であれば、[Rhelixa RNAseqパイプライン](https://sc.ddbj.nig.ac.jp/advanced_guides/Rhelixa_RNAseq/)を使っても良いと思う。
また、このとき、行が遺伝子、列がサンプルとなっている必要がある。必要に応じてt()をつかおう。
後述するかもしれないが、リードカウントファイルは整数である必要がある。*RPKMやFPKMではいけない。*事前に補正する際には注意。

リードカウントファイルができたらそれをRに読み込む。このとき、read.delimでもread.csvでもread.tableでも構わない
'''
df <- read.table("**.txt",header = TRUE,check.names=F)
'''
サンプルごとに異なるファイルになっている場合、readDGEが使える。
このとき、それぞれのファイルの形式は、1列目に遺伝子名、2列目がカウントとなっている必要がある。
また、全サンプルの名前が入ったdfを入れることもできる。
'''
df <- readDGE(file_name_vector)
'''
または
'''
files <- dir(pattern="*\\.txt$")
df <- readDGE(files)
'''

### 形式変換
edgeRおよび、遺伝子発現量解析ツールでは、データを独自の形式に変換する。
edgeRでh、DEGListを用いるが、EDASeqパッケージのSeqExpressionSetクラスも受け付ける。
'''
y <- DGEList(counts=df)
'''
このとき、グループを与えることもできるが、手動のため注意が必要。
'''
group <- c(1,1,2,2)
y <- DGEList(counts=df, group=group)
'''

### フィルタリング
全サンプルにわたってカウントが少ない遺伝子は、後の補正に悪影響を与えるためフィルタリングする必要がある。
見た目がわかりやすい方法では、
'''
keep <- rowSums(y$counts > 10) >=2
y <- y[thresh,,keep.lib.sizes=FALSE]
'''
このコードでは、最低2サンプルで10カウント以上ある遺伝子のみを保持するようにする。

なお、edgeRには、filterByExpr関数がある。
'''
keep <- filterByExpr(y,min.count = 10, large.n = 2)
y <- y[keep, , keep.lib.sizes=FALSE]
'''
filterByExprには、他にも[様々な引数](https://rdrr.io/bioc/edgeR/man/filterByExpr.html)がある。
組み合わせるとかなり便利に使えそう。

### 正規化
DEG解析では、発現レベルの定量化が目的ではないため正規化は少なくて済む。
具体的には、遺伝子長による正規化などは、比較対象の両方で同じ影響をもたらすと考えられるため必要ない。

#### ライブラリサイズ
ライブラリサイズの正規化として[TMM正規化](https://bi.biopapyrus.jp/rnaseq/analysis/normalizaiton/tmm.htmlを行う関数が用意されている。
'''
y <- calcNormFactors(y)
'''
method=c("TMM","RLE","upperquartile","none")で正規化の手法が選べる、

#### GC含有量
GC含有量がサンプル特異的に影響することが最近判明したらしい。
正規化としてはm[EDASeq](https://bioconductor.org/packages/release/bioc/html/EDASeq.html)や[cqn](https://bioconductor.org/packages/release/bioc/html/cqn.html)パッケージが紹介されている。

#### 遺伝子長
GC含有量ほどではないものの、遺伝子長の影響もあるらしい。
特に、方法は示されていなかったが、EDASeqで可能である。

#### その他の正規化
edgeRのglm関数でその他の影響を考慮した正規化が可能であるらしい。


### サンプル間比較

#### 2群間比較
EdgeRでは、単一要因によるDEG解析ではqCML法(the quantile-adjusted conditional maximum likelihood)を用いる。
'''
y <- estimateDisp(y)
'''
'''
et <- exactTest(y) # 比較
topTags(et) # 結果の表示
'''

#### 複雑な比較

 y <- estimateDisp(y, design)とすることで、複雑な因子を考慮することが可能

 