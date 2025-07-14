<style>
  table th, table td {
    padding: 8px;
  }
</style>

<div style="width: 50%; margin: auto;">

## Introduction

This application provides an interactive visualization tool to explore driving behavior in the Strategic Highway Research Program Naturalistic Driving Study (SHRP2 NDS). Specifically, the data visualization app shows the distribution of miles driven by driving distance from center of the lane values and specific subcategories pertaining to roadway characteristics, as well as driver and vehicle info. Users can filter data based on these factors, which include age, gender, vehicle class, road type, and others. 

The SHRP2 NDS data set includes driver and vehicle data on 3,400+ drivers and over 5.4 millions trips, equating to around 34.5 million miles driven. This work analyzed all available trips housed on the storage systems at VTTI using the vehicle speed from GPS and the vehicle network, as well as map matching data to obtain roadway characteristics. The resultant data set is an aggregation of miles driven under each condition. Figure 1 shows the traversal heatmap for the major highways and interstates in the SHRP2 NDS.

</br>
<figure style="width: 100%; margin: 0 auto;">
  <img src="shrp2_highway_traversals.png" 
       alt="SHRP2 Traversal Heatmap" 
       style="display: block; margin-left: auto; margin-right: auto; width: 100%;" />
  <figcaption style="text-align: center;">Figure 1 – SHRP2 NDS traversal heatmap for major highways and interstates.</figcaption>
</figure>
<hr>

## Methodology

The data from the SHRP2 NDS was subject to several stages of data processing before being used in this visualization tool. Figure 2 explains the various data tables and stages of processing.

</div>

</br>
<figure style="width: 75%; margin: 0 auto;">
  <img src="DistanceFromCenterMethodsDocument.svg" 
       alt="Data Process Outline" 
       style="display: block; margin-left: auto; margin-right: auto; width: 100%;" />
  <figcaption style="text-align: center;">Figure 2 – Data flow diagram.</figcaption>
</figure>
<br>

<div style="width: 50%; margin: auto;">
<hr>

## Trip Level Data Table Variables

When the SHRP2 NDS is first processed, all interested variables are binned for each timestep. A grouping operation is performed to aggregate the miles driven for each bin combination across the variables. The group summary for each unique trip is then written to VTTI's database storage. At this point, the data is described at the trip level, no vehicle or driver information has been included. The dataset size, 6.1 Billions rows, is large for this step of the process due to the number of variables being grouped and summarized. Table 1 includes the variables and descriptions of the data at this point.

<h3 style="text-align: center;">Trip Level Variables</h3>

<table style="width: 100%; border-collapse: collapse;" border="1">
  <caption style="caption-side: top; font-weight: bold;">
    Table 1 – Description of trip level variables.
  </caption
  <thead style="background-color: #f2f2f2;">
    <tr>
      <th> Variable Name</th>
      <th> Description</th>	  
    </tr>
  </thead>
  <tbody>
    <tr style="background-color: #ffffff;">
      <td> FILE_ID</td>
      <td> Unique file ID for the trip</td>
    </tr>
    <tr style="background-color: #f9f9f9;">
	  <td> FUNCTIONAL_CLASS</td>
      <td> Functional class value from Here.com map matching</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> SPEED_CATEGORY</td>
      <td> Speed category value from Here.com map matching</td>
    </tr>
	<tr style="background-color: #f9f9f9;">
	  <td> SPEED_LIMIT_MPH</td>
      <td> Speed limit [mph] from OSM and Here.com map matching</td>
	</tr>
    <tr style="background-color: #ffffff;">
      <td> LINK_ID</td>
      <td> Here.com map matching link ID</td>
    </tr>
	<tr style="background-color: #f9f9f9;">
	  <td> WAY_ID_OSM</td>
      <td> OSM map matching way ID </td>
	</tr>
	<tr style="background-color: #ffffff;">
      <td> ROAD_CLASS_OSM</td>
      <td> Road class value from OSM map matching</td>
    </tr>
    <tr style="background-color: #f9f9f9;">
	  <td> SPEED_MPH</td>
      <td> Speed bin [mph]</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> SPEEDING_MPH</td>
      <td> Speeding bin [mph] relative to the speed limit</td>
	</tr>
	<tr style="background-color: #f9f9f9;">
	  <td> DISTANCE_FROM_CENTER_M</td>
      <td> Distance bin [m] vehicle is from center of lane</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> HEADWAY_S</td>
      <td> Time headway bin [s] between leading vehicle in same lane</td>
	</tr>
	<tr style="background-color: #f9f9f9;">
	  <td> FOLLOW_DISTANCE_M</td>
      <td> Distance bin [m] from lead vehicle in same lane</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> TTC_S</td>
      <td> Time to collision bin [s] to lead vehicle</td>
	</tr>
	<tr style="background-color: #f9f9f9;">
	  <td> COUNT</td>
      <td> Number of instances for this bin combination</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> MILES</td>
      <td> Aggregate miles driven in this bin combination</td>
	</tr>
	<tr style="background-color: #f9f9f9;">
	  <td> TIME</td>
      <td> Aggregate time driven in the bin combination</td>
    </tr>
  </tbody>
</table>
</br>

## Driver, Vehicle, and Selected Variable Level Data 

Next, the data is consiladated based on the variables of interest – in this case the distance from center bin. The distance from center bin, along with the roadway characteristics and miles summation bin were extracted, joined with the associated driver demographics and vehicle info from the unique file ID, and summarized based on miles driven. Once completed, the data no longer contained the unique file ID but did include the driver demographic and vehicle information. The dataset was now expressed at the driver, vehicle, and distance from center variable level, as apposed to the file ID level before. This reduces the number of rows considerably and allows the data visualization tool to compare distance from center and roadway characteristics with additional filtering available. Table 2 shows the variables and their description at this level.

<h3 style="text-align: center;">Driver, Vehicle, and Distance From Center Level Variables</h3>
<table style="width: 100%; border-collapse: collapse;" border="1">
  <caption style="caption-side: top; font-weight: bold;">
    Table 2 – Description of driver, vehicle, and distance from center level variables.
  </caption
  <thead style="background-color: #f2f2f2;">
    <tr>
      <th> Variable Name</th>
      <th> Description</th>	  
    </tr>
  </thead>
  <tbody>
    <tr style="background-color: #ffffff;">
      <td> VTTI_VEHICLE_ID</td>
      <td> Anonymized unique vehicle ID</td>
    </tr>
    <tr style="background-color: #f9f9f9;">
	  <td> VTTI_DRIVER_ID</td>
      <td> Anonymized unique driver ID</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> GENDER</td>
      <td> Gender of the driver at time of installation</td>
    </tr>
	<tr style="background-color: #f9f9f9;">
	  <td> AGE_RANGE</td>
      <td> 5 year age range of driver</td>
	</tr>
    <tr style="background-color: #ffffff;">
      <td> VEHICLE_CLASS</td>
      <td> Vehicle class: Car, SUV - Crossover, Truck, or Minivan</td>
    </tr>
	<tr style="background-color: #f9f9f9;">
	  <td> FUNCTIONAL_CLASS</td>
      <td> Functional class value from Here.com map matching </td>
	</tr>
	<tr style="background-color: #ffffff;">
      <td> SPEED_CATEGORY</td>
      <td> Speed category value from Here.com map matching</td>
    </tr>
    <tr style="background-color: #f9f9f9;">
	  <td> SPEED_LIMIT_MPH</td>
      <td> Speed limit [mph] from OSM and Here.com map matching</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> ROAD_CLASS_OSM</td>
      <td> Road class value from OSM map matching</td>
	</tr>
	<tr style="background-color: #f9f9f9;">
	  <td> DISTANCE_FROM_CENTER_M</td>
      <td> Distance bin [m] vehicle is from center of lane</td>
    </tr>
	<tr style="background-color: #ffffff;">
      <td> MILES_TOTAL</td>
      <td> Aggregate miles driven in this bin combination</td>
	</tr>
	<tr style="background-color: #f9f9f9;">
	  <td> TIME_SEC_TOTAL</td>
      <td> Aggregate time driven in the bin combination</td>
    </tr>
  </tbody>
</table>
</br>

## Final Data Filtering

Some final data filtering was required before visualization with this tool. This distance from center visualization tool shows all distances from center where the lane lines are detected with a certain confidence by bins of 0.1 meters from -2 to 2 meters, including the -2 meter endpoint but excluding the 2 meter endpoint. All other instances of the distance from center value, such as those that were less than -2 meters, greater than or equal to 2 meters, or unavailable have been removed. Thus, an aggregate of 21.332 million miles of driving exist in the SHRP2 NDS data set within those distance from center conditions for the visualization tool. Negative distance from center values indicate the vehicle is to the left of the center of the lane, and positive values indicate the vehicle is to the right of the center of the lane.

## Step Plot and Box Plot Tabs

The data is shown in two plots in the 'Data Viewer' tab. The first is a step plot which aggregates the miles driven for each distance from center bin across all the drivers. This can be viewed either as total miles for each distance from center bin or percent of the overall total miles for each distance from center bin. The second is a box and whisker plot. This plot shows the box plots for the percentage of total miles of each distance from center bin relative to the total miles driven by that unique driver. Here the user can view the mileage breakdown within the population of drivers as opposed to solely relative to the total number of miles driven, which can help visualize the difference between drivers. The download button will download the data used to generate the box plots. This data table icludes mileage accumulations based on the driver ID and the percentage of miles driven relative to the total miles of driven for each driver ID.

</br>
</br>
</div>

<style>
h1 {
color: black; /* Change this to the desired color */
}
h2 {
color: #861F41; /* Change this to the desired color */
}
h3 {
color: #861F41; /* Change this to the desired color */
}
h4 {
color: #861F41; /* Change this to the desired color */
}
h5 {
color: #861F41; /* Change this to the desired color */
}
h6 {
color: #861F41; /* Change this to the desired color */
}
</style>

