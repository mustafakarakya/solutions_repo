# Problem 2


# Problem 2: Investigating the Dynamics of a Forced Damped Pendulum

## 1. Introduction

The forced damped pendulum is a rich example of how external driving and energy dissipation can lead to diverse behaviors—from simple periodic motion to resonance and chaos. In this report, we compare four main scenarios, following the instructor’s notes:

1. **Pure Pendulum (no damping, no forcing)**
2. **Damped Pendulum (damping \(\neq 0\), forcing \(=0\))**
3. **Forced Pendulum (forcing \(\neq 0\), damping \(=0\))**
4. **Forced Damped Pendulum (damping \(\neq 0\), forcing \(\neq 0\))**

We will produce time-domain plots (showing \(\theta\) vs. \(t\)) and phase portraits (showing \(\theta'\) vs. \(\theta\)). The transition from regular to chaotic motion, resonance peaks, and other phenomena can be explored by systematically changing parameters such as damping coefficient \(b\), driving amplitude \(A\), and driving frequency \(\Omega\).

---

## 2. Theoretical Foundation

A forced damped pendulum can be described by the equation:

\[
\frac{d^2 \theta}{dt^2} \;+\; b \,\frac{d\theta}{dt} \;+\; \omega_0^2 \,\sin(\theta) \;=\; A \,\sin(\Omega t),
\]

where
- \(\theta\) is the angular displacement,
- \(b\) is the damping coefficient,
- \(\omega_0\) is the natural frequency of the pendulum (e.g., \(\sqrt{g/L}\) for length \(L\)),
- \(A\) and \(\Omega\) are the amplitude and frequency of the external driving force.

**Scenarios**:
1. **Pure Pendulum**: \(b=0\), \(A=0\).
2. **Damped Pendulum**: \(b>0\), \(A=0\).
3. **Forced Pendulum**: \(b=0\), \(A>0\).
4. **Forced Damped Pendulum**: \(b>0\), \(A>0\).

Approximations for small angles (\(\sin\theta \approx \theta\)) lead to near-harmonic solutions, but large-angle or chaotic behavior requires numerical methods (e.g., Runge-Kutta).

---

## 3. Implementation

Below you can see the various simulations, and just below the simulations there is a comprehensive Python script. Each scenario outputs two types of plots (time series and phase portrait) as **GIF** animations:

1. **Pure Pendulum**  
2. **Damped Pendulum**  
3. **Forced Pendulum**  
4. **Forced Damped Pendulum**  

These comparisons highlight how damping reduces amplitude over time, how forcing can drive resonance, and how combining both can yield complex or chaotic motion.

````python

import matplotlib
matplotlib.use('Agg')  # Non-interactive backend to save GIFs without GUI

import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation, PillowWriter

#############################
# 1) Equation of Motion
#############################
def forced_damped_pendulum(t, state, b, omega0, A, Omega):
    """
    state = [theta, theta_dot]
    b = damping coefficient
    omega0 = natural frequency
    A = driving amplitude
    Omega = driving frequency
    Returns dtheta/dt, d^2theta/dt^2
    """
    theta, theta_dot = state
    # dtheta_dt = theta_dot
    # d^2theta_dt^2 = -b * theta_dot - omega0^2 sin(theta) + A sin(Omega t)
    dtheta = theta_dot
    dtheta_dot = -b * theta_dot - omega0**2 * np.sin(theta) + A * np.sin(Omega * t)
    return np.array([dtheta, dtheta_dot])

def rk4_step(func, t, state, dt, *params):
    """
    One step of RK4 integration.
    func(t, state, *params) -> derivatives
    """
    k1 = func(t, state, *params)
    k2 = func(t + dt/2, state + dt*k1/2, *params)
    k3 = func(t + dt/2, state + dt*k2/2, *params)
    k4 = func(t + dt,   state + dt*k3,   *params)
    return state + (dt/6)*(k1 + 2*k2 + 2*k3 + k4)

def simulate_pendulum(b, A, Omega, omega0, theta0, theta_dot0, tmax=20, dt=0.01):
    """
    Simulate the forced damped pendulum with given parameters.
    Returns arrays of time, theta(t), and theta_dot(t).
    """
    steps = int(tmax/dt)
    t_vals = np.zeros(steps)
    theta_vals = np.zeros(steps)
    theta_dot_vals = np.zeros(steps)

    state = np.array([theta0, theta_dot0])
    time = 0.0
    for i in range(steps):
        t_vals[i] = time
        theta_vals[i] = state[0]
        theta_dot_vals[i] = state[1]
        # RK4 step
        state = rk4_step(forced_damped_pendulum, time, state, dt, b, omega0, A, Omega)
        time += dt
    return t_vals, theta_vals, theta_dot_vals

#############################
# 2) Animation Functions
#############################
def animate_time_series(t, theta, scenario_name, gif_name):
    """
    Animate the time series of theta(t) as a GIF.
    """
    fig, ax = plt.subplots(figsize=(6,4))
    ax.set_title(f"{scenario_name}: Time Series")
    ax.set_xlabel("Time (s)")
    ax.set_ylabel("Theta (rad)")
    ax.grid(True)

    line, = ax.plot([], [], lw=2)

    ax.set_xlim(t[0], t[-1])
    ax.set_ylim(1.1*min(theta), 1.1*max(theta))  # some margin

    def init():
        line.set_data([], [])
        return line,

    def update(frame):
        line.set_data(t[:frame], theta[:frame])
        return line,

    frames = len(t)
    anim = FuncAnimation(fig, update, frames=range(frames), init_func=init, blit=False)
    anim.save(gif_name, writer=PillowWriter(fps=30))
    plt.close(fig)

def animate_phase_portrait(theta, theta_dot, scenario_name, gif_name):
    """
    Animate the phase portrait (theta_dot vs. theta).
    """
    fig, ax = plt.subplots(figsize=(6,4))
    ax.set_title(f"{scenario_name}: Phase Portrait")
    ax.set_xlabel("Theta (rad)")
    ax.set_ylabel("Theta Dot (rad/s)")
    ax.grid(True)

    line, = ax.plot([], [], lw=2)

    ax.set_xlim(1.1*min(theta), 1.1*max(theta))
    ax.set_ylim(1.1*min(theta_dot), 1.1*max(theta_dot))

    def init():
        line.set_data([], [])
        return line,

    def update(frame):
        line.set_data(theta[:frame], theta_dot[:frame])
        return line,

    frames = len(theta)
    anim = FuncAnimation(fig, update, frames=range(frames), init_func=init, blit=False)
    anim.save(gif_name, writer=PillowWriter(fps=30))
    plt.close(fig)

#############################
# 3) Scenarios
#############################

def scenario_pure_pendulum():
    """
    b=0, A=0
    """
    # Example parameters
    b=0.0
    A=0.0
    Omega=1.0
    omega0=3.0
    theta0=0.2
    theta_dot0=0.0

    t, theta, theta_dot = simulate_pendulum(b, A, Omega, omega0, theta0, theta_dot0, tmax=20, dt=0.01)
    animate_time_series(t, theta, "Pure Pendulum", "scenario_pure_timeseries.gif")
    animate_phase_portrait(theta, theta_dot, "Pure Pendulum", "scenario_pure_phase.gif")

def scenario_damped_pendulum():
    """
    b>0, A=0
    """
    b=0.2
    A=0.0
    Omega=1.0
    omega0=3.0
    theta0=0.2
    theta_dot0=0.0

    t, theta, theta_dot = simulate_pendulum(b, A, Omega, omega0, theta0, theta_dot0, tmax=20, dt=0.01)
    animate_time_series(t, theta, "Damped Pendulum", "scenario_damped_timeseries.gif")
    animate_phase_portrait(theta, theta_dot, "Damped Pendulum", "scenario_damped_phase.gif")

def scenario_forced_pendulum():
    """
    b=0, A>0
    """
    b=0.0
    A=0.5
    Omega=2.0
    omega0=3.0
    theta0=0.2
    theta_dot0=0.0

    t, theta, theta_dot = simulate_pendulum(b, A, Omega, omega0, theta0, theta_dot0, tmax=20, dt=0.01)
    animate_time_series(t, theta, "Forced Pendulum", "scenario_forced_timeseries.gif")
    animate_phase_portrait(theta, theta_dot, "Forced Pendulum", "scenario_forced_phase.gif")

def scenario_forced_damped_pendulum():
    """
    b>0, A>0
    """
    b=0.2
    A=0.5
    Omega=2.0
    omega0=3.0
    theta0=0.2
    theta_dot0=0.0

    t, theta, theta_dot = simulate_pendulum(b, A, Omega, omega0, theta0, theta_dot0, tmax=30, dt=0.01)
    animate_time_series(t, theta, "Forced Damped Pendulum", "scenario_forced_damped_timeseries.gif")
    animate_phase_portrait(theta, theta_dot, "Forced Damped Pendulum", "scenario_forced_damped_phase.gif")

if __name__ == "__main__":
    scenario_pure_pendulum()
    scenario_damped_pendulum()
    scenario_forced_pendulum()
    scenario_forced_damped_pendulum()
    print("All scenarios done. GIF files generated for each scenario.")

    ````