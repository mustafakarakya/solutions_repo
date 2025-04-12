# Problem 2

# Escape Velocities and Cosmic Velocities

## Motivation

Understanding escape velocity is essential for space exploration. The concepts of the **first**, **second**, and **third cosmic velocities** extend this idea. They define the minimum speeds required to:
- Maintain a **stable circular orbit** near the surface (first cosmic velocity),
- **Escape** the gravitational pull of a celestial body (second cosmic velocity), and
- Escape from the host star's gravitational field once the body’s own gravity has been overcome (third cosmic velocity).

These thresholds are crucial not only for launching satellites but also for interplanetary and potential interstellar missions.

---

## 1. Definitions and Derivations

### 1.1 First Cosmic Velocity (Orbital Velocity)

The **first cosmic velocity** is the speed required for an object to maintain a circular orbit just above the celestial body’s surface. Deriving it starts by balancing the gravitational force with the required centripetal force:

$$
\frac{m\, v_1^2}{R} = \frac{G\, M\, m}{R^2},
$$

which simplifies to

$$
v_1 = \sqrt{\frac{GM}{R}}.
$$

---

### 1.2 Second Cosmic Velocity (Escape Velocity)

The **second cosmic velocity** is the minimum speed to escape the gravitational field from the surface. Using the conservation of energy – setting the total energy (kinetic plus potential) equal to zero – we have:

$$
\frac{1}{2} m\, v_2^2 - \frac{G\, M\, m}{R} = 0,
$$

which leads to

$$
v_2 = \sqrt{\frac{2GM}{R}}.
$$

Notice that

$$
v_2 = \sqrt{2}\, v_1.
$$

---

### 1.3 Third Cosmic Velocity (Heliocentric Escape Velocity)

The **third cosmic velocity** addresses the condition for a spacecraft to escape the gravitational influence of both the celestial body and its host star (e.g., the Sun). After escaping the body’s gravity, the spacecraft inherits the body’s heliocentric orbital speed, \(v_{orb}\). To completely escape the star’s gravity, the spacecraft’s resultant heliocentric speed must exceed the solar escape speed.

Thus, assuming the optimal case where the velocities add vectorially (i.e., when the launch direction is collinear with the orbital motion), the third cosmic velocity is given by:

$$
v_3 = \sqrt{v_2^2 + v_{orb}^2}.
$$

Here, \(v_{orb}\) is the orbital speed of the celestial body around the Sun.

---

## 2. Calculation of Cosmic Velocities

We now compute the three cosmic velocities for Earth and compare them with values for the Moon, Mars, and Jupiter.

### 2.1 Parameters

Let the universal gravitational constant be

$$
G = 6.67430 \times 10^{-11}\; \text{m}^3\,\text{kg}^{-1}\,\text{s}^{-2}.
$$

The properties of the bodies are as follows:

**Earth:**
- Mass: \( M_E = 5.972 \times 10^{24}\, \text{kg} \)
- Radius: \( R_E = 6.371 \times 10^{6}\, \text{m} \)
- Orbital velocity: \( v_{orb,E} \approx 29.78\, \text{km/s} = 29780\, \text{m/s} \)

**Moon:**
- Mass: \( M_M = 7.35 \times 10^{22}\, \text{kg} \)
- Radius: \( R_M = 1.7374 \times 10^{6}\, \text{m} \)
- Orbital velocity (around the Sun, nearly identical to Earth’s): \( v_{orb,M} \approx 29780\, \text{m/s} \)

**Mars:**
- Mass: \( M_{Mars} = 6.39 \times 10^{23}\, \text{kg} \)
- Radius: \( R_{Mars} = 3.3895 \times 10^{6}\, \text{m} \)
- Orbital velocity: \( v_{orb,Mars} \approx 24077\, \text{m/s} \)

**Jupiter:**
- Mass: \( M_J = 1.898 \times 10^{27}\, \text{kg} \)
- Radius: \( R_J = 6.9911 \times 10^{7}\, \text{m} \)
- Orbital velocity: \( v_{orb,J} \approx 13070\, \text{m/s} \)

The formulas used are:

$$
v_1 = \sqrt{\frac{GM}{R}},\quad
v_2 = \sqrt{\frac{2GM}{R}},\quad
v_3 = \sqrt{v_2^2 + v_{orb}^2}.
$$

For **Earth**, this yields approximately:
- \( v_1 \approx 7.91\, \text{km/s} \)
- \( v_2 \approx 11.18\, \text{km/s} \)
- \( v_3 \approx \sqrt{(11.18)^2 + (29.78)^2} \approx 31.60\, \text{km/s} \)

For **Moon**, this yields approximately:
- \( v_1 \approx 1.68\, \text{km/s} \)
- \( v_2 \approx \sqrt{2}\times 1.68 \approx 2.38\, \text{km/s} \)
- \( v_3 \approx \sqrt{(2.38)^2 + (29.78)^2} \approx 29.87\, \text{km/s} \)

For **Mars**, this yields approximately:
- \( v_1 \approx 3.55\, \text{km/s} \)
- \( v_2 \approx \sqrt{2}\times 3.55 \approx 5.03\, \text{km/s} \)
- \( v_3 \approx \sqrt{(5.03)^2 + (24.08)^2} \approx 24.60\, \text{km/s} \)

For **Jupiter**, this yields approximately:
- \( v_1 \approx 42.55\, \text{km/s} \)
- \( v_2 \approx \sqrt{2}\times 42.55 \approx 60.20\, \text{km/s} \)
- \( v_3 \approx \sqrt{(60.20)^2 + (13.07)^2} \approx 61.63\, \text{km/s} \)

---

## 3. Python Implementation and Simulation

Below are Python scripts that (1) calculate the velocities for each celestial body and generate a bar chart for comparison, and (2) create and save an animated GIF illustrating the three stages of cosmic velocity visually.

```python
import math
import matplotlib.pyplot as plt
import numpy as np

# Constants
G = 6.67430e-11  # gravitational constant in m^3 kg^-1 s^-2

# Data for celestial bodies: mass in kg, radius in m, orbital velocity (m/s)
bodies = {
    'Earth':   {'mass': 5.972e24, 'radius': 6.371e6,  'v_orb': 29780},
    'Moon':    {'mass': 7.35e22,  'radius': 1.7374e6, 'v_orb': 29780},
    'Mars':    {'mass': 6.39e23,  'radius': 3.3895e6, 'v_orb': 24077},
    'Jupiter': {'mass': 1.898e27, 'radius': 6.9911e7, 'v_orb': 13070},
}

# Function to compute cosmic velocities: v1, v2, v3
def compute_velocities(mass, radius, v_orb):
    v1 = math.sqrt(G * mass / radius)      # First cosmic velocity (orbital)
    v2 = math.sqrt(2 * G * mass / radius)    # Second cosmic velocity (escape)
    v3 = math.sqrt(v2**2 + v_orb**2)           # Third cosmic velocity (heliocentric escape)
    return v1, v2, v3

# Calculate the velocities and store the results
results = {}
for body, params in bodies.items():
    v1, v2, v3 = compute_velocities(params['mass'], params['radius'], params['v_orb'])
    results[body] = {'v1': v1, 'v2': v2, 'v3': v3}

# Print results in km/s
print("Cosmic Velocities (in km/s):")
for body, v in results.items():
    print(f"{body}:")
    print(f"  First Cosmic Velocity (v1): {v['v1']/1000:.2f} km/s")
    print(f"  Second Cosmic Velocity (v2): {v['v2']/1000:.2f} km/s")
    print(f"  Third Cosmic Velocity (v3): {v['v3']/1000:.2f} km/s")
    print("")

# Visualization: Bar chart to compare velocities for each celestial body
labels = list(bodies.keys())
v1_values = [results[body]['v1']/1000 for body in labels]
v2_values = [results[body]['v2']/1000 for body in labels]
v3_values = [results[body]['v3']/1000 for body in labels]

x = np.arange(len(labels))  # label locations
width = 0.25  # width of each bar

fig, ax = plt.subplots()
rects1 = ax.bar(x - width, v1_values, width, label='First Cosmic Velocity')
rects2 = ax.bar(x, v2_values, width, label='Second Cosmic Velocity')
rects3 = ax.bar(x + width, v3_values, width, label='Third Cosmic Velocity')

# Add labels, title, and custom x-axis tick labels
ax.set_ylabel('Velocity (km/s)')
ax.set_title('Comparison of Cosmic Velocities for Various Celestial Bodies')
ax.set_xticks(x)
ax.set_xticklabels(labels)
ax.legend()

# Function to annotate bars with their height values
def autolabel(rects):
    for rect in rects:
        height = rect.get_height()
        ax.annotate(f'{height:.1f}',
                    xy=(rect.get_x() + rect.get_width()/2, height),
                    xytext=(0, 3),  # vertical offset in points
                    textcoords="offset points",
                    ha='center', va='bottom')

autolabel(rects1)
autolabel(rects2)
autolabel(rects3)

plt.tight_layout()
plt.show()
```



```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation, PillowWriter
import matplotlib.patches as patches

# Frame counts for each stage:
N1 = 100  # Stage 1: Stable Orbit (First Cosmic Speed)
N2 = 100  # Stage 2: Acceleration and Escape (Second Cosmic Speed)
N3 = 100  # Stage 3: Heliocentric Escape (Third Cosmic Speed)

# Orbital radii of the planets in the solar system (scaled values)
orbit_radii = {
    'Mercury': 0.4,
    'Venus': 0.7,
    'Earth': 1.0,
    'Mars': 1.5,
}
# Orbital periods of the planets (in frames; conceptual values)
orbit_periods = {
    'Mercury': 40,
    'Venus': 70,
    'Earth': 100,
    'Mars': 150,
}

# Create a figure with 3 rows and 1 column (subplots stacked vertically).
fig, (ax1, ax2, ax3) = plt.subplots(3, 1, figsize=(8, 18))

def draw_solar_system(ax, frame, lim):
    """
    On the given axis, draw the solar system background: the Sun, the planetary orbits, and the planetary positions.
    """
    ax.set_xlim(-lim, lim)
    ax.set_ylim(-lim, lim)
    ax.set_aspect('equal')
    
    # Sun: at the center, yellow circle
    sun = patches.Circle((0, 0), 0.2, color='yellow')
    ax.add_patch(sun)
    
    # Draw each planet's orbit
    for planet, r in orbit_radii.items():
        orbit_circle = patches.Circle((0, 0), r, fill=False, linestyle='--', color='gray', alpha=0.5)
        ax.add_patch(orbit_circle)
    
    # Compute the positions of the planets based on their periods
    positions = {}
    for planet, r in orbit_radii.items():
        period = orbit_periods[planet]
        angle = 2 * np.pi * (frame / period)
        x = r * np.cos(angle)
        y = r * np.sin(angle)
        positions[planet] = (x, y)
        # Display the planets as small circles. Earth in blue, the others in black.
        color = 'blue' if planet == 'Earth' else 'black'
        planet_circle = patches.Circle((x, y), 0.05 * lim, color=color)
        ax.add_patch(planet_circle)
        ax.text(x + 0.05, y + 0.05, planet, fontsize=8)
    return positions

def update_stage1(frame):
    """
    Stage 1: Animation of an object in a stable orbit around Earth (First Cosmic Speed)
    """
    ax1.cla()  # Clear this subplot
    positions = draw_solar_system(ax1, frame, lim=2)
    earth_pos = positions['Earth']
    
    # The object's orbit around Earth (small satellite orbit):
    r_sat = 0.2
    # The object rotates, making 3 full revolutions (using frame mod N1)
    theta_sat = 2 * np.pi * (3 * frame / N1)
    sat_rel = np.array([r_sat * np.cos(theta_sat), r_sat * np.sin(theta_sat)])
    sat_pos = np.array(earth_pos) + sat_rel
    
    # Plot the object's position (red dot)
    ax1.plot(sat_pos[0], sat_pos[1], marker='o', markersize=8, color='red')
    
    # Calculate the approximate velocity vector (using a small delta for the derivative)
    delta = 0.01
    theta_sat_next = 2 * np.pi * (3 * (frame + delta) / N1)
    sat_rel_next = np.array([r_sat * np.cos(theta_sat_next), r_sat * np.sin(theta_sat_next)])
    sat_pos_next = np.array(earth_pos) + sat_rel_next
    vel = sat_pos_next - sat_pos
    ax1.arrow(sat_pos[0], sat_pos[1], vel[0]*5, vel[1]*5, head_width=0.05, head_length=0.05,
              fc='green', ec='green')
    
    # Information text
    ax1.text(-1.8, 1.8, "First Cosmic Speed:\nv₁ = √(GM/R) = 7.91 km/s", fontsize=10,
             bbox=dict(facecolor='white', alpha=0.5))
    ax1.set_title("Stage 1: Stable Orbit")
    ax1.set_xlabel("X [units]")
    ax1.set_ylabel("Y [units]")
    ax1.grid(True)

def update_stage2(frame):
    """
    Stage 2: Object's escape from Earth orbit (Second Cosmic Speed)
    """
    ax2.cla()
    positions = draw_solar_system(ax2, frame, lim=3)
    earth_pos = positions['Earth']
    
    # Parameter t: from 0 to 1 (using frame mod N2)
    t = frame / float(N2)
    r_sat = 0.2 + 0.5 * (t**2)  # Distance increasing in a spiral
    theta_sat = 2 * np.pi * (3 * t)
    sat_rel = np.array([r_sat * np.cos(theta_sat), r_sat * np.sin(theta_sat)])
    sat_pos = np.array(earth_pos) + sat_rel
    ax2.plot(sat_pos[0], sat_pos[1], marker='o', markersize=8, color='red')
    
    # Compute the velocity vector using a small derivative difference:
    delta = 0.01
    t_next = (frame + delta) / float(N2)
    r_sat_next = 0.2 + 0.5 * (t_next**2)
    theta_sat_next = 2 * np.pi * (3 * t_next)
    sat_rel_next = np.array([r_sat_next * np.cos(theta_sat_next), r_sat_next * np.sin(theta_sat_next)])
    sat_pos_next = np.array(earth_pos) + sat_rel_next
    vel = sat_pos_next - sat_pos
    ax2.arrow(sat_pos[0], sat_pos[1], vel[0]*5, vel[1]*5, head_width=0.05, head_length=0.05,
              fc='green', ec='green')
    
    ax2.text(-2.8, 2.5, "Second Cosmic Speed:\nv₂ = √(2GM/R) = 11.18 km/s", fontsize=10,
             bbox=dict(facecolor='white', alpha=0.5))
    ax2.set_title("Stage 2: Acceleration and Escape")
    ax2.set_xlabel("X [units]")
    ax2.set_ylabel("Y [units]")
    ax2.grid(True)

def update_stage3(frame):
    """
    Stage 3: Object's transition to a heliocentric orbit (Third Cosmic Speed)
    """
    ax3.cla()
    positions = draw_solar_system(ax3, frame, lim=5)
    earth_pos = positions['Earth']
    
    # Parameter t: from 0 to 1 (using frame mod N3)
    t = frame / float(N3)
    # In the heliocentric orbit, the distance increases: starting from Earth's orbit, adding 2 units extra.
    new_radius = 1.0 + 2 * t
    earth_angle = np.arctan2(earth_pos[1], earth_pos[0])
    sat_x = new_radius * np.cos(earth_angle)
    sat_y = new_radius * np.sin(earth_angle)
    sat_pos = np.array([sat_x, sat_y])
    ax3.plot(sat_pos[0], sat_pos[1], marker='o', markersize=8, color='red')
    
    # Calculate the velocity vector
    delta = 0.01
    t_next = (frame + delta) / float(N3)
    new_radius_next = 1.0 + 2 * t_next
    sat_x_next = new_radius_next * np.cos(earth_angle)
    sat_y_next = new_radius_next * np.sin(earth_angle)
    sat_pos_next = np.array([sat_x_next, sat_y_next])
    vel = sat_pos_next - sat_pos
    ax3.arrow(sat_pos[0], sat_pos[1], vel[0]*5, vel[1]*5, head_width=0.1, head_length=0.1,
              fc='green', ec='green')
    
    ax3.text(-4.8, 4.5, "Third Cosmic Speed:\nv₃ = √(v₂² + v_orb²) = 31.60 km/s", fontsize=10,
             bbox=dict(facecolor='white', alpha=0.5))
    ax3.set_title("Stage 3: Heliocentric Escape")
    ax3.set_xlabel("X [units]")
    ax3.set_ylabel("Y [units]")
    ax3.grid(True)

def update_all(global_frame):
    """
    global_frame: A single global frame number for all subplots.
    Each stage uses its own frame count (mod N1, N2, N3).
    """
    frame1 = global_frame % N1
    frame2 = global_frame % N2
    frame3 = global_frame % N3
    
    update_stage1(frame1)
    update_stage2(frame2)
    update_stage3(frame3)
    
    plt.tight_layout()

# Create a single animation where the global frame count increases from 0 to 1000.
anim = FuncAnimation(fig, update_all, frames=np.arange(0, 1000), interval=50, blit=False, repeat=True)

# Use PillowWriter to save the animation as a GIF
writer = PillowWriter(fps=20)
anim.save("cosmic_speeds.gif", writer=writer)

plt.show()
```

## 4. Discussion

- **First Cosmic Velocity, \( v_1 = \sqrt{\frac{GM}{R}} \):**  
  This value indicates the speed required to remain in a circular orbit just above the surface. For Earth, it is about 7.91 km/s.

- **Second Cosmic Velocity, \( v_2 = \sqrt{\frac{2GM}{R}} \):**  
  This is the escape speed from the gravitational pull of the celestial body. Earth's value is approximately 11.18 km/s.

- **Third Cosmic Velocity, \( v_3 = \sqrt{v_2^2 + v_{orb}^2} \):**  
  This speed ensures that, after leaving the planet, the spacecraft not only escapes the planet’s gravity but also has enough speed (when combined with the planet's orbital velocity) to overcome the Sun's gravitational pull. For Earth, this is roughly 31.60 km/s.

The differences in the computed velocities across Earth, the Moon, Mars, and Jupiter illustrate how variations in mass, radius, and orbital velocities influence the energy requirements for space missions. For instance, even though Jupiter has a significantly higher escape velocity because of its large mass, its relatively lower orbital speed around the Sun means that the third cosmic velocity is only marginally higher than its escape velocity.

---

## 5. Conclusion

This document provided a detailed derivation of the three cosmic velocities and computed their numerical values for Earth, while also offering a comparative analysis with the Moon, Mars, and Jupiter. These analyses are critical for planning and executing space missions—from satellite launches to interplanetary travel—by addressing the challenges of overcoming both a celestial body's local gravitational field and the broader gravitational pull of the host star.


![alt text](image-1.png)


![alt text](<cosmic speed.gif>)