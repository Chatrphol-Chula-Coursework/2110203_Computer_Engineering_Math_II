# CEMII Lab — 2110203 Visualizers

เปิด `index.html` ในเบราว์เซอร์ → หน้ารวมที่แบ่งตาม part ของวิชา กดเลือก lecture แล้วหน้านั้นจะเปิดในหน้าเดียวกัน

## โครงสร้าง

```
Visualizer Web/
├── index.html                          ← หน้ารวม (เมนู + รายการตาม part)
├── Signal Part I/
│   ├── convolution-lab.html            ← Lecture 02 Convolution
│   ├── fourier-ct.html                 ← Lecture 03 Fourier (เวลาต่อเนื่อง)
│   ├── fourier-dt.html                 ← Lecture 04 DTFS / DTFT / DFT / FFT
│   ├── sampling-filtering.html         ← Lecture 05 Sampling & Filtering
│   └── quantum-fourier.html            ← เสริม: Quantum Fourier Transform (ต่อจาก Lecture 04)
├── Optimization/
│   ├── exam-2020.html                  ← เฉลย Quiz 1 2020/1 ครบ 8 ข้อ (ทีละขั้น)
│   ├── exam-2021.html                  ← เฉลย Quiz 1 2021 (3 ข้อ formulation)
│   └── exam-2025.html                  ← เฉลย Quiz 2025/1 ข้อ 6–7 (integer program)
└── Prob & Stat/
    ├── probability-review.html         ← Stats Lecture 0 Overview & Probability Review
    ├── lln-clt.html                    ← Stats Lecture 1 LLN & CLT (+ confidence interval)
    └── estimation.html                 ← Stats Lecture 1B Sampling & Estimation (MLE / MAP)

../Midterm Signals/index.html           ← เฉลย Midterm Quiz 2 (ลิงก์จาก Signal Part I ด้วย path)
```

แต่ละไฟล์ `.html` เป็นไฟล์เดียวจบ (CSS + JS อยู่ในตัว) เปิดเดี่ยว ๆ ก็ได้ กราฟทุกอันคำนวณสดจากสูตร ไม่ได้ฝังรูปไว้

## เพิ่ม lecture ใหม่

1. วางไฟล์ `.html` ของ lecture ไว้ในโฟลเดอร์ของ part นั้น เช่น `Optimization/simplex.html`
2. เปิด `index.html` แล้วเพิ่ม object ใน `lectures` ของ part นั้นใน `PARTS`:

```js
{ id: 'simplex', no: '07', title: 'Simplex', th: 'ซิมเพล็กซ์', file: 'simplex.html',
  desc: 'คำอธิบายสั้น ๆ', tags: ['แท็ก 1', 'แท็ก 2'] }
```

3. part ใหม่ = เพิ่ม object ใน `PARTS` (`folder` คือชื่อโฟลเดอร์, `short` คือชื่อสั้นบนแถบเมนู)

ลิงก์ออนไลน์ (Claude artifact) ใช้หน้าเดียวกันนี้ แต่รวมไฟล์ lecture ไว้ข้างในแล้ว จึงเปิดได้จากลิงก์เดียว

## ที่มาของตัวเลขในแต่ละหน้า

| หน้า | โจทย์/ค่าที่ตรวจกับสไลด์แล้ว |
| --- | --- |
| Convolution | Ex 2.1 `y = {1,5,5,−5,−6,4,1,−2}` · Ex 2.2 `y[2] = 0.5278` · Ex 2.5 `y(−0.8) = 1.2` · Ex 2.6 `y(1.2) = 0.251` |
| Fourier CT | คลื่นสี่เหลี่ยม `a₁ = −j2/π` · Ex 3.2 `a₀ = 1, a₁ = 1 − 0.5j, a₂ = 0.5∠π/4` |
| Fourier DT | `x = {1,2,0,−1}` → `X[k] = {2, 1−3j, 0, 1+3j}` · circular conv. `{−6, 6, 7, −5}` |
| Quantum Fourier | วงจร H + R<sub>k</sub> + swap ตรงกับสูตร `y_k = (1/√N) Σ x_n e^{j2πkn/N}` ทุก input (q = 1–6, ต่างกัน < 1e−14) · 15 = 3 × 5 ด้วย a = 7, r = 4 |
| Sampling | `f = 13 Hz, fs = 10 Hz → fp = 3 Hz` · เสียง 5 kHz → `ωs = 62832 rad/s, T = 0.1 ms` |
| Optim · Quiz 2020 | Q3 ช่องว่าง `a = 12, y = −8`, optimal `(5,12), z = 44` · Q4 `z = 5` ที่ `(2,0)` + alternative optima ถึง `(0,5)` · Q5 `z = 20` ที่ `(0,4)` · Q6 `0.61 ฿/g` · Q7 `฿15,500` · ตรวจด้วย scipy `linprog` |
| Optim · Quiz 2021 | คนงาน `h₁ = 54.63, u₂ = 22.22`, ต้นทุน `1,831,759` (จ้างครบเมื่อค่าปรับ > 16,033) · แผนเงินเดือน A `d = 930, e = 100, ฿13,300` |
| Stats · Review | coin `P(H)=0.7`, 10 ครั้ง → `P(X=6) = 0.2001` · ยา COVID 9/40 vs 10/40 · ลอตเตอรี่ `E ≈ 10¹⁰ − 10` · Anscombe ρ = 0.816 ทั้ง 4 ชุด |
| Stats · LLN & CLT | เหรียญ 100 ครั้ง `P(S>55) ≈ P(Z>1) ≈ 0.16` (ค่าตรง 0.1356) · Maxwell `P(S>60) ≈ 0.023` (ค่าตรง 0.0176) · Ham-Hams 240/400 → 95% CI `[0.55, 0.65]` |
| Stats · Estimation | `f(x)=2x → F⁻¹(u)=√u` · Exp `u=0.632 → x=1` · Maxwell 55/100 → `p̂=0.55` · เครื่องจักร 2,3,1,3,4 → `λ̂=5/13` · Cappy 0,0,2 → `p̂=3/5` · ลูกโอ๊ก posterior `0.0954/0.5825/0.3221`, MAP `0.5`, predictive `0.4964` |
| Optim · Quiz 2025 | เลือกตั้ง `248` ที่นั่ง (ใช้ 996 ล้าน) · ชั้นวางตัวอย่าง 7.1–7.4 = `50 / 55.5 / 100 / 104` จาก scipy `milp` (ตัวเลขสินค้าเป็นตัวอย่างสมมติ) |
