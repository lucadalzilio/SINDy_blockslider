# Sparse Identification of Nonlinear Dynamics (SINDy) for System Modeling

This repository contains a Python-based implementation of the **SINDy (Sparse Identification of Nonlinear Dynamics)** method, applied to model dynamic systems from time-series data. The project specifically focuses on identifying governing equations for a mass-spring-damper system, while also discussing its potential application to more complex models like **rate-and-state friction** systems.

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

In many scientific and engineering fields, it is crucial to identify the governing equations of dynamic systems. Traditional methods rely heavily on first-principles derivations, which may not always be feasible or accurate for complex systems. **SINDy** is a data-driven approach that learns governing equations directly from measurements by leveraging sparse regression techniques.

This project demonstrates the application of SINDy to a simulated **mass-spring-damper system** and discusses its potential extension to more complex systems like **rate-and-state friction dynamics** relevant in geophysics.

---

## Sparse Identification of Nonlinear Dynamics (SINDy)

### Mathematical Framework

SINDy seeks to approximate the dynamics of a system described by:
\[
\frac{d\mathbf{X}}{dt} = \mathbf{f}(\mathbf{X}),
\]
where:
- \(\mathbf{X}(t) \in \mathbb{R}^n\) is the state vector (e.g., displacement, velocity),
- \(\mathbf{f}(\mathbf{X})\) represents the system's governing equations.

SINDy approximates \(\mathbf{f}(\mathbf{X})\) as:
\[
\frac{d\mathbf{X}}{dt} \approx \Theta(\mathbf{X}) \Xi,
\]
where:
- \(\Theta(\mathbf{X})\): A **library of candidate functions** (e.g., polynomials, trigonometric functions, etc.),
- \(\Xi\): A **sparse coefficient matrix** that identifies the active terms in the governing equations.

---

### Library of Candidate Functions

The library \(\Theta(\mathbf{X})\) contains potential terms for \(\mathbf{f}(\mathbf{X})\). For example, if \(\mathbf{X} = [x_1, x_2]\), \(\Theta(\mathbf{X})\) might include:
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

To determine \(\Xi\), SINDy solves a sparse regression problem:
\[
\min_{\Xi} \| \dot{\mathbf{X}} - \Theta(\mathbf{X}) \Xi \|_2^2 + \lambda \| \Xi \|_1,
\]
where:
- \(\dot{\mathbf{X}}\): Time derivatives of the state variables,
- \(\lambda\): Regularization parameter encouraging sparsity.

The resulting \(\Xi\) reveals the dominant terms in \(\Theta(\mathbf{X})\), providing a concise and interpretable model of the system.

---

## Rate-and-State Friction Dynamics

### Equations of Motion

Rate-and-state friction describes the behavior of sliding interfaces under dynamic conditions, commonly used in geophysics to model earthquake cycles. The general form includes:
\[
\frac{d\mathbf{X}}{dt} = \mathbf{f}(\mathbf{X}, \mathbf{\theta}),
\]
where:
- \(\mathbf{X} = [x, \dot{x}]\) represents displacement and velocity,
- \(\mathbf{\theta}\): State variable characterizing surface contact properties.

---

### Friction Laws

The evolution of the state variable \(\mathbf{\theta}\) is governed by specific friction laws:
1. **Dietrich's Law**:
   \[
   \frac{d\theta}{dt} = 1 - \frac{\theta \dot{x}}{D_c},
   \]
   where \(D_c\) is a characteristic slip distance.

2. **Ruina's Law**:
   \[
   \frac{d\theta}{dt} = -\frac{\theta \dot{x}}{D_c} \ln \left( \frac{\dot{x}}{V_*} \right),
   \]
   where \(V_*\) is a reference velocity.

---

## Project Overview

### 1. Synthetic Data Generation

Simulated data is generated for a **mass-spring-damper system**:
\[
m \ddot{x} + c \dot{x} + k x = 0,
\]
where:
- \(m\): Mass,
- \(c\): Damping coefficient,
- \(k\): Spring constant.

The system is solved numerically to obtain time-series data for displacement and velocity.

---

### 2. SINDy Model Training

SINDy uses the generated data to identify the governing equations of the system by constructing a library of candidate functions and solving the sparse regression problem.

---

### 3. Model Evaluation

The identified model is validated by comparing its predictions against the true system dynamics.

---

## Setup and Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/<username>/SINDy_blockslider.git
   cd SINDy_blockslider