# Day 7 — Flow of Control: Selection Statements

## 1. What is "Flow of Control"?

Normally, a program runs **line by line, top to bottom**, in the order it's written.

```java
{
    int i = 4;
    System.out.println(i);
    int j = 5;
}
```

But sometimes we need to **change this order** — skip some lines, repeat some lines, or jump somewhere else. This control over the order of execution is called **Flow of Control**.

Flow of Control has 3 main types:

```
Flow of Control
   ├── Selection   →  if(), if-else(), switch()
   ├── Iteration   →  loops (for, while, etc.)
   └── Jumps       →  break, continue, return
```

Today's notes focus on **Selection** statements.

---

## 2. The `if` Statement

```java
if (expression) {
    // do something
}
```

- The `expression` must evaluate to a **boolean** value → `true` or `false`.
- If it's `true`, the code inside `{ }` runs.
- If it's `false`, the code inside `{ }` is **skipped**.

Example:

```java
boolean b = false;

if (b == true) {
    // do something
}
```

---

## 3. The `if-else` Statement

When we want to do one thing if the condition is true, and a **different thing** if it's false:

```java
if (b == true) {
    // do something
} else {
    // do something else
}
```

So basically:
- `if` block → runs when condition is **true**
- `else` block → runs when condition is **false**

---

## 4. `if-else-if` Ladder

When there are **multiple conditions** to check one after another:

```java
int i = 2;

if (i == 1) {
    // do something
} else if (i == 2) {
    // do something
} else if (i == 3) {
    // do something
} else {
    // do something
}
```

Each condition is checked in order until one is `true`. If none match, the final `else` runs.

---

## 5. Switch Statement

A cleaner way to write an `if-else-if` ladder when you're comparing **one variable** against multiple fixed values.

```java
int i = 2;

switch (i) {
    case 1:
        // do something
        break;
    case 2:
        // do something
        break;
    case 3:
        // do something
        break;
    default:
        // do something
        break;
}
```

⚠️ Don't forget `break;` — otherwise execution will "fall through" into the next case.

---

## 6. Switch vs if-else-if Ladder

| Point | Switch | if-else-if Ladder |
|---|---|---|
| 1 | Can only test **equality** (`==`) | Can test **equality AND inequality** (`<`, `>`, `!=`, etc.) |
| 2 | **More efficient** | Less efficient compared to switch |

So: switch is faster but less flexible. if-else is more flexible but relatively slower.

---

## 7. Why is Switch More Efficient? → Jump Tables

Internally, switch doesn't check each case one by one like if-else does. Instead, it often uses a **jump table** — basically an array where each index directly points to the matching case code.

```
i → [2]

Jump Table:
| case 1 | case 2 | case 3 | case 4 |
    0        1        2        3
```

Since the value of `i` is used directly as an **index**, the program jumps straight to the right case — no need to compare one by one. That's what makes switch fast.

---

## 8. Jump Tables Are NOT Always Efficient

If the `case` values are **spread far apart** (sparse), a simple jump table wastes a lot of memory with empty/unused slots.

```java
switch (i) {
    case 1:
        ...
    case 1000:
        ...
    case 100000:
        ...
}
```

Here, a direct jump table would need **100,000 slots**, even though only 3 are actually used. Most of them stay `null`/empty. This is called the **sparse values** problem.

To handle this, Java actually uses **two different strategies** depending on the case values:

### a) Table Switch
- Used when case values are **close together / sequential** (e.g., 0, 1, 2, 3).
- Works like a direct-index array → very fast.

```
| case0 | case1 | case2 | case3 |
    0       1       2       3
```

### b) Lookup Switch
- Used when case values are **sparse / far apart** (e.g., 1, 1000, 100000).
- Stores only the actual `(case, jump-location)` pairs and searches for a match (like a key-value lookup) instead of building a huge array.

```
| case 1 | case 1000 | case 100000 |
    0          1            2
```

This is more **memory-efficient** for spread-out values, even if slightly slower than a table switch.

---

## 9. Nested Statements

Just like `if` can be nested inside another `if`, a `switch` can also be nested inside another `switch`.

```java
if () {
    if () {
        // inner if
    }
}
```

```java
switch () {
    switch () {
        // inner switch
    }
}
```

---

## Quick Recap

- **Flow of Control** = Selection + Iteration + Jumps
- **if / if-else / if-else-if** → flexible, checks equality & inequality
- **switch** → faster, but only checks equality
- Switch is fast because of **jump tables** (direct index lookup)
- For sparse case values, Java uses a **lookup switch** instead of a **table switch** to save memory
- Both `if` and `switch` can be **nested**
