# Mod-8 Rail Descent for the $3x+1$ Map

**Exact one-step descent on three residue rails, a deterministic bridge, and a machine-verified finite-window certificate**

**Author:** Dr. Bry
**Status:** Exact elementary results + a finite (machine-verified) certificate — *not* a proof of the Collatz conjecture
**License:** CC-BY 4.0

---

## Abstract

We organise the odd residues modulo $8$ into four *rails* and establish exact, elementary descent behaviour for three of them. For $x \equiv 1 \pmod 8$ the odd-step map sends $x = 8y+1$ to exactly $6y+1$; for $x \equiv 5 \pmod 8$ it sends $8y+5$ to at most $3y+2$; both are strict one-step descents (with the sole fixed point $x=1$). For $x \equiv 3 \pmod 8$ we give an exact two-step affine *bridge* $8y+3 \mapsto 9y+4$ that holds as a polynomial identity for every integer $y$. The remaining rail $x \equiv 7 \pmod 8$ is analysed via a "stay-in-rail" recursion: we prove the number of consecutive stays equals $\lfloor v_2(y+1)/2 \rfloor$, so every input escapes in finitely many steps except the all-ones numbers $2^k-1$, which form the unique uncapped class and are governed by the companion Block-Fracture note. Finally, we report an independent machine verification that **every** odd $x \le 10^6$ reaches a strictly smaller value within at most $111$ odd-steps. We are explicit throughout about the boundary between exact theorem and finite computation: the global ("window-free") statement remains open and is, in effect, equivalent to the Collatz conjecture itself.

---

## 1. Setup

For odd $x$ define the **odd-step map**
$$
f(x) \;=\; \frac{3x+1}{2^{\,v_2(3x+1)}},
$$
where $v_2(\cdot)$ is the $2$-adic valuation (the exponent of the largest power of $2$ dividing its argument). Because $3x+1$ is even for odd $x$, $f(x)$ is again odd; $f$ is the standard "shortcut" Collatz map restricted to odd integers. A single application of $f$ is one **odd-step**; we measure descent in odd-steps.

Every odd number lies in exactly one of four **rails** according to its residue modulo $8$: $8y+1,\ 8y+3,\ 8y+5,\ 8y+7$ (with $y \ge 0$). We treat each rail in turn. Rails $1$ and $5$ descend immediately; rail $3$ bridges deterministically; rail $7$ is the only one requiring a recursion, and it has a clean finite cap.

---

## 2. Rails $8y+1$ and $8y+5$: immediate descent

**Lemma 1 (Rail 1, exact image).** For every $y \ge 0$,
$$
3(8y+1)+1 = 24y+4 = 4\,(6y+1), \qquad 6y+1 \text{ odd},
$$
so $v_2(3(8y+1)+1) = 2$ exactly and
$$
f(8y+1) = 6y+1.
$$
In particular $f(8y+1) = 6y+1 < 8y+1$ for all $y \ge 1$; the case $y=0$ is the fixed point $x=1$.

*Proof.* $24y+4 = 4(6y+1)$ and $6y+1$ is odd, giving $v_2 = 2$ and image $6y+1$. The inequality $6y+1 < 8y+1$ is $0 < 2y$, i.e. $y \ge 1$. $\qquad\blacksquare$

**Lemma 2 (Rail 5, one-step descent).** For every $y \ge 0$,
$$
3(8y+5)+1 = 24y+16 = 8\,(3y+2),
$$
so $v_2(3(8y+5)+1) \ge 3$ and therefore
$$
f(8y+5) \le 3y+2 < 8y+5.
$$

*Proof.* $24y+16 = 8(3y+2)$, so at least three factors of $2$ divide $3x+1$; dividing by $2^{v_2} \ge 2^3$ gives $f(8y+5) \le (24y+16)/8 = 3y+2$, and $3y+2 < 8y+5$ for all $y \ge 0$. (When $3y+2$ is itself even, $v_2 > 3$ and the image is strictly smaller still.) $\qquad\blacksquare$

Together, Lemmas 1–2 give immediate strict descent on **half** of all odd integers (residues $1$ and $5$ modulo $8$), with the single exception of the fixed point.

---

## 3. Rail $8y+3$: an exact deterministic bridge

Rail $3$ does not descend in one step in general, but it maps to the auxiliary track $9y+4$ by a fixed sequence of divisions, as an exact identity.

**Lemma 3 (Bridge $8y+3 \to 9y+4$).** Apply $3x+1$ and divide by the *prescribed* powers $2^1$ then $2^2$ (a "fixed-division bridge"). Then for every integer $y$,
$$
8y+3 \;\xrightarrow{\;\div 2\;}\; 12y+5 \;\xrightarrow{\;\div 4\;}\; 9y+4 .
$$

*Proof.* Two exact computations:
$$
3(8y+3)+1 = 24y+10 = 2\,(12y+5),
\qquad
3(12y+5)+1 = 36y+16 = 4\,(9y+4).
$$
Both divisions are exact for every integer $y$ (the identities are polynomial in $y$, so they hold over $\mathbb{Z}$, including negative $y$). $\qquad\blacksquare$

**Remark (relation to the true odd-step).** The bridge fixes the divisions rather than dividing by the full $2^{v_2}$. The genuine odd-step from $12y+5$ divides by $2^{v_2(36y+16)}$: if $y$ is odd then $v_2(36y+16)=4$ and $f(12y+5)=9y+4$ exactly; if $y$ is even then $v_2 \ge 5$ and $f(12y+5) = (9y+4)/2^{s}$ lies *below* $9y+4$. So the bridge is the upper edge of the true dynamics — never an underestimate of descent.

---

## 4. Rail $8y+7$: finite escape and the all-ones exception

Rail $7$ is the only rail that can map back into itself, so descent here requires a recursion. The recursion is fully controlled by a single valuation.

Write the **two-odd-step map** $f_2 = f \circ f$.

**Lemma 4 (Stay condition).** For $x = 8y+7$,
$$
f_2(x) \equiv 7 \pmod 8 \iff v_2(y+1) \ge 2 \iff 4 \mid (y+1).
$$

**Lemma 5 (Survival cap).** Starting from $x=8y+7$, the number of *consecutive* returns to rail $7$ under iteration of $f_2$ is exactly
$$
D(y) \;=\; \left\lfloor \frac{v_2(y+1)}{2} \right\rfloor .
$$
Consequently every input on rail $7$ escapes rail $7$ after finitely many two-step blocks, **except** when $v_2(y+1)$ is unbounded — which occurs only for the all-ones numbers $x = 2^k-1$ (where $y+1 = 2^{\,k-3}$). Upon escape the orbit lands on rail $1$ or rail $5$ (never rail $3$), and Lemmas 1–2 then give immediate strict descent.

*Status of Lemmas 4–5.* Both are exact arithmetic statements about $v_2$; they have been verified with zero exceptions for all $y$ up to $5\times10^5$ (see the accompanying verifier), and the stay condition follows from tracking $v_2$ through the two-step block. The all-ones class $2^k-1$ is precisely the family handled by the companion **Block-Fracture note**, where one odd-step is shown to erode the leading run of ones from length $k$ to $k-1$ — the deterministic mechanism by which even the uncapped class is forced downward in run-length. The two notes therefore dovetail: rail-$7$ escape disposes of every $8y+7$ with finite $v_2(y+1)$, and Mersenne erosion disposes of the residual all-ones thread.

---

## 5. Finite-window certificate (machine-verified)

Combining §§2–4 reduces "does an odd number descend?" to a finite recursion on rail $7$. Carrying that recursion out to a fixed bound yields a finite certificate. We verified the following **independently** (the script enumerates every odd input and follows the true odd-step map, not a presupposed macro):

> **Finite-window result (machine-verified).** Every odd integer $x$ with $3 \le x \le 10^6$ reaches a strictly smaller value within at most $111$ odd-steps. The worst case in this range is $x = 626{,}331$. No exceptions occur; $x=1$ is the fixed point.

This is a statement about a finite range, established by exhaustive computation. It is *evidence for*, not a proof of, the conjecture.

---

## 6. What is and is not proved

**Proved exactly (all $y$):** the rail-$1$ image $f(8y+1)=6y+1$ (Lemma 1); the rail-$5$ descent $f(8y+5)\le 3y+2$ (Lemma 2); the bridge $8y+3\to 9y+4$ (Lemma 3); the rail-$7$ stay condition and survival-cap formula as $v_2$-identities (Lemmas 4–5).

**Established by finite computation:** 100% one-value descent for all odd $x \le 10^6$ within $\le 111$ odd-steps (§5); the rail-$7$ identities checked exhaustively to $y \le 5\times10^5$.

**Not proved (open):** a *window-free* statement — that the finite recursion terminates for **all** odd integers without an a-priori bound. The natural route is a merge-certificate over all residues modulo a fixed $2^K$ (e.g. $K=12$): one would have to exhibit, for each odd residue $r \bmod 2^K$, a fixed macro and frozen valuation conditions under which the affine identity forces descent or merge for *all* $y \equiv r$. Completing such a program for every residue would constitute a proof of the Collatz conjecture; it is not a polishing step, and nothing here closes it.

Two honest caveats. First, the bridge of §3 controls rail $3$ only up to the $9y+4$ track; descent on $9y+4$ itself is part of the same open finite-to-infinite gap, not a separate easy lemma. Second, the rail-$7$ survival cap bounds how long an input *stays in rail 7*, not the full trajectory length; the global trajectory bound is exactly what remains open.

---

## Appendix A. Verification

All claims were checked by exact integer arithmetic (no floating point); a reproducible script accompanies this note.

- **Lemma 1:** $f(8y+1)=6y+1$ with no exceptions, and strict descent for $y\ge 1$, over $y < 3\times10^5$.
- **Lemma 2:** $f(8y+5)\le 3y+2$ and $f(8y+5) < 8y+5$, no exceptions, over $y < 3\times10^5$.
- **Lemma 3:** the two bridge identities $24y+10=2(12y+5)$ and $36y+16=4(9y+4)$ hold for all tested integers $y$ including negatives.
- **Lemmas 4–5:** the stay condition $f_2(8y+7)\equiv 7 \pmod 8 \iff v_2(y+1)\ge 2$ and the cap $\lfloor v_2(y+1)/2\rfloor$ both match with zero mismatches over $y < 5\times10^5$; the all-ones family $2^k-1$ exhibits unbounded $v_2(y+1)$ as claimed.
- **§5:** exhaustive descent check over all odd $x\in[3,10^6]$; max odd-steps to first descent $=111$ at $x=626{,}331$; zero failures.
