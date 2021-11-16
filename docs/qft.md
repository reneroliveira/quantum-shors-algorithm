# Quantum Fourier Transform


```python
import numpy as np
from numpy import pi
# importing Qiskit
from qiskit import QuantumCircuit, transpile, assemble, Aer, IBMQ
from qiskit.providers.ibmq import least_busy
from qiskit.tools.monitor import job_monitor
from qiskit.visualization import plot_histogram, plot_bloch_multivector
import warnings
warnings.filterwarnings("ignore")
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


