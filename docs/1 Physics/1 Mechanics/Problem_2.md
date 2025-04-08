# Problem 2: Investigating the Dynamics of a Forced Damped Pendulum

## 1. Theoretical Foundation

We start from the forced damped pendulum equation:
$$
\frac{d^2\theta}{dt^2} + b\,\frac{d\theta}{dt} + \frac{g}{L}\sin\theta = A\cos(\omega_d\,t)
$$

### 1.1 Small-Angle Approximation

For small angles, we use the approximation
$$
\sin\theta \approx \theta
$$
which linearizes the equation to
$$
\frac{d^2\theta}{dt^2} + b\,\frac{d\theta}{dt} + \frac{g}{L}\theta = A\cos(\omega_d\,t)
$$

This differential equation has both homogeneous and particular solutions:

- **Homogeneous Solution:**  
  The characteristic equation is
  $$
  r^2 + b\,r + \frac{g}{L} = 0
  $$
  Depending on the value of \(b\), the roots can be real or complex, leading to overdamped, critically damped, or underdamped responses.

- **Particular Solution:**  
  For the forcing term 
  $$
  A\cos(\omega_d\,t)
  $$
  we try a solution of the form
  $$
  \theta_p(t) = C\cos\bigl(\omega_d\,t - \varphi\bigr)
  $$
  This solution shows maximum amplitude near resonance, where 
  $$
  \omega_d \approx \omega_0 = \sqrt{\frac{g}{L}}.
  $$

### 1.2 Energy Considerations and Resonance

The total energy of the pendulum is given by the sum of the kinetic and potential energies:
$$
E(t) = \frac{1}{2}m\Bigl(L\dot{\theta}\Bigr)^2 + mgL\Bigl(1-\cos\theta\Bigr)
$$

- Under resonance conditions (especially in undamped or weakly damped regimes), the driving force continuously feeds energy into the system, raising the oscillation amplitude.
- In the damped scenario, energy dissipates over time, causing the amplitude to decay.
- In the forced pendulum, the external force introduces complex behaviors—including periodic, quasiperiodic, and chaotic dynamics—which will be examined in the following sections.

## 2. Analysis of Dynamics

We analyze three main cases:

- **Pure Pendulum:**  
  \(b = 0\) and \(A = 0\)  
  The system exhibits undriven, periodic oscillations.

- **Damped Pendulum:**  
  \(b \neq 0\) and \(A = 0\)  
  Due to energy loss from damping, the oscillation amplitude decays over time and the phase space trajectory spirals into the origin.

- **Forced (Driven) Pendulum:**  
  For realistic forced chaotic behavior, a small but nonzero damping is appropriate; hence we choose  
  \(b = 0.25\) and \(A \neq 0\).  
  The external forcing leads to complex behavior that may range from regular periodic motion to chaos. Phase diagrams and Poincaré sections will illustrate these transitions.

In addition, varying the parameters (damping coefficient \(b\), driving amplitude \(A\), and driving frequency \(\omega_d\)) systematically reveals transitions in the dynamics, including resonant amplification and chaotic regimes. The use of bifurcation diagrams will help visualize how changes in, for example, the driving amplitude \(A\) affect the system’s attractors.

## 3. Practical Applications

The forced damped pendulum model applies in various real-world scenarios, including:

- **Energy Harvesting:**  
  Optimizing energy transfer from ambient vibrations.
- **Suspension Bridges:**  
  Managing vibrations due to periodic loads.
- **Oscillating Circuits (Driven RLC Circuits):**  
  Understanding resonance and damping in electrical analogues.
- **Biomechanics:**  
  Modeling periodic motions such as human gait.

## 4. Implementation

The following Python code implements the simulations for the three cases, generates time series, phase space diagrams, a Poincaré section, and includes a bifurkasyon (bifurcation) diagram by varying the driving amplitude \(A\).

```
import numpy as np
import matplotlib.pyplot as plt
from scipy.integrate import solve_ivp

# --- Constants ---
g = 9.81       # acceleration due to gravity (m/s^2)
L = 1.0        # length of the pendulum (m)

# --- Pendulum ODE's ---
# y = [theta, omega]
def pendulum_ode(t, y, b, A, omega_drive):
    theta, omega = y
    dtheta_dt = omega
    domega_dt = -b * omega - (g / L) * np.sin(theta) + A * np.cos(omega_drive * t)
    return [dtheta_dt, domega_dt]

# --- Simulation Settings ---
t_span = (0, 40)                                  # simulation time interval (s)
t_eval = np.linspace(t_span[0], t_span[1], 4000)   # time evaluation points
y0 = [0.2, 0.0]                                   # initial conditions: theta=0.2 rad, omega=0

# --- Case 1: Pure Pendulum (b=0, A=0) ---
sol_pure = solve_ivp(pendulum_ode, t_span, y0, t_eval=t_eval, args=(0.0, 0.0, 0.0))

# --- Case 2: Damped Pendulum (b=0.5, A=0) ---
sol_damped = solve_ivp(pendulum_ode, t_span, y0, t_eval=t_eval, args=(0.5, 0.0, 0.0))

# --- Case 3: Forced (Driven) Pendulum ---
# For chaotic behavior, we use a small damping b = 0.25, nonzero driving amplitude and omega_drive = 2/3.
b_forced = 0.25
A_forced = 1.0
omega_drive = 2/3
sol_forced = solve_ivp(pendulum_ode, t_span, y0, t_eval=t_eval, args=(b_forced, A_forced, omega_drive))

# --- Plots: Time Series Comparison ---
plt.figure(figsize=(12,6))
plt.plot(t_eval, sol_pure.y[0], label="Pure Pendulum", linewidth=2)
plt.plot(t_eval, sol_damped.y[0], label="Damped Pendulum (b=0.5)", linestyle="--", linewidth=2)
plt.plot(t_eval, sol_forced.y[0], label=f"Forced Pendulum (b={b_forced}, A={A_forced}, ω_d=2/3)", linestyle=":", linewidth=2)
plt.xlabel("Time (s)")
plt.ylabel("θ (rad)")
plt.title("Time Series Comparison of Pendulum Scenarios")
plt.legend()
plt.grid()
plt.show()

# --- Plots: Phase Space Diagrams ---
plt.figure(figsize=(12,6))
plt.plot(sol_pure.y[0], sol_pure.y[1], label="Pure Pendulum", linewidth=2)
plt.plot(sol_damped.y[0], sol_damped.y[1], label="Damped Pendulum (b=0.5)", linestyle="--", linewidth=2)
plt.plot(sol_forced.y[0], sol_forced.y[1], label=f"Forced Pendulum (b={b_forced}, A={A_forced}, ω_d=2/3)", linestyle=":", linewidth=2)
plt.xlabel("θ (rad)")
plt.ylabel("ω (rad/s)")
plt.title("Phase Space Diagrams of Pendulum Scenarios")
plt.legend()
plt.grid()
plt.show()

# --- Poincaré Section for Forced Pendulum ---
T_drive = 2 * np.pi / omega_drive  # Driving period
# Select points with t mod T_drive ~ 0 (with tolerance), using all points
indices = [i for i, t in enumerate(t_eval) if np.isclose(t % T_drive, 0, atol=0.01)]
theta_poincare = sol_forced.y[0][indices]
omega_poincare = sol_forced.y[1][indices]

plt.figure(figsize=(8,6))
plt.scatter(theta_poincare, omega_poincare, color="magenta", s=20, alpha=0.75)
plt.xlabel("θ (rad)")
plt.ylabel("ω (rad/s)")
plt.title("Poincaré Section for Forced Pendulum")
plt.grid()
plt.show()

# --- Bifurcation Diagram ---
# Vary the driving amplitude A from 0.5 to 1.5 and record the theta values (Poincaré section) after a transient.
A_values = np.linspace(0.5, 1.5, 100)
b_value = 0.25  # fixed damping for forced pendulum
theta_bifurcation = []  # will store tuples (A, theta)
transient_time = 20  # discard initial transient period

for A in A_values:
    sol = solve_ivp(pendulum_ode, t_span, y0, t_eval=t_eval, args=(b_value, A, omega_drive))
    # Select Poincaré points after transient (t > transient_time)
    indices = [i for i, t in enumerate(t_eval) if (t > transient_time) and (np.isclose(t % T_drive, 0, atol=0.01))]
    for i in indices:
        theta_bifurcation.append((A, sol.y[0][i]))

# Separate the data for plotting
A_bif = [pt[0] for pt in theta_bifurcation]
theta_bif = [pt[1] for pt in theta_bifurcation]

plt.figure(figsize=(10,6))
plt.scatter(A_bif, theta_bif, s=0.5, color='black')
plt.xlabel("Driving Amplitude A")
plt.ylabel("θ (rad) from Poincaré Section")
plt.title("Bifurcation Diagram: Dependence on Driving Amplitude")
plt.grid(True)
plt.show()
```

## 5. Discussion and Extensions

**Resonance and Energy Transfer:**  
Under the small-angle approximation, the system is linear, and resonance occurs when the driving frequency aligns with the natural frequency, i.e., 
$$
\omega_d \approx \frac{g}{L}
$$  
In this resonant condition, the driving force continuously feeds energy into the system, increasing the oscillation amplitude.

**Effect of Damping:**  
In the damped scenario (e.g., \(b = 0.5\)), energy dissipates over time, causing the oscillation amplitude to decay. The phase space diagram shows a spiral trajectory converging toward the origin.

**Forced Scenario and Chaos:**  
In the forced pendulum case, a small but nonzero damping (e.g., \(b = 0.25\)) is chosen along with a nonzero driving amplitude (e.g., \(A = 1.0\)) to facilitate the observation of complex behavior. The time series, phase space diagram, and especially the Poincaré section reveal transitions from regular periodic motion to chaotic dynamics.

**Bifurcation Analysis:**  
By systematically varying the driving amplitude \(A\) and plotting the corresponding Poincaré section values (after removing transients), a bifurcation diagram is obtained. This diagram provides insight into how the system transitions from periodic to chaotic behavior as \(A\) is varied.

**Limitations and Extensions:**  
- While the small-angle approximation (\(\sin\theta \approx \theta\)) simplifies the analytical treatment, the full nonlinear model using \(\sin\theta\) is employed in the simulations.  
- Extensions to the model can include nonlinear damping (e.g., air resistance), non-periodic driving forces, or the examination of other bifurcation parameters (such as the driving frequency \(\omega_d\)).  
- Further analysis via bifurcation diagrams assists in understanding the parameter regimes where the system transitions to chaos.

## 6. Conclusion

This work investigates the dynamics of the forced damped pendulum through both analytical approximations and numerical simulations. By comparing the time series, phase space diagrams, Poincaré sections, and a bifurcation diagram across various scenarios (pure, damped, and forced pendulums), the study illustrates the transition from regular harmonic motion to complex, potentially chaotic behavior. The model provides insights applicable to energy harvesting, vibration control in structures, driven oscillatory circuits, and many other real-world systems. Parameter variations—such as changes in the damping coefficient \(b\), driving amplitude \(A\), and driving frequency \(\omega_d\)—offer a rich context for further exploration of resonance, bifurcation, and chaotic transitions.


![alt text](<time series.png>)


![alt text](<phase space.png>)


![alt text](forced.png)

