# NeuroExcite-HH-A-Computational-Framework-for-Mapping-Neuronal-Excitability.ipynb
# 🧠 NeuroExcite: A Computational Framework for Mapping Neuronal Excitability

A computational framework based on the **Hodgkin–Huxley model** for simulating neuronal action potentials, quantifying excitability, exploring parameter sensitivity, mapping neuronal phenotypes, and studying the effects of noise and bifurcations.

---

## 🔬 Overview

Neuronal excitability emerges from the interaction of membrane properties and voltage-gated ion channels. This project uses the classical **Hodgkin–Huxley (HH) model** to systematically investigate how changes in ionic conductances, membrane capacitance, external current, and noise influence neuronal firing.

The framework moves beyond basic action-potential simulation to build a quantitative map of neuronal excitability.

### Core workflow

```text
Hodgkin–Huxley Model
        ↓
Membrane Potential Simulation
        ↓
Spike Detection & Feature Extraction
        ↓
Rheobase Estimation
        ↓
Parameter Sensitivity Analysis
        ↓
2D Excitability Mapping
        ↓
Noise Robustness Analysis
        ↓
Bifurcation Analysis
        ↓
Neuronal Phenotype Classification
        ↓
ML-based Feature Importance
```

---

## ⚡ Hodgkin–Huxley Model

The framework implements the classical Hodgkin–Huxley equations using voltage-dependent sodium and potassium conductances together with a leak current.

The membrane equation is:

```text
Cₘ dV/dt = Iₑₓₜ − Iₙₐ − Iₖ − Iₗ
```

with:

```text
Iₙₐ = Gₙₐ m³h(V − Eₙₐ)

Iₖ = Gₖ n⁴(V − Eₖ)

Iₗ = Gₗ(V − Eₗ)
```

The gating variables **m, h, and n** are governed by voltage-dependent activation and inactivation kinetics.

The implementation uses the standard HH parameterization as the default model configuration.

---

## 📊 Spike Feature Extraction

The simulation framework extracts quantitative electrophysiological features from generated action potentials.

### Extracted features

* Number of spikes
* Firing rate
* Inter-spike interval (ISI)
* ISI coefficient of variation
* Action-potential amplitude
* APD50
* Maximum dV/dt
* Estimated spike threshold

These features allow neuronal responses to be compared quantitatively rather than relying only on voltage traces.

---

## 🎯 Rheobase Estimation

The framework estimates the **rheobase**, defined here as the minimum constant external current required to generate a spike.

A binary firing test is combined with a **bisection search** to efficiently identify the threshold current.

```text
No firing ──────────────── Firing
     ↓                         ↓
     └────── Bisection ────────┘
                  ↓
             Rheobase
```

This provides a quantitative measure of neuronal excitability.

---

## 📈 F-I Relationship

The framework evaluates neuronal firing responses across different external-current levels.

The resulting **frequency–current (F-I) relationship** demonstrates how firing rate changes as injected current increases.

```text
External Current
       ↓
Neuronal Response
       ↓
Spike Detection
       ↓
Firing Rate
       ↓
F-I Curve
```

---

## 🔧 Parameter Sensitivity Analysis

The framework performs one-dimensional parameter sweeps over:

* `G_Na` — maximum sodium conductance
* `G_K` — maximum potassium conductance
* `G_L` — leak conductance
* `C_m` — membrane capacitance

For each parameter configuration, the framework measures:

* Firing rate
* Action-potential amplitude
* APD50
* Threshold
* Rheobase

This enables systematic investigation of how intrinsic membrane properties affect neuronal excitability.

---

## 🗺️ 2D Excitability Maps

A major component of the project is the construction of two-dimensional excitability maps.

The framework varies pairs of model parameters and computes:

### Firing-rate landscape

```text
G_Na × G_K
      ↓
Firing-rate map
```

### Rheobase landscape

```text
G_Na × G_K
      ↓
Rheobase map
```

These maps provide a visual representation of how combinations of ionic conductances shape neuronal response regimes.

---

## 📐 Phase-Plane Analysis

The project also visualizes the dynamical trajectory of the neuronal system in a phase plane.

The membrane voltage is plotted against the potassium gating variable:

```text
V ↔ n
```

The analysis also evaluates steady-state gating relationships to visualize the underlying dynamical structure of the model.

---

## 🌊 Noise Robustness

Real neuronal systems operate in noisy environments. To explore this, the framework introduces stochastic current fluctuations into the HH model.

Multiple simulations are performed at different noise levels.

The analysis measures:

* Mean firing rate
* Firing-rate variability
* ISI coefficient of variation

The framework also examines **noise-induced firing below rheobase**, demonstrating how stochastic fluctuations can alter neuronal firing behavior.

---

## 🌀 Bifurcation Analysis

The framework performs a current sweep to investigate changes in steady-state membrane dynamics and firing rate.

For each external-current value, it measures:

* Minimum steady-state voltage
* Maximum steady-state voltage
* Steady-state firing rate

The resulting voltage envelope and firing-rate curves are used to characterize the transition from quiescence to repetitive firing.

The notebook interprets the observed discontinuous firing-rate onset as consistent with a **type-II excitability / subcritical Hopf bifurcation** within this model configuration.

---

## 🧬 Neuronal Phenotype Classification

The framework generates a synthetic population of HH neurons by randomly sampling:

```text
G_Na
G_K
G_L
C_m
```

Each simulated neuron is assigned a firing phenotype:

```text
Silent
Low-rate
High-rate
```

based on its simulated firing rate.

This creates a computational dataset linking intrinsic membrane parameters to emergent neuronal phenotypes.

---

## 🤖 Machine Learning Classification

A **Random Forest classifier** is trained to predict neuronal firing phenotype from intrinsic model parameters.

### Input features

```text
G_Na
G_K
G_L
C_m
```

### Target

```text
Neuronal phenotype
```

The dataset is split into training and test sets using stratification, and model performance is evaluated using a classification report.

Feature importance is also extracted to investigate which model parameters contribute most strongly to phenotype classification.

---

## 🔍 Numerical Validation

The project includes numerical convergence checks across different integration time steps:

```text
dt = 0.1
dt = 0.05
dt = 0.02
dt = 0.01
dt = 0.005 ms
```

The notebook identifies disagreement at the coarser `dt = 0.1 ms` setting and checks convergence at finer time steps.

This provides an important numerical validation step before interpreting the simulation results.

---

## ⚡ Sodium–Potassium Current Validation

The framework explicitly checks the temporal ordering of sodium and potassium conductance during an action potential.

The analysis verifies that:

```text
Na⁺ conductance peak
        ↓
K⁺ conductance peak
```

with sodium conductance peaking before potassium conductance, consistent with the expected sequence underlying action-potential generation in the HH model.

---

## 📁 Project Structure

```text
NeuroExcite/
│
├── NeuroExcite_HH_A_Computational_Framework_for_Mapping_Neuronal_Excitability.ipynb
│
└── README.md
```

The notebook contains the complete computational implementation, simulations, analyses, visualizations, validation checks, and machine-learning experiment.

---

## 🛠️ Technology Stack

**Language**

* Python

**Scientific Computing**

* NumPy
* SciPy

**Visualization**

* Matplotlib

**Machine Learning**

* scikit-learn
* Random Forest

**Modeling**

* Hodgkin–Huxley equations
* Numerical integration
* Parameter sweeps
* Bifurcation analysis
* Stochastic simulations

---

## 📌 Key Questions Explored

This project investigates:

1. How does injected current influence neuronal firing rate?
2. How do sodium and potassium conductances shape excitability?
3. How does membrane capacitance affect neuronal dynamics?
4. How can rheobase be computationally estimated?
5. How does noise affect firing reliability?
6. Can noise induce firing below deterministic rheobase?
7. How does the HH system transition from silence to repetitive firing?
8. Can intrinsic membrane parameters predict neuronal firing phenotype?
9. Which parameters are most informative for phenotype classification?
10. How sensitive are the computational results to numerical timestep?

---

## 🔭 Future Extensions

Potential extensions include:

* Parameter inference from experimental electrophysiology
* More detailed ion-channel models
* Calcium dynamics
* Adaptation currents
* Heterogeneous neuronal populations
* Parameter optimization using experimental data
* Bayesian parameter inference
* More sophisticated dynamical-systems analysis
* Network-level simulations
* Data-driven neuron models
* Comparison with experimental patch-clamp recordings

---

## 🧠 Research Context

**NeuroExcite** demonstrates how mechanistic computational models can be combined with quantitative analysis and machine learning to investigate the relationship between **ion-channel properties and emergent neuronal behavior**.

The project connects:

```text
Biophysical Modeling
        +
Dynamical Systems
        +
Numerical Simulation
        +
Statistical Analysis
        +
Machine Learning
        ↓
Quantitative Mapping of Neuronal Excitability
```

---

## ⚠️ Limitations

This is a computational modeling framework based on the Hodgkin–Huxley model. The simulated neurons are governed by model assumptions and parameterizations and therefore should not be interpreted as direct representations of every biological neuron.

The phenotype labels used for machine learning are defined from simulated firing rates rather than experimentally established neuronal classes.

Similarly, bifurcation interpretations apply to the specific model configuration and parameter regime explored in the notebook.
