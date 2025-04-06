# Problem 2: Investigating the Dynamics of a Forced Damped Pendulum

## 1. Introduction

The forced damped pendulum is a rich example of how external driving and energy dissipation can lead to diverse behaviors—from simple periodic motion to resonance and chaos. In this report, we compare four main scenarios, as outlined below:

1. **Pure Pendulum (no damping, no forcing)**
2. **Damped Pendulum (damping $ \neq 0$, forcing $=0$)**
3. **Forced Pendulum (forcing $ \neq 0$, damping $=0$)**
4. **Forced Damped Pendulum (damping $ \neq 0$, forcing $ \neq 0$)**

We will produce time-domain plots (showing $\theta$ vs. $t$) and phase portraits (showing $\theta'$ vs. $\theta$). By systematically varying parameters such as the damping coefficient $b$, driving amplitude $A$, and driving frequency $\Omega$, we can explore transitions from regular to chaotic motion, resonance peaks, and other complex dynamics.

---

## 2. Theoretical Foundation

A forced damped pendulum can be described by the equation:

$$
\frac{d^2 \theta}{dt^2} + b \,\frac{d\theta}{dt} + \omega_0^2 \,\sin(\theta) = A \,\sin(\Omega t),
$$

where
- $\theta$ is the angular displacement,
- $b$ is the damping coefficient,
- $\omega_0$ is the natural frequency (e.g., $\sqrt{g/L}$ for a pendulum of length $L$),
- $A$ and $\Omega$ are the amplitude and frequency of the external driving force.

For small angles, using the approximation $\sin\theta \approx \theta$, the system behaves nearly harmonically. However, for larger angles or stronger forcing, nonlinearities and chaotic dynamics may emerge.

---

## 3. Implementation

Below you can see various simulations, and immediately beneath them is a comprehensive Python script. Each scenario outputs two types of plots (time series and phase portrait) as GIF animations:

1. **Pure Pendulum**  
2. **Damped Pendulum**  
3. **Forced Pendulum**  
4. **Forced Damped Pendulum**



