<h1 style="text-align: center;">
Super-Resolution of BVOC Emission Maps Via Domain Adaptation
</h1>

<p align="center" width="100%"><img width="60%" src="./system.png"></p>

Official Repository of
 **Super-Resolution of BVOC Emission Maps Via Domain Adaptation** paper, accepted at [IGARSS 2023](https://2023.ieeeigarss.org/) and available at [IEEExplore]().
 
## Abstract
Enhancing the resolution of Biogenic Volatile Organic Compound (BVOC) emission maps is a critical task in remote sensing. 
Recently, some Super-Resolution (SR) methods based on Deep Learning (DL) have been proposed, leveraging data from numerical simulations for their training process. 
However, when dealing with data derived from satellite observations, the reconstruction is particularly challenging 
due to the scarcity of measurements to train SR algorithms with. 
In our work, we aim at super-resolving low resolution emission maps derived from satellite observations 
by leveraging the information of emission maps obtained through numerical simulations. 
To do this, we combine a SR method based on DL with Domain Adaptation (DA) techniques, 
harmonizing the different aggregation strategies and spatial information used in simulated and observed domains to ensure compatibility. 
We investigate the effectiveness of DA strategies at different stages by systematically varying the number of simulated and observed emissions used, 
exploring the implications of data scarcity on the adaptation strategies. To the best of our knowledge, 
there are no prior investigations of DA in satellite-derived BVOC maps enhancement. 
Our work represents a first step toward the development of robust strategies for the reconstruction of observed BVOC emissions.

For more details, please check: "[Super-Resolution of BVOC Emission Maps Via Domain Adaptation](IEEE Link)"

## Supplementary material
### Domain Aggregation Comparison
Comparison between isoprene emission maps corresponding to the same geographical area but to different domains: 
$\mathrm{\mathbf{S}}$(imulated) and $\mathrm{\mathbf{O}}$(observed). 
Emission values and patterns differ according to the data aggregation strategies adopted in each domain.

<p align="center" width="100%"><img width="60%" src="./experiments/dataset_difference.png"></p>

### Perfect Knowledge Scenario
###### GOME-2 vs OMI Networks Generalization
We also performs a cross inference test between the two networks, fully trained on the two different datasets from $\mathrm{\mathbf{O}}$.
We found that training on $\mathcal{D}\_{\mathrm{\mathbf{O}}\_1}$ and testing on $\mathcal{D}\_{\mathrm{\mathbf{O}}\_2}$ provides comparable result
with the classical case, where we train and test on the same dataset.
This holds also for the $\mathcal{D}\_{\mathrm{\mathbf{O}}\_2}$ dataset.
This happens because the two datasets are very similar, and the network is able to generalize well on both of them.

[//]: # (###### Generalization on Simulated Data)

### Zero Knowledge Scenario
###### Resolution Effect
Visual difference of two emission maps, from different neural networks, for both the $\mathcal{D}\_{\mathrm{\mathbf{O}}\_1}$ and $\mathcal{D}\_{\mathrm{\mathbf{O}}\_1}$ datasets. 
The first and second columns show the low ($\mathrm{\mathbf{I}\_{LR}^{o}}$) and high resolution ($\mathrm{\mathbf{I}\_{HR}^{o}}$) version of the a map from the $\mathrm{\mathbf{O}}$ domain.
The last three columns show the super-resolved maps ($\mathrm{\mathbf{\hat{I}}\_{HR}^{o}}$), obtained by using 3 different network:
- $\mathcal{N}\_{\mathrm{\mathbf{O}}}$ trained on $\mathcal{D}\_{\mathrm{\mathbf{O}}\_1}$ or $\mathcal{D}\_{\mathrm{\mathbf{O}}\_2}$, based on the dataset used for the super-resolution;
- $\mathcal{N}\_{\mathrm{\mathbf{S}}\_T}@0.25^{\circ}$ trained on $\mathcal{D}\_{\mathrm{\mathbf{S}}\_T}$ and designed to make a resolution change from $0.50^{\circ}$ to $0.25^{\circ}$ ;
- $\mathcal{N}\_{\mathrm{\mathbf{S}}\_T}@0.50^{\circ}$ trained on $\mathcal{D}\_{\mathrm{\mathbf{S}}\_T}$ and designed to make a resolution change from $1.00^{\circ}$ to $0.50^{\circ}$.

Notice that we use the network in a zero-knowledge scenario, thus we do not use any adaptation strategy.

We can observe a substantial difference in the spatial pattern reconstruction between the two networks, trained with data of different spatial resolution.
The $\mathcal{N}\_{\mathrm{\mathbf{S}}\_T}@0.25^{circ}$ provides a more blurred version of the map, but it is able to retrieve the emission values more accurately, as we show in the Sec. 4 of the paper.

[//]: # (Fine-tuning of both the networks should be performed in order to provide more accurate results, since the network trained on the simulated data is not able to capture the satellite-derived emission values.)

<p align="center" width="100%"><img width="60%" src="./experiments/resolution_comparison_1.png"></p>
<p align="center" width="100%"><img width="60%" src="./experiments/resolution_comparison_2.png"></p>

[//]: # (###### Spectral Differences)
[//]: # (<p align="center" width="100%"><img width=55%" src="./experiments/resolution_comparison_frequency.png"></p>)
[//]: # (### Data Tranformation Adaptation)

### Network Operator Adaptation
###### Injection Comparison
Visual difference of the reconstructed $\mathrm{\mathbf{O}}$ emission, obtained using different fine-tuned version of $\mathcal{N}\_{\mathrm{\mathbf{S}}\_T}$ network.
The percentages denote the amount of data from $\mathcal{D}\_{\mathrm{\mathbf{O}}\_1}$ or $\mathcal{D}\_{\mathrm{\mathbf{O}}\_2}$ injected in the learning procedure (train and validation).
We found that most of the time we obtain acceptable results by injecting just a small amount of data from $\mathcal{D}\_{\mathrm{\mathbf{O}}\_1}$ or $\mathcal{D}\_{\mathrm{\mathbf{O}}\_2}$ 
in the learning procedure, as we can notice from the emission patches.  

<p align="center" width="100%"><img width="90%" src="./experiments/injection_comparison_good.png"></p>

For a few particular emission patches, we found that the network is not able to learn the correct mapping between the two domains, leading to unreliable results .
We believe that this behavior is due to the original small dynamic of the $\mathrm{\mathbf{O}}$ patch to be enhanced.
<p align="center" width="100%"><img width="90%" src="./experiments/injection_comparison_bad.png"></p>

###### Performance Comparison
Average NMSE achieved by the Network Operator adaptation strategy, in which we train the neural network by varying the amount of injected patches from the $\mathrm{\mathbf{O}}$ domain.
The solid line represents the trend when we use mixed patches from both domains, i.e., $\mathrm{\mathbf{O}}$ and $\mathrm{\mathbf{S}}$, in the validation phase.
The dashed line represents the trend when we use only patches from the $\mathrm{\mathbf{O}}$ domain in the validation phase.

<p align="center" width="100%"><img width="55%" src="./experiments/exp_2_val_mix.png"></p>

For the sake of clarity, in the Table below we also report the exact values.
The $\mathrm{\mathbf{O}}$ + $\mathrm{\mathbf{S}}$ injection represent the one exposed in the paper, where we use mixed patches from both domains in the validation phase (solid lines of the Figure above).
The $\mathrm{\mathbf{O}}$ injection represent the case where we use only patches from the $\mathrm{\mathbf{O}}$ domain in the validation phase (dashed lines of the Figure above).
<p align="center" width="100%"><img width="55%" src="./experiments/exp_2_val_tab.png"></p>

We can observe a more stable trend when we validate on mixed patches from both domains, 
but validating only with patches from the $\mathrm{\mathbf{O}}$ domain seems to provide better results with less injected patches. 
This also reduces the amount time needed to perform the full training and reach convergence.


<p align="center" width="100%"><img width="25%" src="./experiments/cross_test.png"></p.



## Citation
```BibTex
@article{giganti2023bvoc-da,
      title={Super-Resolution of BVOC Emission Maps Via Domain Adaptation}, 
      author={Antonio Giganti, Sara Mandelli, Paolo Bestagini, Marco Marcon, Stefano Tubaro},
      year={2023},
      DA COMPLETARE
}
```

## Data
The satellite-derived and simulated data used in this work are available from the following links:

**Top-down** invetories:
- $\mathcal{D}\_{\mathrm{\mathbf{O}}\_1}$ - [GOME-2](https://emissions.aeronomie.be/index.php/gome2-based)
- $\mathcal{D}\_{\mathrm{\mathbf{O}}\_2}$ - [OMI](https://emissions.aeronomie.be/index.php/omi-based)

distributed by the Belgian Institute for Space Aeronomy (BIRA-IASB)

<img src="./logos/bira_logo.png" width="30px" alt="logo"></img>
<img src="./logos/EUMETSAT_logo.png" width="40px" alt="logo"></img>
<img src="./logos/nasa_logo.png" width="40px" alt="logo"></img>
<img src="./logos/aura_logo.png" width="50px" alt="logo"></img>

**Bottom-up** inventories:
- $\mathcal{D}\_{\mathrm{\mathbf{S}}}$ - [CAMS-GLOB-BIO](https://permalink.aeris-data.fr/CAMS-GLOB-BIO)

distributed by Emissions of atmospheric Compounds and Compilation of Ancillary Data (ECCAD), Global Emissions InitiAtive (GEIA)

<img src="./logos/ECCAD_logo.png" width="30px" alt="logo"></img>
<img src="./logos/GEIA_logo.png" width="80px" alt="logo"></img>
<img src="./logos/cams_logo.png" width="70px" alt="logo"></img>
<img src="./logos/copernicus_logo.png" width="80px" alt="logo"></img>


## Related Works on BVOC
A. Giganti, S. Mandelli, P. Bestagini, et al., “Super-resolution of
bvoc maps by adapting deep learning methods,” arXiv preprint, 2023

[![arXiv](https://img.shields.io/badge/arXiv-2302.07570v2-b31b1b.svg)](https://arxiv.org/abs/2302.07570v2)

A. Giganti, S. Mandelli, P. Bestagini, et al., “Multi-BVOC Super-Resolution Exploiting Compounds Inter-Connection,” arXiv preprint, 2023

[![arXiv](https://img.shields.io/badge/arXiv-2305.14180v1-b31b1b.svg)](https://arxiv.org/abs/2305.14180v1)

### Acknowledgement
This work was supported by the Italian Ministry of University and
Research (MUR) and the European Union (EU) under the PON/REACT project.

<img src="./logos/ispl_logo.png" width="110px" alt="logo"></img>
<img src="./logos/polimi_logo.png" width="230px" alt="logo"></img>
<img src="./logos/pon_logo.png" width="160px" alt="logo"></img>
<img src="./logos/mur_logo.png" width="80px" alt="logo"></img>
<img src="./logos/ue_logo.png" width="110px" alt="logo"></img>




