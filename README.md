# diffFracQuant

Notes:
9th June
- After discussing the model with Guido (1st meeting in Trieste 9th June), we decided to alter the model to better account for the noise in the sup and pel models. Instead of directly inputting the raw counts into the model, we add latent variables (noiseless sup and pel counts) which we use in the sum to equal the total counts. The raw counts are then sampled from a poisson with gamma prior (i.e neg bin) on the noiseless latent variable. We can then learn three normalising factors: Tot, Sup and Pel.
- Once the model is running we can then look into comparing this model to the original model and DESeq2.
- The rest of 9th June was spent setting up the bifx server to run stan again as its been updated and I forgot how to!

10th June
- Model is now running on bifx. However, it is not converging and so cannot reliably sample from it. Need to triple check that I have defined all the parameters correctly. Could the gamma priors on the latent counts be too restrictive? Means need to cover several orders of magnitude. Do we need training data that has multiple replicates in order to have any chance of converging? Do we really need a scale factor for tot? It's colinear and pointless. As long as pel and sup are comparable it doesn't matter about tot.
- Changed gamma priors to lognorm and no change. Need to better examine latent counts in both regimes. 
