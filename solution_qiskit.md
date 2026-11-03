# IBM Qiskit v2.X — Practice Examination Solutions

**Exam alignment:** C1000-179 — Fundamentals of Quantum Computing Using Qiskit v2.X Developer  
**Companion file:** `practice exam qiskit.md`  
**Questions covered:** 30  
**Purpose:** Answer key, technical reasoning, worked calculations, and authoritative references.

> These solutions correspond to the independently authored 30-question practice examination. The IBM Sample Test PDF is not used as a source of copied questions or answer choices. The technical explanations are grounded in the IBM Study Guide, IBM Quantum documentation, and the OpenQASM 3 specification referenced by IBM.

---

## 1. Answer key at a glance

| Question | Answer | Question | Answer | Question | Answer |
|---:|:---:|---:|:---:|---:|:---:|
| 1 | B | 11 | A | 21 | A |
| 2 | B | 12 | B | 22 | B |
| 3 | C | 13 | B | 23 | B |
| 4 | A | 14 | A | 24 | B |
| 5 | B | 15 | B | 25 | C |
| 6 | B | 16 | C | 26 | A |
| 7 | B | 17 | B | 27 | C |
| 8 | B | 18 | B | 28 | B |
| 9 | B | 19 | A | 29 | B |
| 10 | C | 20 | C | 30 | B |

**Scoring:** Award 1 mark for each correct response. All 30 questions in this version are single-answer questions.

---

# 2. Detailed solutions and explanations

## Section 1 — Perform Quantum Operations

### Q1 — Pauli operator representation

**Correct answer: B — Z on q0, I on q1, X on q2.**

Qiskit's Pauli-string convention places qubit 0 at the **right-most** character. Therefore, the label `XIZ` is interpreted from right to left:

| Qubit | Character | Operation |
|---:|:---:|---|
| q0 | Z | Pauli-Z |
| q1 | I | Identity |
| q2 | X | Pauli-X |

The equivalent tensor-product ordering is X ⊗ I ⊗ Z, where the right-most factor acts on q0.

**Source of truth:** [IBM — Pauli API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.quantum_info.Pauli). The API documentation explicitly states that qubit 0 corresponds to the right-most Pauli character.

### Q2 — Sequential gate transformation

**Correct answer: B — |1⟩.**

The circuit applies H, then Z, then H. The identity

HZH = X

shows that the complete gate sequence is equivalent to a Pauli-X gate. Applying X to the initial state gives

X|0⟩ = |1⟩.

This is a useful way to reason about a short sequence of gates without multiplying all three matrices explicitly.

**Source of truth:** [IBM — QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit) and [IBM — Circuit library](https://quantum.cloud.ibm.com/docs/en/api/qiskit/circuit_library).

### Q3 — Relative phase transformation

**Correct answer: C — (|0⟩ + i|1⟩) / √2.**

The Hadamard gate transforms the initial state into

H|0⟩ = (|0⟩ + |1⟩) / √2.

The S gate has the matrix

`S = [[1, 0], [0, i]]`.

It leaves the amplitude of |0⟩ unchanged and multiplies the amplitude of |1⟩ by i. Thus,

S((|0⟩ + |1⟩) / √2) = (|0⟩ + i|1⟩) / √2.

This changes the **relative phase** between the basis-state amplitudes; it does not change their probabilities, which remain one-half each.

**Source of truth:** [IBM — Circuit library](https://quantum.cloud.ibm.com/docs/en/api/qiskit/circuit_library) and [IBM — QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit).

### Q4 — Quantum reset operation

**Correct answer: A — |0⟩.**

The X gate first maps |0⟩ to |1⟩. The subsequent `reset(0)` operation resets the target qubit to the ground state |0⟩.

Reset is not a unitary gate. It is a non-unitary operation that returns the qubit to the zero state, irrespective of its prior state.

**Source of truth:** [IBM — QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit), including the `reset` circuit instruction.

### Q5 — Pauli operator algebra

**Correct answer: B — XZ = −ZX.**

The Pauli-X and Pauli-Z matrices are

`X = [[0, 1], [1, 0]]; Z = [[1, 0], [0, −1]]`.

Multiplying in the two possible orders gives

`XZ = [[0, −1], [1, 0]]; ZX = [[0, 1], [−1, 0]]`.

Therefore, XZ = −ZX. The operators **anticommute**. Equivalently, XZ = −iY and ZX = iY.

**Source of truth:** [IBM — Pauli API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.quantum_info.Pauli).

---

## Section 2 — Visualize Quantum Circuits, Measurements and States

### Q6 — QSphere representation

**Correct answer: B — Point size represents probability; colour represents phase.**

In Qiskit's QSphere visualization:

- The size of a point is proportional to the probability of its computational-basis component.
- The point colour represents the phase of that component.

The visualization therefore conveys both the magnitude and phase information associated with the nonzero basis-state amplitudes.

**Source of truth:** [IBM — `plot_state_qsphere` API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.visualization.plot_state_qsphere).

### Q7 — Multi-qubit state visualization

**Correct answer: B — `plot_bloch_multivector()`.**

`plot_bloch_multivector()` plots a Bloch sphere for each qubit in the supplied state. It is suitable for examining the single-qubit Bloch-vector representation of a multi-qubit state.

**Important limitation:** For an entangled state, the individual Bloch spheres describe the individual qubits' reduced-state information. They do not, by themselves, fully describe correlations or entanglement across the whole system.

**Source of truth:** [IBM — `plot_bloch_multivector` API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.visualization.plot_bloch_multivector).

### Q8 — Measurement histogram interpretation

**Correct answer: B — The number of occurrences of each measured bitstring.**

The dictionary contains bitstrings as keys and their observed counts as values:

| Bitstring | Count |
|---|---:|
| `00` | 470 |
| `11` | 530 |

A histogram of these counts displays two bars. The `11` bar is slightly taller because it occurred 530 times, compared with 470 occurrences of `00`.

The total number of recorded shots is 470 + 530 = 1000. The corresponding empirical frequencies are 0.47 and 0.53, but the input dictionary itself contains counts.

**Source of truth:** [IBM — Qiskit visualization API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/visualization).

---

## Section 3 — Create Quantum Circuits

### Q9 — Binding circuit parameters

**Correct answer: B — |1⟩.**

The circuit initially contains the symbolic rotation Rᵧ(θ). `assign_parameters()` substitutes θ = π, so the circuit applies Rᵧ(π) to |0⟩.

Using

`Rᵧ(θ) = [[cos(θ/2), −sin(θ/2)], [sin(θ/2), cos(θ/2)]]`.

we obtain

Rᵧ(π)|0⟩ = cos(π/2)|0⟩ + sin(π/2)|1⟩ = |1⟩.

**Source of truth:** [IBM — Parameter API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.Parameter) and [IBM — QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit).

### Q10 — Dynamic circuit control flow

**Correct answer: C — `while_loop()`.**

A `while_loop()` repeats a block of circuit instructions while its classical condition remains true. It is the appropriate control-flow construct when the intended behaviour is conditional repetition.

The related constructs have different purposes:

- `if_test()` executes a conditional block once.
- `switch()` selects among branches based on a value.
- `for_loop()` iterates over a predetermined iterable or range.
- `while_loop()` repeats while a condition holds.

**Hardware note:** Dynamic-circuit support and supported control-flow operations depend on the selected backend and its capabilities.

**Source of truth:** [IBM — Classical feedforward and control flow](https://quantum.cloud.ibm.com/docs/en/guides/classical-feedforward-and-control-flow).

### Q11 — Measurement mapping

**Correct answer: A — q2 → c0 and q0 → c1.**

Qiskit pairs the qubit list and classical-bit list positionally:

```python
qc.measure([2, 0], [0, 1])
```

This means:

1. The first qubit, q2, is measured into the first classical bit, c0.
2. The second qubit, q0, is measured into the second classical bit, c1.

Qubit q1 is not measured by this instruction.

**Source of truth:** [IBM — QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit), particularly the `measure` instruction.

### Q12 — Hardware connectivity constraints

**Correct answer: B — Insert routing operations, such as SWAP gates, to satisfy connectivity constraints.**

The processor only has direct links between P0–P1 and P1–P2. P0 and P2 are not directly connected in the stated topology.

The transpiler's routing stage changes the placement of logical quantum states through routing operations, commonly SWAP gates, so that required two-qubit operations can be implemented using allowed hardware connections.

Routing does not create physical qubits and does not convert the quantum circuit into a classical program.

**Source of truth:** [IBM — Transpiler stages](https://quantum.cloud.ibm.com/docs/en/guides/transpiler-stages) and [IBM — Transpile with pass managers](https://quantum.cloud.ibm.com/docs/en/guides/transpile-with-pass-managers).

### Q13 — Backend-specific transpilation

**Correct answer: B — The backend supplies the compilation target and hardware constraints.**

A backend-aware preset pass manager can use the target's supported instructions and connectivity information when compiling a circuit. This helps produce an ISA-compatible circuit for the selected device.

`optimization_level=1` selects an optimization level; it does not specify a shot count or retrieve measurement results.

**Source of truth:** [IBM — Transpiler stages](https://quantum.cloud.ibm.com/docs/en/guides/transpiler-stages) and [IBM — Transpile with pass managers](https://quantum.cloud.ibm.com/docs/en/guides/transpile-with-pass-managers).

---

## Section 4 — Run Quantum Circuits

### Q14 — Job execution mode

**Correct answer: A — Job mode.**

IBM Quantum Runtime provides three execution modes: job, session, and batch. Job mode is used for an individual primitive request without opening a Session or Batch context.

**Source of truth:** [IBM — Execution modes](https://quantum.cloud.ibm.com/docs/en/guides/execution-modes).

### Q15 — Independent job scheduling

**Correct answer: B — Batch mode.**

Batch mode is designed for workloads containing multiple independent jobs that can be submitted together. The jobs do not need to depend on one another's results.

Batch execution does not guarantee that jobs will run in the exact order in which they were submitted.

**Source of truth:** [IBM — Execution modes](https://quantum.cloud.ibm.com/docs/en/guides/execution-modes) and [IBM — Run jobs in batch](https://quantum.cloud.ibm.com/docs/en/guides/run-jobs-batch).

### Q16 — Iterative quantum workflow

**Correct answer: C — Session mode.**

A variational workflow commonly alternates between classical optimization and quantum execution:

1. The optimizer selects parameter values.
2. A quantum job evaluates the circuit.
3. The classical process uses the result to select the next parameters.
4. Another quantum job is submitted.

Session mode is suited to this iterative pattern because it groups related jobs within a common execution window and can reduce repeated queueing delays after the first job begins.

**Source of truth:** [IBM — Execution modes](https://quantum.cloud.ibm.com/docs/en/guides/execution-modes) and [IBM — Session API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/session).

### Q17 — Primitive broadcasting

**Correct answer: B — `(5,)`.**

The parameter input has shape `(5,)`. The observable is a single scalar-shaped input, so it broadcasts across the five parameter values.

The compatible shapes are therefore:

`(5,)` and `()` → `(5,)`.

The result contains five expectation-value estimates, one for each parameter value. A `(5, 5)` result would require another non-scalar dimension of length five.

**Source of truth:** [IBM — Primitive inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/primitive-input-output).

---

## Section 5 — Use the Sampler Primitive

### Q18 — Sampler result structure

**Correct answer: B — Sampled classical output-register data.**

Sampler is used to sample circuit measurements and return classical output data. It is not the primitive whose main purpose is to calculate expectation values of specified observables; that is the Estimator's role.

**Source of truth:** [IBM — Sampler inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/sampler-input-output) and [IBM — Primitives guide](https://quantum.cloud.ibm.com/docs/en/guides/primitives).

### Q19 — Retrieving register counts

**Correct answer: A — `pub_result.data.readout.get_counts()`.**

A Sampler V2 PUB result contains a `data` object. The data object has an entry for each classical register in the circuit. Since the register is named `readout`, its BitArray is accessed through `pub_result.data.readout`.

Calling `get_counts()` converts the sampled bit-array data into a dictionary mapping bitstrings to their observed counts.

```python
pub_result = job.result()[0]
counts = pub_result.data.readout.get_counts()
```

**Source of truth:** [IBM — Sampler inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/sampler-input-output). The guide documents the `DataBin`, per-register `BitArray`, and `get_counts()` workflow.

### Q20 — Shot configuration

**Correct answer: C — The default number of circuit executions per circuit per configuration.**

`default_shots` controls how many shots are used by default when no more specific shot count is supplied in the PUB or `run()` call. It is a sampling/execution setting, not a qubit count, gate limit, or job limit.

**Source of truth:** [IBM — Sampler options](https://quantum.cloud.ibm.com/docs/en/guides/sampler-options).

### Q21 — Dynamical decoupling configuration

**Correct answer: A — Enable `sampler.options.dynamical_decoupling.enable`.**

The option controls whether dynamical decoupling is enabled. Dynamical decoupling inserts pulse sequences into suitable idle periods to suppress the effects of unwanted interactions and certain sources of noise.

```python
sampler.options.dynamical_decoupling.enable = True
```

The exact available sequences and scheduling options are documented in the Runtime options reference. The benefit depends on the circuit timing, backend, and noise characteristics; enabling the option does not guarantee an improvement in every experiment.

**Source of truth:** [IBM — Sampler options](https://quantum.cloud.ibm.com/docs/en/guides/sampler-options) and [IBM — Error mitigation and suppression techniques](https://quantum.cloud.ibm.com/docs/en/guides/error-mitigation-and-suppression-techniques).

---

## Section 6 — Use the Estimator Primitive

### Q22 — Observable estimation

**Correct answer: B — Estimator.**

Estimator is designed to estimate expectation values of observables with respect to states prepared by quantum circuits. In this question, the requested quantity is

⟨ψ|ZZ|ψ⟩,

which is the expectation value of the two-qubit Pauli-ZZ observable.

**Source of truth:** [IBM — Primitives guide](https://quantum.cloud.ibm.com/docs/en/guides/primitives) and [IBM — Estimator inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/estimator-input-output).

### Q23 — Estimator PUB interpretation

**Correct answer: B — It defines the observable.**

In the PUB tuple

```python
pub = (
    isa_circuit,
    SparsePauliOp("XX"),
    parameter_values
)
```

- `isa_circuit` is the circuit to execute.
- `SparsePauliOp("XX")` specifies the observable whose expectation value is required.
- `parameter_values` supplies values for the circuit's parameters.

The observable is not a backend selector or a shot-count setting.

**Source of truth:** [IBM — Estimator inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/estimator-input-output) and [IBM — Primitive inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/primitive-input-output).

### Q24 — Estimator option precedence

**Correct answer: B — `default_shots` overrides `default_precision`.**

IBM documents that when `default_shots` is set, it overrides `default_precision`. Therefore, in the stated configuration, the explicit default shot count of 2048 determines the shot allocation, assuming no PUB-level or `run()`-level value supersedes it.

`default_precision` is an alternative way to request an estimation precision when a fixed shot count is not being specified.

**Source of truth:** [IBM — EstimatorOptions API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-estimator-options). See the `default_shots` attribute documentation.

### Q25 — Zero Noise Extrapolation

**Correct answer: C — Amplify noise at different levels and extrapolate towards zero noise.**

Zero Noise Extrapolation (ZNE) estimates a zero-noise result from expectation values measured at multiple effective noise levels. The measured values are used to construct an extrapolation toward the zero-noise limit.

ZNE is an error-mitigation technique. It does not physically eliminate all noise, and its estimate depends on the noise-scaling and extrapolation procedure.

**Source of truth:** [IBM — Error mitigation and suppression techniques](https://quantum.cloud.ibm.com/docs/en/guides/error-mitigation-and-suppression-techniques) and [IBM — ZNE options](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-zne-options).

---

## Section 7 — Retrieve and Analyze Results

### Q26 — Retrieve a previous experiment

**Correct answer: A — `service.job(saved_job_id)`, followed by `job.result()`.**

The Runtime service can retrieve a previously submitted job by its job ID:

```python
job = service.job(saved_job_id)
result = job.result()
```

The job ID is the identifier used to recover the job object after restarting the local Python environment. This requires access to the same IBM Quantum account/instance and a job that is still retrievable.

**Source of truth:** [IBM — QiskitRuntimeService API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/qiskit-runtime-service), specifically `job(job_id)`.

### Q27 — Interpreting job status

**Correct answer: C — The job is waiting in the execution queue.**

`QUEUED` means the job has been accepted but has not started execution. It is not the same as successful completion (`DONE`) or failure (`ERROR`).

The exact set of job statuses is documented in Qiskit's `JobStatus` enumeration.

**Source of truth:** [IBM — JobStatus API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.providers.JobStatus).

### Q28 — Closing a Runtime session

**Correct answer: B — New jobs are no longer accepted; existing queued or running jobs can finish.**

IBM's Session API documents that `close()` prevents new jobs from being accepted. Existing queued or running jobs are allowed to run to completion, and the session is terminated when there are no more pending jobs.

This is different from cancelling a session's pending jobs; the Session API provides a separate `cancel()` method for that purpose.

**Source of truth:** [IBM — Session API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/session), specifically `close()` and `cancel()`.

---

## Section 8 — Operate with OpenQASM

### Q29 — OpenQASM 3 measurement storage

**Correct answer: B — `qubit[3]` declares three quantum bits and `bit[3]` declares three classical bits.**

In OpenQASM 3:

```qasm
qubit[3] q;
bit[3] result;
```

- `qubit[3] q;` declares a register of three quantum bits.
- `bit[3] result;` declares a classical register of three bits.

The classical `bit` register can store measurement outcomes. The `qubit` declaration refers to quantum subsystems, not classical storage.

**Source of truth:** [OpenQASM 3.0 — Types and Casting](https://openqasm.com/versions/3.0/language/types.html), also referenced in the IBM Study Guide under Section 8.

### Q30 — OpenQASM interoperability

**Correct answer: B — `qiskit.qasm3.loads(program)`.**

The distinction is:

| Function | Input | Purpose |
|---|---|---|
| `qiskit.qasm3.load(filename)` | File path | Read an OpenQASM 3 program from a file. |
| `qiskit.qasm3.loads(program)` | Python string | Parse an OpenQASM 3 program held in a string. |
| `qiskit.qasm3.dump(circuit, file)` | Circuit and file stream | Write a circuit's OpenQASM 3 representation to a file stream. |
| `qiskit.qasm3.dumps(circuit)` | Circuit | Return the OpenQASM 3 representation as a string. |

For the question's Python string, the correct call is:

```python
import qiskit.qasm3

qc = qiskit.qasm3.loads(program)
```

**Installation note:** IBM's current Qiskit documentation states that the OpenQASM 3 importer requires the optional `qiskit_qasm3_import` package. One documented installation route is:

```bash
pip install "qiskit[qasm3-import]"
```

**Source of truth:** [IBM — Qiskit QASM3 API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qasm3) and [IBM — Interoperate Qiskit and QASM 3](https://quantum.cloud.ibm.com/docs/en/guides/interoperate-qiskit-qasm3).

---

# 3. Official study references

The following references cover the objectives represented in this practice examination:

| Exam area | IBM / standards reference |
|---|---|
| Pauli operators | [Pauli API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.quantum_info.Pauli) |
| Gates and circuit operations | [Circuit library](https://quantum.cloud.ibm.com/docs/en/api/qiskit/circuit_library) · [QuantumCircuit API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.QuantumCircuit) |
| Circuit and state visualization | [Visualization guide/API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/visualization) · [Bloch multivector](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.visualization.plot_bloch_multivector) · [QSphere](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.visualization.plot_state_qsphere) |
| Classical feedforward and dynamic circuits | [Classical feedforward and control flow](https://quantum.cloud.ibm.com/docs/en/guides/classical-feedforward-and-control-flow) |
| Parameterized circuits | [Parameter API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.circuit.Parameter) |
| Transpilation | [Transpiler stages](https://quantum.cloud.ibm.com/docs/en/guides/transpiler-stages) · [Pass managers](https://quantum.cloud.ibm.com/docs/en/guides/transpile-with-pass-managers) |
| Runtime execution modes | [Execution modes](https://quantum.cloud.ibm.com/docs/en/guides/execution-modes) · [Batch jobs](https://quantum.cloud.ibm.com/docs/en/guides/run-jobs-batch) |
| Primitive inputs and outputs | [Primitive inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/primitive-input-output) |
| Sampler | [Sampler inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/sampler-input-output) · [Sampler options](https://quantum.cloud.ibm.com/docs/en/guides/sampler-options) |
| Estimator | [Estimator inputs and outputs](https://quantum.cloud.ibm.com/docs/en/guides/estimator-input-output) · [Estimator options](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-estimator-options) |
| Error mitigation | [Error mitigation and suppression](https://quantum.cloud.ibm.com/docs/en/guides/error-mitigation-and-suppression-techniques) · [ZNE options](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/options-zne-options) |
| Job and session management | [Runtime service API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/qiskit-runtime-service) · [JobStatus API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qiskit.providers.JobStatus) · [Session API](https://quantum.cloud.ibm.com/docs/en/api/qiskit-ibm-runtime/session) |
| OpenQASM 3 | [Qiskit QASM3 API](https://quantum.cloud.ibm.com/docs/en/api/qiskit/qasm3) · [OpenQASM 3 types](https://openqasm.com/versions/3.0/language/types.html) |

## Version and environment note

IBM Quantum documentation is continuously updated. API names and option structures can differ between releases of `qiskit`, `qiskit-ibm-runtime`, and optional interoperability packages. Before executing a code example, check the versions installed in the target environment:

```python
import qiskit
import qiskit_ibm_runtime

print("Qiskit:", qiskit.__version__)
print("Qiskit IBM Runtime:", qiskit_ibm_runtime.__version__)
```

This file is a study aid, not an official IBM answer sheet or a guarantee of the exact content of the certification examination.
