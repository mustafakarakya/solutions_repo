# Problem 1

# Orbital Period and Orbital Radius: Applications of Kepler's Third Law

## 1. Introduction

This document explores the fundamental principles and applications of Kepler's Third Law. According to the law, there is a linear relationship between the square of the orbital period and the cube of the orbital radius for a body in orbit. This law is crucial in analyzing the orbits of planets, moons, and other celestial objects. The work covers theoretical derivation, real-world examples (such as the Moon's orbit around the Earth and the orbits of planets in the Solar System), and computational verification using Python simulations.

---

## 2. Theoretical Background

### 2.1 Derivation of Kepler's Third Law for Circular Orbits

For a circular orbit, a planet or satellite moves around a central mass (e.g., the Sun or the Earth) along a path with a constant radius. In such an orbit, the gravitational force supplies the necessary centripetal force for circular motion.

1. **Gravitational Force:**  
   \[
   F_{g} = \frac{GMm}{r^2}
   \]
   - \(G\): Universal gravitational constant  
   - \(M\): Mass of the central body  
   - \(m\): Mass of the orbiting object  
   - \(r\): Orbital radius  

2. **Centripetal Force for Circular Motion:**  
   \[
   F_{c} = m \frac{v^2}{r}
   \]
   - \(v\): Orbital speed of the object  

3. **Equating the Forces:**  
   In a circular orbit the gravitational force provides the required centripetal force:
   \[
   \frac{GMm}{r^2} = m \frac{v^2}{r}
   \]
   Cancelling the mass \(m\):
   \[
   v^2 = \frac{GM}{r}
   \]

4. **Expressing the Orbital Period:**  
   The orbital period \(T\) is the time required to complete one full orbit. Given that the orbit's circumference is \(2\pi r\), we have:
   \[
   T = \frac{2\pi r}{v}
   \]
   Substituting the expression for \(v\):
   \[
   T = \frac{2\pi r}{\sqrt{GM/r}} = 2\pi \sqrt{\frac{r^3}{GM}}
   \]

5. **Kepler's Third Law:**  
   Squaring both sides gives:
   \[
   T^2 = \frac{4\pi^2}{GM} \, r^3
   \]
   This result shows that the square of the orbital period is directly proportional to the cube of the orbital radius. The proportionality constant is \(\frac{4\pi^2}{GM}\).

### 2.2 Conclusion and Interpretation

The derived relationship indicates:
- **Fundamental Relation:**  
  \[
  T^2 \propto r^3
  \]
- Although derived for circular orbits, this proportionality holds for elliptical orbits when the orbital radius is replaced by the semi-major axis.

---

## 3. Astrophysical Applications

### 3.1 Analysis of Planetary and Satellite Orbits

- **Planetary Orbits:**  
  In the Solar System, the orbital periods and radii of the planets adhere to the relationship defined by Kepler's Third Law. For instance, the Earth's orbital radius is approximately \(1.496 \times 10^{11}\) m and its orbital period is about \(3.16 \times 10^7\) s, which are consistent with the theoretical formula.
  
- **Moon’s Orbit:**  
  The Moon’s orbit around the Earth can be similarly analyzed. With an orbital radius of roughly 384,400 km and a period of about 27.3 days, these values enable calculations of other system properties and central masses using Kepler's relation.

### 3.2 Mass and Distance Calculations

- **Calculating the Central Mass:**  
  If the orbital period and radius are known for a satellite or planet, the mass of the central body (e.g., the Sun) can be calculated by rearranging Kepler’s law:
  \[
  M = \frac{4\pi^2 r^3}{G T^2}
  \]
  
- **Spacecraft and Artificial Satellites:**  
  This law is also applicable in orbit determination for spacecraft and artificial satellites, providing critical insights for mission design and orbital maintenance.

---

## 4. Computational Model and Simulation Using Python

The following Python scripts simulate both circular orbits and the linear relationship between \(T^2\) and \(r^3\). 

### 4.1 Circular Orbit Simulation

In this section, we simulate the Earth’s circular orbit around the Sun.

```python
# Import necessary libraries
import numpy as np
import matplotlib.pyplot as plt

# Universal constants and parameters
G = 6.67430e-11  # Universal gravitational constant (m^3 kg^-1 s^-2)
M = 1.989e30     # Mass of the Sun (kg)
r = 1.496e11     # Average distance of Earth from the Sun (m)

# Earth's orbital speed
v = np.sqrt(G * M / r)

# Orbital period calculation
T = 2 * np.pi * np.sqrt(r**3 / (G * M))
print(f"Orbital Period (T): {T:.2e} s")

# Simulation parameters
num_points = 1000
t = np.linspace(0, T, num_points)
theta = 2 * np.pi * t / T  # Angular position as a function of time

# Orbit coordinates in the x-y plane
x = r * np.cos(theta)
y = r * np.sin(theta)

# Plotting the circular orbit
plt.figure(figsize=(6,6))
plt.plot(x, y, label="Circular Orbit")
plt.scatter(0, 0, color='orange', s=200, label="Sun")
plt.xlabel("x (m)")
plt.ylabel("y (m)")
plt.title("Circular Orbit Simulation")
plt.legend()
plt.grid(True)
plt.axis('equal')
plt.show()
```

### 4.2 Graph of the Linear Relation between \(T^2\) and \(r^3\)

The following code calculates orbital periods for different orbital radii and plots \(T^2\) against \(r^3\) to demonstrate the linear relationship.

```python
# Examples with different orbital radii (from 0.5 AU to 3 AU)
r_vals = np.linspace(0.5, 3, 50) * 1.496e11  # Converting AU to meters (1 AU = 1.496e11 m)
T_vals = 2 * np.pi * np.sqrt(r_vals**3 / (G * M))

# Calculate T^2 and r^3 values
T_squared = T_vals**2
r_cubed = r_vals**3

# Plotting the graph
plt.figure(figsize=(8,6))
plt.scatter(r_cubed, T_squared, color='blue', label="$T^2$ Values")
# Plot the theoretical linear relationship: T^2 = (4π^2/GM)*r^3
slope = 4 * np.pi**2 / (G * M)
r_cubed_fit = np.linspace(min(r_cubed), max(r_cubed), 100)
T_squared_fit = slope * r_cubed_fit
plt.plot(r_cubed_fit, T_squared_fit, color='red', label="Theoretical Linear Relation")
plt.xlabel("$r^3$ (m$^3$)")
plt.ylabel("$T^2$ (s$^2$)")
plt.title("Linear Relation between $T^2$ and $r^3$")
plt.legend()
plt.grid(True)
plt.show()
```

The codes above enable the following:
- **Circular Orbit Simulation:** Visualizes the Earth's orbit around the Sun.
- **\(T^2\) vs. \(r^3\) Graph:** Demonstrates that the computed orbital period values conform to the linear relationship predicted by theory.

---

## 5. Graphical Representations

The Python code yields two main visualizations:
- **Orbit Simulation Graph:** This graph displays the circular orbit along with the Sun at the center.
- **\(T^2\) vs. \(r^3\) Graph:** This scatter plot shows the relationship between the computed values, aligning closely with the theoretical prediction \(T^2 = \frac{4\pi^2}{GM} r^3\).

These graphics confirm that Kepler’s Third Law is both mathematically consistent and practically observable.

---

## 6. Extension to Elliptical Orbits

In reality, many planets and satellites follow elliptical orbits rather than perfect circles. Kepler’s Third Law still applies for elliptical orbits when the orbital radius is replaced by the **semi-major axis (a)**:

\[
T^2 = \frac{4\pi^2}{GM} a^3
\]

Here:
- \(a\): The semi-major axis of the elliptical orbit

This extension is essential when analyzing slightly eccentric orbits. Although minor corrections might be necessary for very elliptical cases, the basic proportionality remains valid.

---

## 7. Conclusion

In this solution:
- **Derivation:** The gravitational and centripetal forces were equated to derive the relation \(T^2 = \frac{4\pi^2}{GM} r^3\) for circular orbits.
- **Astrophysical Applications:** The formula is used to analyze planetary and satellite orbits and to calculate the masses of central objects (e.g., the Sun or Earth).
- **Simulation:** Python simulations were conducted to visualize both the circular orbit and the linear relationship between \(T^2\) and \(r^3\).
- **Extension to Elliptical Orbits:** The solution extends to elliptical orbits using the semi-major axis, maintaining the validity of the relationship.