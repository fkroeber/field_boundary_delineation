Fokus

magdeburg: mrs_scale: 150, n_objects: 406, n_objects: 429
* advice default of 5%, no pre-setted values as no sensible heuristics

Brainstorming with Dirk
* Compactness-based resizing with MRS params based on test fields
* Objects growth under line constraints
* Snake algorithms for line growth
* prior classification & segmentation (using the intermediate watershed output)

* automated approach via watershed mean as upper bound (under-segmentation happening occuring at this parameter value)
* consider mrs internal homogeneity of spectral values

Sigma
* best setting: only extract bright boundaries
* better suited to get linear field boundaries than canny_averaged
* metrics is flawed, realisation that visual inspection (!) is far mor powerful, only afterwards proper metric for automisation needs to be found
* not better with watershed as not always consistent
* differs in double boundary (thicker ones) 

https://medium.com/sentinel-hub/parcel-boundary-detection-for-cap-2a316a77d2f6

Use official parcel datasets
https://catalog.inspire.geoportail.lu/geonetwork/srv/search?any=lpis+&fast=index


Think about the actual problem
* first problem -> lines are not delineated as they should be (lot of noise)
* clearly visible borders not enhanced in edge composite layer
* metrics bias (inner * outer) 

Future ideas / different software & setting
* lee sigma filter or other adaptive filters (Wagner/Oppelt 2020)
* derivate of pixel values to detect sharp ridges vs. smooth transitions
* add S-1 data

Done/Accomplished
* change accuracy computation wrt denominator to relative one instead of hard-coded 20
* change edge layer to average so that constant overflow height can be used
* save highest value in each iteration and compare it with highest value so far instead of latest one
* implement percentage of overlap as evaluation criterion
* let user select what criterium is used to guide parametrisation 
    * mean overlap of fields
    * median overlap to focus more on amount of fields with proper delineation
* NDVI-based classification

Alternatives (discarded)
* thematic object attribute "maximum overlap" not choosen as measures only relative to reference polygon
* relative to larger object would require more effort (convert to seg result to vector and take min of both)
* implementation on object level via parent process -> needs more time than creating raster layer with zero values for non-buffers

Documentation
* good news for region subsetting: speed up is evident (up to 2x)
* user can choose training fields to suit his needs (focus on small fields, special shapes, certain crops, etc)
* default optimiser mean -> tends to spit out smaller segment, oversegmentation usually preferable
* alternative optimiser median -> suitable if one wants to selectively subset correctly delineated fields
* drawback median optimiser -> single fields are substantially overestimated in size
* tried several filters in addition to canny, also for subsequent analysis
    * line extraction filter not adaptable enough (lines length & border width has to be defined globally)
    * other variance including filters such as sigma filter
        * same problem as blurring filters in general 
        * additionally not designed for statistics occuring here
        * may be suited for exclusively detecting larger fields
    * blurring filters decrease resolution which is problematic
    * second edge filters create double edges


Questions
* Noise removal constrained to objects < 50 pxls or for all objects via MRS?
* How to specify layer weights in MRS?
* How to include linear features in workflow?


To-Do - Pratical
priority list (top 3)

* use mrs segmentation for merging small objects
    * if parameter adjustment necessary: ESP tool
    * choose which bands (balance between similarity based on optical bands and still adhering to boundaries)
* segmentation based on initial watershed objects assures recognition of existing boundaries
* note different strengths of segmentation algorithms & ways possibilities to combine them 
* smoothing of resulting polygons
* accuracy assessment


* classification of fields vs. forest, urban area, water -> user interaction in architect
    * output two layers with extended / restrictive field selection
    * output prob layer -> fuzzy rule set -> user can set threshold 
    * two ways to implement
        * automated rule set (based on NDVI timeseries as literature indicates suitability)
        * ask user to label samples (automated RF based classification)

* let watershed algorithm run & explore results in detail
* identify problems & select next steps accordingly
* think big -> what are the best potential areas
    * probably not the refinement of edge & segmentation -> as they already contribute their part of information
    * more the post-processing (object resizing, class modeling, etc) & combination of information
* auto-naming of imports (s2_l3a_xx_band)

* subset possibility in GUI
* multi-step hierachical way to find best overflow height quickly
* noise removal for smaller parcels & field inclusions
    * after segmentation as not expected to influence results significantly
    * after classification to avoid inclusion of man-made objects
* change output to be smoothed vector layer
* systematic accuracy assessment & extended info (csv with field sizes per field, overlap degree, etc)
* evaluation using VHR Maxar images as basemaps (or DOP), only assure that they are taken from same year 2021

* evaluation of problems -> optimise or second approach (theory-based, no trial & error!!!)
    * trying multiple edge detection layers (also averaging them)
    * trying multiple segmentations
        * stacked workflow with contrast-split in local window
        * local window size determined by first initial segmentation
        * accounting for varying field sizes throughout scene
    * two-step procedure: first contrast-split & conversion to vector, then vector-bound segmentation !!!
    * first filter for boundaries that exist for sure via contrast-split, then refine results via watershed

* for contrast-split (not sure if reasonable at all!!!)
    * understand capability, strength & drawbacks of contrast-split
        * basis: increasing intra-homogeneity & external-heterogeneity
        * should be suited as contrast between borders & fields is given
        * leads to the inlusion of single pixels for edge class -> mitigate by merging/grow region 
        * problem: regions which are created do not have to be spatially contiguous (this is no criterion) 
    * test different contrast mode settings - edge ratio, object diff
    * reduce intra-interval (& step size) & substitute for smaller outer intervals
        * reason a: speed-up
        * reason b: actual working mechanism of algorithm not favourable
        * eventually leads to exact same results as with watershed??? (circumventing real working mechanism of contrast-split) 
    * different criteria: ask user to provide edges (or extract edges from delineated fields & measure overlap)
    * extract vector & omit inaccuracies
    * afterwards vector-based segmentation (convergence of evidence approach)

* idea of convergence of evidence
    * stepwise accumulation of evidence for field delineation
    * selection of algorithms for task which they are specified for & particulary good at
    * balancing flaws/weaknesses of other algorithms

* approaches/methods t.b. used
    * chapter 10
        * pixel-based reshaping for removing small inaccuracies (with surface tension)
        * pixel-based filters to detect holes & concavities
    * chapter 9.5 morphology/mrs growth (similar to second segmentation application)
    * vector-based smoothing/simplification

* git repo
    * ruleset
    * ruleset incl. testdata
    * python script for downloading l3a data (germany)
    * documentation
        * incl. extended accuracy assessment
        * selection of 3 different agricultural areas in germany
        * random points sampling of 75 points per area (so that >50 agricultual points are likely)
        * manual delineation of all fields for given points
        * 5x targeted selection of 10 fields for training out of pool of 20 randomly selected fields
        * remaining 40 fields for validation
        * reporting worst/average/best performance

* gui development

* literature-review
    * field boundary detection
    * field delineation
    * Watkins 2019
    * timesen2crop -> Austria