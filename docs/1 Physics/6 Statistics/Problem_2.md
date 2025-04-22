# Estimating π via Monte Carlo Circle Sampling

## Task Option 1: Unit‑Circle Monte Carlo

### 1. Theoretical Background

A unit circle of radius 1 has area  
$$
A_\text{circle} = \pi \times 1^2 = \pi.
$$  
If we sample $N$ points uniformly in the square $[0,1]\times[0,1]$, the fraction that land inside the quarter‑circle $x^2 + y^2 \le 1$ approximates  
$$
\frac{N_{\rm in}}{N} \approx \frac{A_\text{quarter‑circle}}{1} = \frac{\pi}{4}.
$$  
Therefore,  
$$
\boxed{\pi \approx 4 \;\frac{N_{\rm in}}{N}}.
$$

### 2. Python Script

- Estimate π via circle sampling.  
- Display & save a scatter plot of points.  
- Show convergence of the estimate as $N$ increases.

```python
import numpy as np
import matplotlib.pyplot as plt

def estimate_pi_monte_carlo(N):
    """
    Estimate π by sampling N points in [0,1]^2
    and counting how many fall inside x^2 + y^2 ≤ 1.
    """
    pts = np.random.rand(N, 2)
    inside = np.sum(pts[:,0]**2 + pts[:,1]**2 <= 1.0)
    return 4 * inside / N

def plot_monte_carlo(N, save_path=None):
    """
    Plot N random points in [0,1]^2, coloring those inside the quarter‑circle.
    """
    pts = np.random.rand(N, 2)
    d2  = np.sum(pts**2, axis=1)
    inside  = pts[d2 <= 1]
    outside = pts[d2 >  1]

    fig, ax = plt.subplots(figsize=(6,6))
    ax.scatter(outside[:,0], outside[:,1], s=1, label='Outside')
    ax.scatter( inside[:,0],  inside[:,1], s=1, label='Inside')
    ax.add_patch(plt.Circle((0,0),1, fill=False, linewidth=1))
    ax.set_aspect('equal', 'box')
    ax.legend()
    ax.set_title(f'Monte Carlo π (N={N:,})')
    if save_path:
        plt.savefig(save_path, dpi=300, bbox_inches='tight')
    plt.show()

def convergence_curve(method, Ns, trials=5, save_path=None):
    """
    Plot convergence of π estimates vs. trial counts Ns for a given method.
    """
    means, stds = [], []
    for N in Ns:
        estimates = [method(N) for _ in range(trials)]
        means.append(np.nanmean(estimates))
        stds .append(np.nanstd(estimates))
    fig, ax = plt.subplots()
    ax.errorbar(Ns, means, yerr=stds, fmt='o-', capsize=3)
    ax.set_xscale('log')
    ax.set_xlabel('Number of samples $N$')
    ax.set_ylabel('Estimated π ±1σ')
    ax.axhline(np.pi, color='gray', linestyle='--', label='True π')
    ax.legend()
    if save_path:
        plt.savefig(save_path, dpi=300, bbox_inches='tight')
    plt.show()

if __name__ == "__main__":
    np.random.seed(0)

    # 1) Estimate π
    N_samples = 100_000
    pi_est = estimate_pi_monte_carlo(N_samples)
    print(f"Monte Carlo π estimate (N={N_samples:,}): {pi_est:.6f}")

    # 2) Scatter plot
    plot_monte_carlo(N_samples, save_path="monte_carlo_scatter.png")

    # 3) Convergence analysis
    sample_counts = [10**3, 10**4, 10**5, 10**6]
    convergence_curve(estimate_pi_monte_carlo, sample_counts, trials=5,
                      save_path="monte_carlo_convergence.png")
```

### 3. Visualization

- **Scatter Plot**: Displays $N$ points in $[0,1]^2$, with  
  - Points satisfying $x^2 + y^2 \le 1$ colored differently from those with $x^2 + y^2 > 1$.  
  - A quarter‑circle arc of radius 1 is overlaid to show the sampling region.  
- **Convergence Plot**: Plots the estimated $\pi$ versus sample size $N$ on a log scale, with  
  - Error bars representing $\pm1\sigma$ from repeated trials.  
  - A dashed horizontal line at the true value $\pi$ for reference.

### 4. Convergence Rate

The standard error (SE) of the Monte Carlo estimate decays as
$$
\mathrm{SE} \;\sim\; O\!\bigl(N^{-1/2}\bigr).
$$  
In practical terms, to reduce the error by one additional decimal digit (i.e.\ by a factor of 10), one must increase the number of samples by approximately $10^2 = 100$.

