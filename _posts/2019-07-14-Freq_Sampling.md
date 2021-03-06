---
title: 주파수 샘플링과 DFT
sidebar:
  nav: docs-ko
aside:
  toc: true
key: 20190714
tags: 신호처리
---

{% raw %}

## 주파수 샘플링 하는 목적에 대해서

 우리는 지금까지 Analog Signal을 시간 영역에서 sampling 하여 Digital Signal로 변환시키고, Sample된 Digital Signal을 어떻게 하면 다시 Analog Signal로 원상 복구 시킬 수 있는지에 대해서 알아보았다. 이것은 일상생활에서 쉽게 접할 수 있는 아이디어로부터 출발하기 때문에 Nyquist Frequency Theorem이나 Ideal reconstruction의 개념은 그 필요성의 출발이 충분히 납득할 수 있는 것이었으리라 생각한다.

 하지만 주파수는 왜 샘플링 하여야 하는가? 그것은 모든 Digital System은 이산화 되어 있기 때문이다. 즉, Discrete Time Signal을 이용하여, 또는 CT signal을 sample하여 그 신호의 주파수 응답을 분석해보는 것은 마치 컴퓨터에서 가능 할 것 처럼 보이지만, 실제로 분석하고자 하는 DT Signal이 주기신호가 아니라면 그것은 불가능하다. 그것은 DTFS와 DTFT가 정의된 수식에서 충분히 그 근거가 드러난다.
 

 
| DEFINITION: Discrete Time Fourier Series (DTFS) |
| --------- |
| 주기 이산 신호에 대하여,<br><center> $$x[n] = \sum_{k=N_1}^{N_2}a_k exp\left(j\frac{2\pi k}{N}n\right)$$<br>where<br>$$a_k=\frac{1}{N}\sum_{n=N_1}^{N_2}x[n] exp\left(-j\frac{2\pi k}{N}n\right)$$</center>|

| DEFINITION: Discrete Time Fourier Transform (DTFT) |
| --------- |
| 이산 신호에 대하여,<br><center> $$x[n] = \int_{-0.5}^{0.5}X(f) exp(j2\pi fn) df$$<br>where<br>$$X(f)=\sum_{n=-\infty}^{\infty}x[n]exp(-j2\pi fn)$$</center>|

 다시 한번 DTFT의 수식을 살펴보면 $x[n]$ 과 $X(f)$ 모두 연속주파수 $f$ 를 가지고 있다는 것을 알 수 있다. 그렇기 때문에 컴퓨터를 이용해 일반적인 이산신호 $x[n]$ 의 주파수 응답을 알아보고 싶다면 연속 주파수 $f$ 를 샘플링하는 과정은 필연적인 것이다.

## Sampling DTFT

 우리는 연속주파수를 sample하기 전에 몇 가지 사실에 대해서 짚고 넘어가야 할 필요가 있다. 그것은 우리가 컴퓨터를 통해서 받는 신호는 any discrete time signal $x[n]$ 이라고 하지만, 어쨋든 길이가 유한할 수 밖에 없다. 그렇기 때문에 우리는 컴퓨터에서 사용할 일반적 이산신호 $x[n]$ 의 길이를 $N$ 으로 정하도록 하자. 그렇기 때문에 DTFT $X(f)$ 는 다음과 같이 생각될 수 있다.


$$\hat{X}(f)=\sum_{n=0}^{N-1}x[n]exp(-j2\pi fn)$$

 이제, 연속 주파수를 이산 주파수로 바꿔보자. CTFT와 DTFT의 유도과정을 다시 생각해보면 이렇게 바꿀 수 있다는 것을 금방 알 수 있을 것이다. 연속주파수 $f$는 $\lim_{N\rightarrow\infty}{\frac{k}{N}}$ 에서 출발했기 때문이다. 그러므로,

 $$\hat{X}[k] = \sum_{n=0}^{N-1}x[n]exp\left(-j\frac{2\pi k}{N}n\right)$$

이제, 위의 $\hat{X}[k]$ 를 새로운 푸리에 변환인 Discrete Fourier Transform이라고 정의할 수 있다.

| DEFINITION: Discrete Fourier Transform (DFT) |
| --------- |
| 전체 신호의 길이가 N인 이산 신호 $x[n]$ 에 대하여,<br><center>$$X[k] = \sum_{n=0}^{N-1}x[n]exp\left(-j\frac{2\pi k}{N}n\right)$$ </center>|


이제, inverse Discrete Fourier Transform을 유도해보자. 지금껏 유도해왔던 Fourier Analysis Series와 마찬가지로 직교성(orthogonality)의 성질을 이용하면 된다.

---

`PROOF`{:.info} inverse DFT의 유도

DFT는 아래와 같이 정의되었다.

$$X[k] = \sum_{n=0}^{N-1}x[n]exp\left(-j\frac{2\pi k}{N}n\right)$$

두 정수 $p, n$ 에 대하여 다음과 같은 식을 생각하여 이산신호의 직교성의 성질을 이용하자.

$$\sum_{p=0}^{N-1}X[k] exp\left(j\frac{2\pi p}{N}k\right)$$

$$=\sum_{p=0}^{N-1}
\sum_{n=0}^{N-1}x[n]exp\left(-j\frac{2\pi k}{N}n\right)
exp\left(j\frac{2\pi p}{N}k\right)$$

$$=\sum_{p=0}^{N-1}\sum_{n=0}^{N-1}x[n] exp\left(j\frac{2\pi(p-n)}{N}k\right)$$

여기서도 CTFS에서 사용했던 방법과 마찬가지로 $p\neq n$, $p=n$ 의 두 가지 경우를 생각할 수 있다.


1) $p\neq n$인 경우,직교성의 성질에 의해

$$식(9) = \sum_{p=0}^{N-1}X[k] exp\left(j\frac{2\pi p}{N}k\right) = 0$$

이고,

2) $p=n$인 경우 직교성의 성질에 의해

$$식(9) = \sum_{p=0}^{N-1}X[k] exp\left(j\frac{2\pi p}{N}k\right) = Nx[n]$$

 이다.

그러므로, inverse DFT는

$$x[n] = \frac{1}{N}\sum_{k=0}^{N-1}X[k] exp\left(j\frac{2\pi k}{N}n\right)$$

이다.

## 예제를 통해  DFT를 조금 더 자세히 알아보자.

Ex) 다음의 이산신호 $x[n]$ 에 대하여 DFT $X[k]$ 를 구하시오.


$$x[n] =
\begin{cases}
1,  & n = 0, 1, 2, 3 \\
0, & \text{otherwise}
\end{cases}
$$

Sol)

DFT의 정의에 따라,

$$X[k] = \sum_{n=0}^{3} 1 \times exp\left(-j\frac{2\pi k}{4}n\right)$$

$$= 1 + exp\left(-j\frac{\pi}{2}k\right) 
+ exp\left(-j\pi k\right) + exp\left(-j\frac{3\pi}{2}k\right)$$

그러므로, $k=0$ 일 때, $X[0]=4$ 이고, $k= 1,2,3$ 일 때, $X[k]=0$ 이다.

---

 위와 같은 단순한 예제를 풂으로써 알 수 있는 것은 무엇일까? 바로 $x[n]$ 을 DTFT 한 결과물을 frequency domain에서 과연 sampling 했는지 알 수 있다는 것이다. 같은 문제를 가지고 DTFT를 구하면 다음과 같다.

---

ex2) 다음의 이산신호 $x[n]$ 에 대하여 DTFT $X(f)$ 를 구하시오.

$$x[n] =
\begin{cases}
1,  & n = 0, 1, 2, 3 \\
0, & \text{otherwise}
\end{cases}
$$

Sol)

$$X(f) = \sum_{n=0}^{3}\left(1\times exp({-j2\pi fn})\right)$$

$$=\frac{1-exp({-j8\pi f})}{1-exp({-j2\pi f})}$$

여기서 위 식은 다음과 같이 변형할 수 있다.

$$ = \frac
{\frac{exp({j4\pi f})-exp({-j4\pi f})}{exp({j4\pi f})}}
{\frac{exp({j\pi f})-exp({-j\pi f})}{exp({j\pi f})}}$$

$$ = \frac{exp({j\pi f})}{exp({j 4\pi f})} \times \frac{exp({j 4\pi f}) - exp({-j 4\pi f})}{exp({j \pi f}) - exp({-j \pi f})}$$

$$=exp\left(-j 3\pi f \right) \frac{\sin(4\pi f)}{\sin(\pi f)}$$

 Ex2의 답의 결과에서 주파수 $f$를 $\frac{k}{4}$로 치환함으로써 주파수를 이산화해보자. 그렇게하면, 각각의 $k=0,1,2,3$ 에 대하여 Ex1의 결과와 같은 결과물을 얻게 된다는 것을 알 수 있다.

 위 주파수의 이산화의 결과는 다음 그림을 통해서 확인해 볼 수 있다.

<p align="center">
  <img width = "400" src = "https://raw.githubusercontent.com/angeloyeo/angeloyeo.github.io/master/pics/freq_sampling_DFT/pic1.png">
</p>

##  주파수 샘플링된 주파수 응답의 시간 영역에서의 표현

 먼저, 시간을 샘플링 했을 때를 다시 생각해보자. 우리는 시간을 샘플링 했을 때에는 주파수 영역에서 주기성을 나타냈었다는 사실을 모두 증명했었다. 결과론적으로 얘기하자면, 시간과 주파수는 Dual Property를 가지고 있기 때문에, 주파수 영역에서 샘플링 한 신호는 시간 영역에서 주기성을 가지게 된다.
 $X(f)$의 연속 주파수 $f$ 를 $M$ 개로 Sampling한 신호를 $X_s[k]$ 라고 하자. 즉, $X_s[k]$ 는 이산 주파수 $f_k=\frac{k}{M}\space for \space k=0,1,\cdots,M-1$ 를 이용해서 표현될 수 있다. 이것은 Continuous Time Domain을 Sampling 할 때 이용한 기법을 이용해서 수학적으로 증명해 볼 수 있다.

$$X_s[k] = X\left(e^{j\frac{2\pi k}{M}}\right) = X(f) P(f)$$

<center>
where
</center>

$$P(f) = \frac{1}{M}\sum_{k=-\infty}^{\infty}\delta\left(f-\frac{k}{M}\right)$$

또한, 시간 샘플링 이론에서 증명되었던 것을 토대로 생각하면,

$$p[n] = \sum_{l=-\infty}^{\infty}\delta[n-lM]$$

또한, $X_s[k]=X(e^{j\frac{2\pi k}{M}})=X(f)P(f)$ 이므로, $x_s[n]=x[n]\otimes p[n]$ 이다.

그러므로,

$$x_s[n] = x[n] \otimes \sum_{l=-\infty}^{\infty} \delta[n-lM] = \sum_{l=-\infty}^{\infty} x[n]\otimes \delta[n-lM] $$

이것은 다음의 사실을 의미한다.

$$x_s[n] = \sum_{l=-\infty}^{\infty}x[n-lM]$$

---

 즉, 위의 결과에 따르면 inverse DFT $x[n]$ 은 M을 주기로 하는 주기함수라는 것을 알 수 있다. 또한, Discrete Time Domain에서 표현되는 샘플의 갯수가 N인 $x[n]$ 의 aliasing을 방지하기 위해선 연속주파수를 sampling 할 때, $N\leq M$ 인 $M$ 개의 이산 주파수로 나눠야 한다는 것도 알 수 있다.


<center><iframe width="420" height="315" src="https://www.youtube.com/embed/5dXiaE7bIoA" frameborder="0" allowfullscreen></iframe></center>

{% endraw %}
