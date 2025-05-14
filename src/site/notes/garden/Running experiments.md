---
{"dg-publish":true,"permalink":"/garden/running-experiments/"}
---

14.05.25

# Introduction
In machine learning, one of the integral things that a researcher ends up doing is well, training models. What I have realised with my limited time as a researcher is that running experiments isnt just about shoving a lot of data and more complex models on exceedingly bigger and bigger compute and hoping that stuff works, there HAS to be a method to the madness. You can get away with doing the above mentioned thing sometimes, but when you hit a brick wall, it is time to do the actual *research* part of your title. 

## My current dilemna
Something that I am facing right now is the issue with the model performance getting worse the more data we add. New data is added to the dataset every week. The best model that we had until this day had a DICE score of 0.63. now with more data, it is down to 0.55, which when on paper does not feel like a lot, the overall segmentations that are produced are noticeably subpar. Now this sounds pretty weird, all the online lectures and lectures have taught you that more data means better models, and if your teachers were better, more cleaner data means better models. But this is not the case with me right now. 
So here are some of the reasons that I am thinking of that might be the reason:
- the data is form different sources, with different machines that record data at different resolution. Multi-clinical data like this is kind of like a double edged sword, on one hand, the models that are trained have a better likelihood of being agnostic to the source of the data, as there is a chance that the model leans underlying features really well. On the other hand, the more varied your data sources are, the more noise you are introducing into the data, which may make it harder for a *simple* model to generalise.  
- There are particular samples in the recent data that are not as good, or are inherently different from the kind of data that we had earlier. The data that was present earlier might have been easier to generalise on, hence the model just learned better, but now the new data might be too different. 
- The validation/ test set is biased towards the older training set, essentially more or less the same as the previous point

## The solution
Now how should I go about combating this? The goal is to figure out what is causing this drop in the first place, where/ when exactly did this issue start happening. So for this, I have to start running experiments using 