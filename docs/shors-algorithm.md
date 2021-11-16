# Shor's Algorithm

<!-- Description of the algorithm, mathematical statements and proofs. -->


```python
import markdown
md = markdown.Markdown(extensions=['pymdownx.arithmatex'])
```

# Pseudocode

Getting straigth to the point, here's how Shor's algorithm works to factor a prime product $N=pq$:

1. Pick a random integer $a$ between $0$ and $N-1$;

2. Find the smallest $r$ such that $a^r\equiv 1 ~(mod ~N)$, i.e the period of the function $f(x)=a^x~(mod~N)$;

3. If r is odd, get back to step 1

4. If $a^{r/2}+1\equiv 0~(mod~N)$, get back to step 1.

5. Then $\{p,q\}=\{\gcd(a^{r/2}-1,N),\gcd(a^{r/2}+1,N)\}$.


```python

```
