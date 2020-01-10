# Random State Generator
This repository holds both the code (the code that is allowed to be released due to limitations on completeness and publication rules) as well as the images produced by the code. 

The idea behind the construction is as follows. First, grid locations are constructed one at a time which primarily contain rural census blocks in order to form the countryside of the new state using some type of sampling procedure (called `sampling_method` in program. 
Each grid location contains some number of census blocks depending on the arguments set by the user. 
Then each grid location is merged to the countryside by some type of merge procedure (called `merge_method` in program). 

Once each rural grid location is constructed, the program will move on to constructing the urban grid locations. 
The method used to construct the urban grid locations is as follows. Note that this method is the same as the one used for building the rural grid locations. 
The original dual graph of that is separated into grid locations and each grid location is classified as either rural or urban based on whether they had less or more than 50% urban census blocks respectively. 
A uniformly random urban grid location is selected. In this grid location, a random urban census block is selected and a rectangular region is placed around the census block. 
All census blocks in this region are going to be added to the the next grid location in the random state being generated. If there are rural census blocks in these regions, that is acceptable. However, for the rural grid locations, only rural census blocks are allowed.

Two grid locations are joined together in the random state using what we call the 'interval method' and is conducted as follows. 
First, the maximum and minimum values in the x- and y-directions are found. 
The geometric boundary recorded in the shapefile contains a list of points that dictact the boundary of each census block. 
Using these points, the the boundary that creates a sample is used to construct a list of the points on the boundary of the sample. 
These points are ordered and the positive of the maximum and minimum values in the x- and y-directions are recorded. 
Then the median point between each pair of successive maximum and minimum values in the list of ordered boundary points are found and recorded as the "four corners of a rectangle". 
In this way, a pseudo-rectangular boundary region is constructed for the sample. 
After the "four corners of a rectangle" representing the boundary of the sample are recorded, the proportion and order of the census blocks that occupy each side of the rectangle are recorded using interval notation. 
For example, say a sample of census blocks has 6 census blocks that occupy the "right side of the boundary rectangle" on the intervals [0,0.15], [0.15,0.3], [0.3,0.41], [0.41,0.45], [0.45,0.8], [0.8,1.0]. 
When a sample is joined to another sample, the intervals are matched so that edges are added between census blocks that have overlapping invervals. 
For example, a second sample of census blocks has 8 census blocks that occupy the "left side of the boundary rectangle" on intervals [0,0.1], [0.1,0.25], [0.25,0.28], [0.28,0.31], [0.31,0.5], [0.5,0.66], [0.66,0.81],[0.81,1.0]. 
Now suppose that the previous to samples are going to be joined together. 
Then there will be an edge between the census blocks with the following pairs of intervals: {[0,0.15],[0,0.1]}, {[0,0.15],[0.1,0.25]}, {[0.15,0.3],[0.1,0.25]}, {[0.15,0.3],[0.25,0.28]}, {[0.15,0.3],[0.28,0.31]}, {[0.3,0.41],[0.31,0.5]}, {[0.41,0.45],[0.31,0.5]}, {[0.45,0.8], [0.31,0.5]}, {[0.45,0.8],[0.5,0.66]}, {[0.45,0.8],[0.66,0.81]}, {[0.8,1.0],[0.66,0.81]}, {[0.8,1.0],[0.81,1.0]}.

The urban grid locations will be determined by the use of the Schelling Segregation Algorithm. 

## Goal
The purpose of this code is to produce randomly generated states built from the census blocks of 48 of the 50 U.S. states (Alaska and Hawaii are removed due to contiguity complications). 
The output of the code will be a dual graph where each vertex is a census block, edges represent adjacent census blocks and each census block contains the following columns of information:
- `BLOCKID10`: Block identifier; a concatenation of 2010 Census state FIPS code, county FIPS code, census tract code and tabulation block number.
- `geoid`: VTD FIPS code
- `BLOCKCE`: 2010 Census tabulation block number
- `block`: 2010 Census tabulation block number
- `STATEFP10`: 2010 Census state FIPS code
- `state`: 2010 Census state FIPS code
- `COUNTYFP10`: 2010 Census county FIPS code
- `county`: 2010 Census county FIPS code
- `TRACTCE10`: 2010 Census  tract code
- `tract`: 2010 Census  tract code
- `PARTFLG`: Part Flag Indicator – Y = partial block, N = whole block; Part Flag should always be N in these files
- `HOUSING10`: 2010 Census Housing Unit Count
- `C_X`: Longitude of the centroid of the census block
- `C_Y`: Latitude of the centroid of the census block
- `NAME`: Current Alaska Native Regional Corporation name
- `NAME10`: 2010 Census urban area name
- `NAMELSAD10`: Current name and the translated legal/statistical area description for Alaska Native Regional Corporation
- `UR10`: 2010 Census urban/rural indicator
- `area`: Area of the geometry
- `boundary_perim`: Length of this “exterior” boundary of the census block for the boundary of the state
- `boundary_node`: binary attribute to indicate if a census block is on the exterior of a state
- `POP10`: 2010 Census Population Count
- `TOTPOP`: 2010 Census Population Count
- `NH_WHITE`: White, non-hispanic, population in 2010 Census
- `NH_BLACK`: Black, non-hispanic, population in 2010 Census
- `NH_AMIN`: American Indian and Alaska Native, non-hispanic, population in 2010 Census
- `NH_ASIAN`: Asian, non-hispanic, population in 2010 Census
- `NH_NHPI`: Native Hawaiian and Pacific Islander, non-hispanic, population in 2010 Census
- `NH_OTHER`: Other race, non-hispanic, population in 2010 Census
- `NH_2MORE`: Two or more races, non-hispanic, population in 2010 Census
- `HISP`: Hispanic population in 2010 Census
- `H_WHITE`: White, hispanic, population in 2010 Census
- `H_BLACK`: Black, hispanic, population in 2010 Census
- `H_AMIN`: American Indian and Alaska Native, hispanic, population in 2010 Census
- `H_ASIAN`: Asian, hispanic, population in 2010 Census
- `H_NHPI`: Native Hawaiian and Pacific Islander, hispanic, population in 2010 Census
- `H_OTHER`: Other race, hispanic, population in 2010 Census
- `H_2MORE`: Two or more races, hispanic, population in 2010 Census
- `VAP`: Total voting age population in 2010 Census
- `HVAP`: Hispanic voting age population in 2010 Census
- `WVAP`: White, non-hispanic, voting age population in 2010 Census
- `BVAP`: Black, non-hispanic, voting age population in 2010 Census
- `AMINVAP`: American Indian and Alaska Native, non-hispanic, voting age population in 2010 Census
- `ASIANVAP`: Asian, non-hispanic, voting age population in 2010 Census
- `NHPIVAP`: Native Hawaiian and Pacific Islander, non-hispanic, voting age population in 2010 Census
- `OTHERVAP`: Other race, non-hispanic, voting age population in 2010 Census
- `2MOREVAP`: Two or more races, non-hispanic, voting age population in 2010 Census

## How To Run Code

To run the code, change the working directory to the file that contains "rsg_ver1.py". Additionally, change the string for `shp_path` to your directory that contains all of the original shapefiles that can be downloaded from the Census Bureau. 
The folder structure in the folder associated with the original shapefiles must be as indicated by the definition of `BK` in the rsg_ver1.py code. 
Also, change the string for `json_cb_path` to point towards your directory that contains all the json shapefiles that were provided by Daryl DeFord at [daryldeford.com/dual_graphs]. 

Credit for constructing the json files goes entirely toward Daryl DeFord. 
Thank you, Daryl, for all your help acquiring the data and troubleshooting. I could not have built this program without your encouragement and expertise.

Next, run the following command with the appropriate variables filled in (descriptions below):
```
run rsg_ver1.py state state_parameters use_parameters
```

Note that several features in previous versions of this program have not become fixed attributes based on which attributes provided the best representation for a state. Below I list out those features that are now held constant:
- The random seed is set at 42.
- The Schelling Segregation Algorithm will always be used to determine where to place the urban grid locations. 
- The window sampling method will be used when selecting a random grid location. Further, both `gridloc_with_rural` and `gridloc_with_urban` will be used when building the random grid locations. 
That is, instead of simply choosing a rectangular region around a uniformly randomly selected urban census block from the original state dual graph, we divided up the entire state into grid locations, identified the rural and urban grid locations, uniformly randomly selected a rural grid location or urban grid location (depending on which one is being added to the random state, a uniformly random rural or urban census block is chosen for the selected grid location respectively, and all census blocks in the rectangular region around this randomly selected census block constitute the new grid location that will be added to the random state.
In this way, the high density regions (such as city centers and the downtown area) are not over represented in the random state. 
- The shape of the random state will be the same as the state that is being sampled.
- The merge method (manner in which two grid locations are joined) will be the interval method.
- The sampling method will be the window method. That is, a fixed rectangle will be placed over the random grid location selected to generate a random grid location that will be added to the random state.
- The variable `min_gridloc_size` will be set to 2. That is, The minimum number of census blocks in a grid location is 2.
- The population will be the only parameter that has Gaussian noise added.
- No modification will be made to each grid location.
- Instead of determining the number of cities to be placed in the random state, this is all handled by the Schelling Segregation Algorithm.
- `interval_prob` is set to 0.8. The `interval_prob` is the mean used to determine the probability of whether an edge should be kept when placing edges between two grid locations (called the `interval` merge method).
That is, a Gaussian random number between 0 and 1 with mean equal to 0.8 and standard deviation 0.1 is chosen as the probability, say p, that each edge identified between two grid locations is kept in the random state. 

## Variable Description

Here is a description of each variable in the above script:
- `state`: This argument is either the string 'random' or a two digit string that represents the U.S. from which data is sampled. 
The possible two-digit values and their corresponding states are as follows:
01 -- Alabama, 
04 -- Arizona, 
05 -- Arkansas, 
06 -- California, 
08 -- Colorado, 
09 -- Connecticut, 
10 -- Delaware, 
12 -- Florida, 
13 -- Georgia, 
16 -- Idaho, 
17 -- Illinois, 
18 -- Indiana, 
19 -- Iowa, 
20 -- Kansas, 
21 -- Kentucky, 
22 -- Louisiana,
23 -- Maine,
24 -- Maryland,
25 -- Massachusetts,
26 -- Michigan,
27 -- Minnesota,
28 -- Mississippi,
29 -- Missouri,
30 -- Montana,
31 -- Nebraska,
32 -- Nevada,
33 -- New Hampshire,
34 -- New Jersey,
35 -- New Mexico,
36 -- New York,
37 -- North Carolina,
38 -- North Dakota,
39 -- Ohio,
40 -- Oklahoma,
41 -- Ohio,
42 -- Pennsylvania,
44 -- Rhode Island,
45 -- South Carolina,
46 -- South Dakota,
47 -- Tennessee,
48 -- Texas,
49 -- Utah,
50 -- Vermont,
51 -- Virginia,
53 -- Washington,
54 -- West Virginia,
55 -- Wisconsin,
56 -- Wyoming.
Both Alaska and Hawaii are not included because of contiguity complications.
- `state_parameters`: This argument is a tuple of length 3. 
The purpose of the 3rd element in the tuple will change depending on whether the 1st or 2nd element of `state_specs` is true. 
	- The 1st element in `state_parameters` represents the variable 'mean_samples_per_state' which is a non-negative integer. 
	In particular, `mean_samples_per_state` represents the average number of grid locations desired by the user. A grid location is a small piece of the state that contains some positive number of census blocks. 
	An acceptable number of grid locations depends on the population and density of the state per census block.
	A value of `mean_samples_per_state` that is too large will mean that each grid location contains one census block, which will make the state look incredibly sparse.
	A value of `mean_samples_per_state` that is too small will have the opposite effect. 
	The actual number of grid locations (called `samples_per_state`) is set by finding a random number using a normal distribution with mean equal to `mean_samples_per_state` and with standard deviation equal to the standard deviation of the population over all of the census blocks in the original state.
	- The 2nd element in `state_parameters` represents the variable 'water' which is a boolean variable. 
	If `water==True`, then all census blocks that contain only water (no land) will be deleted from the possible set of census blocks that can be chosen during the construction of the state.	
- `use_parameters`: This argument is a tuple of length 6.
	- `use_parameters` is a tuple of length 6 where the elements of the tuple represent the variables 'ratio', 'iterations', 'nbr_dist','no isolates', 'metric', and 'number of urban grid locations'. See the section below on the Schelling Segregation Algorithm for a description of these variables.

## Example

An example of a fully filled out command to run in the terminal would be the following:
```
run rsg_ver1.py '44' (1000,False) (0.5,30,1,True,'cityblock',None)
run rsg_ver1.py '13' (11758,False) (0.30,40,2,False,'cityblock',1309)
```
<!--Here is a description of each variable in the above command:
- `grid_placement` is set to 'mixed' which means that it is a mixture of the 'random' `grid_placement` method and 'fixed' `grid_placement` method when constructing the countryside (mostly rural census blocks). 
The '3' and '4' represent the length and width of the rectangle built using the 'fixed' method that will be constructed using samples of size `sample_num` respectively. The '5' represents the number of additional samples of census blocks added iteratively to the boundary of the rectangle using the 'random' method.
- `sampling_method` is set to 'diam' for diameter.
- `merge_method` is set to 'intervals'.
- `sample_num` is set to 50. That is, the number of census blocks chosen in each sample is approximately 50.
- `state` is set to '44' which is Rhode Island.
- `pop_category` is set to 'all'. This means that all possible population sizes are allowed when choosing census blocks to build the cities.
- `city_specs` is set to (0,5). That is, the code will build 5 cities.
- `mod` is set to none. That is, no modification will be made to the edges/vertices in each sample.
- `mod_prob` is set to 0.5. That is, there is a probability of 0.5 that a modification will take place for the edges/vertices according to the `mod` chosen. 
- `demo_cols` is set to ('BVAP','HVAP','WVAP','VAP'). That is, the only demographic columns that will be randomly modified (if at all) are Black, non-hispanic, voting age population in 2010 Census (BVAP), Hispanic voting age population in 2010 Census (HVA) White, non-hispanic, voting age population in 2010 Census (WVAP), and voting age population in 2010 Census (VAP). 
- `sampling_parameters` is set to (1,1,0). That is, the total population and the demographics will have noise added to the data in each census block randomly. 
- `save_status` is set to (1,1). That is, an image of the final graph will be saved as well as the json file of the graph with its corresponding data. 
- `interval_prob` is set to 0.8. That is, a Gaussian random number is chosen with mean 0.8 and standard deviation 0.1 to determine the proportion of edges that are kept between two samples. 
- `mono_rural_ur` is set to 1. That is, when building the countryside, samples that start with a rural census block must only be built using rural census blocks. 
- `mono_city_ur` is set to 0. That is, when building the citites, rural census blocks are allowed to contain rural census blocks. 
- `mean_samples_per_state` is set to 1000. That is, the average number of samples that will be used to build the countryside is 1000. 
- `city_placement` is set to 'random'. That is, each city will be randomly inserted into the countryside. 
-->
# Schelling Segregation Algorithm

The Schelling Segregation Algorithm (SSA) is an agent-based model created by Thomas Schelling, an American economist, in 1971. 
As the name suggest, the goal was to construct a model that would resemble the movement of agent of different races on some surface. 
Specifically, an agent will move to a random open location if they are not surrounded by at least 'ratio' agents similar to themselves. The 'ratio' is called the similarity threshold and the proportion of similar agents that are in the neighborhood of an agent is called the similarity ratio. 
In the context of segregation, a person will move from their location to an open location randomly if they are not surrounded by at least 'ratio' people. 
Unfortunately, SSA does not accurately model segregation since the model fails to account for economic difficulties, gentrification, and other external and internal forces.

Despite these limitations, we have created a modification of the SSA similar to that proposed by Cottrell his unpublished [manuscript](http://www-personal.umich.edu/~dcott/pdfs/Chapter_1.pdf). Note that Cottrell was redistributing the vote whereas we are redistributing the urban vs. rural locations. 
The neighborhood of a particular grid location is based on 'nbr_dist' using the distance metric specified by 'metric. 
Two of the most successful metrics in practice for our geometry have been Chebyshev metric (also know as L-infinity metric) and the city block metric (also known as the Manhatten metric). The possible metrics can be found on the [scipy documentation](https://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.cdist.html). The possible metrics are as follows: 'braycurtis', 'canberra', 'chebyshev', 'cityblock', 'correlation', 'cosine', 'dice', 'euclidean', 'hamming', 'jaccard', 'jensenshannon', 'kulsinski', 'mahalanobis', 'matching', 'minkowski', 'rogerstanimoto', 'russellrao', 'seuclidean', 'sokalmichener', 'sokalsneath', 'sqeuclidean', 'wminkowski', 'yule'.
The number of iterations by which the SSA is run is controlled by the variable 'iterations'. 
Sometimes, if 'iterations' is not large enough or 'ratio' is not tuned properly, there are many isolated grid locations that primarily have urban census blocks. To resolve this, the program has the ability to delete these locations by setting `no_isolates==True`. 
The last variable that can be chanced is the 'fixed_urban_gl", that is, the variable that can fix the number of grid locations that primarily have urban census blocks. 

To clarify all of the variables, the `use_parameter` argument takes six elements when `use_ssa=True`. In this case, we have the following variables in order:
- The first element of `use_parameters` is 'ratio'. This variable is a number between 0 and 1, and determines the proportion of neighbors that need to be simiar to a particular urban grid location in order for the grid location not to change.
- The second element of `use_parameters` is 'iterations'. This integer variable is the number of iterations the Schelling Segregation Algorithm will perform before stopping. 
- The third element of `use_parameters` is 'nbr_dist'. This float variable is the positive number that indicates any vertex as being a neighbor if the distance between a designated vertex and another vertex is less than 'nbr_dist'. 
- The fourth element of `use_parameters` is 'no_isolates'. This boolean varaible indicates whether isolated grid locations are deleted. That is, if `no_isolates==True`, then all urban grid locations that are not adjacent to another urban grid location are deleted from the list of urban grid locations.
- The fifth element of `use_parameters` is 'metric'. This string variable represents how distance is calculated between two verices. 
- The sixth element of `use_parameters` is 'fixed_urban_gl'. This string or integer variable represents the number of urban grid locations that will be used in the Schelling Segregation Algorithm. If `fixed_urban_gl==None`, then the number of urban grid locations is based on the number of urban grid locations in the original state, but also depends on the value of 'sampling_method' and 'state_shape'. 
If 'fixed_urban_gl' is a positive integer, then 'fixed_urban_gl' is the number of urban grid locations used in the Schelling Segregation Algorithm. 
It is particularly useful to set the value for 'fixed_urban_gl' when `no_isolates==True` since otherwise the number of urban grid locations is not necessarily going to be the number of urban grid locations intended. 


# Images 
The files in images are examples of instances run by text_main.py. The description of each is recorded based on the same variables and order as listed above. 
