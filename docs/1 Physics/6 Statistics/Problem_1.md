# Problem 1

# Exploring the Central Limit Theorem via Simulations

## 1. Introduction

The Central Limit Theorem (CLT) states that, given a population with finite mean \(\mu\) and variance \(\sigma^2\), the distribution of the sample mean
\[
\overline{X}_n = \frac{1}{n} \sum_{i=1}^n X_i
\]
approaches a normal distribution as the sample size \(n\) increases, regardless of the original population’s distribution.

- **Mathematical Statement**:  
  $$\overline{X}_n \xrightarrow{d} \mathcal{N}\bigl(\mu,\;\sigma^2/n\bigr)\quad\text{as }n\to\infty.$$
- **Implications**:  
  1. Even if the underlying population is skewed, the sampling distribution of the mean becomes symmetric.  
  2. The standard deviation of \(\overline{X}_n\) decreases as \(1/\sqrt{n}\).  

This report demonstrates the CLT by simulating three population distributions—Uniform, Exponential, and Binomial—and animating how their sampling‐mean histograms converge to normality as \(n\) grows.

---

## 2. Methodology

1. **Population Generation**  
   - Uniform: \(U(0,1)\)  
   - Exponential: \(\mathrm{Exp}(\lambda=1)\)  
   - Binomial: \(\mathrm{Binomial}(N=10, p=0.5)\)  
   For each, generate a “population” of \(100\,000\) samples.

2. **Sampling and Means**  
   - Sample sizes \(n = [5,\,10,\,30,\,50]\).  
   - For each \(n\), repeat \(M=2000\) draws (with replacement), compute \(\overline{X}_n\), and store the results.

3. **Animation of Histograms**  
   - Using Matplotlib’s `FuncAnimation`, produce an animated histogram that transitions through the four sample sizes.  
   - Overlay the theoretical normal density
     \[
     f(x) = \frac{1}{\sqrt{2\pi\,(\sigma^2/n)}}\exp\!\Bigl(-\tfrac{(x-\mu)^2}{2\,(\sigma^2/n)}\Bigr)
     \]
     for reference.

4. **Parameter Exploration**  
   - Observe how skewness in the exponential case slows convergence.  
   - Compare spreads: a population with larger \(\sigma^2\) yields a wider sampling‐mean distribution for small \(n\).

---

## 3. Python Implementation

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from scipy.stats import norm

# 1. Define populations
pop_size = 100_000
populations = {
    'Uniform(0,1)': np.random.rand(pop_size),
    'Exponential(λ=1)': np.random.exponential(scale=1.0, size=pop_size),
    'Binomial(n=10,p=0.5)': np.random.binomial(n=10, p=0.5, size=pop_size)
}

sample_sizes = [5, 10, 30, 50]
M = 2000  # number of samples per n

def simulate_means(data, n, M):
    """Draw M samples of size n and return their means."""
    idx = np.random.randint(0, len(data), size=(M, n))
    return data[idx].mean(axis=1)

# 2. Prepare figure
fig, axes = plt.subplots(1, len(populations), figsize=(15, 4))
plt.tight_layout()

# 3. Animation function
def animate(k):
    n = sample_sizes[k]
    for ax, (label, pop) in zip(axes, populations.items()):
        ax.clear()
        means = simulate_means(pop, n, M)
        mu, sigma = pop.mean(), pop.std(ddof=1)
        # Histogram of sample means
        ax.hist(means, bins=30, density=True, alpha=0.6)
        # Theoretical normal curve
        x = np.linspace(means.min(), means.max(), 200)
        ax.plot(x, norm.pdf(x, loc=mu, scale=sigma/np.sqrt(n)),
                lw=2, label=f'Normal PDF\n($n={n}$)')
        ax.set_title(f'{label}\nSample size $n={n}$')
        ax.legend()
        ax.set_xlabel('Sample mean')
        ax.set_ylabel('Density')

ani = animation.FuncAnimation(
    fig, animate, frames=len(sample_sizes), interval=1500, repeat_delay=2000
)

# To display in Jupyter: from IPython.display import HTML; HTML(ani.to_jshtml())
# Or save: ani.save('clt_animation.mp4', writer='ffmpeg')
plt.show()
```

## 4. Results

- **Uniform Distribution**  
  - At \(n=5\), the histogram already shows a bell‑shape, and by \(n=10\) it closely aligns with the normal PDF.  
  - Variance of the sampling distribution shrinks by a factor of \(1/\sqrt{n}\), yielding narrower histograms as \(n\) increases.

- **Exponential Distribution**  
  - For \(n=5\), the sampling‑mean histogram remains noticeably right‑skewed.  
  - By \(n=30\), the shape is nearly symmetric, and at \(n=50\) it overlays the theoretical normal curve almost perfectly.

- **Binomial Distribution**  
  - The original binomial (with parameters \(N=10, p=0.5\)) is already approximately symmetric.  
  - Convergence is subtler: even at \(n=5\), the sampling mean distribution is very close to Gaussian, tightening further at larger \(n\).

---

## 5. Discussion

1. **Sample Size and Shape**  
   - Smaller samples (\(n\le10\)) retain features of the original distribution (e.g., skewness in the exponential case).  
   - Moderate sizes (\(n\ge30\)) yield sampling distributions almost indistinguishable from \(\mathcal{N}(\mu,\sigma^2/n)\).

2. **Variance Effects**  
   - The standard deviation of the sampling distribution is \(\sigma/\sqrt{n}\).  
   - Populations with larger \(\sigma^2\) (like the exponential) produce wider histograms for each fixed \(n\), but still converge as \(n\) grows.

3. **Real‑World Implications**  
   - **Parameter Estimation**: Allows construction of confidence intervals for unknown means.  
   - **Quality Control**: Control charts use sample means and assume normality for threshold limits.  
   - **Financial Modeling**: Aggregated returns over days or weeks approximate normality, supporting risk‑management techniques.

---

## 6. Conclusion

The animated histograms convincingly illustrate that, regardless of the original population distribution—uniform, exponential, or binomial—the sampling distribution of the mean approaches a normal distribution as the sample size increases. Convergence rate depends on the original shape and variance, but by \(n\approx30\), the normal approximation is highly accurate, validating the Central Limit Theorem’s foundational role in statistical inference.  
