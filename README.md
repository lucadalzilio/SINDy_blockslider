# Sparse Identification of Nonlinear Dynamics (SINDy) for System Modeling

This repository contains a Python-based implementation of the **SINDy (Sparse Identification of Nonlinear Dynamics)** method. It demonstrates how SINDy can be used to discover governing equations of a dynamic system from time-series data, with applications to both simple systems like a mass-spring-damper system and more complex systems like **rate-and-state friction dynamics**.

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Sparse Identification of Nonlinear Dynamics (SINDy)](#sparse-identification-of-nonlinear-dynamics-sindy)  
    - [Mathematical Framework](#mathematical-framework)  
    - [Library of Candidate Functions](#library-of-candidate-functions)  
    - [Sparse Regression](#sparse-regression)  
3. [Rate-and-State Friction Dynamics](#rate-and-state-friction-dynamics)  
    - [Equations of Motion](#equations-of-motion)  
    - [Friction Laws](#friction-laws)  
4. [Project Overview](#project-overview)  
    - [Synthetic Data Generation](#synthetic-data-generation)  
    - [SINDy Model Training](#sindy-model-training)  
    - [Model Evaluation](#model-evaluation)  
5. [Setup and Installation](#setup-and-installation)  
6. [Usage Instructions](#usage-instructions)  
7. [Results](#results)  
8. [Conclusion and Future Work](#conclusion-and-future-work)  
9. [References](#references)  

---

## Introduction

Dynamic systems are found in many scientific and engineering disciplines. Traditional methods for deriving governing equations require deep physical insights, which can be challenging for complex systems. **SINDy** provides a data-driven approach to identify governing equations by leveraging sparse regression techniques. 

This repository applies SINDy to a simulated **mass-spring-damper system** and explores its application to geophysical models like **rate-and-state friction dynamics**.

---

## Sparse Identification of Nonlinear Dynamics (SINDy)

### Mathematical Framework

SINDy approximates the dynamics of a system as:
\[
\frac{d\mathbf{X}}{dt} = \mathbf{f}(\mathbf{X}),
\]
where:
- $\mathbf{X}(t) \in \mathbb{R}^n$: State vector (e.g., displacement, velocity),
- $\mathbf{f}(\mathbf{X})$: Governing dynamics.

SINDy assumes:
\[
\frac{d\mathbf{X}}{dt} \approx \Theta(\mathbf{X}) \Xi,
\]
where:
- $\Theta(\mathbf{X})$: A **library of candidate functions**,
- $\Xi$: A sparse matrix representing coefficients of active terms.

---

### Library of Candidate Functions

The library $\Theta(\mathbf{X})$ may include:
- Constant terms: $1$
- Linear terms: $x_i$
- Quadratic terms: $x_i^2, x_i x_j$
- Trigonometric terms: $\sin(x_i), \cos(x_i)$

For example, if $\mathbf{X} = [x_1, x_2]$, the library may look like:
\[
\Theta(\mathbf{X}) = 
\begin{bmatrix}
1 & x_1 & x_2 & x_1^2 & x_1 x_2 & x_2^2 & \sin(x_1) & \cos(x_2) \\
1 & x_1 & x_2 & x_1^2 & x_1 x_2 & x_2^2 & \sin(x_1) & \cos(x_2) \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots
\end{bmatrix}.
\]

---

### Sparse Regression

The sparse coefficients $\Xi$ are determined by solving the optimization problem:
\[
\min_{\Xi} \| \dot{\mathbf{X}} - \Theta(\mathbf{X}) \Xi \|_2^2 + \lambda \| \Xi \|_1,
\]
where:
- $\dot{\mathbf{X}}$: Time derivatives of $\mathbf{X}$,
- $\lambda$: Regularization parameter encouraging sparsity.

This process ensures only the most relevant terms in $\Theta(\mathbf{X})$ are retained.

---

## Rate-and-State Friction Dynamics

### Equations of Motion

Rate-and-state friction models describe the behavior of sliding interfaces under dynamic conditions, often used in geophysics for earthquake simulations. The general form is:
\[
\frac{d\mathbf{X}}{dt} = \mathbf{f}(\mathbf{X}, \theta),
\]
where:
- $\mathbf{X} = [x, \dot{x}]$: Displacement and velocity,
- $\theta$: State variable representing surface contact properties.

---

### Friction Laws

The evolution of $\theta$ follows specific friction laws:

1. **Dietrich's Law**:
   \[
   \frac{d\theta}{dt} = 1 - \frac{\theta \dot{x}}{D_c},
   \]
   where $D_c$ is the characteristic slip distance.

2. **Ruina's Law**:
   \[
   \frac{d\theta}{dt} = -\frac{\theta \dot{x}}{D_c} \ln \left( \frac{\dot{x}}{V_*} \right),
   \]
   where $V_*$ is a reference velocity.

---

## Project Overview

### 1. Synthetic Data Generation

Simulated data for a **mass-spring-damper system** is generated based on:
\[
m \ddot{x} + c \dot{x} + k x = 0,
\]
where:
- $m$: Mass,
- $c$: Damping coefficient,
- $k$: Spring constant.

The system is numerically solved to produce time-series data for displacement and velocity.

---

### 2. SINDy Model Training

The SINDy algorithm uses the generated data to:
- Construct a library of candidate functions $\Theta(\mathbf{X})$,
- Identify sparse coefficients $\Xi$.

---

### 3. Model Evaluation

The accuracy of the identified model is assessed by comparing its predictions with the true system dynamics.

---

## Setup and Installation

### Prerequisites

Ensure Python 3.7 or higher is installed.

### Installation Steps

1. Clone the repository:
   ```bash
   git clone https://github.com/<username>/SINDy_blockslider.git
   cd SINDy_blockslider