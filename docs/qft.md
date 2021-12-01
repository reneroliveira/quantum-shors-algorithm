# Quantum Fourier Transform

The Quantum Fourier Transform is just a change of basis, from the computational basis to Fourier one. It takes a quantum state $\vert x \rangle = \vert x_1\ldots x_n \rangle$ and maps it to:

$$\underbrace{\vert \tilde x\rangle}_{\text{Fourier basis}}=QFT_N \vert x \rangle =  \dfrac{1}{\sqrt{N}} \sum_{y=0}^{N-1} e^{2 \pi i xy / N} \vert y \rangle$$

Where $N=2^n$ ($n$ = number of qubits) and $\vert y\rangle$ represents its respective binary quantum state $\vert y_1y_2\ldots y_n\rangle$, with $y_i\in\{0,1\}$. (Actually, the sum $\sum_{y=0}^{N-1}$ is a shortcut notation for $\displaystyle\sum_{y_1=0}^1\sum_{y_2=0}^{1}\ldots\sum_{y_n=0}^{1}$)

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

## Quantum states math formulation

The above animation of quantum states in the Fourier basis can be formalized with a bit of math manipulation on the QFT formula. As we briefly stated before, when we write $\vert y\rangle$ we are representing the state $\vert y_1y_2\ldots y_n\rangle$, so $y$ in decimal base is $2^{n-1}y_1+2^{n-2}y_2+\ldots +2^0y_n$. So replacing $\vert y\rangle$ by its decimal representation, and the sum from $0$ to $N-1$ to the binary equivalent, we get:

\begin{align*}
QFT_N\vert x\rangle & =  \dfrac{1}{\sqrt{N}} \displaystyle\sum_{y=0}^{N-1} e^{2 \pi i xy / N} \vert y \rangle\\
&=\displaystyle\dfrac{1}{\sqrt N}\sum_{y_1=0}^1\sum_{y_2=0}^{1}\ldots\sum_{y_n=0}^{1}\exp\left(\frac{2\pi ix}{N}\sum_{k=1}^n2^{n-k}y_k\right)\vert y_1y_2\ldots y_n\rangle\\
&=\dfrac{1}{\sqrt N}\sum_{y_1,y_2,\ldots,y_n}\prod_{k=1}^n\exp\left(\frac{2\pi ix}{N}y_k2^{n-k}\right)\vert y_1y_2\ldots y_n\rangle\\
&=\dfrac{1}{\sqrt N}\sum_{y_1,y_2,\ldots,y_n}\prod_{k=1}^n\exp\left(2\pi ixy_k2^{-k}\right)\vert y_1y_2\ldots y_n\rangle
\end{align*}

By swapping the order of the product and the sum, we got a tensor product:

\begin{align*}
&\dfrac{1}{\sqrt N}\bigotimes_{k=1}^n\left(e^{2\pi ix(0)2^{-k}}\vert 0\rangle + e^{2\pi ix (1)2^{-k}}\vert1\rangle\right)\\
&=\dfrac{1}{\sqrt N}\bigotimes_{k=1}^n\left(\vert 0\rangle + e^{2\pi ix 2^{-k}}\vert1\rangle\right)\\
&=\dfrac{1}{\sqrt N}\left(\vert0\rangle+e^{2\pi ix/2}\vert1\rangle\right)\otimes\left(\vert0\rangle+e^{2\pi ix/4}\vert1\rangle\right)\otimes\ldots\otimes\left(\vert0\rangle+e^{2\pi i x/2^n}\vert1\rangle\right)
\end{align*}

Notice we went from $\vert x\rangle = \vert x_1\rangle\otimes\vert x_2\rangle\otimes\ldots\otimes\vert x_n\rangle$ to the state above, which tells us exatly what the the transform is doing to each entry of $x$. Regard constants, we have:

$$\vert x_k\rangle \mapsto \vert 0\rangle + e^{2\pi ix 2^{-k}}\vert1\rangle$$

Such an operation explains the above animation: the complex exponential term is responsible for the rotation seen. Moreover, this gives us hints for building a quantum circuit to implement QFT since we explicitly know what to do on each qubit.

## QFT Circuit

We'll need two atomic elements for building a circuit that implements QFT: the already known Hadamard gate and the controlled rotation gate.

We know that 

$$H\vert0\rangle=\frac{1}{\sqrt2}(\vert0\rangle+\vert1\rangle)$$ and $$H\vert1\rangle=\frac{1}{\sqrt2}(\vert0\rangle-\vert1\rangle)$$

We can also write Hadamard operation acting on $x_k\in\{0,1\}$ as:

$$H\vert x_k\rangle\dfrac{1}{\sqrt2}\left(\vert0\rangle+e^{2\pi i x_k/2}\vert1\rangle\right)$$

The other element, controlled rotation acts on two-qubit states $\vert x_ix_j\rangle$ and is based on the following unitary operator:

$$UROT_k = \left[\begin{matrix}
1&0\\
0&e^{2\pi i/2^k}\\
\end{matrix}\right]
$$

The, Controlled Rotation operator $CROT_k$ will be the 4x4 matrix:

$$CROT_k = \left[\begin{matrix}
I&0\\
0&UROT_k\\
\end{matrix}\right]$$

Remember that $\vert x_ix_j\rangle=\vert x_i\rangle\otimes\vert x_j\rangle$ which is a columns vector with four entries, so the operation $CROT_k\vert x_ix_j\rangle$ is dimentionally consistent. The first qubit $x_i$ is the control qubit and tha second, $x_j$ is the target one. So, as an example, se what happens when applying CROT to $\vert0x_j\rangle$ and $\vert1x_j\rangle$:

\begin{align*}CROT_k\vert 0x_j\rangle&=CROT_k\vert 0\rangle\otimes\vert x_j\rangle=\left[\begin{matrix}
I&0\\
0&UROT_k\\
\end{matrix}\right]\left(\begin{matrix}x_{j1}\\x_{j2}\\0\\0\end{matrix}\right)\\
&=\left(\begin{matrix}x_{j1}\\x_{j2}\\0\\0\end{matrix}\right)\\&=\vert0x_j\rangle\end{align*}

\begin{align*}CROT_k\vert 1x_j\rangle&=CROT_k\vert 1\rangle\otimes\vert x_j\rangle=\left[\begin{matrix}
I&0\\
0&UROT_k\\
\end{matrix}\right]\left(\begin{matrix}0\\0\\x_{j1}\\x_{j2}\end{matrix}\right)\\&=\left(\begin{matrix}0\\0\\x_{j1}\\e^{2\pi i/2^k}x_{j2}\end{matrix}\right)=\vert1\rangle\otimes\left(\begin{matrix}e^{2\pi i\cdot0/2^k}x_{j1}\\e^{2\pi i/2^k}x_{j2}\end{matrix}\right)\\&=e^{2\pi x_j/2^k}\vert1x_j\rangle\end{align*}

The QFT circuit is represented below:

![](images/qft.png)



Let's see what's the state becomes on each checkpoited step:

The input is the $n$-qubit state $\vert x\rangle=\vert x_1x_2\ldots x_n\rangle$

- **Step 1:** Applys Hadamard gate on the first qubit. The state after that will be:

$$\dfrac{1}{\sqrt2}\left[\vert0\rangle+e^{2\pi i x_12^{-1}}\vert1\rangle\right]\otimes\vert x_2x_3\ldots x_n\rangle$$

- **Step 2:** Applys Unitary Rotarion $UROT_2$ on the first qubit, controlled by the second. State after that:

$$\dfrac{1}{\sqrt2}\left[\vert0\rangle+e^{2\pi i x_22^{-2}}e^{2\pi i x_12^{-1}}\vert1\rangle\right]\otimes\vert x_2x_3\ldots x_n\rangle$$

- **Step 3:** After applying $UROT_k$ on qubit 1 controlled on qubit $k$ with $k$ from 2 to $n$, well get, following the above pattern:

$$\dfrac{1}{\sqrt2}\left[\vert0\rangle+e^{2\pi i x_n2^{-n}}e^{2\pi i x_{n-1}2^{-(n-1)}}\ldots e^{2\pi i x_22^{-2}}e^{2\pi i x_1 2^{-1}}\vert1\rangle\right]\otimes\vert x_2x_3\ldots x_n\rangle$$

which is equal:

$$\dfrac{1}{\sqrt2}\left[\vert0\rangle+\exp{\left(2\pi i\sum_{k=1}^nx_k2^{-k}\right)}\vert1\rangle\right]\otimes\vert x_2x_3\ldots x_n\rangle$$

But $x$ in decimal base is $x=\displaystyle\sum_{k=1}^nx_k2^{n-k}$, so whats inside the above exponential is just $2\pi i x/2^n$, we then simplyfies the output to:

$$\dfrac{1}{\sqrt2}\left[\vert0\rangle+e^{2\pi i x2^{-n}}\vert1\rangle\right]\otimes\vert x_2x_3\ldots x_n\rangle$$

- **Step 4:** We now apply the above block of steps to the remaining qubits, the process is very similar, and in the end the final state will be:

$$\frac{1}{\sqrt{2}}
\left[\vert0\rangle + 
e^{2\pi i x2^{-n}}
\vert1\rangle\right]
\otimes
\frac{1}{\sqrt{2}}
\left[\vert0\rangle + 
e^{2\pi i x2^{-(n-1)}}
\vert1\rangle\right]
\otimes
\ldots
\otimes
\frac{1}{\sqrt{2}}
\left[\vert0\rangle + 
e^{2\pi i x2^{-2}}
\vert1\rangle\right]
\otimes
\frac{1}{\sqrt{2}}
\left[\vert0\rangle + 
e^{2\pi i x2^{-1}}
\vert1\rangle\right]
$$

The constants multiplies to $\frac{1}{\sqrt N}$, so what we get as final state is just the formula derived previously, but in reversed order. Notice that the first transformed qubit is $\vert0\rangle + 
e^{2\pi i x2^{-n}}
\vert1\rangle$, where it was supposed to be $\vert0\rangle + 
e^{2\pi i x2^{-1}}
\vert1\rangle$ in our desired QFT formula, but that's not a problem at all, we just reverse the order of qubits and done!


```python
import numpy as np
from numpy import pi
import warnings
warnings.filterwarnings("ignore")
# importing Qiskit
from qiskit import QuantumCircuit, transpile, assemble, Aer, IBMQ
from qiskit.providers.ibmq import least_busy
from qiskit.tools.monitor import job_monitor
from qiskit.visualization import plot_histogram, plot_bloch_multivector

```


```python
qc = QuantumCircuit(3)
IBMQ.load_account()
```


    ---------------------------------------------------------------------------

    IBMQAccountCredentialsNotFound            Traceback (most recent call last)

    ~\AppData\Local\Temp/ipykernel_20620/4028026050.py in <module>
          1 qc = QuantumCircuit(3)
    ----> 2 IBMQ.load_account()
    

    C:\ProgramData\Anaconda3\envs\qiskit2\lib\site-packages\qiskit\providers\ibmq\ibmqfactory.py in load_account(self)
        172 
        173         if not credentials_list:
    --> 174             raise IBMQAccountCredentialsNotFound(
        175                 'No IBM Quantum Experience credentials found.')
        176 
    

    IBMQAccountCredentialsNotFound: 'No IBM Quantum Experience credentials found.'



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


