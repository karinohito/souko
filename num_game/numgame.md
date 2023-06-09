  
質問回数制限以内に秘密の数を当てることができたでしょうか?   
毎回確実に当てられた、何回かやって運よく当たった、当たられなかった等々の方がいると思いますが、実はどんな数が用意されていても10回以内に当てる方法が存在します。  

ここでは「二分探索」と呼ばれるその方法を紹介します。  
その解法に至るまでの過程も合わせて紹介するので、解法だけを先に知っておきたい方は下の方の"解法まとめ"という節をご覧ください。  

## 方法1. 愚直に順に質問するのはどうか?
まずは $1,2,\dots,1000$ を順番に質問する方法について考えてみましょう。  
もし用意されていた数が $3$ なら見事たった $3$ 回目の質問で当てることができます。  
しかし、もし用意されていた数が $1000$ であれば、それを当てるまで $1000$ 回も質問しなければなりません。  
目標質問回数を遥かに越えてしまい、効率が良い方法とは言えまなさそうです。  
  
### なぜ効率が悪いのか? 
効率が悪い原因を考える前に、このゲームの質問では何を行っているかを考えてみましょう。  
当てるべき数を $x$ として、以下の通りに質問及び返答があったとします。  

| 質問 | 返答 | 
| --- | --- | 
| $2$ | $2<x$ |
| $7$ | $7>x$ | 
| $4$ | $4>x$ |

この時、$x$ は $2<x$ かつ $7>x$ かつ $4>x$ を満たす数ということになります。  

もう少し具体的に見てみましょう。  
秘密の数 $x$ が、$1$ 以上 $10$ 以下の中から選ばれるとします。  
質問する前は、$1$, $2$, $\dots$, $10$ の全てが答えの候補です。  
この候補が質問によってどう変化するかに注目してみます。  

| 質問 | 返答 | 答えの候補 |
| --- | --- | --- |
||| $1,2,3,4,5,6,7,8,9,10$ |
| $2$ | $2<x$ | $3,4,5,6,7,8,9,10$ |
| $7$ | $7>x$ | $3,4,5,6$ |
| $4$ | $4>x$ | $3$ |

この通り、このゲームでは質問することによって答えの候補を絞り、候補が $1$ つになったらクリアと思う事ができます。  
これを踏まえて、先ほどの効率の悪い質問を見てみましょう。  

以下は、答えが $10$ である場合の応答例です。 

| 質問 | 返答 | 秘密の数の候補 |
| --- | --- | --- |
||| $1,2,3,4,5,6,7,8,9,10$ |
| $1$ | $1<x$ | $2,3,4,5,6,7,8,9,10$ |
| $2$ | $2<x$ | $3,4,5,6,7,8,9,10$ |
| $3$ | $3<x$ | $4,5,6,7,8,9,10$ |
|$\vdots$|$\vdots$|$\vdots$|
| $8$ | $8<x$ | $9,10$ |
| $9$ | $9<x$ | $10$ |

これはいかにも効率が悪いですね。  
質問 $1$ 回に対して、候補を $1$ つずつしか減らせていません。  

何とかして一度に候補をたくさん減らせる質問方法は無いでしょうか?  
次節ではその方法を考えていきます。

## 方法2. 効率の良い質問方法~二分探索~
前節の観察から、$1$ 回の質問で候補をたくさん減らすことを目標にすれば良さそうです。  

とはいえ、いきなり全ての質問について考えるのは大変なので、最初の一回の質問について考えてみましょう。  
今回も最初の答えの候補が $1,2,3,4,5,6,7,8,9,10$ であったとします。  
例えば、最初に $1$ と質問したとすると、答えが $2\sim 10$ だった場合、答えの候補は $1$ つしか減りません。  
それでは思い切って、候補内最大の $10$ と質問したとすると、答えが $1\sim 9$ だった場合、やはり答えの候補は $1$ つしか減りません。  

ここで大事なのは、答えが何であっても答えの候補が十分に減らせる質問をすることです。  
結論から述べると、これは候補の真ん中 ( $5$ か $6$ ) を質問すれば良いです。  

最初に $5$ と質問したとすると、答えが $1\sim 4$ だった場合、答えの候補は $10$ 個から、 $4$ 個まで減ります。  
答えが $6\sim 10$ だった場合、答えの候補は $10$ 個から、 $5$ 個まで減ります。  
つまり、いずれの場合でも、答えの候補を $5$ 個以下にすることが出来ます。  

中央値付近を質問するのが最適というのが、直感的になるほど確かにとなった方はそれで良いでしょう。  
そうでない場合、$1,2,\dots,10$ を質問した場合に答えの候補がどうなるかをそれぞれ具体的に調べてみることで最適である事実が確かめられます。  
あるいは、式の扱いに慣れている方は $y$ と質問した時に、残る答えの候補が最大で $\max(y-1,10-y)$ となる事を用いて示すことも出来ます。  

ともかく、答えの候補の中央値辺りを質問すれば、$1$ 回の質問で答えの候補を確実に大体半分にすることができます。  

それでは次に、$2$ 回目の質問を考えてみましょう。  
ここで、もう一つ観察や証明から分かる事として、質問後の答えの候補は連続した整数で表されます。($\{3,4,5,6\}$ や $\{8,9,10\}$ 等)  
つまり、$1$ 回目の質問の時と同様に、その時点の答えの候補の中央値を質問すれば、$2$ 回目の質問でさらに答えの候補を半分にすることが出来ます。  

これを繰り返すことで、答えの候補を効率よく減らすことができます。  
つまり、効率の良い質問方法とは、
```
毎回答えの候補の中央値 (中央値が整数でない場合はそれに近い整数) を質問する
```
となるのです。  

具体例を見てみましょう。  

以下は答えが $7$ である場合の応答例です。  
| 質問 | 返答 | 秘密の数の候補 | 候補の中央値に近い整数 |
| --- | --- | --- | --- | 
||| $1,2,3,4,5,6,7,8,9,10$ | $5,6$ |
| $5$ | $5<x$ | $6,7,8,9,10$ | $8$ |
| $8$ | $8>x$ | $6,7$ | $6,7$ |
| $6$ | $6<x$ | $7$ | 

愚直に試す場合に比べて、候補の減り方が早い事が分かります。 

## 解法まとめ

解法をまとめてみましょう。

1. 初め、答えの候補は $1,2,\dots,1000$ である. 
2. 答えの候補が $1$ つになるまで、以下のように質問を繰り返す.  
"答えの候補の中央値(ただし中央値が整数にならなければそれに最も近い整数)を $M$ とし、$M$ を質問する."   
この質問の結果によって、答えの候補を更新する.

$1$ 回の質問で答えの候補は(ほぼ)半分になっていきます。
よって、$1$ 回目の質問の後は答えの候補は $500$ 個、$2$ 回目の後は $250$ 個、$\dots$ となり、およそ $10$ 回の質問で答えの候補を $1$ つに絞ることができ、ゲームにクリアすることができます。  

## 図で比較
以下は $2$ つの質問方法を行ったときの様子です.  

![線形探索](liner_search.gif)
![二分探索](bi_search.gif)
  
上が最初に考えた、$1,2,\dots,1000$ を順番に質問する方法、下が後から考えた候補を半分ずつにしていく方法です。  
赤い部分が答えの候補で、毎回の質問での候補を更新しています。双方で、各質問後の候補の様子を表す画像の表示時間は同じです。 つまり、赤いバーの減る速さが質問回数の少なさを表しています。  

## 競技プログラミングでの応用について
今回のゲームのように、答えの候補を真ん中で区切り、答えがそれ以上か以下かを判定することによって効率よく答えを求める方法の事を「二分探索」と言います。

愚直に答えを求めるのは大変でも、判定だけなら簡単に出来るというのがピンと来ないかもしれませんが、これが意外にも度々目にするのです。  

例えば、以下のような形式がよく見かけます。  

問題文
--------
小さい順に並んだ $10^6$ 個の $10^9$ 以下の正整数の列 $a_1,a_2,\dots,a_{10^6}$ が与えられます。  

質問が $10^6$ 個与えられるので、答えてください。  
質問内容は以下の通りです。  
- 各質問では、正整数 $x$ が与えられる。正整数の列 $a_1,a_2,\dots,a_{10^6}$ の要素で、$x$ 以下の数は何個あるか?

一見すると各質問に対して、数列の全ての要素を調べて数を数える事で答えが求まるような気がします。  
実際、それは正しいです。しかし、それをすると数列の要素を合計で (質問の数) $\times$ (数列の長さ) $=10^6\times10^6=10^{12}$ 回確かめる必要が出てきます。  
いくらコンピュータと言えど、この回数の確認にはかなりの時間を要してしまいます。  

このような計算回数を削減するよう考えるのも競技プログラミングの醍醐味の一つです。  
今回の問題の場合、解決方法は色々ありますが、その一つに「二分探索」を用いるものがあります。  

問題文を観察すると、数列が小さい順に並んでいると書かれています。  
つまり、$a_i>x$ となる最小の $i$ が分かれば、$a_1,a_2,\dots, a_{i-1}$ は $x$ 以下、$a_i,\dots,a_{10^6}$ は $x$ より大きい、すなわち、$x$ 以下の個数は丁度 $i$ 個であるとわかります。  
この最小の $i$ を求めるのに,「二分探索」を用います。  

初め、最小の $i$ の候補は $1,2,\dots,10^6$ の全てです。  
ここから、候補の中央値を $M$ として、$a_M$ が $x$ より大きいか、以下かを判定して、候補を狭くするというのを繰り返します。  
すると、ゲームと同様に効率良く答えを求めることが出来るのです。  

一回で候補が半分になるので、この判定は大体 $\log_{2}^{10^6}\risingdotseq 20$ 回だけ行えば良く、
(質問の数) $\times$ (各質問での判定の回数) $10^6\times20=2\times10^{6}$ 回の計算で全ての質問に対する答えを求めることができます。 これは、通常のコンピュータでもすぐに計算が終了します。  

## おまけ 先ほどの問題を解くプログラミングのコード

### C++というプログラミング言語で解いたもの

```c++
#include<bits/stdc++.h>
using namespace std;
//ここより上はおまじないのようなもの

int main(){
    
    int N=1000000,Q=1000000;//数列の長さと質問の数
    vector<int> A(N);//与えられる数列を格納する
    for(int i=0;i<N;i++)cin>>A[i];//数列を受け取る
    for(int q=0;q<Q;q++){
        int x;//質問で与えられる整数
        cin>>x;
        int L=0,R=N;
        //初め, iの候補が0以上N未満であることを表す. (プログラミングでは数列がa_1,a_2,...でなくa_0,a_1,...と表されることが多い)
        while(R-L>1){//候補が2つ以上ある間繰り返す
            int M=(R+L)/2;//候補の中央値をMとする
            if(A[M]>x)R=M;//Mがxより大きければ, iの候補を L以上M未満 に更新
            else L=M; //Mがx以下ならば, iの候補を M以上R未満 に更新
        }
        cout<<R<<"\n"; //Rを出力する
    }
}
```
### Pythonというプログラミング言語で解いたもの
```Python
N, Q = 10**6,10**6
a = list(map(int, input().split()))
x = list(map(int, input().split()))
for q in range(Q):
    l, r = 0, N
    while r - l > 1:
        m = (l + r) // 2
        if a[m] > x[q]:
            r = m
        else:
            l = m
    print(r)
```

## 最後に
今回の解説は以上になります。  
ここまで読んでいただきありがとうございました。  