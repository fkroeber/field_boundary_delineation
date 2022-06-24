Experimental ideas

----------------------------------------------------------------------------------
General guidelines
* Ockhams razor -> keep it simple (-> transferability)
* Theory-guided approach based on empirically encountered issues
    * define the actual problem -> find targeted solutions
* Power of visual inspection, metrics for operationalisation only afterwards

----------------------------------------------------------------------------------

Category: Linear Filter
* problem
    * false positives, lot of noise
    * clearly visible borders with gaps -> leakages
* different input data, median- or voting-based canny edge layer
    * garbage-in-garbage-out -> initial reduction of noise
    * keep canny edge layer as other did not prove to be valuable
* shape-split to deal with under-segmentation issues
* morphology/ pixel-based shaping with surface tension

Category: Accuracy Measures
* IoU may be implemented via additional hierachical level
* refined accuracy measures specific to boundary overlap, metrics bias (inner * outer)

Catgory: Classification
* improve feature selection by selecting all object features & print out most important ones

Category: Additional data sources
* S-1 data -> requires automated script to download preprocessed data
* get it from google earth engine