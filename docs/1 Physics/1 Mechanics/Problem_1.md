# Investigating the Range as a Function of the Angle of Projection

## 1. Introduction

Projectile motion is a cornerstone of classical mechanics, illustrating how an object behaves under uniform gravitational acceleration after being launched with an initial velocity. Although the basic equations of projectile motion are straightforward, the problem offers a rich landscape of analysis. In this report, we investigate how the horizontal range depends on the angle of projection, exploring the theoretical foundation, computational implementation, and real-world applications. We also discuss how variations in initial conditions—such as velocity, gravity, and launch height—expand the family of solutions, giving rise to multiple trajectories that can model diverse physical phenomena.

---

## 2. Theoretical Foundation

### 2.1 Governing Equations

The motion of a projectile in two-dimensional space \((x, y)\) can be derived from Newton’s laws under constant gravitational acceleration \(g\). Neglecting air resistance, the differential equations of motion are:

$$
\frac{d^2 x}{dt^2} = 0, 
\quad
\frac{d^2 y}{dt^2} = -g.
$$

These simplify to:

1. **Horizontal Motion** (no acceleration):
   $$
   \frac{d^2 x}{dt^2} = 0 
   \quad \Longrightarrow \quad
   \frac{dx}{dt} = v_0 \cos(\theta), 
   \quad
   x(t) = v_0 \cos(\theta)\, t.
   $$

2. **Vertical Motion** (constant acceleration \(-g\)):
   $$
   \frac{d^2 y}{dt^2} = -g 
   \quad \Longrightarrow \quad
   \frac{dy}{dt} = v_0 \sin(\theta) - g t, 
   \quad
   y(t) = v_0 \sin(\theta)\, t - \tfrac{1}{2} g t^2.
   $$

Here, 
- \(v_0\) is the initial speed, 
- \(\theta\) is the angle of projection (relative to the horizontal), 
- \(x(t)\) and \(y(t)\) are the horizontal and vertical positions as functions of time.

---

### 2.2 Time of Flight and Range

For a projectile launched from ground level and returning to the same vertical level \((y = 0)\), the time of flight \(T\) is obtained by solving \(y(T) = 0\):

$$
y(T) = v_0 \sin(\theta)\, T - \tfrac{1}{2} g\, T^2 = 0.
$$

This yields two solutions: \(T = 0\) (initial launch) and

$$
T = \frac{2\,v_0 \sin(\theta)}{g}.
$$

Substituting \(T\) into the expression for \(x(t)\) gives the **range** \(R\):

$$
R = x(T) 
  = v_0 \cos(\theta)\,\frac{2\,v_0 \sin(\theta)}{g}
  = \frac{v_0^2 \sin(2\theta)}{g}.
$$

---

### 2.3 Family of Solutions

Varying parameters like \(\theta\), \(v_0\), or \(g\) leads to a family of possible trajectories:

- **Angle \(\theta\)**: Influences the shape of the trajectory. The maximum range is achieved when \(\sin(2\theta) = 1 \implies \theta = 45^\circ\).
- **Initial Velocity \(v_0\)**: The range depends quadratically on \(v_0\). Doubling \(v_0\) quadruples the theoretical range.
- **Gravitational Acceleration \(g\)**: Lower \(g\) (e.g., on the Moon) increases the range; higher \(g\) (e.g., on Jupiter) reduces it.
- **Initial Height \(h\)**: If launched from a nonzero height, the time in the air changes, altering the total range and trajectory shape.

---

## 3. Analysis of the Range

1. **Range vs. Angle**

   $$
   R(\theta) = \frac{v_0^2}{g} \,\sin(2\theta).
   $$

   - **Maximum Range**: \(\theta = 45^\circ\).
   - **Symmetry**: \(\sin(2\theta)\) has the same value for angles \(\theta\) and \((90^\circ - \theta)\).  
     For example, \(30^\circ\) and \(60^\circ\) produce the same range (neglecting air resistance).

2. **Influence of Velocity**  
   - Higher launch velocity shifts the entire range curve upward (since \(v_0^2\) appears in the numerator).

3. **Impact of Gravity**  
   - A smaller \(g\) value extends the flight time and range; a larger \(g\) reduces both.

---

## 4. Practical Applications

Projectile motion underpins many real-world scenarios:

## Sports
- **Football (Soccer):** Kicking the ball with an optimal angle to maximize distance.
- **Basketball:** Adjusting angle and velocity to achieve precise arcs into the hoop.

## Engineering
- **Cannons and Ballistics:** Calculating the range for shells or projectiles under Earth’s gravity.
- **Launch Systems:** Designing rocket trajectories for short suborbital flights.

## Astrophysics
- **Satellite Launches:** Transfer orbits rely on projectile-like arcs under planetary gravity.
- **Interplanetary Missions:** Calculating gravitational assists and slingshots around celestial bodies.

## Environmental Studies
- **Pollutant Dispersion:** Modeling how particles travel in the atmosphere or water.
- **Wildfire Smoke Trajectories:** Predicting how far smoke travels and in which direction.

---

## 5. Implementation

Below is a comprehensive Python script that demonstrates several simulations:

1. **Three different initial velocities** on the same plot (angle fixed).  
2. **Same initial conditions on three different gravitational fields** (e.g., Earth, Moon, Jupiter).  
3. **Different initial heights** with the same velocity and angle.  
4. **With and without air resistance** for a chosen angle and velocity.

```python
import numpy as np
import matplotlib.pyplot as plt

# -------------------------
# 1. Projectile Trajectory (No Air Resistance)
# -------------------------
def projectile_no_drag(v0, angle_deg, g=9.81, h=0, dt=0.02):
    """
    Returns the (x, y) arrays for a projectile launched
    with initial velocity v0 (m/s), at angle_deg (degrees),
    from height h (m), under gravitational acceleration g (m/s^2).
    """
    angle_rad = np.radians(angle_deg)
    vx = v0 * np.cos(angle_rad)
    vy = v0 * np.sin(angle_rad)
    
    x, y = [0], [h]
    t = 0
    while y[-1] >= 0:  # Continue until the projectile hits the ground
        t += dt
        x_new = x[-1] + vx * dt
        vy_new = vy - g * dt
        y_new = y[-1] + vy * dt - 0.5 * g * dt**2
        
        x.append(x_new)
        y.append(y_new)
        vy = vy_new
        
    return np.array(x), np.array(y)

# -------------------------
# 2. Projectile Trajectory (With Air Resistance)
# -------------------------
def projectile_with_drag(v0, angle_deg, g=9.81, h=0, k=0.1, dt=0.02):
    """
    Returns the (x, y) arrays for a projectile launched
    with initial velocity v0 (m/s), at angle_deg (degrees),
    from height h (m), under gravity g (m/s^2),
    with a linear drag coefficient k (kg/s).
    """
    angle_rad = np.radians(angle_deg)
    vx = v0 * np.cos(angle_rad)
    vy = v0 * np.sin(angle_rad)
    
    x, y = [0], [h]
    t = 0
    while y[-1] >= 0:
        t += dt
        # Drag force approximated as -k * v
        ax = -k * vx
        ay = -g - k * vy
        
        # Update velocities
        vx += ax * dt
        vy += ay * dt
        
        # Update positions
        x_new = x[-1] + vx * dt
        y_new = y[-1] + vy * dt
        
        x.append(x_new)
        y.append(y_new)
        
    return np.array(x), np.array(y)

# -------------------------
# Example Simulations
# -------------------------

# 1. Three different velocities, same angle
plt.figure(figsize=(8,5))
velocities = [10, 20, 30]
angle = 45
for v in velocities:
    x_arr, y_arr = projectile_no_drag(v, angle)
    plt.plot(x_arr, y_arr, label=f'v0 = {v} m/s')
plt.title("Projectile Motion with Different Initial Velocities (Angle = 45°)")
plt.xlabel("Horizontal Distance (m)")
plt.ylabel("Vertical Distance (m)")
plt.grid(True)
plt.legend()
plt.show()

# 2. Same initial velocity and angle, different gravitational accelerations
plt.figure(figsize=(8,5))
g_planets = {'Earth': 9.81, 'Moon': 1.62, 'Jupiter': 24.79}
v0 = 20
angle = 45
for planet, g_val in g_planets.items():
    x_arr, y_arr = projectile_no_drag(v0, angle, g=g_val)
    plt.plot(x_arr, y_arr, label=f'{planet} (g={g_val:.2f} m/s^2)')
plt.title("Projectile Motion on Different Planets (v0=20 m/s, Angle=45°)")
plt.xlabel("Horizontal Distance (m)")
plt.ylabel("Vertical Distance (m)")
plt.grid(True)
plt.legend()
plt.show()

# 3. Different initial heights, same velocity and angle
plt.figure(figsize=(8,5))
heights = [0, 10, 20]
for h in heights:
    x_arr, y_arr = projectile_no_drag(20, 45, h=h)
    plt.plot(x_arr, y_arr, label=f'Height = {h} m')
plt.title("Projectile Motion with Different Launch Heights")
plt.xlabel("Horizontal Distance (m)")
plt.ylabel("Vertical Distance (m)")
plt.grid(True)
plt.legend()
plt.show()

# 4. With and without air resistance
plt.figure(figsize=(8,5))
x_no_drag, y_no_drag = projectile_no_drag(20, 45)
x_drag, y_drag = projectile_with_drag(20, 45, k=0.08)
plt.plot(x_no_drag, y_no_drag, label="No Air Resistance")
plt.plot(x_drag, y_drag, '--', label="With Air Resistance (k=0.08)")
plt.title("Projectile Motion Comparison: No Drag vs. With Drag")
plt.xlabel("Horizontal Distance (m)")
plt.ylabel("Vertical Distance (m)")
plt.grid(True)
plt.legend()
plt.show()
```