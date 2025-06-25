# Microeconometrics
These are presentation slides used in a coursework of Microeconometrics at Graduate School of Economics, Kyoto University.

2025年に京都大学大学院経済学研究科で柳貴英准教授(当時)が担当してくださった「ミクロ計量経済学」で私が作成・使用したスライド

・授業内容：経済学または統計学のtop-journalに載っている論文のreview

・reviewed papers

1. Susan Athey, Mohsen Bayati, Nikolay Doudchenko, Guido Imbens, and
 Khashayar Khosravi (2021), Matrix Completion Methods for Causal Panel Data
 Models, Journal of the American Statistical Association.

https://www.tandfonline.com/doi/full/10.1080/01621459.2021.1891924

matrix completionとは、行列補完法で、行列の要素間の相関（factor構造）をimplicitに仮定することで、その関係から欠損値を浮かび上がらせる（行列が収束する＝implicitなfactorの関連度合いに辻褄があった状態）

https://www.youtube.com/watch?v=MYKb5KcI55s&t=81s

その意味で行列内の相関構造を余すことなく記述するわけであるから、penalized synthのように、synthetic controlの数を減らすのではなく、逆にすべてを利用してきれいに記述しようとする取り組みのように思える（coherenceの意味で）。（これに関してはCandes and Recht(2009)あたりのガチ数学論文を読んで本質を理解するしかない。）そもそもpenalized synthにおける正則化は推定の良さのためのもので、構造をとらえるための本質ではないためmatrix completionのほうが、もっと後の方で正則化しておりより解釈的にいいのではないか？

→model-based factor modelにおいて最も抽象的かつ大域的な推定方法ではないか。

DiDのcounterfactual matrixなんて縦にも横にも相関だらけなんだから、いい感じに補完はできそう。とても相性がいい。そして、staggered adoptionであればあるほど当てはまりがよさそう（＝これは$\mathcal{O}$の数が多ければ多いほど当てはまりがいいということみたいなお話だが、ただサンプルが増えるわけでなく縦横の関係を色濃く増やすからもっと嬉しいお話。）

問題点としては、Lのところにfixed effectをいれて固定化するかしないかの話。Atheyは固定化部分にcovariateも入れようとしているが、Choi and Yuanは入れずそのままLを推定しようとしている。この固定部分についてはサンプル数と収束速度の関係が効いてくるように思える。これも元論文をあたるしかない。empiricalな感覚が必要なのかなぁ。factor testみたいなんないかなぁ。

あと、incoherenceについても、特異ベクトルが同質的であってほしい、という条件は、FEにおいては特異ベクトルが単位行列になったりしてまあうれしいってのもあるけど、individual effect側の左特異ベクトルはいろいろバラバラじゃね？その意味では、FEをできるだけ同質にするため、Choi and Yuanみたいにtreatment groupごとに行列補完する部分を分割してあげるほうが推定精度がよくなりそう？

→そもそもincoherenceという条件が、どういう意味（数式関係）で収束とかに効いてくるか本質をつかまないと何も始まらん。

Soberingだったのが、逆にtreatment indiceの行列についても補完することができて、ATEなどもdata次第では推定できるのではないか、との話で、すごく可能性を感じた。しかし、通常のデータ状況の補完だと、missingの初期値を0にしたらcomputational errorがでてしまいそう。なぜなら、0が多いと、特異値が0になり、最小化問題の解になり続けるので。だとすると、初期値はmean imputation?（これもガチ数学論文で本質を理解する必要がある。）

余談：説明がへたくそすぎる。人と喋ってない弊害出まくり。自分の頭の中だけで完結しており、上記のような本質的な階層構造に言及できず、質問に答える中で分かってしまった。理解が浅い。同じことがこのReadMeにも言えてナニコレというお気持ち。Twitterのつぶやきじゃないねん

2. Victor Chernozhukov, Mert Demirer, Esther Duflo, and Iván Fernández-Val (2024), Generic machine learning inference on heterogeneous treatment effects in randomized experiments, with an application to immunization in India, Econometrica.

https://economics.mit.edu/sites/default/files/2022-08/2020.12%20HeterogTE-RE-v26.pdf
