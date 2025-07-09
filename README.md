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

あと、incoherenceについても、特異ベクトルが同質的であってほしい、という条件は、FEにおいては片側の特異ベクトルが単位行列になったりしてまあうれしいってのもあるけど、individual effect側の左特異ベクトルはいろいろバラバラじゃね？その意味では、FEをできるだけ同質にするため、Choi and Yuanみたいにtreatment groupごとに行列補完する部分を分割してあげるほうが推定精度がよくなりそう？

→そもそもincoherenceという条件が、どういう意味（数式関係）で収束とかに効いてくるか本質をつかまないと何も始まらん。

Soberingだったのが、逆にtreatment indiceの行列についても補完することができて、ATEなどもdata次第では推定できるのではないか、との話で、すごく可能性を感じた。しかし、通常のデータ状況の補完だと、missingの初期値を0にしたらcomputational errorがでてしまいそう。なぜなら、0が多いと、特異値が0になり、最小化問題の解になり続けるので。だとすると、初期値はmean imputation?（これもガチ数学論文で本質を理解する必要がある。）

余談：説明がへたくそすぎる。人と喋ってない弊害出まくり。自分の頭の中だけで完結しており、上記のような本質的な階層構造に言及できず、質問に答える中で分かってしまった。理解が浅い。同じことがこのReadMeにも言えてナニコレというお気持ち。Twitterのつぶやきじゃないねん

2. Victor Chernozhukov, Mert Demirer, Esther Duflo, and Iván Fernández-Val (2024), Generic machine learning inference on heterogeneous treatment effects in randomized experiments, with an application to immunization in India, Econometrica.

https://economics.mit.edu/sites/default/files/2022-08/2020.12%20HeterogTE-RE-v26.pdf

Machine learning method-based CATE estimationというのは、基本的に一致性を満たさず（高次元において）、inferenceが複雑（特に一様な推論は厳しい）ということが問題点に挙げられる。その問題を1段階目の推定で、MLの点推定としての強みを生かして、CATEを大域的に推定し、2段階目で、overfitした部分を線形射影によって、バラツキを吸収しつつ推定することで、ある種、複雑度が小さくなり（VC-dimension, Rademacher complexity）、一致性を満たすようになる。（ほんでここ嘘ついてもうたやないかい、線形回帰のほうがより推定量の最終的な意味での一致性（これはriskが漸近的に小さくなることであり、misspecified modelだと厳密な意味での一致性にはならないことに注意！）に対して本質的、やからあの最初のfigureがあってん）

さらにinferenceの部分に関しては、線形回帰推定量のGaussian approximationによって通常通りサンドウィッチの分散でCIが容易に作れる。（うれしい）

2段階推定の弱みであるsample splitのuncertaintyに対しては、推定量をpermuateしてそのmedianを取ることで解消され、それはまたOracle propertyを満たす。（これも一致性に関わってくるが、consistencyに代わって重要視されている）

なんか機械学習のメソッドってoverfittingの問題が結構怖いんかな。もしそれが恐ろしいレベルで起こってるなら、この2段階推定は画期的ではある。話にもでたけど、実務家への売り込みではcausal forestのように異質性がvisualizeされず、ただ定量化されるだけだからウケが悪そう。でもこうするしかないもんな。CLANの方に期待するしかないね。

余談：話もシンプルで全部説明できた。今回軽めのスライド20枚でちょうど40分使ったので、前回レベルの重みのスライドだと10枚くらいが限度ではないか。それにしても話が冗長で涙止まらん
