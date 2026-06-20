# Day 8 — Jump Statements: `break` and `continue`

Yesterday we covered **Selection** (if, if-else, switch). Today we look at **Jumps** — specifically `break` and `continue`, which are used to control the flow **inside loops**.

```
Flow of Control
   ├── Selection   →  if(), if-else(), switch()
   ├── Iteration   →  loops (for, while, etc.)
   └── Jumps       →  break, continue, return   ← today
```

---

## 1. `break` Statement

`break` **immediately stops the loop** and jumps to the code right after it — no more iterations happen, even if the loop condition was still true.

```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;   // loop stops completely here
    }
    System.out.println(i);
}
```

**Output:**
```
1
2
3
4
```

Once `i` becomes `5`, the loop **exits entirely**. It never even gets to `i = 6, 7, ...`.

### Where else does `break` show up?
You already saw it in `switch`:

```java
switch (i) {
    case 1:
        // do something
        break;   // exits the switch block
}
```

Same idea — `break` exits whatever block (loop or switch) it's inside.

---

## 2. `continue` Statement

`continue` does **not** stop the loop. It only **skips the current iteration** and moves on to the **next** one.

```java
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        continue;   // skip just this iteration
    }
    System.out.println(i);
}
```

**Output:**
```
1
2
3
4
6
7
8
9
10
```

Notice `5` is missing, but the loop **keeps going** after it — unlike `break`, which would have stopped completely.

---

## 3. `break` vs `continue` — Key Difference

| Point | `break` | `continue` |
|---|---|---|
| Effect | **Exits** the loop completely | **Skips** only the current iteration |
| Remaining iterations | Stopped | Continue normally |
| Used in | loops, switch | loops only |

A simple way to remember:
- `break` = "stop, I'm done with this loop"
- `continue` = "skip this one, but keep going"

---

## 4. `break` and `continue` in `while` loops

Same behavior applies to `while` and `do-while` loops too.

```java
int i = 0;
while (i < 10) {
    i++;
    if (i == 5) {
        continue;   // skips printing 5, loop still runs
    }
    if (i == 8) {
        break;      // loop stops completely at 8
    }
    System.out.println(i);
}
```

**Output:**
```
1
2
3
4
6
7
```

Walkthrough:
- At `i = 5` → `continue` skips the print, but loop carries on.
- At `i = 8` → `break` stops the loop entirely, so `8, 9, 10` never print.

---

## 5. `break` and `continue` with Nested Loops

By default, `break` and `continue` only affect the **innermost loop** they are written in.

```java
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        if (j == 2) {
            break;   // breaks only the inner (j) loop
        }
        System.out.println(i + " " + j);
    }
}
```

**Output:**
```
1 1
2 1
3 1
```

The outer `i` loop keeps running normally — only the inner `j` loop gets cut short each time.

### Labeled `break` / `continue`
If you actually want to break out of an **outer** loop from inside a nested loop, Java allows **labels**:

```java
outer:
for (int i = 1; i <= 3; i++) {
    for (int j = 1; j <= 3; j++) {
        if (j == 2) {
            break outer;   // breaks the OUTER loop, not just inner
        }
        System.out.println(i + " " + j);
    }
}
```

**Output:**
```
1 1
```

This is a bit advanced and not commonly needed, but good to know it exists.

---

## Quick Recap

- `break` → completely exits the loop (or switch)
- `continue` → skips just the current iteration, loop keeps running
- Both normally affect only the **innermost** loop
- Labeled `break`/`continue` can target an outer loop when needed
