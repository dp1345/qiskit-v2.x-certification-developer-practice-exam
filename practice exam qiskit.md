# IBM Qiskit v2.X — Original Practice Examination

**Exam:** C1000-179 — Fundamentals of Quantum Computing Using Qiskit v2.X Developer  
**Questions:** 30  
**Suggested time:** 40 minutes  
**Marks:** 1 mark per question  
**Negative marking:** None

## Question development and source policy

These 30 questions are independently authored practice questions. The attached IBM Sample Test PDF is treated only as background reference material; its questions and answer choices are not reproduced. The technical scope follows the eight objectives in the IBM Study Guide. The technical references listed in the answer key point to IBM Quantum documentation and the OpenQASM 3 specification referenced by IBM.

---

# Part 1 — Questions

## Section 1 — Perform Quantum Operations

**Q1. Pauli operator representation**

Consider the following Qiskit code:

```python
from qiskit.quantum_info import Pauli

p = Pauli("XIZ")
```

Considering Qiskit's Pauli-string indexing convention, which operations are assigned to the three qubits?

A. X on q0, I on q1, Z on q2.  
B. Z on q0, I on q1, X on q2.  
C. I on q0, X on q1, Z on q2.  
D. X on q0, Z on q1, I on q2.

**Q2. Sequential gate transformation**

A qubit begins in the computational basis state \(|0\rangle\). The following operations are applied:

```python
qc = QuantumCircuit(1)
qc.h(0)
qc.z(0)
qc.h(0)
```

What is the final state?

A. \(|0\rangle\)  
B. \(|1\rangle\)  
C. \((|0\rangle+|1\rangle)/\sqrt{2}\)  
D. \((|0\rangle-|1\rangle)/\sqrt{2}\)

**Q3. Relative phase transformation**

You prepare a qubit in an equal superposition and then apply an S gate:

```python
qc = QuantumCircuit(1)
qc.h(0)
qc.s(0)
```

Which expression represents the resulting state?

A. \((|0\rangle+|1\rangle)/\sqrt{2}\)  
B. \((|0\rangle-|1\rangle)/\sqrt{2}\)  
C. \((|0\rangle+i|1\rangle)/\sqrt{2}\)  
D. \((|0\rangle-i|1\rangle)/\sqrt{2}\)

**Q4. Quantum reset operation**

Consider:

```python
qc = QuantumCircuit(1)

qc.x(0)
qc.reset(0)
```

What is the state immediately after the reset operation?

A. \(|0\rangle\)  
B. \(|1\rangle\)  
C. \((|0\rangle+|1\rangle)/\sqrt{2}\)  
D. The state remains unchanged because reset is a unitary operation.

**Q5. Pauli operator algebra**

Two operations, Pauli-X and Pauli-Z, act on the same qubit. Which mathematical relationship correctly describes their multiplication?

A. \(XZ=ZX\)  
B. \(XZ=-ZX\)  
C. \(XZ=I\)  
D. \(XZ=X+Z\)

## Section 2 — Visualize Quantum Circuits, Measurements and States

**Q6. QSphere representation**

You use `plot_state_qsphere()` to visualize the state of a multi-qubit system.

Which statement correctly describes the information encoded by the visualization?

A. Point size represents phase, while colour represents probability.  
B. Point size represents probability, while colour represents phase.  
C. Point size represents circuit depth, while colour represents gate count.  
D. Point size represents the number of qubits, while colour represents measurement shots.

**Q7. Multi-qubit state visualization**

You have obtained a three-qubit quantum state and want to inspect the Bloch-sphere representation of each individual qubit.

Which visualization function should you use?

A. `plot_histogram()`  
B. `plot_bloch_multivector()`  
C. `plot_gate_map()`  
D. `plot_distribution()`

**Q8. Measurement histogram interpretation**

A measurement experiment returns:

```python
counts = {
    "00": 470,
    "11": 530
}
```

You pass this dictionary to `plot_histogram(counts)`.

What does the height of each bar represent by default?

A. The complex amplitudes of the computational basis states.  
B. The number of occurrences of each measured bitstring.  
C. The relative phase between the measured states.  
D. The number of gates applied to each qubit.

## Section 3 — Create Quantum Circuits

**Q9. Binding circuit parameters**

A symbolic parameter is used to construct a rotation circuit:

```python
from qiskit import QuantumCircuit
from qiskit.circuit import Parameter
import numpy as np

theta = Parameter("theta")

qc = QuantumCircuit(1)
qc.ry(theta, 0)

bound_qc = qc.assign_parameters({
    theta: np.pi
})
```

Assuming the circuit starts in \(|0\rangle\), what state does `bound_qc` prepare?

A. \(|0\rangle\)  
B. \(|1\rangle\)  
C. \((|0\rangle+|1\rangle)/\sqrt{2}\)  
D. \((|0\rangle-|1\rangle)/\sqrt{2}\)

**Q10. Dynamic circuit control flow**

You are implementing an algorithm that repeatedly executes a quantum operation while a classical measurement bit retains the value `1`.

Which Qiskit control-flow construct is designed for this conditional repetition?

A. `if_test()`  
B. `switch()`  
C. `while_loop()`  
D. `assign_parameters()`

**Q11. Measurement mapping**

Consider:

```python
from qiskit import QuantumCircuit

qc = QuantumCircuit(3, 2)

qc.measure([2, 0], [0, 1])
```

Which measurement mapping is implemented?

A. q2 → c0 and q0 → c1.  
B. q2 → c1 and q0 → c0.  
C. q0 → c0 and q1 → c1.  
D. All three qubits are measured into both classical bits.

**Q12. Hardware connectivity constraints**

A quantum processor has a linear connectivity arrangement:

```text
P0 —— P1 —— P2
```

A transpiled circuit needs a two-qubit operation between logical qubits currently mapped to P0 and P2.

What is the purpose of the transpiler's routing stage?

A. Increase the number of physical qubits.  
B. Introduce suitable routing operations, such as SWAP gates, to satisfy connectivity constraints.  
C. Replace all quantum gates with measurement operations.  
D. Convert the quantum circuit into a classical program.

**Q13. Backend-specific transpilation**

Consider:

```python
from qiskit.transpiler import generate_preset_pass_manager

pass_manager = generate_preset_pass_manager(
    backend=backend,
    optimization_level=1
)

isa_circuit = pass_manager.run(qc)
```

Why is the backend supplied when creating the pass manager?

A. To retrieve previous measurement counts.  
B. To provide the backend's compilation target and constraints, including supported instructions and qubit connectivity.  
C. To determine the number of shots automatically.  
D. To convert the circuit into a statevector.

## Section 4 — Run Quantum Circuits

**Q14. Job execution mode**

You need to submit one independent primitive request to an IBM Quantum backend without opening a Session or Batch context.

Which execution mode is appropriate?

A. Job mode.  
B. Session mode.  
C. Batch mode.  
D. Dynamic mode.

**Q15. Independent job scheduling**

A research workflow contains twelve independent circuits. Each circuit can execute without depending on the result of another circuit.

Which execution mode is appropriate when submitting these jobs together?

A. Session mode.  
B. Batch mode.  
C. Individual interactive mode only.  
D. Classical simulation mode.

**Q16. Iterative quantum workflow**

You are implementing a variational optimization workflow. A classical optimizer processes the result from one quantum execution and then submits the next quantum job.

Which execution mode is particularly appropriate for grouping these related iterative submissions?

A. Job mode.  
B. Batch mode.  
C. Session mode.  
D. OpenQASM mode.

**Q17. Primitive broadcasting**

Consider an Estimator workload:

```python
parameter_values = np.linspace(0, np.pi, 5)

observables = SparsePauliOp("ZZ")
```

The parameter array has shape `(5,)`, while the single observable is a scalar-shaped input.

What shape should the broadcasted result have?

A. `()`  
B. `(5,)`  
C. `(5, 5)`  
D. `(1, 5, 5)`

## Section 5 — Use the Sampler Primitive

**Q18. Sampler result structure**

A measured quantum circuit is submitted using `SamplerV2`.

Which type of information is the primitive primarily designed to return?

A. Expectation values of specified observables.  
B. Sampled classical output-register data.  
C. Transpiler optimization reports.  
D. Quantum-state density matrices.

**Q19. Retrieving register counts**

You constructed a circuit with a classical register named `readout`:

```python
from qiskit import QuantumCircuit, ClassicalRegister

readout = ClassicalRegister(2, "readout")
qc = QuantumCircuit(2, readout)

qc.h(0)
qc.cx(0, 1)
qc.measure([0, 1], readout)
```

After a successful Sampler V2 execution:

```python
pub_result = job.result()[0]
```

Which statement correctly retrieves the sampled counts from the register?

A. `pub_result.data.readout.get_counts()`  
B. `pub_result.readout.measure()`  
C. `job.readout.counts()`  
D. `pub_result.data.get_statevector()`

**Q20. Shot configuration**

You configure a Sampler primitive:

```python
sampler.options.default_shots = 8192
```

No alternative shot count is supplied in the PUB or `run()` call.

What does this configuration specify?

A. The total number of physical qubits involved.  
B. The number of gates executed in each circuit.  
C. The default number of circuit executions per circuit per configuration.  
D. The maximum number of primitive jobs allowed in a session.

**Q21. Dynamical decoupling configuration**

A circuit contains several periods during which qubits remain idle. You want to use Qiskit Runtime options to enable dynamical decoupling.

Which configuration is appropriate?

A.
```python
sampler.options.dynamical_decoupling.enable = True
```

B.
```python
sampler.options.default_shots = True
```

C.
```python
sampler.options.measurement.enable = True
```

D.
```python
sampler.options.transpilation = True
```

## Section 6 — Use the Estimator Primitive

**Q22. Observable estimation**

A quantum circuit prepares a state \(|\psi\rangle\). Your research requires evaluating the expectation value:

\[
\langle\psi|ZZ|\psi\rangle
\]

Which primitive is intended for this task?

A. Sampler.  
B. Estimator.  
C. Circuit drawer.  
D. QSphere visualizer.

**Q23. Estimator PUB interpretation**

Consider the following workload:

```python
pub = (
    isa_circuit,
    SparsePauliOp("XX"),
    parameter_values
)

job = estimator.run([pub])
```

What role does `SparsePauliOp("XX")` perform in this PUB?

A. Specifies the parameter values for the circuit.  
B. Defines the observable whose expectation value is to be estimated.  
C. Selects the physical backend.  
D. Specifies the number of circuit execution shots.

**Q24. Estimator option precedence**

You configure an Estimator as follows:

```python
estimator.options.default_precision = 0.02
estimator.options.default_shots = 2048
```

Assuming no PUB-level or run-level value supersedes these settings, which option determines the shot count?

A. `default_precision` always overrides `default_shots`.  
B. `default_shots` overrides `default_precision`.  
C. Both options are multiplied together.  
D. Neither option affects the execution.

**Q25. Zero Noise Extrapolation**

An experiment produces expectation values at multiple effective noise levels. You want to estimate the ideal expectation value corresponding to a noise-free system.

Which procedure describes Zero Noise Extrapolation?

A. Remove all measurements and reconstruct the statevector.  
B. Increase the number of physical qubits and repeat the experiment.  
C. Amplify noise to obtain data at several noise levels and extrapolate towards zero noise.  
D. Apply a Hadamard gate to every qubit before measurement.

## Section 7 — Retrieve and Analyze Results

**Q26. Retrieve a previous experiment**

You restarted your Python environment and no longer have the original job object in memory. However, you saved the job ID.

Which implementation retrieves the previous IBM Quantum job?

A.
```python
job = service.job(saved_job_id)
result = job.result()
```

B.
```python
job = service.backend(saved_job_id)
result = job.measure()
```

C.
```python
job = QuantumCircuit(saved_job_id)
result = job.draw()
```

D.
```python
job = service.session(saved_job_id)
result = job.status()
```

**Q27. Interpreting job status**

You submit a quantum workload and inspect:

```python
status = job.status()
```

The returned status is `QUEUED`.

What does this indicate?

A. The job has completed successfully.  
B. The job has failed.  
C. The job is waiting in the execution queue.  
D. The job has been permanently deleted.

**Q28. Closing a Runtime session**

You have finished submitting jobs to a Session and execute:

```python
session.close()
```

What is the expected behaviour?

A. All previously submitted jobs are immediately deleted.  
B. The session stops accepting new jobs, while already queued or running jobs can finish.  
C. All quantum circuits are automatically recompiled.  
D. The session transfers all results to a new backend.

## Section 8 — Operate with OpenQASM

**Q29. OpenQASM 3 measurement storage**

Consider the following OpenQASM 3 declarations:

```qasm
qubit[3] q;
bit[3] result;
```

Which statement is correct?

A. Both declarations represent classical integer arrays.  
B. `qubit[3]` declares three quantum bits, while `bit[3]` declares three classical bits.  
C. `qubit[3]` declares three classical bits, while `bit[3]` declares three quantum bits.  
D. Both declarations represent three quantum registers.

**Q30. OpenQASM interoperability**

You have an OpenQASM 3 program stored in a Python string:

```python
program = """
OPENQASM 3.0;
include "stdgates.inc";

qubit[2] q;
bit[2] output;

h q[0];
cx q[0], q[1];

output[0] = measure q[0];
output[1] = measure q[1];
"""
```

Which Qiskit API is intended to import the program directly from the string into a `QuantumCircuit`?

A. `qiskit.qasm3.load(program)`  
B. `qiskit.qasm3.loads(program)`  
C. `qiskit.qasm3.dump(program)`  
D. `QuantumCircuit.from_measurements(program)`

---

# Part 2 — Separate Answer Key

| Q | Answer | Q | Answer |
|---:|:---:|---:|:---:|
| 1 | B | 16 | C |
| 2 | B | 17 | B |
| 3 | C | 18 | B |
| 4 | A | 19 | A |
| 5 | B | 20 | C |
| 6 | B | 21 | A |
| 7 | B | 22 | B |
| 8 | B | 23 | B |
| 9 | B | 24 | B |
| 10 | C | 25 | C |
| 11 | A | 26 | A |
| 12 | B | 27 | C |
| 13 | B | 28 | B |
| 14 | A | 29 | B |
| 15 | B | 30 | B |

# Part 3 — Answer Explanations and Sources of Truth

| Q | Answer | Concept | Explanation | Source of truth |
|---:|:---:|---|---|---|
| 1 | B | Pauli string ordering | Qiskit assigns the rightmost Pauli character to q0. Therefore, `XIZ` represents Z on q0, I on q1 and X on q2. | [IBM Pauli API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.quantum_info.Pauli) |
| 2 | B | Sequential gate transformation | The identity \(HZH=X\) means that the sequence changes the initial state \(|0\rangle\) into \(|1\rangle\). | [IBM QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit) |
| 3 | C | Phase gates | Hadamard creates an equal superposition. The S gate multiplies the \(|1\rangle\) amplitude by \(i\), giving \((|0\rangle+i|1\rangle)/\sqrt{2}\). | [IBM Circuit Library](https://quantum.cloud.ibm.com/docs/en/api/qiskit/circuit_library) |
| 4 | A | Reset operation | The X gate first prepares \(|1\rangle\). Reset then returns the qubit to \(|0\rangle\). Reset is a nonunitary operation. | [IBM QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit) |
| 5 | B | Pauli algebra | Pauli-X and Pauli-Z anticommute: \(XZ=-ZX\). Reversing their multiplication order changes the sign of the operator. | [IBM Pauli API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.quantum_info.Pauli) |
| 6 | B | QSphere | QSphere point size is proportional to the probability of a computational-basis component, while point colour represents its phase. | [IBM QSphere API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.visualization.plot_state_qsphere) |
| 7 | B | Bloch multivector | `plot_bloch_multivector()` creates a Bloch-sphere representation for each qubit in the supplied quantum state. | [IBM Bloch Multivector API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.visualization.plot_bloch_multivector) |
| 8 | B | Measurement visualization | Histogram data uses bitstrings as keys and measurement counts as values. Each bar represents the observed frequency of a bitstring. | [IBM Visualization API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/visualization) |
| 9 | B | Parameter binding | `assign_parameters()` substitutes the symbolic angle with \(\pi\). Since \(R_y(\pi)|0\rangle=|1\rangle\), the resulting state is \(|1\rangle\). | [IBM Parameter API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.Parameter) |
| 10 | C | Dynamic circuits | `while_loop()` provides conditional repetition based on classical data. `if_test()` executes a conditional block rather than implementing a repetition loop. | [IBM Classical Feedforward and Control Flow](https://quantum.cloud.ibm.com/docs/en/guides/classical-feedforward-and-control-flow) |
| 11 | A | Measurement mapping | Qiskit pairs the supplied qubit and classical-bit sequences positionally: q2 is measured into c0, and q0 into c1. | [IBM QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit) |
| 12 | B | Transpiler routing | The routing stage must respect hardware connectivity. Suitable routing operations, including SWAP gates when needed, allow nonadjacent logical interactions to be implemented. | [IBM Transpilation Guide](https://quantum.cloud.ibm.com/docs/en/guides/transpile) |
| 13 | B | Backend target | Supplying a backend allows the pass manager to use backend-specific compilation information, such as its target, basis gates and connectivity. | [IBM Pass Manager API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/2.1/qiskit.transpiler.generate_preset_pass_manager) |
| 14 | A | Job mode | Job mode submits an individual primitive request without requiring a Session or Batch context. | [IBM Execution Modes](https://quantum.cloud.ibm.com/docs/en/guides/execution-modes) |
| 15 | B | Batch mode | Batch mode is intended for independent, multi-job workloads. These jobs can be submitted together without requiring conditional relationships between them. | [IBM Execution Modes](https://quantum.cloud.ibm.com/docs/en/guides/execution-modes) |
| 16 | C | Session mode | Sessions group iterative quantum-computing calls. Subsequent jobs within an active session are prioritized by the scheduler. | [IBM Session API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/session) |
| 17 | B | Broadcasting | A scalar observable broadcasts across the five parameter-value entries, resulting in five expectation-value estimates. | [IBM Primitive Inputs and Outputs](https://quantum.cloud.ibm.com/docs/en/guides/primitive-input-output) |
| 18 | B | Sampler V2 | Sampler V2 returns sampled data from the circuit's output registers, represented through result structures containing bit-array data. | [IBM Sampler V2 API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/sampler-v2) |
| 19 | A | Sampler result retrieval | Each classical register is represented in the result data under its register name. `get_counts()` retrieves the associated counts. | [IBM Primitive Inputs and Outputs](https://quantum.cloud.ibm.com/docs/en/guides/primitive-input-output) |
| 20 | C | Sampler shots | `default_shots` specifies the default number of shots for each circuit when no other applicable shot count is supplied. | [IBM Sampler Options](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-sampler-options) |
| 21 | A | Dynamical decoupling | The `dynamical_decoupling.enable` option activates dynamical decoupling. Pulse sequences are used during idle periods to suppress unwanted interactions. | [IBM Error Mitigation and Suppression](https://quantum.cloud.ibm.com/docs/en/guides/error-mitigation-and-suppression-techniques) |
| 22 | B | Estimator | Estimator computes expectation values of specified observables with respect to states prepared by quantum circuits. | [IBM Primitives Guide](https://quantum.cloud.ibm.com/docs/en/guides/primitives) |
| 23 | B | Estimator PUB | `SparsePauliOp("XX")` represents the observable for which the expectation value is calculated. The parameter values bind the circuit's symbolic parameters. | [IBM Primitive Inputs and Outputs](https://quantum.cloud.ibm.com/docs/en/guides/primitive-input-output) |
| 24 | B | Estimator precision and shots | When both default options are configured, the explicit `default_shots` setting takes precedence over `default_precision`, assuming no PUB-level or run-level override. | [IBM Estimator Options](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-estimator-options) |
| 25 | C | Zero Noise Extrapolation | ZNE amplifies noise at different levels, obtains expectation-value estimates, and extrapolates towards the zero-noise limit. | [IBM Error Mitigation and Suppression](https://quantum.cloud.ibm.com/docs/en/guides/error-mitigation-and-suppression-techniques) |
| 26 | A | Historical job retrieval | `QiskitRuntimeService.job(job_id)` retrieves an existing job. The returned job object can be used to retrieve its result with `job.result()`. | [IBM QiskitRuntimeService API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/qiskit-runtime-service) |
| 27 | C | Job status | `QUEUED` indicates that the job is waiting for execution. It is different from `RUNNING`, `DONE` and `ERROR`. | [IBM JobStatus API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.providers.JobStatus) |
| 28 | B | Session lifecycle | `close()` stops acceptance of new jobs. Previously submitted queued or running jobs can finish before the session terminates. | [IBM Session API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/session) |
| 29 | B | OpenQASM data types | `qubit[3]` declares a three-qubit quantum register, while `bit[3]` declares a three-bit classical register. | [OpenQASM 3.0 Types Specification](https://openqasm.com/versions/3.0/language/types.html) |
| 30 | B | OpenQASM import | `qiskit.qasm3.loads()` imports a QASM program from a string. `load()` is intended for a filename. The Qiskit QASM 3 importer package may need to be installed separately. | [IBM Qiskit/OpenQASM 3 Interoperability](https://quantum.cloud.ibm.com/docs/en/guides/interoperate-qiskit-qasm3) |

---

## Part 4 — Performance Evaluation

| Score | Practice assessment |
|---:|---|
| 27–30 | Excellent — strong performance on this practice set. |
| 23–26 | Good — review the weaker technical areas. |
| 18–22 | Intermediate — additional practical work is recommended. |
| 13–17 | Needs improvement — revisit the relevant study objectives. |
| 0–12 | Revisit foundational concepts before attempting another practice test. |

These are practice-test performance bands, not official IBM passing thresholds.

## Part 5 — IBM Study References

The question coverage follows the eight objective areas in the IBM C1000-179 Study Guide. Use the documentation links below as the technical references when reviewing answers.

1. [IBM Qiskit Pauli API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.quantum_info.Pauli)
2. [IBM Qiskit Circuit Library](https://quantum.cloud.ibm.com/docs/en/api/qiskit/circuit_library)
3. [IBM QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit)
4. [IBM Quantum Circuit Visualization](https://quantum.cloud.ibm.com/docs/en/guides/visualize-circuits)
5. [IBM Classical Feedforward and Control Flow](https://quantum.cloud.ibm.com/docs/en/guides/classical-feedforward-and-control-flow)
6. [IBM Parameter API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.Parameter)
7. [IBM Transpiler Stages](https://quantum.cloud.ibm.com/docs/en/guides/transpiler-stages)
8. [IBM Transpile with Pass Managers](https://quantum.cloud.ibm.com/docs/en/guides/transpile-with-pass-managers)
9. [IBM Execution Modes](https://quantum.cloud.ibm.com/docs/en/guides/execution-modes)
10. [IBM Primitive Inputs and Outputs](https://quantum.cloud.ibm.com/docs/en/guides/primitive-input-output)
11. [IBM Sampler V2 API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/sampler-v2)
12. [IBM Sampler Options](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-sampler-options)
13. [IBM Estimator Options](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-estimator-options)
14. [IBM Error Mitigation and Suppression Techniques](https://quantum.cloud.ibm.com/docs/en/guides/error-mitigation-and-suppression-techniques)
15. [IBM Runtime Job API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/runtime-job)
16. [IBM Runtime Session API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/session)
17. [IBM Qiskit Runtime Service API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/qiskit-runtime-service)
18. [IBM Qiskit OpenQASM 3 Interoperability](https://quantum.cloud.ibm.com/docs/en/guides/interoperate-qiskit-qasm3)
19. [OpenQASM 3.0 Language Types](https://openqasm.com/versions/3.0/language/types.html)

**Version note:** IBM's documentation and Qiskit Runtime interfaces evolve. Verify API details against the installed Qiskit and `qiskit-ibm-runtime` versions before running code. This document is an independently authored study resource, not an official IBM examination or a prediction of actual examination questions.
