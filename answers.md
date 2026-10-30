# CMPS 2200 Assignment 4
## Answers

**Name:**_Srikanya Balaji Garuda________________________

Place all written answers from `assignment-04.md` here for easier grading.


**1 Quicksort probabilities**

- **1a. (5 pts)** What is the probability that Quicksort does $\Omega({n^2})$
  comparisons? 
  Expected Work: From lecture, Quicksort's expected work is $\mathbf{E}[X] = c_1 n \log n$ for some constant $c_1$.
  Markov's Inequality: $\mathbf{P}[X \ge \alpha] \le \frac{\mathbf{E}[X]}{\alpha}$
  Calculation: Setting $\alpha = c_2 n^2$ to find the probability of $\Omega(n^2)$ work:
  $$\mathbf{P}[X \ge c_2 n^2] \le \frac{c_1 n \log n}{c_2 n^2} = O\left(\frac{\log n}{n}\right)$$
  Conclusion: As $n$ grows large, $\frac{\log n}{n}$ approaches $0$, meaning the probability of Quicksort taking $O(n^2)$ work is practically zero.




- **1b. (5 pts)** What is the probability that Quicksort does $10^c n \lg n$
comparisons, for a given $c>0$? What does this say about the
deviation of the actual work from the expected work for Quicksort?
Calculation: Using $\mathbf{E}[X] = C \cdot n \log n$ and $\alpha = 10^c n \log n$:
$$\mathbf{P}[X \ge 10^c n \log n] \le \frac{C \cdot n \log n}{10^c n \log n} = \frac{C}{10^c} = O(10^{-c})$$
Because the formula ends up being $10^{-c}$, as $c$ gets bigger, the chance of a massive slowdown shrinks super fast. This shows that Quicksort is consistent and doesn't really take much longer than its expected time.

**2 From "Maybe" to "Definitely"**

- **2a. (10 pts)** Give an algorithm to improve the guaranteed success probability and state its work.
First we run $\mathcal{A}(\mathcal{I})$ and verify it using $\mathcal{C}$. If it fails, repeat independently up to $k$ times until a run is verified as a correct answer.
($k$ is the number of tries). A single run fails with probability $1 - \epsilon$. Running it $k$ times means all runs fail with probability $(1 - \epsilon)^k$. To guarantee a success probability of at least $\delta$:
$$(1 - \epsilon)^k \le 1 - \delta \implies k = \left\lceil \frac{\ln(1 - \delta)}{\ln(1 - \epsilon)} \right\rceil$$
Total Work: Each run takes $O(w(n))$ work. Multiplying by $k$ trials gives:
$$O\left( w(n) \cdot \frac{\ln(1 - \delta)}{\ln(1 - \epsilon)} \right)$$



- **2b. (6 pts)** Show how to convert $\mathcal{A}$ into an
  algorithm that always produces the correct result, but has an
  expected runtime that depends on $w(n)$ and a success probability
  $\epsilon$.
(Las Vegas Algorithm from class): Continuously run $\mathcal{A}(\mathcal{I})$ and check the result with $\mathcal{C}$ until verification returns True, then return that output.
Expected Work: Since each run succeeds with probability $\epsilon$, the number of tries will have an expected value of $\frac{1}{\epsilon}$. Multiplying by the work per run ($O(w(n))$), the total expected work is:$$O\left( \frac{w(n)}{\epsilon} \right)$$

**3 Determinism versus Randomization in Quicksort**


- **3b. (8 pts)** Compare running times using `compare-qsort` between variants of
Quicksort and the provided implementation of selection sort (`ssort`).
Random Permutations: Both fixed-pivot and random-pivot Quicksort run in about $O(n \log n)$ time, while selection sort takes $O(n^2)$ time and quickly lags behind as shown by the table.
Fixed-pivot Quicksort picks the smallest element every time, leading worst-case $O(n^2)$ time, not depicted here as n is still $< 800$, but it may later reach recursion depths. Random-pivot Quicksort is unaffected by initial order, maintaining $O(n \log n)$ performance. ssort remains $O(n^2)$ regardless of input order.

|   n |   ssort |   qsort-fixed-pivot |   qsort-random-pivot |
|-----|---------|---------------------|----------------------|
| 100 |   0.277 |               0.187 |                0.182 |
| 200 |   0.864 |               0.355 |                0.372 |
| 400 |   3.377 |               0.735 |                0.893 |
| 800 |  12.216 |               2.041 |                2.865 |



- **3c. (8 pts)** Compare the fastest of your sorting implementations to the Python sorting function `sorted`, conducting the tests in 3b above. 
Python's Timsort outperforms random-pivot Quicksort and performs well with massive inputs.

|      n |   qsort-random-pivot |   tim-sort |
|--------|----------------------|------------|
|   1000 |                2.433 |      0.198 |
|   5000 |               15.721 |      0.741 |
|  10000 |               33.749 |      1.528 |
|  50000 |              183.398 |      8.426 |
| 100000 |              480.361 |     28.031 |