THIS REPO HAS BEEN SUPERSEDED BY THE [mixrsvp](https://github.com/alexholcombe/mixRSVP) R package I programmed. However this repo is preserved because it contains the start of analyzing the backwards paper 2 experiments.

Patrick Goodbourn programmed mixture modeling RSVP serial position errors in MATLAB. [Certifiedwaif](https://github.com/certifiedwaif/) did initial [port](https://github.com/certifiedwaif/AttentionalBlink) it to R, and then Alex functionfied and improved everything.

### To-do

- For BackwardsLtrsPaper2, work out how to get the directories right when it calls 'analyzeOneCondition.R' and tries to find the mixtureModeling directory from there

            % Test for a significant difference in log likelihoods
            [h,pValue,stat,cValue] = lratiotest(-minNegLogLikelihood,-uniformNegLogLikelihood,nFreeParameters,pCrit);
        
- Instead of using parameterBounds.R, should probably create a list of everything specific to a particular experiment/implementation, a bit like optim has a list of parameters.
- Look at variation in fit across fits so know how many different starting points I need. That's the effectOfNumberOfFits.Rmd file, but I still need to do the empirical SE calculation.
- Why is the blue continuous Gaussian in AA right canonical MATLAB not intersect the light blue discretised Gaussian in the bin middles.

### Data to analyze

- Callum

- Jackie


        
## Issues

Learn how to catch errors that seem uncatchable, like
*  Still having trouble capturing error msgs like "Error in eigen(nhatend) : infinite or missing values in 'x'". which optimx's author [says might be fine](
http://r.789695.n4.nabble.com/Error-in-eigen-nhatend-td4708274.html)

* Error in grad.default(ufn, ans$par, ...) :

For similar error, JC Nash [says](
http://r.789695.n4.nabble.com/Re-optim-bbmle-function-returns-NA-at-td4673616.html) to  find a way to make sure 
your likelihood is properly defined. This seems to be the issue for 
about 90% of failures with optim(x) or other ML methods in my recent 
experience. Note that returning a large value (and make it a good deal 
smaller than the .Machine$double.xmax, say that number *1e-6 to avoid 
computation troubles) often works, but it is a quick and dirty fix. 

* This prevents compatibility with separate local repo setting path into mixture modeling:  Need to adjust path because Testthat might not work because path gets set to mixtureModeling/tests/
Best thing to do is figure out how to avoid testthat needing that. I guess could call testthat from elsewhere?
But that won't be sufficient to solve the problem, because can't source things still without having the path to mixture modeling which local file can't know about.

Someday switch to Bayesian  Stan via brms. See "mixture" function in [brms manual](https://cran.r-project.org/web/packages/brms/brms.pdf) and [this post](http://andrewgelman.com/2017/08/21/mixture-models-stan-can-use-log_mix/) by Gelman on mixture models in stan

## Analysis to-do

* Work out something for excluding participants, see if different number excluded in discarded backwards-ltrs subjects than happened in MATLAB


## Questions for Pat

Why did he pad with zeros the pseudo_uniform distribution?

### Known discrepancies with Pat's MATLAB code

* the area of bin thing
* I use actual targetSPs to calculate guessing component, he used theoretical?


## Implementation choices that could be revised

We can now accomplish truncation of the Gaussian by summing the area of all the bins and dividing that into each bin, to normalize it so that the total of all the bins =1.
An alternative, arguably better way to do it would be to assume that anything in the Gaussian tails beyond the  bins on either end ends up in those end bins.

Also ideally wouldn't have to taper the Gaussian component, instead would send the target position accompanying each SPE into the likelihood calculation as well, so that instead of generic tapering, could calculate the precise predicted probability because would know the domain of possible errors for that particular observation. That would also make it easier to pile up the tails at the extrema rather than using a truncated Gaussian.