参考資料

- https://park.itc.u-tokyo.ac.jp/tajika/class/kiso2/fourier1.pdf
- https://gochikika.ntt.com/Visualization_and_EDA/auto_cross_corr.html

フーリエ変換を考える前に自己相関関数というものについて考えます.名前の通り自分との相関を示す関数で今 $x(t)$ という関数について自己相関関数は

$$
C(t, \tau) = E[x(t)x(t+\tau)]
$$

と定義されます.CはcorrelationのCです.また$\tau$をラグと呼びます.(どれだけ遅れているかを表しています.)ここで$x(t)x(t+\tau)$がでてきましたが$x(t)$という信号を$\tau$という時間だけずらしたものと元の関数$x(t)$との積をとることを意味しています.$E[...]$は母集団平均と呼ばれるもので例えば信号$f(t)$に対して$N$回サンプルを観測するとして

$$
E[f _ k(t)] = \lim _ {N \rightarrow \infty} \frac{1}{N} \sum _ {k=1} ^ {N} f _ k(t)
$$

と定義されます.しかしこの母集団平均が時間に依らないとき(いつでも同じ結果が出るとき)

$$
E[f _ k(t)] = \lim _ {T \rightarrow \infty} \frac{1}{T} \int _ {-\frac{T}{2}} ^ {\frac{T}{2}} f _ k(t)dt
$$

のように積分の形で書けます.(**時間平均で考えられる.**)つまりこのように書けるとき,自己相関関数は

$$
C(\tau) = \lim _ {T \rightarrow \infty} \frac{1}{T} \int _ {-\frac{T}{2}} ^ {\frac{T}{2}} x(t)x(t+ \tau)dt
$$

となり時間に依らないです.(ラグ$\tau$のみに依存.)

$x(t)$は周期をもたない関数で$-\frac{T}{2} < t < \frac{T}{2}$でのみ値を持つものとします.(この範囲以外は0.)

$x(t)$をフーリエ変換すると

$$
X(\omega) = \frac{1}{2\pi} \int _ {-\infty} ^ {\infty} x(t) e^{-i\omega t} dt
$$

ですので逆に考えると

$$
\begin{aligned}
    x(t+\tau)
    &=
    \int _ {-\infty} ^{\infty} X(\omega) e^{i\omega(t+\tau)} d\omega \newline
    &=
    \int _ {-\infty} ^{\infty} X(\omega) e^{i\omega t}e^{i\omega \tau} d\omega
\end{aligned}
$$

です.これを用いると自己相関関数$C(\tau)$は

$$
\begin{aligned}
    C(\tau)
    &=
    \lim _ {T \rightarrow \infty} \frac{1}{T} \int _ {-\frac{T}{2}} ^ {\frac{T}{2}} x(t)\Bigl(\int _ {-\infty} ^{\infty} X(\omega) e^{i\omega t}e^{i\omega \tau} d\omega\Bigr)dt \newline
    &=
    \lim _ {T \rightarrow \infty} \frac{1}{T} \int _ {-\infty} ^{\infty} X(\omega) e^{i\omega \tau} \Bigl(\int _ {-\frac{T}{2}} ^ {\frac{T}{2}} x(t) e^{i\omega t}\Bigr)d\omega
\end{aligned}
$$

と書けます.ここで$-\frac{T}{2} < t < \frac{T}{2}$でのみ値をもつので積分範囲を$-\infty < t < \infty$としても結果は同じです.よって

$$
\begin{aligned}
    \int _ {-\frac{T}{2}} ^ {\frac{T}{2}} x(t) e^{i\omega t} dt
    &=
    \int _ {-\infty} ^ {\infty} x(t) e^{i\omega t} dt
\end{aligned}
$$

とも書けます.上式の右辺はフーリエ変換のような形をしており中身の複素共役を考えることで$e^{i\omega t} \rightarrow e^{-i\omega t}$とできるのでちゃんとフーリエ変換の形にできます.つまり

$$
\begin{aligned}
    \int _ {-\infty} ^ {\infty} x(t) e^{i\omega t} dt
    &=
    \int _ {-\infty} ^ {\infty} (x^\ast(t) e^{-i\omega t})^\ast dt \newline
    &=
    \int _ {-\infty} ^ {\infty} x^\ast(t) e^{-i\omega t} dt \newline
    &=
    2\pi X^\ast (\omega)
\end{aligned}
$$

と書けます.これを用いれば

$$
\begin{aligned}
    C(\tau)
    &=
    \lim _ {T \rightarrow \infty} \frac{1}{T} \int _ {-\infty} ^{\infty} X(\omega) e^{i\omega \tau} (2\pi X^\ast (\omega))d\omega \newline
    &=
    \int _ {-\infty} ^{\infty} \lim _ {T \rightarrow \infty} \frac{2\pi}{T}  X(\omega)X^\ast (\omega) e^{i\omega \tau} d\omega \newline
    &=
    \int _ {-\infty} ^{\infty} \lim _ {T \rightarrow \infty} \frac{2\pi}{T}  \vert X(\omega) \vert^2 e^{i\omega \tau} d\omega
\end{aligned}
$$

となります.ここで積分の中身の指数以外の項

$$
\lim _ {T \rightarrow \infty} \frac{2\pi}{T}  \vert X(\omega) \vert^2
$$

をパワースペクトルと言い, 通常$S(\omega)$と書きます.

$$
\boxed{S(\omega) = \lim _ {T \rightarrow \infty} \frac{2\pi}{T}  \vert X(\omega) \vert^2}
$$

<!--ので自己相関関数のフーリエ変換は

$$
\begin{aligned}
X _ c(\omega)
&=
\frac{1}{2\pi} \int _ {-\infty} ^ {\infty} \Bigl( \lim _ {T \rightarrow \infty} \frac{1}{T} \int _ {-\frac{T}{2}} ^ {\frac{T}{2}} x(t')x(t'+ \tau)e^{-i\omega t'}dt'\Bigr)  dt \newline
&=
 \lim _ {T \rightarrow \infty} \frac{1}{T} \int _ {-\frac{T}{2}} ^ {\frac{T}{2}} \Bigl(\frac{1}{2\pi}\int _ {-\infty} ^ {\infty} x(t')x(t'+ \tau)e^{-i\omega t'}dt' \Bigr) dt \newline
\end{aligned}
$$

これは同じ関数のたたみこみのフーリエ変換であるので $t'$ についての積分は

$$
\frac{1}{2\pi} \int _ {-\infty} ^ {\infty} x(t')x(t'+ \tau)e^{-i\omega t'}dt' = X(\omega)
$$ です -->

<div style="page-break-before: always;"></div>

## DFT

<div style="page-break-before: always;"></div>

## FFT

### 行列からの理解

- https://qiita.com/51_24_11_/items/11c48395603670ea98d8
- https://taketake2.com/N80.html

<div style="page-break-before: always;"></div>

### バタフライ演算

- https://qiita.com/kob58im/items/c082a7904926aa8479e4

<div style="page-break-before: always;"></div>
