This folder provides some sample data used for setting up the algorithm and perform some preliminary evaluation on its performance. In order to achieve a high level of transferability, three different AoIs with different field structures and varying quality of the corresponding Sentinel-2 imagery were chosen. The characteristics of the individual sample data sets are as follows:

* upper rhine area
    * AoI: ~120km<sup>2</sup>, variety of field sizes, mixed use, mostly arable farming
    * S-2 data: Apr-Sep, cloudfree
* magdeburger boerde
    * AoI: ~85km<sup>2</sup>, large field sizes, exclusively arable farming
    * S-2 data: Mar-Oct, partly cloudy
* coastal landscape
    * AoI: ~25km<sup>2</sup>, mostly meadows, in some parts delineated by bushes
    * S-2 data: June-Oct, cloudfree

Sentinel-2 imagery (June 2021) for each of the sample data sets alongside with centroid coordinates: 

<p>
<table width="100%" cellspacing="5" cellpadding="1" border="0">
	<tbody>
		<tr>
		<td valign="top" align="center" width=30%>
            <a href="upper_rhine">
            <img src="_docs/preview_upper_rhine.png" width="90%"></a><br>
            <i>
                upper rhine</br>
                7.67°E 48.00°N</br>
                <a href="https://www.google.at/maps/@48,7.67,12000m/data=!3m1!1e3?hl=en">view on google maps</a>   
            </i>
        </td>
		<td valign="top" align="center" width=30%>
            <a href="magdeburger_boerde">
            <img src="_docs/preview_magdeburger_boerde.png" width="90%"></a><br>
            <i>
                magdeburger boerde</br>
                11.34°E 52.19°N</br>
                <a href="https://www.google.at/maps/@52.19,11.34,10000m/data=!3m1!1e3?hl=en">view on google maps</a>  
            </i>
        </td>
		<td valign="top" align="center" width=30%>
            <a href="coastal_landscape">
            <img src="_docs/preview_coastal_landscape.png" width="90%"></a><br>
            <i>
                coastal landscape</br>
                7.98°E 53.57°N</br>
                <a href="https://www.google.at/maps/@53.57,7.98,6000m/data=!3m1!1e3?hl=en">view on google maps</a>
            </i>
        </td>
		</tr>
	</tbody>
</table>
<p>

<!-- 
Note that the decription may be complemented by providing an interactive leaflet map, which could be integrated in the following manner. Iframe-Tag is not allowed for github markdowns but via embed the map could be displayed:
<embed type="text/html" src="https://fkroeber.de/wp-content/uploads/2022/06/study_area_map.html" width="500" height="200">    -->

Note that the transferability of the algorithm is thus still only tested in the context of certain climates and agricultural systems. The extent to which it can be applied beyond the areas presented here - all located in temperate mid-latitudes (Koeppen-Geiger: Cfb & Dfb) - has not been explicitly investigated yet.

For each of the selected AoIs, training and validation fields have been delineated manually using very high-resolution imagery whose date of recording coincides with that of the S2 scenes (i.e. they were recorded in 2021). Thus, every AoI is accompained by a subfolder containing the following vector data sets:
* training_n10
* training_n25
* validation_n25

Two training data sets are available in order to evaluate the sensitivity of the algorithm to the specific fields selected for parameter optimisation. By providing different number of fields for each training data set it should also be possible to get a feel for the required number of fields necessary for achieving certain level of delineation accuracy. Both training data sets were created by choosing manually fields within the AoI which cover the whole breadth of fields to be delineated in an appropriate manner. Results obtained based on the basis of the two training sets can then be assessed using the validation data set containing 25 fields selected based on random sampling in order to avoid biases in the selection of fields.
