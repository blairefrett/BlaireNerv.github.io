# BlaireNerv.github.io
Research Question: 

What is the relationship between road conditions and the frequency of vehicle-pedestrian and vehicle-bicycle collisions in Oakland, California?
Background: 

In 2023, the City of Oakland released a report stating traffic collisions within the city are a “public safety epidemic” and that the rate of severe and fatal car collisions are “unacceptably high.”  The report stated several known factors leading to high collision rates, such as speeding, running red lights, and driving under the influence, but did not mention whether road conditions, such as the existence of potholes or construction, played a role in high collision rates (City of Oakland, 2023; Fermoso, 2023). 

While research shows that road conditions can increase the risk of car collisions more broadly, there is not as much research showing the impact of road conditions on the rates of vehicle-pedestrian and vehicle-bicycle collisions. 

In order to study the specific impact of road conditions on the rate of vehicle-pedestrian collisions, road conditions must be specifically and systematically identified and investigated, a task that can be difficult to carry out and thus has not been fully explored. 

One systematic literature review that examined several studies that explored the factors leading to pedestrian-vehicle collisions mentioned factors such as land use, lighting conditions, number of lanes, and road surface conditions, to name a few. Road surface conditions were categorized as either “bad or wet” or “good or dry.” From this review, variables such as land use and road type were statistically significantly correlated with increased pedestrian-vehicle collisions, but variables such as road surface condition was not significant. 

Importantly, further research is required to deepen our understanding of how road conditions relate to vehicle-pedestrian collisions, and there is a need to systematically identify which road conditions to investigate and agree on how they're categorized, as well as which specific characteristics of road conditions to examine (Shrinivas et al., 2023). 

As far as bicycle collisions are concerned, most research on safety considerations looks into the effects of transportation infrastructure, specifically the existence and type of intersection and straightaway. Intersections refer to features like roundabouts and junctions, whereas straightaways refer to bike routes, bike lanes, speedbumps, etc. 

Research consistently demonstrates that designated facilities tailored for bikes, like cycle tracks at roundabouts, designated bike routes, lanes, and paths, offer better safety for cyclists compared to cycling alongside traffic on roads or sharing pathways with pedestrians and other users (Reynolds et al., 2009). 

However, there are many other road features that merit investigation, such as the existence of stop signs, numbers of roads intersecting, junctions such as driveways and lanes, cyclist lane of travel in relation to parked cars, traffic calming measures such as diverters or road humps, road/lane/path curvature, and the surface features such as cobble stones or street-car (tram) tracks. 

Notably, little research to date addresses the specifications of road conditions, and there is a need for clear and specific road condition categorization to better understand if/how these conditions impact bicyclist safety (Reynolds et al., 2009). In all, there is a need for further research to document the specific relationship between vehicle collisions and pedestrian and bicyclist safety. 

This project was an initial attempt to explore the relationship between vehicle-pedestrian and vehicle-bicycle collisions and road condition in Oakland. We utilized the road conditions data from TIMS, which categorizes road condition into the following categories: Holes, Deep Ruts; Loose Material on Roadway; Obstruction on Roadway; Construction or Repair Zone; Reduced Roadway Width; Flooded; Other; No Unusual Condition. We will determine if there is a relationship between pedestrian collisions in 2022 and the corresponding road condition, and will conduct the same analysis for bicycle collisions. 
Research Goals:
Our research seeks to comprehensively understand and address the factors influencing pedestrian and bicycle accidents within the urban environment of Oakland. By leveraging data from the Transportation and Injury Mapping System (TIMS) and the American Communities Survey (ACS) 2020 Decennial Census Population Data, our study aims to utilize geospatial analysis techniques to examine the spatial distribution and clustering of pedestrian and bicycle accidents across Oakland. By mapping the incidents and identifying hotspots, we aim to delineate areas with higher accident frequencies and explore potential contributing factors such as road condition.

Data:

TIMS Crash and Road Condition Data Jan 1-Dec 31 2022 
U.S. Census Bureau’s 2020 Census Demographic and Housing Characteristics
Open Street Map


Methodology:
Data Acquisition
We acquired data from the Transportation and Injury Mapping System (TIMS) covering the period from January 1 to December 31, 2022. This dataset includes information on the count and geographical location of pedestrian-vehicular accidents, bicycle-vehicle accidents, and road condition data. We also used data from U.S. Census Bureau’s 2020 Census Demographic and Housing Characteristics. This data includes the geography of each census tract within the Oakland city boundary, and its population characteristics. Data from OpenStreetMap was used to visualize the Oakland Street Network. 
Importing Libraries
Our initial step involved importing the necessary libraries to facilitate data visualization and spatial analysis. We utilized pandas, geopandas, the Oakland street map network, seaborn, and shapely.geometry. These libraries enabled spatial operations and geospatial data handling, ensuring accurate representation and analysis of spatial data.
Creating GeoDataFrame
We converted the TIMS data, initially stored in a CSV file, into a GeoDataFrame. Firstly, we imported the data into a pandas DataFrame named 'df'. Subsequently, we created a list of shapely point objects by iterating over pairs of 'POINT_X' and 'POINT_Y' coordinates from the DataFrame 'df'. Each pair of coordinates was transformed into a point object using the 'Point' function. We then assigned a Coordinate Reference System (CRS) of EPSG 4326 to the GeoDataFrame 'gdf', aligning with the standard CRS for latitude and longitude coordinates (WGS 84).
Filtering Pedestrian and Bicycle Accidents
We filtered the GeoDataFrame 'gdf' to select rows representing pedestrian and bicycle accidents. Specifically, we isolated rows where the 'PEDESTRIAN_ACCIDENT' column contained the value 'Y' for pedestrian accidents and repeated this step for bicycle accidents by examining the 'BICYCLE_ACCIDENT' column. Additionally, we filtered rows where the 'POINT_X' and 'POINT_Y' columns were not null to ensure valid coordinates. The resulting GeoDataFrames, 'pedestrian_accidents_gdf' and 'bicycle_accidents_gdf', contained only valid rows representing pedestrian and bicycle accidents, respectively.
Spatial Join
Given that the Oakland Census Tract Data was in the form of a shapefile and already in a GeoDataFrame, we performed a spatial join operation between the 'pedestrian_accidents_points_gdf' GeoDataFrame and the 'oakland_census_tracts' GeoDataFrame. This operation facilitated the analysis and visualization of pedestrian accidents within the context of Oakland census tracts. The spatial join was executed with an inner join type, retaining only the intersections between geometries, based on the specified spatial relationship of 'intersects'.
Plotting the Census Tracts Map and Joined GeoDataFrame
We visualized the pedestrian accidents from the resulting joined GeoDataFrame ('joined_gdf') on a map, overlaying the point locations of pedestrian and bicycle accidents onto the map of Oakland census tracts.
Creating a Kernel Density Estimation (KDE) Plot of Pedestrian and Bicycle Accidents
Utilizing interpolation, we generated a Kernel Density Estimation plot to convert discrete data points into a continuous surface. This KDE plot visually represented the density of pedestrian and bicycle accidents in Oakland, aiding in the identification of areas with higher concentrations of accidents. The plot employed a smooth Kernel Density Estimation technique to visualize the distribution of accidents across latitude and longitude coordinates.
Addressing the Modifiable Areal Unit Problem
To mitigate the modifiable areal unit problem, we normalized the data by population. Using data from the 2020 Decennial Census Population Data, which included information on the total population by census tract, we calculated the density of pedestrian-vehicular collisions and bicycle-vehicular collisions per capita within each census tract. Subsequently, we divided the total number of each collision type by the population of the tract. Finally, we visualized the calculated density of collisions per capita on a map.
Bivariate Analysis
We conducted two bivariate analyses to assess the correlation and statistical significance between pedestrian collisions and road conditions, and bicycle collisions and road conditions. From the TIMS data, we utilized pandas to extract the road condition when a pedestrian collision occurred. Road condition in TIMS is denoted by the column name “ROAD_COND_1,” and is split up into the following conditions: 'A': 'Holes, Deep Ruts'; 'B': 'Loose Material on Roadway'; 'C': 'Obstruction on Roadway'; 'D': 'Construction or Repair Zone'; 'E': 'Reduced Roadway Width'; 'F': 'Flooded'; 'G': 'Other'; 'H': 'No Unusual Condition.’ We then plotted the number of pedestrian collisions based on road condition. After, we created a dummy variable called “Unusual Condition,” consolidating the road conditions into a binary variable, whereby A-G were combined to create ‘Unusual Condition.” We conducted a point biserial correlation, deriving the correlation coefficient and p-value for the relationship between pedestrian collisions and unusual road conditions. We followed this exact same process for bicycle collisions. 



Maps:


Figure 1. Point location of pedestrian accidents across census tracts in Oakland, CA 


Figure 2. Kernel Density Estimation of Pedestrian Accidents across Oakland census tracts

Figure 3. Number of pedestrian accidents in 2022 per census tract in Oakland, California 

Figure 4. Pedestrian Accidents normalized by total population in 2022 per census tract in Oakland, California. 

Figure 5. Point location of bicycle accidents across census tracts in Oakland, CA 


Figure 6. Kernel Density Estimation of Bicycle Accidents across Oakland census tracts

Figure 7. Number of bicycle accidents in 2022 per census tract in Oakland, California 


Figure 8. Bicycle Accidents normalized by total population in 2022 per census tract in Oakland, California. 

Figure 9. Annual sum of pedestrian and vehicle collisions in Oakland, California from 2018-2022





Figure 10. Number of pedestrian and bicycle collisions in Oakland, California in the year 2022



Figure 11. Total number of pedestrian collisions in the year 2022 in Oakland, California by road condition



Figure 12. In other words, the correlation coefficient and P-value came back as NaN, perhaps due to the limited variability in or data. 



Figure 13. Total number of bicycle collisions in the year 2022 in Oakland, California by road condition

Figure 14. In other words, there is a very weak negative correlation between icicle accidents and unusual road condition, however this relationship is not statistically significant. 


Figure 15. 


Figure 16. 


Figure 17. 

In other words, there is no evidence to reject the null hypothesis, suggesting that the occurrence of pedestrian or bicycle accidents is not dependent on the road condition.



Findings: 
This project highlights the importance of spatial analysis and visualization techniques in understanding and addressing active transit safety issues within the city of Oakland. Although this research was an initial exploration of the relationship between vehicle-pedestrian & vehicle-bicycle collisions and road condition in Oakland, further analysis of contributing factors to active transit collisions is needed to develop a more comprehensive analysis. 
The data indicates that census tracts exhibiting high pedestrian-vehicle collision rates are also the same census tracts with high bicycle-vehicle collision rates. Census tracts in Downtown Oakland and East Oakland, particularly near the Fruitvale Bart Station along the 880, exhibit elevated rates of pedestrian and bicycle collisions, reflective of the high pedestrian and bicycle activity in these areas. When normalized by the total population per census tracts, this analysis remained true. The combined incidents of bicycle and pedestrian collisions declined steadily from 2018 to 2021, yet saw a resurgence in 2022. We conducted a bivariate analysis to gauge the correlation strength between pedestrian and bicycle accidents based on road type.

In terms of road condition, a significant limiting factor was limited data. Because we explored TIMS data only for the year 2022, only 256 pedestrian collisions and 123 bicycle collisions were recorded. When we analyzed this based on road condition, 246 pedestrian collisions and 122 bicycle collisions occurred with no unusual road condition. That left limited variation within our data, and accordingly limited analysis strength. However, we conducted a correlation analysis for practice purposes. Both the correlation coefficient and p-value for the relationship between pedestrian accidents and road condition came back as NaN, given the limited variability in the data. For the relationship between bicycle collisions and unusual road condition, the point-biserial correlation coefficient was -0.014, indicating a very weak negative correlation between ‘Unusual Condition’ and bicycle collisions. The p-value is 0.825, meaning we can't reject the null hypothesis and the relationship between road condition and bicycle collisions is not statistically significant. 

Finally, when we combined both pedestrian and bicycle collisions, and explored the relationship between these collisions and road conditions, we derived a chi-square of 0 and a p-value of 1. Together this means that there is no association between the occurrence of pedestrian or bicycle collisions and road conditions, and that their relationship is not statistically significant. 

In the future, it would be useful to explore multiple years to ensure more data points, and expand the characteristics of road conditions. TIMS data also has a column called road surface that indicates whether the road is wet, dry, snowy, etc. This could help create a more comprehensive assessment. It would also be useful to utilize other data sets that identify road conditions, because this data set lacked variability, and the fat majority of collisions were reported under normal road conditions. 

Citations:
City of Oakland. (2023). Year-One Safe Oakland Streets (SOS) Report to City Council. City of Oakland. https://www.oaklandca.gov/documents/year-one-safe-oakland-streets-sos-report-to-city-council
Fermoso, J. (2023, May 25). Report: Oakland traffic collisions are a ‘public safety epidemic.’ The Oaklandside. http://oaklandside.org/2023/05/25/report-oakland-traffic-collisions-are-a-public-safety-epidemic/
Reynolds, C. C., Harris, M. A., Teschke, K., Cripton, P. A., & Winters, M. (2009). The impact of transportation infrastructure on bicycling injuries and crashes: A review of the literature. Environmental Health, 8, 47. https://doi.org/10.1186/1476-069X-8-47
Shrinivas, V., Bastien, C., Davies, H., Daneshkhah, A., & Hardwicke, J. (2023). Parameters influencing pedestrian injury and severity – A systematic review and meta-analysis. Transportation Engineering, 11, 100158. https://doi.org/10.1016/j.treng.2022.100158
