# Breaking RSA Encryption

The security of RSA encryption, as briefly described below, is based on the computational difficulty of factoring large numbers. The best-known algorithm, [General Number Field Sieve](https://en.wikipedia.org/wiki/General_number_field_sieve) performs this task in sub-exponential time $O\left(e^{1.9(\log N)^{1/3}(\log\log N)^{2/3}}\right)$, where $N$ is the number being factored (then $\log N$ would be the order of the number of input bits)

In 1994, [Peter Shor](http://www-math.mit.edu/~shor/) came up with a Las Vegas algorithm for prime factorization and discrete logarithms that take advantage of quantum computation to achieve a polynomial complexity in the number of bits, which is $O\left((\log N)^2(\log\log N)(\log \log\log N)\right)$.

## RSA Preliminaries

Suppose Alice wants to send a message $a$ to Bob (as an ASCII integer). Bob picks two prime numbers (preferably large) $p$ and $q$ and multiplies them to get $N=pq$. Also, he chooses a large number $c$, coprime with $(p-1)(q-1)$. After that, he shares with Alice the public keys $(N,c)$.

Alice uses the keys received from Bob to encrypt the message, calculating $b\equiv a^c~(mod ~N)$, and sends it to Bob. Since he knows $p$ and $q$, she can compute $d$ (private key) such that $cd\equiv1 ~(mod~ (p-1)(q-1))$ thought Bézout coefficients of [Extended Euclidean algorithm](https://en.wikipedia.org/wiki/Extended_Euclidean_algorithm), then he can decrypt the message $b$, through $b^d~(mod~N)$. This works because $a^{(p-1)(q-1)}\equiv 1 ~(mod~N)$, and since $cd$ can be written as $1+k(p-1)(q-1)$ for some $k\in\mathbb{Z}$, we get:

\begin{align*}
&b^d~(mod~N)\\
\equiv&~a^{cd}~(mod~N)\\
\equiv&~a^{1+k(p-1)(q-1)}~(mod~N)\\
\equiv&~a~(mod~N)
\end{align*}

This is just a summary, for a deeper explanation of the group theory involved in this encryption, you can check sections 3.2 and 3.3 of [(Mermin, 2007)](bib.md). But now, we understand the "vulnerability" of RSA. If a hacker gets $p$ and $q$ just knowing its product $N$, he could effortlessly generate the private key $d$ and decrypt the message. That's why in practical applications, $p$ and $q$ are huge numbers with hundreds of digits of length.


## Project Structure

[Section 2](shors-algorithm.md) describes Shor's Algorithm and the number theory behind it. It relies on finding the period $r$ of a discrete periodic function, in that case $f(x)=a^x~(mod~N)$. That's the key part of the algorithm which can be done efficiently on a quantum computer, using the Quantum Fourier Transform (QFT), presented in [Section 4](qft.md).

[Section 3](intro-quantum.md) gives a basic background in quantum computing concepts, and how to implement quantum circuits using [Qiskit library](https://qiskit.org/), for then you jump into the QFT section.

Finally, [Section 5](quantum-shors.md) shows a practical example of factoring using a quantum computer. We use $15$ for the experiment and explain the caveats for factoring higher order numbers.

[Section 6](further.md) is a selection of excellent materials available on the internet regarding quantum computing and cryptography. And [Section 7](bib.md) contains the bibliographical references.


