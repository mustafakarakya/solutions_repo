# Problem 1
## Exploring the Central Limit Theorem through Simulations

### 1. Motivation

The Central Limit Theorem (CLT) states that the sampling distribution of the sample mean approaches a normal distribution as the sample size increases, regardless of the population’s original distribution. By running computational experiments, we can observe how different population shapes and variances affect the convergence to normality.

---

### 2. Methods

**Population Distributions**  
- Uniform: \(U(0,1)\)  
- Exponential: \(\mathrm{Exp}(\lambda=1)\)  
- Binomial: \(\mathrm{Binomial}(n=10, p=0.5)\)  

**Sampling Parameters**  
- Sample sizes: \(n = [5, 10, 30, 50]\)  
- Number of repeated samples per simulation: \(R = 500\)

**Approach**  
1. Generate a large “population” of size \(N = 100\,000\) for each distribution.  
2. For each \(n\), repeat \(R\) times: draw a sample of size \(n\), compute its mean.  
3. Animate the histogram of sample means as the number of repeats grows from 1 to \(R\).

---

### 3. Python Implementation (in one block)

```python
import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
from IPython.display import HTML

# Configure global parameters
pop_size = 100_000
sample_sizes = [5, 10, 30, 50]
R = 500
bins = 30

# Generate populations
populations = {
    'Uniform(0,1)': np.random.rand(pop_size),
    'Exponential(1)': np.random.exponential(scale=1.0, size=pop_size),
    'Binomial(10,0.5)': np.random.binomial(n=10, p=0.5, size=pop_size)
}

def animate_clt(pop_data, label):
    """Create and return HTML animation of sample‐mean histograms."""
    fig, ax = plt.subplots(figsize=(6,4))
    ax.set_xlabel('Sample mean')
    ax.set_ylabel('Density')
    ax.set_title(f'{label} → CLT convergence')

    def update(k):
        ax.clear()
        means = np.mean(
            np.random.choice(pop_data, size=(k+1, current_n)),
            axis=1
        )
        ax.hist(means, bins=bins, density=True, alpha=0.7)
        # Overlay normal pdf
        mu, sigma = np.mean(means), np.std(means)
        x = np.linspace(means.min(), means.max(), 200)
        ax.plot(x, 
                1/(sigma*np.sqrt(2*np.pi)) * np.exp(-(x-mu)**2/(2*sigma**2)),
                'r--', label='Normal PDF')
        ax.legend()
        ax.set_title(f'{label}, n={current_n}, repeats={k+1}')
    
    anim = FuncAnimation(fig, update, frames=R, interval=50, repeat=False)
    return HTML(anim.to_jshtml())

# Run animations for each distribution and sample size
for dist_label, pop in populations.items():
    for current_n in sample_sizes:
        display(animate_clt(pop, dist_label))
```

---

### 4. Results

- As \(n\) increases from 5 to 50, the histogram of sample means rapidly approaches the familiar bell curve of a normal distribution.
- The rate of convergence varies:  
  - **Uniform**: converges fairly quickly due to bounded support.  
  - **Exponential**: a bit slower, since the original distribution is skewed.  
  - **Binomial**: resembles normality even for moderate \(n\) because of its discrete symmetric shape when \(p=0.5\).

---

### 5. Discussion

1. **Convergence Behavior**  
   - For small \(n\), histograms retain the skew or shape of the parent distribution.  
   - For larger \(n\), the influence of the population’s higher moments diminishes:  
     $$
       \mathrm{Var}(\bar X) = \frac{\sigma^2}{n}\,,
     $$
     so dispersion around the mean shrinks as \(n\) grows, sharpening the normal peak.

2. **Influence of Variance**  
   - Populations with larger variance (e.g., exponential) yield broader sampling distributions for a given \(n\).  
   - Doubling \(n\) halves \(\mathrm{Var}(\bar X)\), tightening the histogram.

3. **Practical Applications**  
   - **Parameter Estimation**: Confidence intervals rely on approximate normality of \(\bar X\).  
   - **Quality Control**: Sampling batches of products and monitoring the mean.  
   - **Financial Modeling**: Aggregating returns (often non‐normal) still yields near‐normal portfolio returns.

---

### 6. Conclusion

Through animated simulations, the CLT’s power is clear: regardless of the original distribution—uniform, skewed, or discrete—the mean of sufficiently large samples is approximately normal, with variance decreasing as \(1/n\). This underpins many inferential techniques across science, engineering, and finance.