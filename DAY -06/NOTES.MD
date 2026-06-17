# ☕ Java Full Stack – 100 Days Challenge
## Day 6: Java Operators — Complete Notes

**Date:** June 17, 2026
**Topics:** Arithmetic · Assignment · Relational · Logical · Precedence · Edge Cases · Truth Tables

---

## What Are Operators?

Operators are special symbols that tell the JVM to perform a specific operation on one or more values (called **operands**) and produce a result.

- **Unary operator** → acts on 1 operand → `!x`, `-x`, `++x`
- **Binary operator** → acts on 2 operands → `a + b`, `x == y`
- **Ternary operator** → acts on 3 operands → `condition ? val1 : val2`

Java operators fall into 4 main categories today: **Arithmetic, Assignment, Relational, Logical.**

---

## 1. Arithmetic Operators

Used to perform mathematical calculations. The result type depends on operand types.

| Operator | Name | Example | Result |
|----------|------|---------|--------|
| `+` | Addition | `10 + 3` | `13` |
| `-` | Subtraction | `10 - 3` | `7` |
| `*` | Multiplication | `10 * 3` | `30` |
| `/` | Division | `10 / 3` | `3` ⚠️ |
| `%` | Modulo (Remainder) | `10 % 3` | `1` |

### ⚠️ Critical: Integer Division Trap

When **both operands are `int`**, Java performs **integer division** — the decimal part is **truncated, not rounded**.

```java
int a = 7, b = 2;

int wrong  = a / b;          // 3  ← decimal part lost!
double fix = (double) a / b; // 3.5 ← cast one side first
```

> **Rule:** If you need a decimal result, cast at least one operand to `double` before dividing.

### String Concatenation with `+`

The `+` operator is overloaded — when used with a `String`, it **concatenates** instead of adding.

```java
String msg = "Score: " + 95;   // "Score: 95"
int x = 5 + 3;                 // 8   (both ints → addition)
String s = "Val: " + 5 + 3;   // "Val: 53"  ← watch order!
String t = "Val: " + (5 + 3); // "Val: 8"   ← parentheses fix it
```

### Modulo (`%`) — More Than Just Remainder

```java
// Even / Odd check
boolean isEven = (number % 2 == 0);

// Circular index — wrap around an array without if-check
index = (index + 1) % arraySize;

// Extract last digit
int lastDigit = number % 10;

// Check divisibility
boolean divisibleBy5 = (number % 5 == 0);
```

---

## 2. Assignment Operators

Used to assign a value to a variable. Compound forms perform an operation AND assign in one step.

| Operator | Meaning | Example | Equivalent to |
|----------|---------|---------|---------------|
| `=` | Simple assign | `x = 5` | `x = 5` |
| `+=` | Add & assign | `x += 3` | `x = x + 3` |
| `-=` | Subtract & assign | `x -= 3` | `x = x - 3` |
| `*=` | Multiply & assign | `x *= 3` | `x = x * 3` |
| `/=` | Divide & assign | `x /= 3` | `x = x / 3` |
| `%=` | Modulo & assign | `x %= 3` | `x = x % 3` |

```java
int score = 100;

score += 25;  // 125 — bonus points added
score -= 10;  // 115 — penalty applied
score *= 2;   // 230 — double XP event
score /= 4;   //  57 — integer truncation applies here too!
score %= 10;  //   7 — only the last digit remains
```

### ⚠️ Implicit Narrowing Cast with Compound Assignment

This is a subtle but important Java behaviour:

```java
byte b = 10;

b += 5;       // ✅ Compiles fine — compiler inserts cast automatically
b = b + 5;    // ❌ Compile error! b + 5 widens to int, can't fit back in byte
b = (byte)(b + 5); // ✅ Manual cast fixes it
```

> Compound operators (`+=`, `-=`, etc.) automatically include a hidden narrowing cast. Simple assignment does not.

---

## 3. Relational (Comparison) Operators

Used to compare two values. **Always return a `boolean` — either `true` or `false`.**
This is the language of conditions and control flow.

| Operator | Meaning | Example | Result |
|----------|---------|---------|--------|
| `==` | Equal to | `5 == 5` | `true` |
| `!=` | Not equal to | `5 != 3` | `true` |
| `>` | Greater than | `7 > 3` | `true` |
| `<` | Less than | `2 < 1` | `false` |
| `>=` | Greater than or equal to | `5 >= 5` | `true` |
| `<=` | Less than or equal to | `4 <= 3` | `false` |

```java
int age = 20;

System.out.println(age == 20);  // true
System.out.println(age != 18);  // true
System.out.println(age > 18);   // true
System.out.println(age < 18);   // false
System.out.println(age >= 20);  // true  — boundary included
System.out.println(age <= 19);  // false
```

### ⚠️ Critical: `==` vs `.equals()` for Strings

This is one of the most common beginner bugs in Java:

- For **primitives** (`int`, `char`, etc.) → `==` compares **values** ✅
- For **objects** (including `String`) → `==` compares **memory addresses** (references), NOT content ❌

```java
String s1 = new String("Java");
String s2 = new String("Java");

System.out.println(s1 == s2);        // false ← different objects in memory
System.out.println(s1.equals(s2));   // true  ← content is the same ✅

// Exception: String literals share the pool
String a = "Java";
String b = "Java";
System.out.println(a == b);   // true ← same object from String pool
```

> **Rule:** Always use `.equals()` to compare String content. Never rely on `==` for Strings.

---

## 4. Logical Operators

Used to combine multiple boolean expressions. These are the backbone of every condition block.

| Operator | Name | Returns `true` when... |
|----------|------|------------------------|
| `&&` | Logical AND | **Both** sides are true |
| `\|\|` | Logical OR | **At least one** side is true |
| `!` | Logical NOT | The operand is false (flips the value) |

### Truth Table — AND (`&&`)

| A | B | A && B |
|---|---|--------|
| true | true | **true** |
| true | false | false |
| false | true | false |
| false | false | false |

### Truth Table — OR (`||`)

| A | B | A \|\| B |
|---|---|---------|
| true | true | **true** |
| true | false | **true** |
| false | true | **true** |
| false | false | false |

### NOT (`!`)

```java
boolean isActive = true;
boolean inactive = !isActive;  // false

boolean isDone = false;
boolean notDone = !isDone;     // true
```

### Code Example

```java
int marks = 75;
boolean hasAttendance = true;
boolean isLateSubmit  = false;

// AND: both must be satisfied
boolean passes  = (marks >= 40) && hasAttendance;  // true

// OR: either condition is enough
boolean flagged = (marks < 40) || isLateSubmit;    // false

// NOT: invert the result
boolean clean   = !flagged;                        // true
```

---

## 5. Short-Circuit Evaluation ⚡

This is one of the most important concepts for writing safe, efficient code.

### `&&` Short-Circuits on `false`

If the **left side is false**, Java **skips the right side entirely** — the result is already determined.

```java
// Safe null check using short-circuit:
String name = null;
boolean valid = (name != null) && (name.length() > 0);
//              ↑ false here → right side never runs → no NullPointerException!
```

### `||` Short-Circuits on `true`

If the **left side is true**, Java **skips the right side entirely**.

```java
boolean hasAccess = isAdmin || checkDatabase();
// If isAdmin is true → checkDatabase() is NEVER called (saves a DB query!)
```

> **Why it matters:** Short-circuit prevents crashes (null checks), avoids unnecessary computation, and is used constantly in real-world code. This is an interview favourite.

---

## 6. Operator Precedence

When multiple operators appear in one expression, Java evaluates them in this order:

| Priority | Operators | Associativity |
|----------|-----------|---------------|
| 1 (Highest) | `()` `[]` `.` | Left → Right |
| 2 | `++` `--` `!` `~` (unary) | Right → Left |
| 3 | `*` `/` `%` | Left → Right |
| 4 | `+` `-` | Left → Right |
| 5 | `<` `>` `<=` `>=` | Left → Right |
| 6 | `==` `!=` | Left → Right |
| 7 | `&&` | Left → Right |
| 8 | `\|\|` | Left → Right |
| 9 (Lowest) | `=` `+=` `-=` `*=` `/=` `%=` | Right → Left |

```java
int result = 2 + 3 * 4;    // 14, NOT 20 — * before +
int fixed  = (2 + 3) * 4;  // 20 — parentheses override everything

boolean check = 5 > 3 && 10 != 5;
// Step 1: 5 > 3   → true
// Step 2: 10 != 5 → true
// Step 3: true && true → true
```

> **Golden Rule:** When unsure about precedence, **add parentheses.** It makes intent clear and prevents bugs.

---

## 7. Real-World Mental Models

| Scenario | Operator | Code |
|----------|----------|------|
| Add item to cart | `+=` | `total += itemPrice` |
| Apply discount | `*=` | `total *= 0.9` |
| Login: username AND password match | `&&` | `user.equals(db) && pass.equals(hash)` |
| Access: admin OR VIP | `\|\|` | `isAdmin \|\| hasVipPass` |
| OTP verification | `==` | `enteredOtp == generatedOtp` |
| Even/odd check | `%` | `number % 2 == 0` |
| Wrap array index | `%` | `(index + 1) % size` |
| Null-safe method call | `&&` short-circuit | `obj != null && obj.isActive()` |

---

## 8. Common Mistakes to Avoid

**1. Using `=` instead of `==` in conditions**
```java
if (x = 5)  { }  // ❌ Compile error — assignment, not comparison
if (x == 5) { }  // ✅ Correct
```

**2. Integer division returning 0**
```java
double avg = 1 / 4;          // 0.0 — both ints, result is 0 before cast
double avg = 1.0 / 4;        // 0.25 ✅
double avg = (double) 1 / 4; // 0.25 ✅
```

**3. String comparison with `==`**
```java
if (input == "yes") { }        // ❌ Compares references
if (input.equals("yes")) { }   // ✅ Compares content
```

**4. Forgetting short-circuit order**
```java
// WRONG order — crashes if list is null:
if (list.size() > 0 && list != null) { }

// RIGHT order — null check first:
if (list != null && list.size() > 0) { }
```

---

## 9. Quick Reference Cheatsheet

```
ARITHMETIC     +   -   *   /   %
ASSIGNMENT     =   +=  -=  *=  /=  %=
RELATIONAL     ==  !=  >   <   >=  <=
LOGICAL        &&  ||  !

KEY RULES:
• int / int  = int (truncated, not rounded)
• Cast one side: (double) a / b  → real division
• String content: use .equals(), never ==
• && stops at first false (short-circuit)
• || stops at first true  (short-circuit)
• Parentheses always override precedence
```

---

## Progress Tracker

- [x] Arithmetic operators — all 5, integer division trap, modulo use cases
- [x] Assignment operators — compound forms, implicit narrowing cast
- [x] Relational operators — all 6, `==` vs `.equals()` for Strings
- [x] Logical operators — AND / OR / NOT, truth tables, short-circuit evaluation
- [x] Operator precedence table
- [x] Common mistakes and mental models
- [ ] Practice edge cases in IDE (overflow, casting, short-circuit side effects)
- [ ] Day 7 — Conditional Statements: `if / else if / else / switch`

---

*Day 6 / 100 — Java Full Stack Challenge | Arya · GEC Sheikhpura · BEU Patna*
