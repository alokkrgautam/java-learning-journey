Perfect! Ab banata hoon Day 4 notes — same tone mein! 👇

---

# ☕ Java — Day 04 Notes

**Source:** Coder Army | Aditya Tandon Sir
**Topic:** Negative Numbers, 2's Complement, Floating Point Storage, Precision Errors

---

## Pehle Ye Socho — Computer Ko Negative Kaise Pata Chalega?

Kal humne data types padhe — `byte`, `int`, `float` sab। Par ek sawaal reh gaya tha —

**Jab computer sirf 0 aur 1 samjhta hai — toh `-5` kaise store karega?**

Minus ka sign toh binary mein hota hi nahi! Toh kya karte hain?

Yehi aaj samjha। 🎯

---

## Pehle Samjho — Sign Bit

Har integer type ka pehla bit **sign bit** hota hai।

```
byte = 8 bits

0 0 0 0 0 1 0 1
↑
Sign bit:
0 = Positive number
1 = Negative number
```

**Toh seedha sign bit 1 kar dein?**

`+5` = `00000101`
`-5` = `10000101` ← sirf sign bit badla

Lagta hai simple hai — but ye **galat method** hai! Kyun?

```
+5 + (-5) = 0 hona chahiye

  00000101  (+5)
+ 10000101  (-5)
──────────
  10001010  = -10 ❌ Wrong!
```

Matlab sirf sign bit badalne se kaam nahi chalta — addition hi toot jaati hai!

Isliye computers ne **2's Complement** method adopt kiya। 💡

---

## 2's Complement — Sahi Method

### Step by Step — `-5` store karna hai

**Step 1: Positive number ka binary banao**
```
+5 = 00000101
```

**Step 2: 1's Complement — har bit ulta karo**
```
00000101
↓ (0 → 1, 1 → 0)
11111010  ← 1's Complement
```

**Kyun ulta karte hain?**
Taaki baad mein addition karne par carry properly propagate ho aur result sahi aaye!

**Step 3: 2's Complement — 1 add karo**
```
  11111010
+        1
──────────
  11111011  ← Ye hai -5 ka binary!
```

**Toh memory mein `-5` store hoga = `11111011`** ✅

---

## Proof — Kya Ye Sach Mein Kaam Karta Hai?

Ab `+5` aur `-5` add karte hain — answer `0` aana chahiye:

```
  00000101  (+5)
+ 11111011  (-5)
──────────
 100000000  ← 9 bits ho gaye!
```

`byte` sirf 8 bits store karta hai — extra bit (carry) **automatically discard** ho jaati hai:

```
= 00000000 = 0 ✅ Bilkul sahi!
```

**Isliye 2's Complement use hota hai — kyunki isse addition perfectly kaam karti hai!** 🔥

---

## Byte Ka Range -128 to 127 Kyun?

Ab ye bhi clear ho jaata hai —

```
byte = 8 bits
├── 1 bit → sign ke liye
└── 7 bits → actual data ke liye

Positive max: 0 1111111 = +127
Negative max: 1 0000000 = -128
```

**Kyun -128 aur +127 — symmetric kyun nahi?**

`10000000` ka 2's complement nikalo toh wapas `10000000` hi aata hai — iska positive version 8 bits mein fit nahi hota! Isliye `-128` ka koi `+128` nahi hota `byte` mein।

Yehi wajah hai range **-128 to 127** hai — symmetric nahi! 🎯

---

## Floating Point Numbers Kaise Store Hote Hain?

Ab `3.14` ya `9.8` jaisi decimal values ka sawaal —

Computer binary mein decimal kaise rakhe? Seedha nahi ho sakta — isliye **IEEE 754 Standard** follow kiya jaata hai।

### Float = 32 bits — 3 parts mein divide

```
32 bits:
┌─────┬──────────┬───────────────────────┐
│  1  │    8     │          23           │
│Sign │ Exponent │       Mantissa        │
└─────┴──────────┴───────────────────────┘
```

### Double = 64 bits — zyada precision

```
64 bits:
┌─────┬──────────┬───────────────────────────┐
│  1  │    11    │            52             │
│Sign │ Exponent │         Mantissa          │
└─────┴──────────┴───────────────────────────┘
```

**Zyada bits = zyada accurate** — isliye `double` zyada precise hota hai!

---

## Normalization — Scientific Notation Jaisa

Floating point store karne se pehle number ko **normalize** karte hain — jaise maths mein scientific notation:

```
Decimal: 12345.678 = 1.2345678 × 10⁴

Binary:  1101.01   = 1.10101 × 2³
```

**Kyun normalize karte hain?**
- Ek standard form mein store karna easy hota hai
- Binary mein pehla bit hamesha `1` hota hai — toh use store hi nahi karte! (1 bit bachta hai = zyada precision)

---

## Bias Kya Hai? — Kyun Zaroori Hai?

Exponent positive bhi ho sakta hai, negative bhi:

```
1.5 × 2³  → exponent = +3
1.5 × 2⁻³ → exponent = -3
```

**Problem:** Negative exponent kaise store karein binary mein?

**Solution — Bias add karo!**

```
float mein Bias = 127

Store karna hai exponent = +3
Actually store karte hain = 3 + 127 = 130

Retrieve karte waqt = 130 - 127 = 3 ✅
```

**Kyun bias?**
Taaki exponent hamesha **positive number** ke roop mein store ho — alag se negative handling nahi karni padti! Simple aur efficient। 💡

---

## Precision Error — Jab Computer Math "Fail" Karta Hai

Ye **sabse interesting aur important concept** hai — interviews mein puchha jaata hai!

```java
float a = 0.1f + 0.2f;
System.out.println(a);
// Expected: 0.3
// Actual:   0.30000001 ❌
```

**Kyun hota hai ye?**

`0.1` ko binary mein **exactly represent nahi kar sakte** —

Jaise `1/3 = 0.333...` decimal mein kabhi khatam nahi hota, waise hi:

```
0.1 (decimal) = 0.000110011001100... (binary) — infinite!
```

Memory limited hai — toh ek point par **truncate** karna padta hai → wahan se precision error aa jaata hai!

**Real life impact:**
- Bank balance calculate karo `float` se → galat answer aa sakta hai!
- Isliye financial applications mein `BigDecimal` class use hoti hai

---

## Normal vs Edge Cases

```java
System.out.println(1.0 / 0.0);   // Infinity
System.out.println(-1.0 / 0.0);  // -Infinity
System.out.println(0.0 / 0.0);   // NaN (Not a Number)
```

| Case | Kab hota hai | Output |
|------|-------------|--------|
| Normal | Regular calculations | Normal number |
| Infinity | Koi number / zero | Infinity |
| NaN | 0.0 / 0.0 | NaN |
| Zero | Special representation | 0.0 |

---

## Poora Picture Ek Saath

```
Number Storage in Java
│
├── Negative Integers → 2's Complement
│   ├── Step 1: Positive ka binary banao
│   ├── Step 2: 1's Complement (bits flip karo)
│   └── Step 3: +1 karo → 2's Complement ready!
│
└── Floating Point → IEEE 754
    ├── Sign bit (1 bit)
    ├── Exponent + Bias (8/11 bits)
    ├── Mantissa — normalized (23/52 bits)
    └── ⚠️ Precision error possible!
```

---

## Quick Revision

| Sawaal | Jawab |
|--------|-------|
| Negative number store karne ka method? | 2's Complement |
| 1's Complement kya hai? | Saare bits ulte karo |
| 2's Complement kya hai? | 1's Complement + 1 |
| 2's Complement kyun use karte hain? | Kyunki addition correctly kaam karti hai |
| Byte ka range kyun -128 to 127? | 1 bit sign ke liye, 7 bits data — asymmetric |
| Float kitne bits? | 32 bits |
| Double kitne bits? | 64 bits |
| Bias kyun use hota hai? | Negative exponent ko positive mein store karne ke liye |
| Precision error kyun aata hai? | Binary mein exact decimal represent nahi hota |
| Exact values ke liye? | BigDecimal class use karo |

---

## Ek Line Mein Poora Concept

> **Negative numbers 2's Complement mein store hote hain — kyunki tabhi addition sahi kaam karti hai। Aur floating point IEEE 754 mein store hota hai — jisme precision error isliye aata hai kyunki binary mein kuch decimals infinite hote hain par memory limited hai!** 🔥

---

*Day 05 → Operators in Java* 🚀

---

GitHub update karo aur LinkedIn post chahiye toh bol do! 🔥
