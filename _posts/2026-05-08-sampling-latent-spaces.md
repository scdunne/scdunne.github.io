## Sampling in Latent Variable Models: Variational Lifting for Convergent, Exact Sampling


![alt text]({{ site.baseurl }}/assets/lifting-explainer.png "Visual explainer for lifted Monte Carlo"){: width="500px"}


This post is a work in progress! The goal is to describe the intuition behind the sampling method we propose in the ArXiv preprint [Variational Lifting: Convergent, Exact Sampling with Latent Generative Models](fill_in_this_link_TODO.com). The method is called variational lifting, and it is a way to use latent variable models to perform exact sampling from complex distributions.

---

### The Setup

Suppose we want to sample from a Boltzmann distribution of the form \(\pi(x) = \frac{1}{Z} e^{-U(x)}\), where \( U(x) \) is a potential energy function and \( Z \) is the normalizing constant. This is a common problem in statistical physics, machine learning, and many other fields. However, sampling from this distribution can be challenging, especially when \(U(x)\) is complex and high-dimensional.


Testing: $ U(x) $ ; $$ U(x) $$ ; \[ U(x) \] ; \( U(x) \)

