# The earliest start for a negative Mertens average

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.24411641.svg)](https://doi.org/10.5281/zenodo.24411641)

This result is now maintained in [quadratic-mertens-first-crossings](https://github.com/leecrossley/quadratic-mertens-first-crossings/tree/main/results/yokoi-mertens-optimal-starts). This repository is archived and keeps its releases and DOI.

In the field $\mathbb Q(\sqrt{2{,}380{,}853})$, the prime-ideal Mertens product first dips below its main term at about 143,054.04. This first crossing is certified directly. To exclude all earlier starts for a negative weighted average, including weights extending to infinity, the tail bound assumes GRH for $\zeta(s)L(s,\chi_d)$. The infimum of all successful starting points lies between **137,123.79815 and 137,123.79818**. Every earlier start at or below the lower endpoint fails for every bounded nonnegative decreasing weight, however far the average runs.

The field gives the unique largest value of $\log Y_d/\log d$ across all 8,950 squarefree $d=m^2+4$ with odd $m\le20001$, where $Y_d$ is the starting-point infimum. Its exponent is between 0.80560267 and 0.80560269. Each other field in that range has a successful start below $d^{4/5}$. This is a finite obstruction to the coefficient-one bound $Y_d\le d^{4/5}$. The asymptotic question for sufficiently large fields remains open.

Two interval-arithmetic implementations recover the same minimum at prime-ideal norm 154,877. They check 3,554,753 norm jumps through 125 million. An explicit formula bounds the total area that can be lost between that cutoff and **any** later point. For $d=9{,}413$ the same method puts the infimum between 337.87434 and 337.87436.

The tail estimate also has a general consequence. For any real quadratic field satisfying this GRH hypothesis, a negative decreasing-weight average from a start $y\le d^a$ implies a first crossing by $C_a d^{2a}$, regardless of how slight the negative average is. Finding such a start for every field would suffice to obtain a polynomial first-crossing bound. [Proof](PROOF.md) [Prior work](PRIOR-WORK.md) [Full census](crossings.txt)

## Reproducing the numbers

Use a C++17 compiler and Python with python-flint 0.9.0. Run in the release directory.

~~~sh
shasum -a 256 -c SHA256SUMS
work=$(mktemp -d)
c++ -O3 -std=c++17 class_numbers.cpp -o "$work/classes"
"$work/classes" 20001 > "$work/class-numbers.txt"
cmp class-numbers.txt "$work/class-numbers.txt"
python3 -B verify.py "$work/class-numbers.txt" --crossings "$work/crossings.txt"
cmp crossings.txt "$work/crossings.txt"
python3 -B optimal_starts.py
python3 -B independent_windows.py
~~~

The census enumeration uses integer cycles of reduced quadratic irrationals. The two record calculations use Arb interval arithmetic. Three auxiliary inequalities have Lean checks using the pinned Lean toolchain and [Mathlib revision](https://github.com/leanprover-community/mathlib4/tree/0df444a360eaa60ab8c11dca51a86af692955474). Set NOTE to the absolute release directory, change to the Mathlib checkout and run the following.

~~~sh
lake env lean -R "$NOTE" "$NOTE/TailLoss.lean"
~~~

The tail estimate is a written analytic proof using the critical-line theorem for the two Dirichlet factors of the quadratic Dedekind zeta function. [Primary output](evidence/optimal.txt) · [Independent output](evidence/independent.txt) · [Census verification](evidence/census.txt) · [Lean axioms for the three auxiliary inequalities](evidence/lean.txt) · [Upstream audit](https://math.mit.edu/~drew/grh/)
