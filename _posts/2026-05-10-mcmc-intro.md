## An introduction to Markov Chain Monte Carlo (MCMC)
<!-- 

![alt text]({{ site.baseurl }}/assets/lifting-explainer.png "Visual explainer for lifted Monte Carlo"){: width="500px"}


This post is a work in progress! The goal is to describe the intuition behind the sampling method we propose in the ArXiv preprint [Variational Lifting: Convergent, Exact Sampling with Latent Generative Models](fill_in_this_link_TODO.com). The method is called variational lifting, and it is a way to use latent variable models to perform exact sampling from complex distributions. -->

---

### MCMC Basics (skip if you know this!)

Suppose we want to sample from a (Boltzmann) probability distribution of the form $\pi(x) = \frac{1}{Z} e^{-\beta U(x)} $, where $ U(x) $ is a potential energy function and $ Z $ is the normalizing constant. $\beta$ is an inverse temperature, but it will be kept constant for the purposes of this article, so don't worry about it. This is a common problem in statistical physics, machine learning, and many other fields. However, sampling from this distribution can be challenging, especially when $U(x)$ is complex and high-dimensional.

Let's start with a simple example. Suppose we have two states in one dimension separated by an energy barrier. Below, we'll picture both the energy and the probability density induced by the Boltzmann distribution.

![alt text]({{ site.baseurl }}/assets/unseparated_distribution.png "Double Well potential"){: width="500px"}

In most use cases, we don't have access to the normalizing constant $Z$, but we do have access to $U$. You can think about this as being able to evaluate an energy, but only evaluating *relative* probabilities: say we're comparing state $x$ and state $x'$, then 

$\frac{\pi(x')}{\pi(x)} = \frac{ Z^{-1} \exp (- \beta U(x'))}{Z^{-1} \exp (- \beta U(x))} = \exp (- \beta U(x') + \beta U(x))$

The ability to compare the relative probabilities of two states allows us to use a class of sampling algorithms called Markov Chain Monte Carlo (MCMC). The basic idea is to set up a set of random 'moves' that will always go down in energy when possible, but occasionally go up in energy. This leads to having more samples in lower energy states, but still a few in higher energy states. If you collect enough samples from this process, you end up with a set of $x$ values whose coordinates are distributed according to $\pi(x)$; which we write using the notation $x \sim \pi(\cdot)$. 

To actually implement this algorithm, you need a *proposal* distribution which we call $q$. This just gives you a way to propose the next state $x'$, given that you are currently at state $x$, and we have to also know its probability density $q(x' \mid x)$.

The exact algorithm balances this property of going to lower energy states more often with the bias induced by your proposal distribution: if you're really likely to go from state $x$ to $x'$ under $q$, you'll want to occasionally reject a lower energy transition to correct for the bias in your proposal. The exact algorithm implements is as follows:


<pre id="my-algorithm" class="pseudocode">
\begin{algorithm}
\caption{Basic MCMC}
\begin{algorithmic}
\Require Initial coordinates $x_0$, energy function $U(x)$, inverse temperature $\beta$, easy-to-sample proposal distribution $q(x' \mid x)$, number of simulation steps $T$, burn-in time $b$ \\
\FOR{$t \in \{1, \dots, T\}$}
\STATE Sample proposal $x' \sim q(\cdot \mid x_t)$
\STATE Compute $\alpha = \frac{\pi(x')}{\pi(x)} \frac{q(x \mid x')}{q(x' \mid x)} = \exp(-\beta U(x') + \beta U(x)) \frac{q(x \mid x')}{q(x' \mid x)}$
\STATE Sample uniform random $u \in [0,1]$
\IF{$u < \alpha$}
    \STATE $ x_{t+1} \gets x' $ 
\ELSE
    \STATE $ x_{t+1} \gets x_t $ 
\ENDIF
\ENDFOR
\RETURN $ \{ x_t \}_{t=b}^T $
\end{algorithmic}
\end{algorithm}
</pre>

<script>
  document.addEventListener('DOMContentLoaded', function() {
    var elem = document.getElementById("my-algorithm");
    if (elem && typeof pseudocode !== 'undefined') {
      pseudocode.renderElement(elem);
    }
  });
</script>


We can implement this on our example distribution with modes at $x=-2$ and $x=2$ and using a gaussian proposal distribution $q(x' \mid x) = \mathcal{ \cdot ; x, \sigma}$ where $\sigma$ sets the scale of the perturbation. Here is an example trajectory, showing the empirical probability distribution we collect on the bottom.

![GIF of MCMC in a double well potential]({{ site.baseurl }}/assets/local_mcmc.gif)

Using these samples, let's calculate the true mean value of $x$ under the distribution $\pi$. This can be a high-dimensional integral in general, so it's useful to use the Monte Carlo samples as an estimator: $\mu = \int x \cdot \pi(x) dx \approx \frac{1}{N} \sum_{i=1}^N x_i ; x_i \sim \pi(\cdot)$ where our samples come from the MCMC chain. The longer we run the chain, the closer we get to the ground truth value:

![alt text]({{ site.baseurl }}/assets/estimated_mean_over_time.png "MCMC estimate of the mean of the distribution"){: width="500px"}


Now suppose we separate these modes even further, placing them at $x=-4$ and $x=4$. Let's see what happens when we try to run the same code:

![GIF of MCMC in a double well potential with larger mode separation]({{ site.baseurl }}/assets/local_mcmc_sep.gif)

Annoyingly, we start off in the right mode and never seem to escape it! This gives us a good local estimate of the mode density, but we don't see anything about the second mode. Let's try again to see the mean estimate over time:
![alt text]({{ site.baseurl }}/assets/estimated_mean_over_time_sep.png "MCMC estimate of the mean of the distribution, with larger separation"){: width="500px"}
