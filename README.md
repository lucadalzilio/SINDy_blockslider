# SINDy_blockslider
Sparse Identification of Nonlinear Dynamics (SINDy) for System Modeling
=======
# Sparse Identification of Nonlinear Dynamics (SINDy) for System Modeling

This repository provides a Python-based implementation of the **SINDy (Sparse Identification of Nonlinear Dynamics)** method. It demonstrates how SINDy can be used to discover the governing equations of a dynamic system from time-series data. The example focuses on a **mass-spring-damper system** and compares the true dynamics with the identified model.

---

## Table of Contents

1. [Introduction](#introduction)
2. [Sparse Identification of Nonlinear Dynamics (SINDy)](#sparse-identification-of-nonlinear-dynamics-sindy)
    - [Problem Statement](#problem-statement)
    - [Mathematical Framework](#mathematical-framework)
    - [Library of Candidate Functions](#library-of-candidate-functions)
    - [Sparse Regression](#sparse-regression)
3. [Project Overview](#project-overview)
    - [Synthetic Data Generation](#synthetic-data-generation)
    - [SINDy Model Training](#sindy-model-training)
    - [Model Evaluation](#model-evaluation)
4. [Setup and Installation](#setup-and-installation)
5. [Usage Instructions](#usage-instructions)
6. [Results](#results)
7. [Conclusion](#conclusion)
8. [References](#references)

---

## Introduction

Modeling dynamic systems is essential in many scientific and engineering fields. While traditional approaches rely on first-principles derivation of governing equations, **SINDy** offers a data-driven approach for identifying the equations directly from measurements. This repository demonstrates the use of SINDy to uncover the dynamics of a **mass-spring-damper system** based on simulated data.

---

## Sparse Identification of Nonlinear Dynamics (SINDy)

### Problem Statement

A dynamical system is typically represented as:
\[
\frac{dX}{dt} = f(X),
\]
where:
- \(X(t) \in \mathbb{R}^n\) is the state vector (e.g., displacement, velocity),
- \(f(X)\) represents the governing dynamics.

The goal of SINDy is to approximate \(f(X)\) by identifying a sparse combination of basis functions from a predefined library, making it interpretable and efficient.

---

### Mathematical Framework

SINDy approximates the system dynamics as:
\[
\frac{dX}{dt} \approx \Theta(X) \Xi,
\]
where:
- \(\Theta(X) \in \mathbb{R}^{m \times p}\): A **library of candidate functions** (e.g., polynomials, trigonometric functions) of the state \(X\),
- \(\Xi \in \mathbb{R}^{p \times n}\): A sparse coefficient matrix.

The sparse nature of \(\Xi\) ensures that only a small number of terms from \(\Theta(X)\) contribute significantly to the dynamics.

---

### Library of Candidate Functions

The library \(\Theta(X)\) can include:
- Constant: \(1\),
- Linear terms: \(x_i\),
- Quadratic terms: \(x_i x_j\),
- Trigonometric terms: \(\sin(x_i), \cos(x_i)\),
- Higher-order polynomials: \(x_i^2, x_i^3\).

For example, if \(X = [x_1, x_2]\), \(\Theta(X)\) might look like:
\[
\Theta(X) = \begin{bmatrix}
1 & x_1 & x_2 & x_1^2 & x_1 x_2 & x_2^2 & \sin(x_1) & \cos(x_2) \\
1 & x_1 & x_2 & x_1^2 & x_1 x_2 & x_2^2 & \sin(x_1) & \cos(x_2) \\
\vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots & \vdots
\end{bmatrix}.
\]

---

### Sparse Regression

The sparse coefficients \(\Xi\) are identified by solving the following regression problem:
\[
\min_\Xi \| \dot{X} - \Theta(X) \Xi \|_2^2 + \lambda \| \Xi \|_1,
\]
where:
- \(\dot{X}\) is the time derivative of \(X\),
- \(\lambda\) is a regularization parameter that controls sparsity.

This approach ensures that only the most relevant terms are retained in the model, leading to interpretable equations.

---

## Project Overview

This project demonstrates the application of SINDy to identify the dynamics of a mass-spring-damper system. The notebook follows these steps:

### 1. Synthetic Data Generation

Simulated data for the displacement and velocity of a mass-spring-damper system is generated using the following equations of motion:
\[
\ddot{x} + c \dot{x} + kx = 0,
\]
where:
- \(x\): Displacement,
- \(\dot{x}\): Velocity,
- \(c\): Damping coefficient,
- \(k\): Spring constant.

The system is integrated over time to produce time-series data for training the SINDy model.

---

### 2. SINDy Model Training

The SINDy algorithm is applied to the simulated data. Key steps include:
- Constructing the library of candidate functions \(\Theta(X)\),
- Computing time derivatives \(\dot{X}\),
- Solving the sparse regression problem to identify \(\Xi\).

---

### 3. Model Evaluation

The identified equations are used to simulate the system, and the results are compared to the true dynamics. This comparison assesses the accuracy of the SINDy model.

---

## Setup and Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/<username>/blockslider_model.git
   cd blockslider_model
