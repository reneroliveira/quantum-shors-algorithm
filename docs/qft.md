# Quantum Fourier Transform

The Quantum Fourier Transform is just a change of basis, from the computational basis to Fourier one. It takes a quantum state $\vert x \rangle = \vert x_1\ldots x_n \rangle$ and maps it to:

$$\underbrace{\vert \tilde x\rangle}_{\text{Fourier basis}}=QFT_N \vert x \rangle =  \dfrac{1}{\sqrt{N}} \sum_{y=0}^{N-1} e^{2 \pi i xy / N} \vert y \rangle$$

Where $N=2^n$ ($n$ = number of qubits) and $\vert y\rangle$ is representing its respective binary quantum state $\vert y_1y_2\ldots y_n\rangle$, with $y_i\in\{0,1\}$.

## Examples

Let's begin with the 1-qubit case, where $N=2^1$:

\begin{align*}
\vert \tilde 0 \rangle&=\dfrac{1}{\sqrt2}\displaystyle\sum_{y=0}^{1}e^{2\pi i (0)y/2}\vert y\rangle\\
&=\dfrac{1}{\sqrt2}\displaystyle\sum_{y=0}^{1}\vert y\rangle\\
&=\dfrac{1}{\sqrt2}(\vert 0\rangle + \vert 1\rangle)\\
&=\vert + \rangle
\end{align*}

\begin{align*}
\vert \tilde 1 \rangle&=\dfrac{1}{\sqrt2}\displaystyle\sum_{y=0}^{1}e^{2\pi i (1)y/2}\vert y\rangle\\
&=\dfrac{1}{\sqrt2}\left(e^{2\pi i(0)/2}\vert 0\rangle+e^{2\pi i(1)/2}\vert 1\rangle\right)\\
&=\dfrac{1}{\sqrt2}(\vert 0\rangle + (-1)\vert 1\rangle)\\
&=\dfrac{1}{\sqrt2}(\vert 0\rangle -\vert 1\rangle\\
&=\vert - \rangle
\end{align*}

So states $\vert0\rangle$ and $\vert1\rangle$ are mapped to states $\vert+\rangle$ and $\vert-\rangle$ respectively. That's what Hadamard gate does! Geometrically, this transformation brings the z-axis poles states to the x-axis poles in the Bloch sphere.

[Qiskit Textbook](further.md) have a nice animation which helps build intuition of what QFT does in the general case. A four-qubit state $\vert x\rangle = \vert x_1x_2x_3x_4\rangle$ is stored using $\vert0\rangle$ and $\vert1\rangle$ in the computational basis. Observe that the leftmost qubit ($x_4$) flips with with every number increment, the next (qubit 1, $x_3$) flips with 2 number increments, qubit 2 ($x_2$) flips after 4 turns, and so on.

![](images/zbasis-counting.gif)

In the Fourier basis, all states are on the equatorial plane. The leftmost is rotated $2\pi/16$ radians by number increment, the next qubit ($x_3$) rotates $2\pi/8$ per time, $x_2$ rotates $2\pi/4$ and $x_1$ with an angle of $2\pi/2=\pi$ radians. We are doubling the angle of rotation on each qubit.

![](images/fourierbasis-counting.gif)




```python

```


```python

```

![](images/qft.png)


```python
import numpy as np
from numpy import pi
warnings.filterwarnings("ignore")
# importing Qiskit
from qiskit import QuantumCircuit, transpile, assemble, Aer, IBMQ
from qiskit.providers.ibmq import least_busy
from qiskit.tools.monitor import job_monitor
from qiskit.visualization import plot_histogram, plot_bloch_multivector
import warnings
```


```python
qc = QuantumCircuit(3)
```


```python
qc.h(2)
qc.draw()
```




<pre style="word-wrap: normal;white-space: pre;background: #fff0;line-height: 1.1;font-family: &quot;Courier New&quot;,Courier,monospace">          
q_0: ─────

q_1: ─────
     ┌───┐
q_2: ┤ H ├
     └───┘</pre>




```python
qc.cp(pi/2, 1, 2) # CROT from qubit 1 to qubit 2
qc.draw()
```




<pre style="word-wrap: normal;white-space: pre;background: #fff0;line-height: 1.1;font-family: &quot;Courier New&quot;,Courier,monospace">                   
q_0: ──────────────

q_1: ──────■───────
     ┌───┐ │P(π/2) 
q_2: ┤ H ├─■───────
     └───┘         </pre>


