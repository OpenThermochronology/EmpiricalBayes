This Jupyter notebook demonstrates the use of a form of empirical Bayes resampling (also known as hierarchical Bayes) for more robust (U-Th)/He data uncertainty estimation for use in thermal-history inversions. See [Malinverno and Briggs (2004)](https://doi.org/10.1190/1.1778243) for description of the hierarchical Bayesian method. As described and implemented here, it is particularly suited for zircon datasets (high n) that span a broad range of effective uranium (eU). The aim is to expand uncertainty accounting where the hyperparameters (i.e., observed He dates) will have a prior distribution that expresses their initial uncertainty and a posterior distribution that is determined by the data directly. The individual date errors are treated as hyperparameters drawn from a probability distribution and the variance is used to infer the 'empirical' date uncertainty. This weighted 1σ uncertainty is inferred from the scatter of the data as determined by the standard deviation of the data weighted by a Gaussian kernel in eU space (σeU = 100 ppm). The 100 ppm eU kernel is taken to represent the range over which zircon grains with similar eU should have similar ages. This value was chosen as a good balance because smaller values tend to converge on the internal uncertainty and larger values begin to reduce the influence of individual dates during thermal modelling. The empirical uncertainty is estimated by summing the internal and external uncertainties in quadrature.

Helium data are typically overdispersed with respect to analytical uncertainties (approx. 2-5% for apatite and zircon). The age reproducibility of helium age standards such as the Durango (DUR) apatite and Fish Canyon Tuff (FCT) zircon suggest uncertainties are greater, on the order of 6-7% for apatite and 8-10% for zircon. For more information regarding date reproducibility and ICPMS measurements, see Reiners and Nicolescu (2006) (https://www.geo.arizona.edu/~reiners/arhdl/arhdlrep1.pdf), [Guenthner et al. (2016)](https://doi.org/10.1002/2016GC006311) for U–Th/He, and [Gleadow et al. (2015)](https://doi.org/10.1016/j.epsl.2015.05.003) for FCT specifically. 

These are still likely minimum estimates, since those grains are laboratory standards with well-behaved diffusion behavior demonstrated experimentally. Natural, 'wild' samples typically have total uncertainties that exceed 10% due various reasons such as (unaccounted for) isotopic zoning, imperfect grain measurement/grain morphology characterization for alpha-loss correction, or unidentified mineral or fluid inclusions (to name a few). There is also the possibility for trapping of pre- and post-'closure' 4He in various grain sinks and imperfections that further complicate the age reproducibility of slowly cooled apatite; see [Zeitler et al. (2017)](https://doi.org/10.1016/j.gca.2017.03.041), [McDannell et al. (2018)](https://doi.org/10.1016/j.gca.2017.11.031), and [Guo et al. (2021)](https://doi.org/10.1016/j.gca.2021.07.015) for recent experiments and discussion of He age dispersion and diffusion systematics in apatite.

Note: some publications bin single-grain dates by eU and then average the dates in each bin to create 'synthetic' He dates, but this approach lacks rigor and quickly becomes an arbitrary exercise depending on data quality and quantity. This is even more problematic for apatite dates that normally span a narrow eU range. Binning is usually carried out for nondirected Monte Carlo time-temperature modelling, however, it removes temporal information and artificially increases date errors, and as a result, makes it easier to meet p-value statistical thresholds of acceptance for pure Monte Carlo. Thus its only purpose is to allow more paths to be found more easily. This approach is strongly advised against. Since most age dispersion is explainable to first-order by the effects of radiation damage on diffusivity, each grain is a separate thermochronometer (that is the advantage of using radiation-damage models in the first place). Averaging dates results in a loss of valuable t-T information and decreased t-T resolution. Importantly, the empirical Bayes approach retains the observed date calculated from the measured isotopic data but the uncertainties are estimated from the data directly. Error resampling is helpful during t-T modelling when errors are underestimated and prevents the t–T search algorithm from becoming trapped in local minima by reducing overprecision (in the case of learning algorithms such as Markov chain Monte Carlo). A notable point is that the empirical uncertainties only act to assist in improving the search, as the observed age is still the target datum, therefore, it is different from a simple Monte Carlo approach where increasing the uncertainties (or reducing the number of precise data by averaging) actually results in more paths being found and accepted during the random sifting of t-T space.

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/kmcdannell/helium-empirical-bayes.git/main?filepath=%2FEmpirical-Bayes-Resampling.ipynb)
