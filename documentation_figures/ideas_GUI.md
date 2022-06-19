part I - data input
* ask to load test data or own area of interest
* if own data
    * ask if python script was used or not
        * yes: use xml-template for taking layer names automatically from filename
        * no: ask user to rename bands manually
    * ask for training file
        * yes: import
        * no: enable to take training samples
            * show two rgb composites from different dates to enable delineation of fields
            * take rgb composite from 25% and 75% of vegetation period

part II - initial delineation
* button for start preperation & prodcution of edge layers
* button for start delineation process

part III - refinement/topology cleaning
* show slider bar for scale parameter to enable making adjustments to region growth
* high default value for slider bar scale parameter -> merge everything
* lowering the scale parameter, e.g. to enable exclusion of artificial objects (windmills, single trees, etc) 

part IV - field extraction
* ask to label at least 30 samples covering the whole diversity of fields & non-agricultural parcels
* explicitly ask user to label boundary objects as non-agricultural parcels
* enable to refine selection of objects after first classification
* option to delete all samples/select new set

part V - statistics report & export
* export fields (choose format & location)
* ask user if only fields (and not non-agricultural areas) should be exported (default: no)
* ask user if smoothing should be performed (default: no)