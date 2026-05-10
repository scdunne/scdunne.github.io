## An introduction to Markov Chain Monte Carlo (MCMC)
<!-- 

![alt text]({{ site.baseurl }}/assets/lifting-explainer.png "Visual explainer for lifted Monte Carlo"){: width="500px"}


This post is a work in progress! The goal is to describe the intuition behind the sampling method we propose in the ArXiv preprint [Variational Lifting: Convergent, Exact Sampling with Latent Generative Models](fill_in_this_link_TODO.com). The method is called variational lifting, and it is a way to use latent variable models to perform exact sampling from complex distributions. -->

---

### MCMC Basics (skip if you know this!)

Suppose we want to sample from a Boltzmann distribution of the form $\pi(x) = \frac{1}{Z} e^{-U(x)} $, where $ U(x) $ is a potential energy function and $ Z $ is the normalizing constant. This is a common problem in statistical physics, machine learning, and many other fields. However, sampling from this distribution can be challenging, especially when $U(x)$ is complex and high-dimensional.

Let's start with a simple example. Suppose we have two states in one dimension separated by an energy barrier. Below, we'll picture both the energy and the probability density induced by the Boltzmann distribution.

#TODO: make the visualizations

In most use cases, we don't have access to the normalizing constant $Z$, but we do have access to $U$. You can think about this as being able to evaluate an energy, but only evaluating *relative* probabilities: say we're comparing state $x$ and state $x'$, then 

$\frac{\pi(x')}{\pi(x)} = \frac{ Z^{-1} \exp (- \beta U(x'))}{Z^{-1} \exp (- \beta U(x))} = \exp (- \beta U(x') + \beta U(x))$

The ability to compare the relative probabilities of two states allows us to use a class of sampling algorithms called Markov Chain Monte Carlo (MCMC). The basic idea is to set up a set of random 'moves' that will always go down in energy when possible, but occasionally go up in energy. This leads to having more samples in lower energy states, but still a few in higher energy states. If you collect enough samples from this process, you end up with a set of $x$ values whose coordinates are distributed according to $\pi(x)$; which we write using the notation $x \sim \pi(\cdot)$. 

To actually implement this algorithm, you need a *proposal* distribution which we call $q$. This just gives you a way to propose the next state $x'$, given that you are currently at state $x$, and we have to also know its probability density $q(x' \mid x)$.


The exact algorithm balances this property of going to lower energy states more often with the bias induced by your proposal distribution: if you're really likely to go from state $x=1.0$ to $x' = 1.2$ under $q$, you might want to reject a lower energy transition to correct for the bias in your proposal. The exact algorithm implements this as follows:


{% raw %}
<pre id="my-algorithm">
\begin{algorithm}
\caption{My Algorithm}
\begin{algorithmic}
\Require Initial coordinates $x_0$, energy function $E(x)$, inverse temperature $\beta$, easy-to-sample proposal distribution $q(x' \mid x)$, number of simulation steps $T$, burn-in time $b$ \\
\For{$t \in \{1, \dots, T\}$}
\State Sample proposal $x' \sim q(x' \mid x_t)$
\State Compute acceptance ratio $\alpha$ from energy and proposal distributions
\State Sample uniform random $u \in [0,1]$
\If{$u < \alpha$}
    \State $x_{t+1} \gets x'$
\Else{}
    \State $x_{t+1} \gets x_t$
\EndIf
\Return $ \{ x_t \}_{t=b}^T$
\EndFor
\end{algorithmic}
\end{algorithm}
</pre>
{% endraw %}

<script>
  document.addEventListener('DOMContentLoaded', function() {
    console.log("DOMContentLoaded fired");
    console.log("pseudocode library loaded:", typeof pseudocode !== 'undefined');
    const elem = document.getElementById("my-algorithm");
    console.log("Algorithm element found:", elem);
    if (elem && typeof pseudocode !== 'undefined') {
      try {
        pseudocode.renderElement(elem);
        console.log("Algorithm rendered successfully");
      } catch(e) {
        console.error("Error rendering algorithm:", e);
      }
    }
  });
</script>

debugging... can you see this?