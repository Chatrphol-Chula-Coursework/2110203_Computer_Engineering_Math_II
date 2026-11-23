# CP203 Part · Probability & Statistics — สรุปเนื้อหา (เฟส 1)

สรุปละเอียดของ 3 lecture แรกของ part สถิติ พร้อมตัวอย่างจากสไลด์แบบทำทีละขั้น
Visualizer ที่เล่นได้อยู่ใน [`Visualizer Web/Prob & Stat/`](../Visualizer%20Web/Prob%20%26%20Stat/) (เปิดผ่าน `Visualizer Web/index.html` → แถบ **Stats**)

| # | Lecture | หัวข้อหลัก | Visualizer |
|---|---|---|---|
| 0 | Overview & Probability Review | Bayes, RV, PMF/PDF/CDF, joint, E/Var, Cov, การแจกแจง | `probability-review.html` |
| 1 | Law of Large Numbers & Central Limit Theorem | LLN, CLT, ประมาณความน่าจะเป็น, confidence interval | `lln-clt.html` |
| 1B | Sampling & Estimation | inverse transform, likelihood, MLE, bias/variance, MAP, credible interval | `estimation.html` |

> แหล่งอ้างอิง: สไลด์ CP203 Stats (2110203, 2026) · MIT OCW 18.05 *Introduction to Probability and Statistics* · Kohavi, Tang & Xu *Trustworthy Online Controlled Experiments* · White *Bandit Algorithms for Website Optimization*

---

## สารบัญ

- [Lecture 0 · Overview & Probability Review](#lecture-0--overview--probability-review)
  - [0.1 ทำไมต้องเรียนสถิติ](#01-ทำไมต้องเรียนสถิติ)
  - [0.2 Probability vs Statistics](#02-probability-vs-statistics)
  - [0.3 Conditional probability, independence, Bayes](#03-conditional-probability-independence-bayes)
  - [0.4 Frequentist vs Bayesian](#04-frequentist-vs-bayesian)
  - [0.5 Random variable · PMF · PDF · CDF](#05-random-variable--pmf--pdf--cdf)
  - [0.6 Joint & marginal distribution](#06-joint--marginal-distribution)
  - [0.7 ผลบวกของตัวแปรสุ่ม = convolution](#07-ผลบวกของตัวแปรสุ่ม--convolution)
  - [0.8 Expected value & variance](#08-expected-value--variance)
  - [0.9 Quantile](#09-quantile)
  - [0.10 Covariance, correlation, covariance matrix](#010-covariance-correlation-covariance-matrix)
  - [0.11 การแจกแจงที่ต้องรู้จัก](#011-การแจกแจงที่ต้องรู้จัก)
- [Lecture 1 · LLN & CLT](#lecture-1--law-of-large-numbers--central-limit-theorem)
- [Lecture 1B · Sampling & Estimation](#lecture-1b--sampling--estimation)
- [สูตรรวมสำหรับสอบ](#สูตรรวมสำหรับสอบ-cheat-sheet)
- [กับดักที่พบบ่อย](#กับดักที่พบบ่อย)

---

## Lecture 0 · Overview & Probability Review

### 0.1 ทำไมต้องเรียนสถิติ

**คำถามจริงที่ต้องใช้ความน่าจะเป็นตอบ — ยา COVID ควรใช้ไหม?**

| | ตาย | รอด | รวม | อัตราตาย |
|---|---|---|---|---|
| ยา A | 9 | 31 | 40 | 22.5% |
| ไม่ให้ยา | 10 | 30 | 40 | 25% |

ต่างกันแค่ 2.5% — เป็นผลของยาหรือแค่บังเอิญ? ถ้าอีกการทดลองหนึ่งได้ ยา A ตาย 0/5 (0%) ดูดีมาก แต่มีแค่ 5 คน เชื่อได้แค่ไหน? → ต้องใช้เครื่องมือสถิติ (confidence interval, hypothesis test)

**Optimization ที่มีความไม่แน่นอน** — Hamtaro ทำเว็บ e-commerce ใช้ linear optimization ตรง ๆ หากำไรสูงสุดไม่ได้ เพราะพฤติกรรมลูกค้าเป็นสุ่ม → ต้องทดลอง (A/B testing, multi-armed bandit) ซึ่งเป็นปลายทางของ part นี้

**สถิติใช้ทำอะไร**
1. **Experimental design** — ออกแบบการเก็บข้อมูลให้ใช้ได้จริง
2. **Summary** — สรุปข้อมูลจาก poll / แบบสอบถาม
3. **Inference** — ใช้ข้อมูลอนุมานพารามิเตอร์ของโมเดล

ปรัชญาวิชา: คณิตศาสตร์ที่ใช้กับงานคอมพิวเตอร์จริง = ทฤษฎี + ปฏิบัติ (Lab)

### 0.2 Probability vs Statistics

| | Probability | Statistics |
|---|---|---|
| รู้อะไร | รู้โมเดล เช่น เหรียญเอียง $P(H) = 0.7$ | รู้ข้อมูล เช่น H T H H T H H T H T |
| ถามอะไร | โยน 10 ครั้ง ได้หัวพอดี 6 ครั้งมีโอกาสเท่าไร? | $P(H) = ?$ |
| คำตอบ | $\binom{10}{6}(0.7)^6(0.3)^4 \approx 0.200$ | ประมาณได้ $\hat p = 6/10 = 0.6$ (พิสูจน์ด้วย MLE ใน 1B) |

> Probability: โมเดล → ข้อมูล · Statistics: ข้อมูล → โมเดล

### 0.3 Conditional probability, independence, Bayes

**ความน่าจะเป็น** (กรณีผลลัพธ์เท่ากันทุกแบบ): $P(A) = \dfrac{\text{จำนวนผลลัพธ์ใน } A}{\text{จำนวนผลลัพธ์ทั้งหมด}}$

**Conditional probability** — "ความน่าจะเป็นของ A เมื่อรู้ว่า B เกิดแล้ว" = ย่อจักรวาลให้เหลือ B แล้วดูว่า A กินไปเท่าไร

$$P(A \mid B) = \frac{P(A \cap B)}{P(B)}$$

**Independence** — การเกิดของเหตุการณ์หนึ่งไม่เปลี่ยนความน่าจะเป็นของอีกเหตุการณ์

$$P(A \cap B) = P(A)\,P(B) \iff P(B \mid A) = P(B)$$

ตัวอย่าง: P(Sarah ใส่เสื้อขาว | หุ้น AIS ขึ้น) = P(Sarah ใส่เสื้อขาว)

**Bayes' rule** — ได้จากการเขียน $P(A\cap B)$ สองแบบ

$$P(A\cap B) = P(A\mid B)P(B) = P(B\mid A)P(A) \quad\Longrightarrow\quad \boxed{P(A \mid B) = \frac{P(B \mid A)\,P(A)}{P(B)}}$$

ตัวส่วนหาได้จาก **law of total probability**: $P(B) = P(B\mid A)P(A) + P(B\mid A^c)P(A^c)$

> ตัวอย่างคลาสสิก (ตรวจโรคหายาก): $P(\text{ป่วย}) = 0.01$, $P(+\mid\text{ป่วย}) = 0.99$, $P(+\mid\text{ไม่ป่วย}) = 0.05$
> $P(\text{ป่วย}\mid +) = \dfrac{0.99 \times 0.01}{0.99 \times 0.01 + 0.05 \times 0.99} \approx 0.167$ — ผลบวกแต่โอกาสป่วยจริงแค่ ~17% เพราะ prior ต่ำมาก (ลองใน visualizer หัวข้อ 02)

### 0.4 Frequentist vs Bayesian

| | Frequentist | Bayesian |
|---|---|---|
| ความน่าจะเป็นคือ | อัตราที่เกิดเมื่อทำซ้ำจำนวนมาก | ความไม่แน่นอนในความรู้ของเรา |
| โยนเหรียญ | $P(\text{head}) = \theta$, ประมาณ $\theta = \#\text{heads}/\#\text{tosses}$ | $\theta$ มีการแจกแจงเอง เช่น $\theta \sim U(0.6, 1.0)$ |
| พารามิเตอร์ | ค่าคงที่ที่ไม่รู้ | ตัวแปรสุ่ม — มี prior แล้วอัปเดตเป็น posterior |
| จุดเด่น | เป็นกลาง ไม่ต้องเลือก prior | ใส่ความรู้เดิมได้ (subjective) และอัปเดตตามข้อมูลได้ |

วิชานี้เน้น **frequentist** เป็นหลัก แต่จะพูดถึงข้อจำกัดและแตะ Bayesian บ้าง (MAP, bandit)

### 0.5 Random variable · PMF · PDF · CDF

**Random variable (RV)** = ฟังก์ชันที่ให้ค่าตัวเลขกับแต่ละผลลัพธ์ เช่น เหตุการณ์ A → 100 (Pr 0.5), B → 150 (Pr 0.3), C → 200 (Pr 0.2) — ค่าของ RV *ขึ้นกับผลลัพธ์*

| | Discrete RV | Continuous RV |
|---|---|---|
| ฟังก์ชัน | **PMF** $p_X(x) = P(X = x)$ | **PDF** $f_X(x)$ |
| รวมเป็น 1 | $\sum_x p_X(x) = 1$ | $\int_{-\infty}^{\infty} f_X(x)\,dx = 1$ |
| ความน่าจะเป็น | $p_X(5) = P(X=5) = 0.5$ เป็นความน่าจะเป็นจริง | $P(a \le X \le b) = \int_a^b f_X(x)\,dx$ |
| ข้อควรระวัง | — | $f_X(x)$ **ไม่ใช่ความน่าจะเป็น** (เกิน 1 ได้) และ $P(X = x_0) = 0$ ทุกจุด |

ตัวอย่าง continuous: เวลาเริ่มคลาส 9:00, 9:01, …, 11:59 — โอกาสที่จะเริ่ม "ตรง 9:00:00.000…" เป็น 0 ต้องถามเป็นช่วงเวลา

**CDF** (ใช้ได้ทั้งสองแบบ) — "ความน่าจะเป็นที่ RV ไม่เกินค่าหนึ่ง"

$$F_X(x) = P(X \le x) = \int_{-\infty}^{x} f_X(t)\,dt, \qquad f_X(x) = \frac{dF_X(x)}{dx}$$

$$P(a < X \le b) = \int_a^b f_X(x)\,dx = F_X(b) - F_X(a)$$

คุณสมบัติ CDF: ไม่ลด, $F(-\infty) = 0$, $F(\infty) = 1$ · คำเรียก: pdf = "density", cdf = "distribution", ผลต่างของ cdf = "probability"

### 0.6 Joint & marginal distribution

**Joint distribution** $f_{X,Y}(x,y)$ — สองเหตุการณ์เกิดพร้อมกัน

$$P(A) = \iint_A f_{X,Y}(x,y)\,dx\,dy$$

**Marginal** — integrate ตัวแปรอีกตัวทิ้ง (discrete: รวมแถว/คอลัมน์)

$$f_X(x) = \int_{-\infty}^{\infty} f_{X,Y}(x,y)\,dy, \qquad f_Y(y) = \int_{-\infty}^{\infty} f_{X,Y}(x,y)\,dx$$

**อิสระ** ⇔ $f_{X,Y}(x,y) = f_X(x)\,f_Y(y)$ **ทุกจุด** (เช็คทุกช่องของตาราง แค่ช่องเดียวไม่เท่าก็ไม่อิสระ)

### 0.7 ผลบวกของตัวแปรสุ่ม = convolution

ถ้า $Z = X + Y$ โดย X, Y อิสระ: โอกาสที่ $Z = z$ คือรวมทุกวิธีที่ $X = x$ และ $Y = z - x$

$$\boxed{f_{X+Y}(z) = (f_X * f_Y)(z) = \int_{-\infty}^{\infty} f_X(x)\,f_Y(z - x)\,dx}$$

Discrete: $p_Z(z) = \sum_x p_X(x)\,p_Y(z-x)$ — สูตรเดียวกับ convolution ใน part Signal (พลิก → เลื่อน → คูณ → รวม)

| X + Y | ผล |
|---|---|
| ลูกเต๋า + ลูกเต๋า | สามเหลี่ยม 2…12, $P(Z=7) = 6/36$ |
| $U(0,1) + U(0,1)$ | สามเหลี่ยมบน [0, 2] (rect ∗ rect) |
| $\text{Exp}(1) + \text{Exp}(1)$ | $z e^{-z}$ (Gamma(2,1)) |
| $N(\mu_1,\sigma_1^2) + N(\mu_2,\sigma_2^2)$ | $N(\mu_1+\mu_2,\ \sigma_1^2+\sigma_2^2)$ |

### 0.8 Expected value & variance

**Expected value** — "ค่าเฉลี่ย" ของ RV เรียกว่า $\mu$ (discrete: เปลี่ยน ∫ เป็น Σ)

$$E[X] = \int x\,f(x)\,dx, \qquad E[g(X)] = \int g(x)\,f(x)\,dx, \qquad E[X \mid A] = \int x\,f(x \mid A)\,dx$$

**Variance** — RV กระจายแค่ไหน · $\sigma^2$ คือ variance, $\sigma$ คือ standard deviation

$$\operatorname{Var}(X) = E\big[(X - \mu)^2\big] = \int (x - \mu)^2 f(x)\,dx = \boxed{E[X^2] - (E[X])^2}$$

**คุณสมบัติ (ต้องจำ)**

| Expected value | Variance |
|---|---|
| $E[c] = c$ | $\operatorname{Var}(c) = 0$ |
| $E[aX + b] = aE[X] + b$ | $\operatorname{Var}(aX + b) = a^2\operatorname{Var}(X)$ |
| $E[X + Y] = E[X] + E[Y]$ (**เสมอ** แม้ไม่อิสระ) | $\operatorname{Var}(X + Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) + 2\operatorname{Cov}(X,Y)$ |
| $E[X - Y] = E[X] - E[Y]$ | $\operatorname{Var}(X - Y) = \operatorname{Var}(X) + \operatorname{Var}(Y) - 2\operatorname{Cov}(X,Y)$ |
| $E[XY] = E[X]E[Y]$ ถ้าอิสระ | ถ้าอิสระ: $\operatorname{Var}(X \pm Y) = \operatorname{Var}(X) + \operatorname{Var}(Y)$ (บวกทั้งคู่!) |

> **กับดัก:** $\operatorname{Var}(2X) = 4\operatorname{Var}(X)$ แต่ $\operatorname{Var}(X_1 + X_2) = 2\operatorname{Var}(X)$ เมื่อ $X_1, X_2$ อิสระและแจกแจงเหมือน X — "X บวกตัวเอง" ≠ "บวกสำเนาอิสระ" เพราะ $\operatorname{Cov}(X,X) = \operatorname{Var}(X)$

**ตัวอย่าง (ลอตเตอรี่)** — $P(\text{win}) = 10^{-20}$ ถูกได้ $10^{30}$, ไม่ถูกเสีย 10

$$E[\text{Lottery}] = 10^{30}\cdot 10^{-20} + (1 - 10^{-20})(-10) \approx 10^{10} - 10$$

ค่าคาดหมายสูงมากแต่แทบไม่มีทางถูก — E เป็นค่าเฉลี่ยถ่วงน้ำหนัก ไม่ได้บอกว่าอะไร "มักเกิด" และมนุษย์ประเมินความน่าจะเป็นที่ปลายสุดได้แย่

### 0.9 Quantile

$x_q$ คือค่าที่ $P(X \le x_q) = q$ (อ่านจาก CDF ในแนวนอน)

- **Median**: $P(X \le m) = 0.5$
- **Quartile** $q_1, q_2, q_3$: ที่ 0.25, 0.5, 0.75
- **Decile** $d_1, …, d_9$: เช่น $P(X \le d_4) = 0.4$ (ตัวอย่างจริง: บทความ "The wealth of Chula students" แบ่งรายได้ครัวเรือนเป็น decile)
- **Percentile** $p_1, …, p_{99}$

### 0.10 Covariance, correlation, covariance matrix

**Covariance**

$$\operatorname{Cov}(X_1, X_2) = E\big[(X_1 - \mu_1)(X_2 - \mu_2)\big] = \boxed{E[X_1X_2] - \mu_1\mu_2}$$

- $\operatorname{Cov}(X, X) = \operatorname{Var}(X)$
- $\operatorname{Cov}(aX + b,\ cY + d) = ac\,\operatorname{Cov}(X, Y)$ (ค่าคงที่ที่บวกไม่มีผล)
- $\operatorname{Cov}(X, Y + Z) = \operatorname{Cov}(X, Y) + \operatorname{Cov}(X, Z)$

**Correlation** (ไม่มีหน่วย อยู่ใน [−1, 1])

$$\rho = \frac{\operatorname{Cov}(X, Y)}{\sqrt{\operatorname{Var}(X)}\sqrt{\operatorname{Var}(Y)}}$$

**ข้อควรจำ**
1. Correlation วัดเฉพาะ**ความสัมพันธ์เชิงเส้น** — Anscombe's quartet 4 ชุดมี $\rho = 0.816$ เท่ากันหมดแต่หน้าตาต่างกันสิ้นเชิง → plot ข้อมูลก่อนเสมอ
2. **Correlation = 0 ⇏ independent** (เช่น $Y = X^2$ กับ X สมมาตรรอบ 0) แต่ independent ⇒ uncorrelated เสมอ
3. **Correlation ⇏ causation** (spurious correlations)

**Covariance matrix** ของ $X_1, …, X_n$: $\Sigma_{ij} = \operatorname{Cov}(X_i, X_j)$ (แนวทแยง = variance)
- สมมาตร $\Sigma = \Sigma^T$ และ positive semi-definite
- $\Sigma$ เป็น diagonal ⇒ ทุกคู่ uncorrelated (เป็นอิสระด้วยถ้า **jointly Gaussian**)
- positive definite ⇒ invertible (ยกเว้นมี linear combination ของ RV ที่ variance = 0)

### 0.11 การแจกแจงที่ต้องรู้จัก

**Discrete**

| Distribution | ค่าที่เป็นไปได้ | PMF $p(k)$ | Mean | Variance | ใช้เมื่อ |
|---|---|---|---|---|---|
| Bernoulli($p$) | 0, 1 | $p^k(1-p)^{1-k}$ | $p$ | $p(1-p)$ | ทดลอง 1 ครั้ง สำเร็จ/ไม่ |
| Binomial($n, p$) | 0, …, n | $\binom{n}{k}p^k(1-p)^{n-k}$ | $np$ | $np(1-p)$ | นับสำเร็จใน n ครั้งอิสระ |
| Uniform($n$) | 1, …, n | $1/n$ | $\frac{n+1}{2}$ | $\frac{n^2-1}{12}$ | ลูกเต๋า n หน้า |
| Geometric($p$) | 0, 1, 2, … | $(1-p)^k p$ | $\frac{1-p}{p}$ | $\frac{1-p}{p^2}$ | **จำนวนครั้งที่ล้มเหลว**ก่อนสำเร็จครั้งแรก |
| Poisson($\lambda$) | 0, 1, 2, … | $\frac{\lambda^k e^{-\lambda}}{k!}$ | $\lambda$ | $\lambda$ | จำนวนเหตุการณ์ต่อช่วงเวลา (rate λ) |

**Continuous**

| Distribution | ช่วง | PDF | CDF | Mean | Variance |
|---|---|---|---|---|---|
| Normal($\mu, \sigma^2$) | $(-\infty, \infty)$ | $\frac{1}{\sqrt{2\pi\sigma^2}}e^{-(x-\mu)^2/2\sigma^2}$ | ใช้ตาราง $\Phi$ | $\mu$ | $\sigma^2$ |
| Exponential($\lambda$) | $[0, \infty)$ | $\lambda e^{-\lambda x}$ | $1 - e^{-\lambda x}$ | $1/\lambda$ | $1/\lambda^2$ |
| Uniform($a, b$) | $[a, b]$ | $\frac{1}{b-a}$ | $\frac{x-a}{b-a}$ | $\frac{a+b}{2}$ | $\frac{(b-a)^2}{12}$ |
| Pareto($x_m, \alpha$) (power law) | $[x_m, \infty)$ | $\frac{\alpha x_m^\alpha}{x^{\alpha+1}}$ | $1 - (x_m/x)^\alpha$ | $\frac{\alpha x_m}{\alpha-1}$ ($\alpha>1$) | $\frac{x_m^2\alpha}{(\alpha-1)^2(\alpha-2)}$ ($\alpha>2$) |

**ความสัมพันธ์ระหว่างการแจกแจง**

```
Bernoulli ──ทำซ้ำ n ครั้ง──▶ Binomial ──n→∞, p→0, np=λ──▶ Poisson
    │                                                     ▲
    └──ทำจนกว่าจะสำเร็จ──▶ Geometric ──เวอร์ชันต่อเนื่อง──▶ Exponential
                                                (เวลารอระหว่างเหตุการณ์ของ Poisson, rate λ)
```

---

## Lecture 1 · Law of Large Numbers & Central Limit Theorem

### 1.1 จาก Sample สู่ Population

- **Population**: ทุกหน่วยที่อยากรู้ (คนไทยทุกคน, นิสิตจุฬาฯ ทุกคน) — มักรู้ไม่ครบ ค่าจริง ($\mu, \sigma, p$) เป็นค่าคงที่ที่ไม่รู้
- **Sample**: ส่วนที่วัดได้ (คะแนนนักเรียนในห้อง) — มีประโยชน์**เฉพาะเมื่อสุ่มมาอย่างสุ่มจริง**
- คำถาม: sample เป็นตัวแทนได้ดีแค่ไหน? เชื่อได้แค่ไหน? margin of error เท่าไร?

**สัญลักษณ์:** $X_1, …, X_n$ **i.i.d.** (independent & identically distributed) มี mean $\mu$ และ SD $\sigma$

$$S_n = X_1 + \cdots + X_n, \qquad \bar X_n = \frac{S_n}{n} = \frac{1}{n}\sum_{i=1}^n X_i \quad (\bar X_n \text{ เป็นตัวแปรสุ่ม})$$

### 1.2 Law of Large Numbers (LLN)

ยิ่งมีตัวอย่างอิสระมาก ค่าเฉลี่ยตัวอย่างยิ่งเข้าใกล้ค่าเฉลี่ยของประชากร — โอกาสที่จะได้ผล "ผิดปกติ" (ห่างจาก μ) ลดลงเป็นศูนย์ (**convergence in probability**)

$$\text{สำหรับทุก } \varepsilon > 0: \qquad \lim_{n\to\infty} P\big(|\bar X_n - \mu| < \varepsilon\big) = 1$$

**ผลที่ตามมา**
- ค่าเฉลี่ยตัวอย่างใกล้ค่าจริงเมื่อ n ใหญ่
- **Histogram เข้าใกล้ pdf จริง** (สัดส่วนในแต่ละ bin ก็เป็นค่าเฉลี่ยของ indicator)

**หมายเหตุ**
- ที่เรียนคือ **weak LLN** (convergence in probability) — แบบที่ใช้บ่อยที่สุด
- **Strong LLN**: $\bar X_n \to \mu$ "almost surely" — ตัวค่าเฉลี่ยเองนิ่งที่ μ ด้วยความน่าจะเป็น 1
- เงื่อนไข: ต้องมี mean ($E|X| < \infty$) — Cauchy ไม่มี mean จึงไม่ลู่เข้า (ดู visualizer)

### 1.3 Central Limit Theorem (CLT)

**สิ่งที่รู้แน่นอน (ทุก n)** — จากกฎของ E/Var

| | Sum $S_n$ | Mean $\bar X_n$ |
|---|---|---|
| Expected value | $E[S_n] = n\mu$ | $E[\bar X_n] = \mu$ |
| Variance | $\operatorname{Var}(S_n) = n\sigma^2$ | $\operatorname{Var}(\bar X_n) = \sigma^2/n$ |
| SD | $\sigma_{S_n} = \sqrt n\,\sigma$ | $\sigma_{\bar X_n} = \sigma/\sqrt n$ |

(ใช้ $E$ บวกกันได้, Var ของผลบวกของอิสระบวกกันได้, และ $\operatorname{Var}(aX) = a^2\operatorname{Var}(X)$ กับ $a = 1/n$)

**CLT** — สำหรับ i.i.d. ที่ variance จำกัด และ n ใหญ่:

$$\boxed{\bar X_n \approx N\!\left(\mu, \frac{\sigma^2}{n}\right)}, \qquad S_n \approx N(n\mu,\ n\sigma^2)$$

Standardized mean ลู่เข้า N(0,1) **in distribution** (CDF ของมันเข้าใกล้ CDF ของ N(0,1)):

$$Z_n = \frac{\bar X_n - \mu}{\sigma/\sqrt n} = \frac{S_n - n\mu}{\sigma\sqrt n} \;\longrightarrow\; N(0, 1)$$

**CLT notes**
- $\bar X_n$ มี mean เท่าเดิมแต่ variance เล็กกว่า $X_i$
- "n ใหญ่" ที่ใช้บ่อยคือ **n ≥ 30** — ข้อมูลเบ้มากต้องใช้มากกว่านั้น
- ใช้กับ **discrete** ได้ แม้ Normal เป็น continuous — เพราะนิยามการลู่เข้าบนการแจกแจง (CDF)
- มี CLT รุ่นอื่นที่ผ่อนเงื่อนไข independent / identically distributed
- ใช้ไม่ได้ถ้า variance ไม่จำกัด (Cauchy, Pareto α ≤ 2)

### 1.4 ใช้ CLT ประมาณความน่าจะเป็น

สูตรทำโจทย์: **หา mean & SD ของผลรวม → standardize → เปิดตาราง Φ**

**Problem (สไลด์).** โยนเหรียญเที่ยง 100 ครั้ง ประมาณ P(หัวมากกว่า 55 ครั้ง)

1. $S_{100} = X_1 + \cdots + X_{100}$ โดย $X_i \sim \text{Bernoulli}(0.5)$
2. วิธีตรง (หนัก): $\sum_{k=56}^{100}\binom{100}{k}0.5^k0.5^{100-k} = 0.1356$
3. $E[S] = 100 \times 0.5 = 50$, $\operatorname{Var}(S) = 100 \times 0.25 = 25$ → SD = 5
4. $S \approx N(50, 5^2)$ ⇒ $P(S > 55) = P\!\left(\frac{S - 50}{5} > \frac{55-50}{5}\right) = P(Z > 1)$
5. $P(Z > 1) = 1 - \Phi(1) \approx \mathbf{0.16}$

> เสริม: continuity correction (ใช้ 55.5 แทน 55) ได้ $P(Z > 1.1) = 0.1357$ แทบตรงกับค่าจริง

**Example 1.** Maxwell อ่าน 100 หน้า แต่ละหน้าอ่านเร็วด้วยความน่าจะเป็น 0.5 อย่างอิสระ ประมาณ P(อ่านเร็วมากกว่า 60 หน้า)

$\mu_S = 50$, $\sigma_S^2 = 25$, $\sigma_S = 5$ → $P(S_{100} > 60) \approx P\!\left(Z > \frac{60 - 50}{5}\right) = P(Z > 2) \approx \mathbf{0.023}$ (ค่าตรง 0.0176)

### 1.5 Confidence interval จากโพล

**Problem.** ประชากรชอบ A ด้วยสัดส่วน $p_0$ (ไม่รู้) ถาม n คนได้สัดส่วน $\bar x$ (หรือ $\hat p$) — หา 95% CI ของ $p_0$

1. แต่ละคนเป็น Bernoulli($p_0$) → $\sigma^2 = p_0(1 - p_0)$
2. CLT: $\bar X \sim N(p_0, \sigma_{\bar X}^2)$, $\sigma_{\bar X} = \sigma/\sqrt n$
3. ~95% ของ Normal อยู่ใน ±2 SD: $P(|\bar X - p_0| \le 2\sigma_{\bar X}) \approx 0.95$ (ค่าแม่นคือ 1.96)
4. $\bar x$ อยู่ใน $p_0 \pm 2\sigma_{\bar X}$ **พอดีกับที่** $p_0$ อยู่ใน $\bar x \pm 2\sigma_{\bar X}$
5. ไม่รู้ $p_0$ → ใช้กรณีแย่สุด $\sigma^2 = p_0(1-p_0) \le \tfrac14$ → $2\sigma_{\bar X} \le 2\cdot\tfrac12\cdot\tfrac{1}{\sqrt n} = \tfrac{1}{\sqrt n}$

$$\boxed{\text{95\% CI: } \bar x \pm \frac{1}{\sqrt n}}$$

โพล 1,000 คน → ±3.2% · ต้องการ ±1% ต้องถาม 10,000 คน (ความแม่นยำเพิ่มตาม $\sqrt n$)

แบบ **plug-in** (เมื่อ $\hat p$ ไม่ใกล้ 0.5): $\hat p \pm 1.96\sqrt{\hat p(1-\hat p)/n}$

**Example 2.** Ham-Hams ถามแฮมสเตอร์ 400 ตัว: เมล็ดทานตะวันหรือลูกโอ๊ก? 240 ตัวเลือกเมล็ดทานตะวัน

$\hat p = 240/400 = 0.6$, $1/\sqrt{400} = 0.05$ → **95% CI ≈ [0.55, 0.65]** (plug-in: $2\sqrt{0.6\cdot0.4/400} = 0.049$ → [0.551, 0.649]) — ช่วงไม่คร่อม 0.5 จึงค่อนข้างมั่นใจว่าส่วนใหญ่ชอบเมล็ดทานตะวัน

### 1.6 95% CI แปลว่าอะไร

- ✅ **ถูก:** *วิธีการ*สร้างช่วงของเราจะจับค่าจริงได้ 95% ของเวลา — เก็บตัวอย่าง 100 ชุด คำนวณ 100 ค่า $\bar x$ และ 100 ช่วง → ค่าจริง μ จะอยู่ในประมาณ 95 ช่วง
- ❌ **ผิด:** "มีความน่าจะเป็น 95% ที่ค่าจริงอยู่ในช่วงนี้"
- เหตุผล: **μ คงที่ — ช่วงต่างหากที่สุ่ม** (ถ้าอยากพูดแบบนั้นต้องใช้ credible interval แบบ Bayesian, ดู 1B.9)

---

## Lecture 1B · Sampling & Estimation

### 1B.1 จากโมเดลสู่ข้อมูล: simulation & sampling

| Blueprint (โมเดล) | Virtual factory (simulation) | Product (sample) |
|---|---|---|
| น้ำหนักเมล็ดทานตะวันสด $\sim N(5, 0.5^2)$ | สร้างเมล็ดเสมือน 1,000 เมล็ดตามกฎนี้ | [5.1 g, 4.4 g, 5.8 g, 4.9 g, …] |

NumPy/SciPy มีฟังก์ชันสุ่มของการแจกแจงมาตรฐานอยู่แล้ว — แต่ถ้าการแจกแจงไม่มาตรฐานล่ะ?

### 1B.2 Inverse transform sampling

**ปัญหา:** คอมพิวเตอร์สุ่มได้จริงแค่ $U \sim \text{Uniform}(0,1)$ · **เป้าหมาย:** สุ่มตามการแจกแจงอื่น · **เครื่องมือ:** CDF — มีค่าอยู่ใน [0, 1] พอดี!

แทนที่จะใช้ CDF เดินจาก x → ความน่าจะเป็น ให้**เดินถอยหลัง**:
1. สุ่ม $u \sim U(0, 1)$
2. วาง u บนแกน y ของ CDF เป้าหมาย
3. ลากแนวนอนไปชนเส้น CDF แล้วลากลงแกน x
4. ค่า x ที่ได้คือตัวอย่างสุ่ม: $\boxed{x = F^{-1}(u)}$

ทำไมใช้ได้: $P(F^{-1}(U) \le x) = P(U \le F(x)) = F(x)$ ✓

**Sampling example.** pdf $f_X(x) = 2x$ ($0 \le x \le 1$) หา $F_X^{-1}(u)$
1. CDF: $F_X(x) = \int_0^x 2t\,dt = x^2$ ($0 \le x \le 1$)
2. ตั้ง $u = F_X(x) = x^2$ → $x = \sqrt u$
3. $\boxed{F_X^{-1}(u) = \sqrt u}$

**Exponential (เวลานอนของ Hamtaro):** $u = 1 - e^{-\lambda x}$ ⇒ $x = -\ln(1 - u)/\lambda$ · λ = 1, u = 0.632 → x ≈ 1 ชั่วโมง

### 1B.3 เป้าหมายของ estimation

จากตัวอย่าง → เดาค่าพารามิเตอร์จริงของประชากร
- ค่าเฉลี่ย μ → ใช้ LLN (ค่าเฉลี่ยตัวอย่าง)
- พารามิเตอร์อื่น ๆ: variance $\sigma^2$ (น้ำหนักเมล็ดสม่ำเสมอแค่ไหน), rate λ (ขวดน้ำแฮมสเตอร์หมดเร็วแค่ไหน) ฯลฯ
- คำถามหลัก: **หา estimator ที่ดีของพารามิเตอร์ใด ๆ ได้อย่างไร** และ **estimate ดีแค่ไหน**

### 1B.4 Likelihood

**ตัวอย่าง:** Maxwell ลองเทคนิคอ่านเร็ว 100 หน้า สำเร็จ (อ่านจบใน 1 นาที) 55 หน้า พลาด 45 หน้า — อัตราสำเร็จ p ที่น่าจะเป็นที่สุดคือ?

| ศัพท์ | ในตัวอย่างนี้ |
|---|---|
| Experiment | ลองอ่านเร็ว 100 หน้าแล้วนับว่าสำเร็จกี่หน้า |
| Data | ผลลัพธ์: "55 หน้า" |
| Parameter of interest | p ที่ไม่รู้ (ทั่วไปเขียน θ) |
| Likelihood | $P(55 \text{ fast pages} \mid p) = \binom{100}{55}p^{55}(1-p)^{45}$, ทั่วไป $= P(\text{Data} \mid \theta)$ |

Likelihood คือ "ความน่าจะเป็นของข้อมูลเมื่อกำหนด θ" — มองเป็น**ฟังก์ชันของ θ** (ข้อมูลตรึงไว้) ไม่ใช่การแจกแจงของ θ จึงไม่ต้องรวมเป็น 1

### 1B.5 Maximum Likelihood Estimation (MLE)

แนวคิด: ในบรรดาค่า θ ทั้งหมด ค่าไหนทำให้ข้อมูลที่เห็นจริง **น่าเกิดที่สุด / น่าแปลกใจน้อยที่สุด**

$$\hat\theta_{MLE} = \arg\max_\theta\, L(\theta; D) = \arg\max_\theta\, P(D \mid \theta)$$

(หมวก ^ = ค่าประมาณ อ่านว่า "MLE estimator of θ")

**Interlude: หาจุดสุดขีด** — $f(x) = x^3 - 3x + 11$
- $f'(x) = 3x^2 - 3 = 0$ ⇒ $x = \pm1$
- $f''(x) = 6x$: $f''(1) = 6 > 0$ → **minimum** ($f = 9$) · $f''(-1) = -6 < 0$ → **maximum** ($f = 13$)

**วิธีที่ 1 — อนุพันธ์ตรง (product rule)**

$$\frac{d}{dp}\left[\binom{100}{55}p^{55}(1-p)^{45}\right] = \binom{100}{55}\left[55p^{54}(1-p)^{45} - 45p^{55}(1-p)^{44}\right] = 0$$

$$55p^{54}(1-p)^{45} = 45p^{55}(1-p)^{44} \;\Rightarrow\; 55(1-p) = 45p \;\Rightarrow\; 55 = 100p \;\Rightarrow\; \boxed{\hat p_{MLE} = 0.55}$$

### 1B.6 Log-likelihood

ทำไมใช้ log:
- **ln เป็นฟังก์ชันเพิ่ม (monotonic):** ถ้า $a > b$ แล้ว $\ln a > \ln b$ → θ ที่ทำให้ log-likelihood สูงสุดก็ทำให้ likelihood สูงสุด
- **เปลี่ยนการคูณเป็นการบวก:** $\ln(ab) = \ln a + \ln b$, $\ln a^b = b\ln a$ → อนุพันธ์ง่ายกว่ามาก
- **กัน underflow:** คอมพิวเตอร์คูณเลขเล็ก ๆ จำนวนมากไม่ไหว

**วิธีที่ 2 — log-likelihood**

$$\ln P(55 \mid p) = \ln\binom{100}{55} + 55\ln p + 45\ln(1-p)$$

$$\frac{d}{dp}\ln L = \frac{55}{p} - \frac{45}{1-p} = 0 \;\Rightarrow\; 55(1-p) = 45p \;\Rightarrow\; \boxed{\hat p_{MLE} = 0.55}$$

ทั่วไป: $k$ สำเร็จจาก $n$ ครั้ง → $\hat p = k/n$

### 1B.7 ตัวอย่าง MLE

**Likelihood ของแต่ละการแจกแจง** (discrete ใช้ pmf, continuous ใช้ pdf; ข้อมูลอิสระ → likelihood รวม = ผลคูณ)

| | Likelihood ของ 1 ค่า | MLE จาก n ข้อมูล |
|---|---|---|
| Binomial | $p(k \mid n, p) = \binom nk p^k(1-p)^{n-k}$ | $\hat p = k/n$ |
| Geometric (k = จำนวนครั้งที่ล้ม) | $p(k \mid p) = (1-p)^k p$ | $\hat p = \dfrac{n}{n + \sum k_i}$ |
| Exponential | $f(x \mid \lambda) = \lambda e^{-\lambda x}$ | $\hat\lambda = \dfrac{n}{\sum x_i} = \dfrac{1}{\bar x}$ |
| Normal | $f(x \mid \mu, \sigma) = \frac{1}{\sqrt{2\pi}\sigma}e^{-(x-\mu)^2/2\sigma^2}$ | $\hat\mu = \bar x$, $\hat\sigma^2 = \frac1n\sum(x_i - \bar x)^2$ |

**Example 1 (Exponential).** เครื่องจักรในโรงงานของ Hamtaro มีอายุการใช้งานแบบ exponential เสียหลัง 2, 3, 1, 3, 4 ปี (อิสระกัน) ประมาณพารามิเตอร์

1. ให้ $X_i$ คืออายุของเครื่อง i, $f_{X_i}(x_i) = \lambda e^{-\lambda x_i}$
2. Joint pdf: $f(x_1, …, x_5 \mid \lambda) = \lambda^5 e^{-\lambda(x_1 + \cdots + x_5)} = \lambda^5 e^{-13\lambda}$ (เพราะ 2+3+1+3+4 = 13)
3. $\ln f(2,3,1,3,4 \mid \lambda) = 5\ln\lambda - 13\lambda$
4. $\dfrac{d}{d\lambda}(\ln L) = \dfrac{5}{\lambda} - 13 = 0$ ⇒ $\boxed{\hat\lambda = 5/13 \approx 0.385}$ ต่อปี (= $1/\bar x$, อายุเฉลี่ย 2.6 ปี)

**Example 2 (Geometric).** Cappy หัดทำหมวกแบบใหม่ จำนวนครั้งที่ล้มเหลวก่อนสำเร็จเป็น Geometric — 3 แบบหมวก สำเร็จในครั้งที่ 1, 1, 3 ใช้ MLE หา p

1. ⚠️ จำนวน**ครั้งที่ล้ม** = 0, 0, 2 (สำเร็จครั้งที่ 3 = ล้มมาก่อน 2 ครั้ง)
2. $L(p) = (1-p)^0p \times (1-p)^0p \times (1-p)^2p = (1-p)^2p^3$
3. $\dfrac{d}{dp}\big[2\ln(1-p) + 3\ln p\big] = -\dfrac{2}{1-p} + \dfrac{3}{p} = 0$
4. $\boxed{\hat p = 3/5}$ — สมเหตุสมผล: สำเร็จ 3 ครั้งจากการลองทั้งหมด 1 + 1 + 3 = 5 ครั้ง

### 1B.8 Goodness of the estimator

- **Bias** (ความคลาดเคลื่อนเชิงระบบ): โดยเฉลี่ยค่าประมาณห่างจากค่าจริงแค่ไหน · **Unbiased**: $E[\hat\theta] = \theta$
- **Variance** (ความคลาดเคลื่อนแบบสุ่ม): ถ้าได้ข้อมูลชุดใหม่ ค่าประมาณจะเปลี่ยนไปแค่ไหน

**คุณสมบัติของ MLE**
1. **Consistency** — ข้อมูลมากขึ้นทำให้ $\hat\theta_{MLE}$ เข้าใกล้ θ จริง
2. **Asymptotically unbiased** — ไม่สูง/ต่ำเชิงระบบเมื่อ n ใหญ่ (n เล็กอาจมี bias)
3. **Asymptotically minimal variance** — เมื่อข้อมูลมาก MLE มี variance ต่ำที่สุดในบรรดา unbiased estimator ทั้งหมด

> ตัวอย่างใน visualizer: MLE ของ $\sigma^2$ คือ $\frac1n\sum(x_i - \bar x)^2$ มี $E[\hat\sigma^2] = \frac{n-1}{n}\sigma^2$ (ต่ำไปเสมอ = biased) แต่ bias → 0 เมื่อ n → ∞ (asymptotically unbiased) · sample variance $s^2 = \frac{1}{n-1}\sum(x_i - \bar x)^2$ unbiased

### 1B.9 Bayes theorem & MAP

$$\underbrace{P(\theta \mid D)}_{\text{posterior}} = \frac{\overbrace{P(D \mid \theta)}^{\text{likelihood}}\;\overbrace{P(\theta)}^{\text{prior}}}{\underbrace{P(D)}_{\text{evidence}}}$$

- **MLE** สนใจแค่ likelihood $P(D\mid\theta)$ — เหมือนสมมติว่า prior และ evidence คงที่
- ถ้าเรารู้ prior อยู่แล้วล่ะ? (เราเชื่อจริงหรือว่า p ของเหรียญ ~ U(0, 1)?)

**Maximum A Posteriori (MAP)** — maximize posterior

$$\hat\theta_{MAP} = \arg\max_\theta P(\theta \mid D) = \arg\max_\theta \frac{P(D\mid\theta)P(\theta)}{P(D)} = \boxed{\arg\max_\theta P(D \mid \theta)\,P(\theta)}$$

($P(D)$ ไม่ขึ้นกับ θ จึงทิ้งได้ตอนหา argmax)

**Evidence $P(D)$** — ตัว normalize ที่ทำให้ posterior เป็นความน่าจะเป็นที่ถูกต้อง: $P(D) = \sum_{\text{all }\theta} P(D \mid \theta)P(\theta)$
- treat เป็นค่าคงที่ได้ตอนหา argmax (ไม่ขึ้นกับ θ)
- แต่ **ไม่ใช่ค่าคงที่ในตัวมันเอง** — ข้อมูลชุดใหม่ D′ ให้ $P(D') \ne P(D)$

**Example 3 (MAP).** Hamtaro หาลูกโอ๊กที่ Boss ซ่อน — จุดซ่อน 3 แบบ (= ค่าที่เป็นไปได้ 3 ค่าของ θ):
Type A เจอ 80% · Type B เจอ 50% · Type C เจอ 40% · Boss ใช้ A 40%, B 40%, C 20% ของเวลา (prior)
Hamtaro ค้นจุดใหม่ 5 ครั้ง เจอ 2 ครั้ง — หา MAP

| Type | θ | prior | likelihood $\binom52\theta^2(1-\theta)^3$ | likelihood × prior | posterior (÷ 0.2146) |
|---|---|---|---|---|---|
| A | 0.8 | 0.4 | 0.0512 | 0.02048 | 0.0954 |
| B | 0.5 | 0.4 | 0.3125 | **0.12500** | **0.5825** |
| C | 0.4 | 0.2 | 0.3456 | 0.06912 | 0.3221 |
| | | | | $P(D) = 0.2146$ | รวม = 1 |

$\Rightarrow \hat\theta_{MAP} = \mathbf{0.5}$ (Type B มี posterior สูงสุด) · เทียบ $\hat\theta_{MLE} = 0.4$ (Type C มี likelihood สูงสุด)

**Example 4 (posterior predictive).** หลังค้น 5 ครั้งแรก โอกาสเจอในการค้นครั้งถัดไป?

$$P(\text{Acorn} \mid D) = \sum_\theta P(\text{Acorn} \mid \theta)\,P(\theta \mid D) = 0.8(0.0954) + 0.5(0.5825) + 0.4(0.3221) \approx \mathbf{0.4964}$$

≠ 0.5 ($\hat\theta_{MAP}$) และ ≠ 0.4 ($\hat\theta_{MLE}$) — เป็นการผสมระหว่าง likelihood (ชี้ไป 40%) กับ prior

**Example 5.** ก่อนค้น Pashmina บอกว่า "เห็น Boss ถือแผนที่จุดซ่อน Type A ออกมาจากที่นี่!" → prior Type A = 100% → $\hat\theta_{MAP} = 0.8$ ไม่ว่าข้อมูลจะเป็นอย่างไร

**ข้อควรระวังของ MAP**
- อิทธิพลของ prior แรงมาก (Example 5)
- **MLE เป็นกรณีพิเศษของ MAP** ที่ prior เป็น uniform
- ต้องอธิบายได้ว่า prior มาจากไหน — ถ้า Pashmina ผิดล่ะ?

### 1B.10 Credible interval vs confidence interval

| | Credible interval (Bayesian) | Confidence interval (Frequentist) |
|---|---|---|
| นิยาม | $[a, b]$ ที่ $P(a \le \theta \le b \mid D) = 0.95$ | ทำการทดลองซ้ำหลายครั้ง 95/100 ครั้งช่วงคร่อมค่าจริง |
| ความน่าจะเป็นของ | θ เมื่อเห็นข้อมูลแล้ว | วิธีการสร้างช่วง (อัตราสำเร็จระยะยาว) |
| หมายเหตุ | มีได้หลายช่วง | ไม่ใช่ข้อความเกี่ยวกับช่วงใดช่วงหนึ่ง |

> **เสริม (ต่อยอดจากสไลด์ ใน visualizer หัวข้อ 08):** ถ้า θ ต่อเนื่อง (อัตราสำเร็จ p ∈ [0,1]) ใช้ prior $\text{Beta}(a,b)$ → เห็น k สำเร็จจาก n → posterior $\text{Beta}(a+k,\ b+n-k)$ · $\hat\theta_{MAP} = \frac{a+k-1}{a+b+n-2}$ · prior uniform = Beta(1,1) ⇒ MAP = MLE = k/n
> Maxwell 55/100 กับ prior uniform → posterior Beta(56, 46), 95% credible ≈ [0.452, 0.644] ใกล้กับ 95% CI [0.452, 0.648]

---

## สูตรรวมสำหรับสอบ (cheat sheet)

| เรื่อง | สูตร |
|---|---|
| Conditional | $P(A\mid B) = P(A\cap B)/P(B)$ |
| Bayes | $P(A\mid B) = P(B\mid A)P(A)/P(B)$ |
| Total probability | $P(B) = \sum_i P(B\mid A_i)P(A_i)$ |
| อิสระ | $P(A\cap B) = P(A)P(B)$ · $f_{X,Y} = f_Xf_Y$ |
| CDF | $F(x) = P(X\le x) = \int_{-\infty}^x f$, $P(a<X\le b) = F(b) - F(a)$ |
| Marginal | $f_X(x) = \int f_{X,Y}(x,y)\,dy$ |
| ผลบวก RV อิสระ | $f_{X+Y} = f_X * f_Y$ |
| Variance | $\operatorname{Var}(X) = E[X^2] - (E[X])^2$ |
| Linear | $E[aX+b] = aE[X]+b$, $\operatorname{Var}(aX+b) = a^2\operatorname{Var}(X)$ |
| Sum | $\operatorname{Var}(X+Y) = \operatorname{Var}X + \operatorname{Var}Y + 2\operatorname{Cov}(X,Y)$ |
| Covariance | $\operatorname{Cov}(X,Y) = E[XY] - E[X]E[Y]$, $\rho = \operatorname{Cov}/(\sigma_X\sigma_Y)$ |
| LLN | $\lim_{n\to\infty}P(\lvert\bar X_n - \mu\rvert < \varepsilon) = 1$ |
| CLT | $\bar X_n \approx N(\mu, \sigma^2/n)$, $S_n \approx N(n\mu, n\sigma^2)$, $Z = \frac{\bar X - \mu}{\sigma/\sqrt n}$ |
| 95% CI (โพล) | $\hat p \pm 1/\sqrt n$ (worst case) · $\hat p \pm 1.96\sqrt{\hat p(1-\hat p)/n}$ |
| Inverse transform | $x = F^{-1}(u)$, $u\sim U(0,1)$ |
| MLE | $\hat\theta = \arg\max_\theta \ln P(D\mid\theta)$ → ตั้ง $\frac{d}{d\theta}\ln L = 0$ |
| MAP | $\hat\theta = \arg\max_\theta P(D\mid\theta)P(\theta)$ |
| Posterior predictive | $P(\text{new}\mid D) = \sum_\theta P(\text{new}\mid\theta)P(\theta\mid D)$ |

ค่า Φ ที่ใช้บ่อย: $P(Z>1) \approx 0.159$ · $P(Z>2) \approx 0.023$ · $P(\lvert Z\rvert<1.96) = 0.95$ · $P(\lvert Z\rvert<2) \approx 0.954$ · $P(\lvert Z\rvert<2.576) = 0.99$

## กับดักที่พบบ่อย

1. **PDF ไม่ใช่ความน่าจะเป็น** — $f(x)$ เกิน 1 ได้, $P(X = x_0) = 0$ สำหรับ continuous
2. **Var(X − Y) ของอิสระ = Var X + Var Y** (บวก ไม่ใช่ลบ)
3. **Var(2X) ≠ Var(X₁ + X₂)** — 4Var(X) vs 2Var(X)
4. **ρ = 0 ⇏ อิสระ** — correlation วัดแค่เชิงเส้น
5. **Geometric นับจำนวนครั้งที่ล้ม** (0, 1, 2, …) — "สำเร็จครั้งที่ 3" = ล้ม 2 ครั้ง
6. **95% CI ไม่ได้แปลว่า "μ อยู่ในช่วงนี้ 95%"** — ช่วงสุ่ม μ คงที่
7. **CLT ต้องการ variance จำกัด** และ n ใหญ่พอ (≥ 30, มากกว่านั้นถ้าข้อมูลเบ้)
8. **Likelihood ไม่ใช่การแจกแจงของ θ** — ไม่ต้องรวมเป็น 1
9. **MLE ของ σ² หารด้วย n (biased)** · sample variance หารด้วย n − 1 (unbiased)
10. **P(D) ทิ้งได้ตอนหา argmax** แต่ต้องใช้ตอนคำนวณ posterior จริง (normalize ให้รวม = 1)
