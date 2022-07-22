# diffFracQuant

Notes:
9th June
- After discussing the model with Guido (1st meeting in Trieste 9th June), we decided to alter the model to better account for the noise in the sup and pel models. Instead of directly inputting the raw counts into the model, we add latent variables (noiseless sup and pel counts) which we use in the sum to equal the total counts. The raw counts are then sampled from a poisson with gamma prior (i.e neg bin) on the noiseless latent variable. We can then learn three normalising factors: Tot, Sup and Pel.
- Is the variance in the pellet higher than the supernatent?
- Once the model is running we can then look into comparing this model to the original model and DESeq2.
- The rest of 9th June was spent setting up the bifx server to run stan again as its been updated and I forgot how to!

10th June
- Model is now running on bifx. However, it is not converging and so cannot reliably sample from it. Need to triple check that I have defined all the parameters correctly. Could the gamma priors on the latent counts be too restrictive? Means need to cover several orders of magnitude. Do we need training data that has multiple replicates in order to have any chance of converging? Do we really need a scale factor for tot? It's colinear and pointless. As long as pel and sup are comparable it doesn't matter about tot.
- Changed gamma priors to lognorm and no change. Need to better examine latent counts in both regimes. 
- How do I set up the model to deal with replicates? Is tot count rep1 related to pel/sup rep1 or should I pair them with rep2/rep3 counts?
- I ran to model only determining one scale factor (to avoid collinearity), but it didn't seem to have an effect.
- I changes the simulated data and model so the I could input multiple replicates

22nd Jul
- The bayesian model of differential fractionation successfully works on simulated datasets and gives fewer false positives and false negatives than DESeq2 and a basic ratio comparison
- I have begun running the model on a experimental data set created by iserman et al (this work is contained in the iserman_dataset branch)
- Further work to be completed: add linear terms to latent counts to predict condition/strain effects
- scale factors are not correlating with total reads for the iserman dataset. Total and sup scale factors group together and pel counts cluster at a significantly higher value. I have changed the negative binomials to log_neg_bin as I think the scale factors are being warped by the selective behaviour (on highly expressed genes) in the pel counts. However, you cannot do a linear addition of pel_latent and sup_latent in log space. If I transform pel_latent and sup_latent to linear space to add them does that mean I have to account for the non_linear transformation in the log likelihood? But there is no inverse for this transformation so I can’t create a jacobian to do this transformation. Currently I leave the tot neg bin distribution (not logged) will this cause an issue? 
