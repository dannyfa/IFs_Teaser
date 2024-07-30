---
layout: page
title: Experiments on Image datasets
subtitle: Using Inflationary Flows in higher-dimensional data
mathjax: true
---

### 8.1 Experiments on Image Benchmark datasets 

To verify that inflationary flows successfully compress high-dimensional data, we compressed two 
benchmark datasets (CIFAR-10 [Krizhevsky et al., 2009](https://www.cs.toronto.edu/~kriz/learning-features-2009-TR.pdf) 
AFHQv2 [Choi et al., 2020](https://arxiv.org/pdf/1912.01865)) 
to 20%, 10%, and 2% of their nominal dimensionality and examined their round-trip and pure generative 
performance. For estimating score, we used DBM networks trained using the scheme of 
[Karras et al., 2022](https://arxiv.org/pdf/2206.00364) combined with the novel scaling, noise, and preconditioning proposed above 
(see Appendices B.1, B.4.2 of our paper for details). 

In the table below, we show computed Frechet Inception Distance (FID) 
scores [Heusel et al., 2017](https://proceedings.neurips.cc/paper_files/paper/2017/file/8a1d694707eb0fefe65871369074926d-Paper.pdf) 
over three different sets of 50,000 randomly generated images for each of 
these schedules and for both datasets. Note that preserving 
intrinsic dimension (PRP schedules) leads to better (smaller) FID scores, as expected. Perhaps surprisingly, 
increasing the number of preserved dimensions in the PR-Reducing regimes leads to <em>worse</em> (i.e., higher) 
FID scores. This is because retaining more dimensions in our PR-Reducing schedules leads to larger scale gaps 
between our preserved and compressed dimensions (i.e., larger \\( e^{\rho(g_* - g_i)T} \\)), thus increasing the 
required noise range over which networks must optimally estimate scores. That is, PR-Reducing schedules 
with higher numbers of preserved-dimensions pose a more challenging learning problem 
(Appendix B.2 of our paper). 

In the same table, we also include mean squared errors (MSEs) for round-trip integration experiments
(i.e., from data to compressed space and back) across all schedules and datasets, computed over 3 randomly 
sampled sets, each with 10K images. Trends observed here are similar to the ones seen for FID experiments. 
PR-Preserving networks produced lower MSEs overall, whereas PR-Reducing networks with higher percentage of 
preserved dimensions (i.e., 20%) yielded higher MSEs than PR-Reducing networks with smaller number of 
preserved dimensions (i.e., 2%). Together, these results suggest that dimension reduction with 
inflationary flows may necessitate trade-offs between effective compression and the difficulty of score estimation, 
as noted above. 

<caption><strong><center> FID and Round-Trip MSE Experiment Results for Image Datasets </center></strong></caption>
<table style="border:1px solid black; margin-left:auto;margin-right:auto;table-layout:auto">
  <tr>
    <td style='width:15%'><b><center>Metric</center></b></td>
    <td style='width:14%'><b><center>Dataset</center></b></td>
    <td style='width:17%'><b><center>PRP</center></b></td>
    <td style='width:18%'><b><center>PRR to 2%</center></b></td>
    <td style='width:18%'><b><center>PRR to 10%</center></b></td>
    <td style='width:18%'><b><center>PRR to 20%</center></b></td>
  </tr>
  <tr>
    <td>FID</td>
    <td>CIFAR-10</td>
     <td> 17.01 <span>&#177;</span>  0.10 </td>
     <td> 22.23 <span>&#177;</span> 0.16 </td>
     <td> 23.63 <span>&#177;</span>0.13 </td>
     <td> 25.93 <span>&#177;</span> 0.40 </td>
  </tr>
  <tr>
    <td>Round-Trip MSE</td>
    <td>CIFAR-10</td>
    <td> 0.23 <span>&#177;</span> 0.03 </td>
    <td> 2.06 <span>&#177;</span> 0.04 </td>
    <td> 2.25 <span>&#177;</span> 0.01 </td>
    <td> 4.16 <span>&#177;</span> 0.39 </td>
  </tr>
  <tr>
    <td>FID</td>
    <td>AFHQv2</td>
    <td> 11.89 <span>&#177;</span> 0.08 </td>
    <td> 13.07 <span>&#177;</span> 0.07 </td>
    <td> 13.67 <span>&#177;</span> 0.09 </td>
    <td> 16.77 <span>&#177;</span> 0.14 </td>
  </tr>
  <tr>
    <td>Round-Trip MSE</td>
    <td>AFHQv2</td>
    <td> 0.38 <span>&#177;</span> 0.04 </td>
    <td> 5.57 <span>&#177;</span> 0.20 </td>
    <td> 7.95 <span>&#177;</span> 0.31 </td>
    <td> 8.17 <span>&#177;</span> 0.08 </td>
  </tr>
</table>

Finally, we include below animations showcasing roundtrip-integration and generation experiments for
some sample small batches of the AFHQv2 dataset ([Choi et al., 2020](https://arxiv.org/pdf/1912.01865)). 
For the round-trip experiments, reconstructions are qualitatively good across 
all conditions. Of note, networks trained with proposed dimension-reducing schedules produce inflated representations that resemble true low-rank Gaussian 
samples, consistent with a reduced-dimensional latent space. 

### 8.2 Animations 

### 8.2.1 PR-Preserving

#### Round-trip 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/NV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRP_roundtrip.mp4" type="video/mp4">
</video></p>---> 

#### Generation 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/M14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRP_gen.mp4" type="video/mp4">
</video></p>--->

### 8.2.2 PR-Reducing to 2% 

#### Round-trip 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/OV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto62D_roundtrip.mp4" type="video/mp4">
</video></p>--->  

#### Generation 


<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/N14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto62_gen.mp4" type="video/mp4">
</video></p>---> 

### 8.2.3 PR-Reducing to 10% 

#### Round-trip 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/PV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto307D_roundtrip.mp4" type="video/mp4">
</video></p>--->

#### Generation 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/O14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto307D_gen.mp4" type="video/mp4">
</video></p>---> 

### 8.2.4 PR-Reducing to 20% 

#### Round-trip 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/QV4IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto615D_roundtrip.mp4" type="video/mp4">
</video></p>---> 

#### Generation 

<center><iframe height="360" width="640" src="https://warpwire.duke.edu/w/P14IAA/" frameborder="0" scrolling="0" allow="autoplay *; encrypted-media *; fullscreen *; picture-in-picture *;" allowfullscreen></iframe></center>

<!---<p><video muted autoplay controls loop="loop" width="768" height="512" >
  <source src="/assets/videos/PRRto615D_gen.mp4" type="video/mp4">
</video></p>--->  




