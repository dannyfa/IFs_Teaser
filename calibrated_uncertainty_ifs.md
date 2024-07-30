---
layout: page
title: Uncertainty Estimation
subtitle: Using Inflationary Flows for Calibrated Inference 
mathjax: true
---

### 7.1 Sources of error when doing inference and generation with Inflationary Flows 

As with other implicit models, our inflationary flows provide a deterministic link between complex data and 
simplified distributions with tractable sampling properties. This mapping requires integrating the proposed 
[general pfODE]({% link inflationary_flows_model.md %}) for a given choice of \\( \mathbf{C}(t) \\) and 
\\( \mathbf{A}(t) \\) and an estimate of the score function of the original data. As a result, sampling-based estimates of 
uncertainty are trivial to compute: given a prior \\( \pi(\mathbf{x}) \\)  over the data (e.g., a Gaussian ball centered on 
a particular example \\( \mathbf{x}_0 \\)) , this can be transformed into an uncertainty on the dimension-reduced space by sampling 
\\( \{ \mathbf{x}_j\} \sim \pi(\mathbf{x}) \\) and integrating the  [general pfODE]({% link inflationary_flows_model.md %}) 
forward to generate samples from \\( \int p(\mathbf{x}_T|\mathbf{x}_0)\pi(\mathbf{x}_0)\,d\mathbf{x}_0 \\). 

As with MCMC, these samples can be used to construct either estimates of the posterior or credible intervals. Moreover, 
because the pfODE is unique given \\( \mathbf{C} \\), \\( \mathbf{A} \\), and the score, the model is <em>identifiable</em> 
when conditioned on these choices. The only potential source of error, apart from Monte Carlo error, in the above procedure
arises from the fact that the score function used in [general pfODE]({% link inflationary_flows_model.md %}) is 
only an <em>estimate</em> of the true score. 

Below we explain the set up and discuss the results for coverage experiments performed on sample
toy datasets using 2D alpha-shapes or 3D meshes. These experiments showcase that both sources of error (i.e., ODE integration, score estimation) can be properly controlled, 
allowing us to perform <em> calibrated </em> inference with our proposed model.

### 7.2 Toy Dataset Coverage Experiments 

### 7.2.1 Experiment Set up 
To assess numerical error incurred when integrating our proposed pfODEs, we performed coverage experiments using 3D meshes 
and 2D alpha-shapes ([Akkiraju et al., 1995](http://wcl.cs.rpi.edu/papers/b11.pdf); [Edelsbrunner & Mucke, 1994](https://dl.acm.org/doi/pdf/10.1145/174462.156635)) 
on select toy datasets (i.e., 2D circles and 3D S-curve). If our numerical integration and score estimation were perfect, 
points initially inside these sets should remain inside at the end of 
integration; failure to do so indicates miscalibration of the set's coverage. For these experiments, 
we sampled 20K test points from a Gaussian latent space with appropriate diagonal covariance. For PR-Preserving schedules, 
this is simply a standard multivariate normal with either 2 or 3 dimensions. For PR-Reducing experiments, this diagonal covariance 
matrix contains 1's for dimensions being preserved and a smaller value (\\( 10^{-2} \\) for Circles, \\( 2.5\times 10^{-3} \\) 
for S-curve) for dimensions being compressed. 

Next, we sampled uniformly from the surfaces of balls centered at zero and with linearly spaced Mahalanobis radii ranging 
from 0.5 to 3.5 (200 pts per ball, see schematic below). We then fit either a 2D alpha-shape (2D Circles) or a mesh (3D SCurve) to each one of 
these sets of points. These points thus represent ''boundaries'' that we use to assess coverage prior to and after 
integrating our pfODEs. We define the initial coverage of the boundary to be the set of points (out of 20K test points) 
that lie inside the boundary. We then integrate the pfODE backward in time (the ''generation'' direction) for each sample and 
boundary point. At the end of integration, we again calculate the mesh or 2D alpha-shape and assess the number of samples inside, 
yielding our final coverage numbers. Similarly, we take our samples and boundary points at the end of generation, 
simulate our pfODEs forwards (i.e., the ''inflation'' direction), and once again, use 2D alpha-shapes and meshes to 
assess coverages at the end of this round-trip procedure. 

<figure>
    <figcaption><strong><center><font size="+2.5">Schematic for Toy Coverage Experiments</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/07/Toy_Mesh_Exp_Schematic.jpeg" alt="schematic">
</figure>  



### 7.2.2 Results and Discussion 

As shown below (Toy Coverage Experiments Results, Panels (B), (C)), we are able to preserve coverage up to some small,
controllable amount of error for both schedules and datasets using this process. Lines represent means and shaded regions \\( \pm 2 \\) standard
deviations across three sets of random seeds.


<figure>
    <figcaption><strong><center><font size="+2.5">Toy Coverage Experiments Results</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/07/Toy_Mesh_Exp_Results.jpeg" alt="results">
</figure>  


While it is possible that integrating noisy estimates of the score could produce errant sample mappings, results shown above suggest that such numerical 
errors do not seem to accumulate in cases where they can be compared with exact results. This is because, empirically, score estimates do not appear to be 
strongly auto-correlated in time (see plots of network residual auto-correlations (i.e., \\( R(t_1, t_2) \\)) below and Appendix C.3 of our paper for details), suggesting that \\( \mathbf{\hat{s}}(\mathbf{x}, t) \\), 
the score estimate, is well approximated as a scaled colored noise process and the corresponding pfODE as an SDE. In such a case, 
standard theorems for SDE integration show that while errors due to noise do accumulate, these can be mitigated by a careful choice 
of integrator and ultimately controlled by reducing step size 
[Kloeden & Platen, 1992](https://link.springer.com/book/10.1007/978-3-662-12616-5); [Mout et al., 2019](https://arxiv.org/pdf/1907.11331). 


<figure>
    <figcaption><strong><center><font size="+2.5">Network Residual Auto-Correlations</font></center></strong></figcaption>
    <img src="https://sites.duke.edu/ifsprojectassets/files/2024/07/Network_ACs.jpeg" alt="auto-correlations">
</figure>  



Finally, we also include below animations showcasing PR-Preserving and PR-Reducing ''inflation'' and ''generation'' for a sample 2D dataset (moons) 
and for a set of test points initially sampled to be contained inside two small spheres, centered at a random data points.  Note that integrating our pfODE 
leads to substantial geometry distortion of the original spherical boundaries and of the test points contained within them. However, despite this distortion, 
our proposed pfODE method still allows to adequately preserve original probability coverages up to a small, controllable error. 

### Moons PR-Preserving ''inflation'' and ''generation'' 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/GV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/moons_prp_melt_gen_cluster_distortion.mp4" type="video/mp4">
</video></p>--->

### Moons PR-Reducing ''inflation''

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/HV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/moons_prr_melt_cluster_distortion.mp4" type="video/mp4">
</video></p>--->

### Moons PR-reducing ''generation''

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/G14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/moons_prr_gen_cluster_distortion.mp4" type="video/mp4">
</video></p>--->

#### Go to next section [Experiments on image benchmark datasets]({% link hd_exps.md %})

