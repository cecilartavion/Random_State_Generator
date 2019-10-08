# Random State Generator
This repository holds both the code (the code that is allowed to be released due to limitations on completeness and publication rules) as well as the images produced by the code. 

The idea behind the construction is as follows. First, grid locations are constructed one at a time which primarily contain rural census blocks in order to form the countryside of the new state using some type of sampling procedure (called `sampling_method` in program. 
Each grid location contains some number of census blocks depending on the arguments set by the user. 
Then each grid location is merged to the countryside by some type of merge procedure (called `merge_method` in program). 

Once the entire countryside is constructed, the program will move on to constructing the urban landscape. 
There are three primary method in which the urban landscape is constructed. 
First, only cities will be constructed similar to how the countryside was constructed, and then each city will be stitched into the countryside one at a time. 
Second, cities will be constructed as above. 
However, once all of the cities have been placed, the program will add individual grid locations that primarily contain urban census blocks. 
Third, only individual grid locations that primarily contain urban census blocks are added to the countryside. 
The quantity of the cities and the individual grid locations is determined by the user. 

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

To run the code, change working directory to the file that contains "test_main.py". Then run the following command with the appropriate variables filled in (descriptions below):
```
run random_state_generator.py state state_specs state_parameters sampling_merging_method sampling_parameter use_specs use_parameters city_specs noisy_parameters demo_cols support_parameters save_status
```
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
- `state_specs`: This argument is a tuple of length 2 with binary elements. Exactly one of the elements in the tuple must be True. 
	- The 1st element in `state_specs` represents the variable 'state_shape'. 
	In particular, if 'state_shape' is True, then the coordinates for the grid locations will form an approximate shape of a state based on the 'mean_samples_per_state' value. 
	Note that when 'state_shape' is set to True, the 3rd element in `state_parameters` will indicate the state for which the program will approximate the shape of a state. 
	That means the distributions of population can be applied to the particular shape of a different state, thus allowing for the ability to determine the effects of shape and distribution on the state. 
	- The 2nd element in `state_specs` represents the variable 'build_state'. 
	In particular, if 'build_state' is True, then the state will be built randomly according to the directions provided by the 3rd element in `state_parameters`. 
	Again, note that the 3rd element in `state_parameters` will indicate directions for how to build the state.
- `state_parameters`: This argument is a tuple of length 3. 
The purpose of the 3rd element in the tuple will change depending on whether the 1st or 2nd element of `state_specs` is true. 
	- The 1st element in `state_parameters` represents the variable 'mean_samples_per_state' which is a non-negative integer. 
	In particular, 'mean_samples_per_state' represents the average number of grid locations desired by the user. A grid location is a small piece of the state that contains some positive number of census blocks. 
	An acceptable number of grid locations depends on the population of the state, the `sampling_parameter` when `sampling_method=='diam' or sampling_method=='rect'`, and on whether 'mimic_city' (8th element of `support_parameters`) is True or False. 
	A value of 'mean_samples_per_state' that is too large will mean that each grid location contains one census block, which will make the state look incredibly sparse when `sampling_method=='window'` and incredibly dense when `sampling_method=='diam' or sampling_method=='rect'`. 
	A value of 'mean_samples_per_state' that is too small will have the opposite effect. 
	The actual number of grid locations (called 'samples_per_state') is set by finding a random number using a normal distribution with mean equal to 'mean_samples_per_state' and with standard deviation equal to the standard deviation of the population over all of the census blocks in the original state.
	If `mimic_city==True`, then the number of census blocks per grid location will mimic the city by setting the number of census blocks per grid location to (sum of total population)/((samples_per_state)(average state population per census block)). 
	- The 2nd element in `state_parameters` represents the variable 'water' which is a boolean variable. 
	If `water==True`, then all census blocks that contain only water (no land) will be deleted from the possible set of census blocks that can be chosen during the construction of the state.
	- The 3rd element in `state_parameters` represents either a state (possibly not the same as `state`) when `state_shape==True` or a variable called 'grid_placement' (a tuple of length 2, 3, or 4) when `build_state==True`. 
		- If `state_shape==True`, then the 3rd element in `state_parameters` represents a state (call this variable 'state2') that may or may not be the same as that specified in the `state` argument. 
		The value for 'state2' will follow the same restrictions as specified for `state`. 
		The value for 'state2' will indicate for which state will the approximate shape of the state resemble. For example, if `state=='44'` and `state2=='47'`, then all census blocks, census block adjacency information, and census block data will come from Rhode Island (state FIPS code 44), and the overall shape of the state will come from Tennessee (state FIPS code 47). 
		- If `build_state==True`, then the 3rd element in `state_parameters` represents the variable 'grid_placement' which is a tuple of length 2, 3, or 4.
		The grid locations depend on the 1st element of 'grid_placement'. The first element of 'grid_placement' is always a string and the remaining elements are positive integers. 
		The possible values for the first element of 'grid_placement' are as follows: 'random', 'fixed', and 'mixed'.
		The first element of 'grid_placement' describes the method used to build the overall shape of the countryside and possibly the overall shape of the cities as well (depending on the value of `use_specs`). 
		The length of the tuple 'grid_placement' is 2, 3, or 4 if the first element of 'grid_placement' is 'random', 'fixed', or 'mixed' respectively. 
		In what follows, I describe the function of 'random', 'fixed', and 'mixed' as well as the remaining elements of the tuple 'grid_placement.
			- When the 1st element of 'grid_placement' is 'random', the first grid location of census blocks is placed at (0,0) (using Cartesian coordinate system, that is, x,y coordinate locations). 
			Then all of the subsequent samples are placed iteratively at adjacent integer coordinates so that each new grid location is 1 away (in either the x- or y-direction) from another grid location already placed. 
			So for example, the second grid location has option of being placed at (0,1), (1,0), (-1,0), or (0,-1). 
			As the name suggest, the second grid location is randomly choosen from among these 4 options. 
			- When the 2nd element of 'grid_placement' is 'fixed', let ('fixed',m,n) be a generic representation of 'grid_placement' tuple. 
			Then the grid locations will form a rectangular m by n grid using mn grid locations. 
			The first sample is placed in location (0,0) and the rectangle is constructed with corners (0,0), (m-1,0), (n-1,0), and (m-1,n-1). 
			- When the 1st element of 'grid_placement' is 'mixed', let ('mixed',m,n,p) be a generic representation of the 'grid_placement' tuple.  
			Then the grid locations with form a rectangular m by n grid using mn samples as when the 1st element of 'grid_placement' was 'fixed'. Then p additional grid locations are added to the m by n rectangle iteratively as when the 1st element of 'grid_placement' was set to 'random'. In total, there will be mnp samples.
- `sampling_merging_method`: This argument is a tuple of length 2 where each element is a string. The 1st element represents the variable `sampling_method` and the 2nd element represents `merge_method`. 
	- `sampling_method`: This element is a string which represents how each sample is built. The options for this string are 'rect', 'diam', and 'window'. 
		- For 'rect', a random census block is chosen (urban or rural depends which part of the code this variable is executed). 
		Then a window is drawn around one census block. 
		Then at each iteration, one side of the rectangular window is expanded by 0.1 degrees (between 6.8 and 7 miles in terms of distance in latitudinal and longitudinal directions). 
		The window will stop growing once it reaches at least `sample_num` census blocks in the window. 
		- For 'diam', a random census block, call it v, is chosen (urban or rural depends which part of the code this variable is executed).
		Then the vertices distance 1 (graph theoretic distance) from v are added to the sample. 
		If the number of vertices in this subgraph is less than `sample_num`, then add all of the vertices distance 2 from v to the sample. 
		Once there are at least `sample_num` vertices in the sample, vertices are deleted uniformly at random from the list of vertices furtherest away from v in our sample until only `sample_num` vertices remain. 
		- For 'window', a width and height is calculated for a region around the centroid of a census block. 
		This window represents the size of the grid location when a census blocks are placed into any grid location to form a new state. 
		A sample of census blocks is found by uniformly randomly selecting one census block from the original state. 
		As long as there are at least 'min_gridloc_size' (the value for `sampling_parameter`) census blocks in the window, this window is inserted into a grid location on the overall state based on either the 'grid_placement' variable or the shape of the state found by setting `state_shape==True`. 
	- `merge_method`: This element is a string which represents how a sample is added to a grid of already merged samples. The options for this string are 'intervals' and 'intersect'. 
		- For 'intervals', the procedure for joining a single grid location to one or more grid locations is as follows. First, the maximum and minimum values in the x- and y-directions are found. 
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
		- For 'intersect', the procedure for joining a single grid location to one or more grid locations is as follows.
		First, a bounding rectangle has been placed around the sample of census block centroids. 
		For each census block, say v, inside the bounding rectangle that is adjacent to a census block, say u, outside of the bounding rectangle, the intersection point of the edge and the bounding rectangle is recorded.
		The order of the intersection points between each edge pair, {u,v}, and the bounding rectangle for each side of the bounding rectangle are recorded. 
		Then we will have an order to the adjacencies between that will be made between two samples. 
		For example, suppose that sample A has 6 census blocks (call them c_1, c_2, c_3, c_4, c_5, and c_6) inside the bounding rectangle that have 12 intersection points on the right side of the bounding rectangle of sample A which are ordered from top to bottom as follows: c_1, c_2, c_1, c_1, c_3, c_3, c_4, c_4, c_5, c_4, c_6, c_6. 
		Also, suppose that sample B has 8 census blocks (call them d_1, d_2,...,d_8) inside the bounding rectangle that have 10 intersection points on the left side of the bounding rectangle of sample B which are ordered from top to bottom as follows:
		d_1, d_2, d_2, d_3, d_4, d_5, d_6, d_7, d_7, d_8. 
		Then a random number between 10 and 12, inclusive, is chosen (say 11) which will be the target number of edges to join between the right side of sample A and left side of sample B.
		For sample A, one edge will be randomly chosen not to be include, say c_2 is not included in the intersection point list. 
		Now, sample B will have one intersection point added to the list of possible intersection points, say an extra d_7 is added, and it will be placed close to one of the already existing intersection points, say our new ordering of 11 intersection points are d_1, d_2, d_2, d_3, d_4, d_5, d_6, d_7, d_7, d_7, d_8. 
		Then join the vertices of the centroids of census blocks according to the order we found above. 
		That is, add the following edges to the graph so that the resulting graph is connected: {c_1,c_1}, {c_1,d_2}, {c_1,d_2}, {c_3,d_3}, {c_3,d_4}, {c_4,d_5}, {c_5,d_6}, {c_4,d_7}, {c_4,d_7}, {c_6,d_7}, {c_6,d_8}. 
		WARNING: It is possible that by doing this merging method, the resulting graph may not be planar. 
- `sampling_parameter`: This argument is an integer, but its function depends on whether `sampling_method=='window'` or `sampling_method=='rect' or sampling_method=='rect'`. 
	- If `sampling_method=='window'`, then `sampling_parameter` represents the variable 'min_gridloc_size'. 
	This variable must be a positive integer. 
	'min_grid_size' represents the minimum allowable number of census blocks in a grid location when building a sample of census blocks using the 'window' method.
	- If `sampling_method=='rect' or sampling_method=='rect'`, then `sampling_parameter` represents the variable 'sample_num'. 
	This variable must be a positive integer. 
	'sample_num' represents the minimum number of census blocks allowed in a grid location when building a sample of census blocks using the 'diam' or 'rect' methods. 
	Note that it is possible that 'sample_num' may be overwritten if `mimic_city==True`. 
- `use_specs`:
- `use_parameters`:
- `city_specs`: 
- `noisy_parameters`: A binary tuple of length 3. The three elements indicate for which data will noise be added.
If the first coordinate is 1, the total population will have random Gaussian noise added. If the second coordinate is 1, the variables in `demo_cols` will have random noise added. If the third coordinate is 1, the vote totals will have random noise added.
WARNING: Currently, there is no vote total column, so anything but 0 will likely produce an error, or it should produce an error if it does not. 
- `demo_cols`: A list of strings where each string represents the demographic columns that will have Gaussian random noise applied to each census block in each of the samples. 
The options for elements in the list are the following: 
'NH_WHITE',
'NH_BLACK',
'NH_2MORE',
'NH_AMIN',
'NH_ASIAN',
'NH_NHPI',
'NH_OTHER',
'HISP',
'H_2MORE',
'H_AMIN',
'H_ASIAN',
'H_BLACK',
'H_NHPI',
'H_OTHER',
'H_WHITE',
'VAP',
'WVAP',
'BVAP',
'HVAP',
'AMINVAP',
'ASIANVAP',
'NHPIVAP',
'2MOREVAP', and
'OTHERVAP'.
- `save_status`: A binary tuple of length 2. If the first coordinate is 1, a png will be save of the final graph after cities have been merged into the graph. In the png image, the red vertices represent the rural census blocks while the blue vertices represent the cities. 
If the second coordinate is 1, a json file of the graph with the data will be saved into a folder called OUTPUT. 




- `sample_num`: two digit string that represents the approximate number of census blocks in each sample taken. 
- `pop_category`: integer or string. The only string accepted is 'all'. The intergers allowed are from 0 to the length of the population division. The population is currently divided into cities with 
less than 50,000 people (0), 
between 50,000 and 60,000 people (1),
between 60,000 and 70,000 people (2),
between 70,000 and 80,000 people (3), 
between 80,000 and 90,000 people (4), 
between 90,000 and 100,000 people (5),
between 100,000 and 200,000 people (6),
between 200,000 and 300,000 people (7),
between 300,000 and 400,000 people (8),
between 400,000 and 500,000 people (9),
between 500,000 and 600,000 people (10),
between 600,000 and 700,000 people (11),
between 700,000 and 800,000 people (12),
between 800,000 and 900,000 people (13),
between 900,000, and 1,000,000 people (14),
between 1 and 2 million people (15),
between 2 and 3 million people (16), 
between 3 and 4 million people (17), 
between 4 and 5 million people (18), 
between 5 and 6 million people (19), 
between 6 and 7 million people (20), 
between 7 and 8 million people (21), 
and over 8 million people  (22). 
The population between 50K and 60K are labeled as 0 in `pop_category` and increases incrementally as the population increases. 
Another option is to set the variable to 'all' which indicates that all city sizes are possible in the creation of new cities. 
- `city_specs`: tuple that will help determine the number of cities using various methods. 
The possible values for the first coordinate are 0, 1, 2, and 3. The lengths of the tuple depends on the first coordinate. If the first coordinate is 0, 1, 2, or 3, then the length of the tuple is 2, 3, 2, 2 respectively. 
  -If the first coordinate is 0, then the number of samples used to build a city is the value in the second coordinate of the `city_specs` tuple. 
  -If the first coordinate is 1 and the tuple is (1,x,y), then x is a string that represents the 2-digit code for one of the possible states listed above in `state`, and y is an integer greater than or equal to 1000. The number of cities when the first coordinate is 1 is the number of cities in state x with a population over y people.
  -If the first coordinate is 2 and the tuple is (1,x), then x is a string that represents the 2-digit code for one of the possible states listed above in `states`. 
The number of cities when the first coordinate is 2 is equal to the number of cities over a population of 5000 in state x. 
  -If the first coordinate is 3 and the tuple is (1,x), then x is a string that represents the 2-digit code for one of the possible states listed above in `states`. The number of cities when the first coordinate is 2 is equal to the number of cities over a population of 5000 in state x. The sizes of the cities will be the same sizes as those that appear in state x. 
- `mod`: string that represents the modification that is to be made on each sample.
This modification would be to the edges between census blocks in the sample or the vertices (census blocks) themselves. 
The options for modification are as follows: 'add_vert', 'add_edges', 'add_remove_edges', 'delete_vert', 'delete_edges', and 'none'.
WARNING: It is possible that all of these modification may make the graph non-planar. 
WARNING: add_vert is suppressed at the moment since no method was implemented to build the data from a new vertex that is added to the graph. 
The purpose of modifying the graph is to create random samples that are similar to the original sample graphically, but not exactly the same. 
  - 'add_vert': Adds a vertex to a random face uniformly at random and adds a random number of edges based on `mod_prob` probability. The sample must be planar for this to work. Currently, this method is suppressed since it is not clear how to generate data out of nowhere for the new census block.
  - 'add_edges': Add a random number of edges between vertices based on the `mod_prob` probability while retaining planarity. The sample must be planar in order for any edges to be added. 
  - 'add_remove_edges': Add and remove a random number of edges based on the `mod_prob` probability while retaining planarity. The sample must be planar in order for any edges to be added. 
  - 'delete_vert': A random probability of vertices are deleted based on the `mod_prob` probability. It is possible the sample becomes disconnected.
  - 'delete_edges': A random probability of edges are deleted based on the `mod_prob` probability. It is possible the sample becomes disconnected.
  - 'none': No modification is made. 
- `mod_prob`: A Float between 0 and 1 that represents the probability of a change occurring based on the modification selected in `mod`.
- `interval_prob`: Probability associated when the merge_method is 'interval'. This probability is the mean used to determine the probability of whether an edge should be kept. That is, a Gaussian random number is chosen to the this probability, say p, by using the value `interval_prob` as the mean and 0.1 as the standard deviation. This is set to 0.8 by default. The probability p is taken and multiplied by the number of edges joining two samples and the floor of this product is the number of edges that will be used to join two samples. Uniformly at random, edges are chosen to be joined between the two samples. 
- `mono_rural_ur`: A binary variable that is either 0 or 1. This variable determines whether urban census blocks are allowed to be included when building the countryside. 
 If `mono_rural_ur` equals 1, then only rural census blocks are allowed in the construction of the countryside. Otherwise, urban census blocks are allowed to be included. 
- `mono_city_ur`: A binary variable that is either 0 or 1. This variable determines whether rural census blocks are allowed to be included when building the countryside. 
 If `mono_city_ur` equals 1, then only urban census blocks are allowed in the construction of the city samples. Otherwise, rural census blocks are allowed to be included. 
- `mean_samples_per_state`: A positive integer that represents the average number of samples that will be used to build the countryside. A standard number for this would be around 1000. If `mean_samples_per_state` is too large, there will be one census block per sample, which is undesirable for many reasons. If `mean_samples_per_state` is too small, there will be so few samples used to build the map, it will hardly look like a state and there will be large sections of the map that will be copy pasted directly from the state. 
- `city_placement`: A variable that is either a tuple of length 4 or a string. All of the different options for `city_placement` will affect the distribution of where to place the next city in the countryside. 
That is, a probability distribution will be placed at each grid location and when it is time to place the city in the countryside, a grid location will be randomly selected based on this probability distribution. 
  -If `city_placement` is a tuple, the first element must be 'neighbor_cluster' and the tuple has the form ('neighbor_cluster',u_score,r_score,u_penalty). 
  Both u_score and r_score are non_negative numbers (not at the same time) value for the prevelance of being closer or farther away from an urban location.
  Specifically, for each census block x, the neighbors of x are summed together based on the value of u_score and r_score. 
  Then the total is penalized by a factor of u_penalty (positive number) if x is going to land on an urban census block. 
  Finally, all scores are normalized so that their sum is 1. 
  - If `city_placement` is a string equal to 'random', then the probability distribution is a uniform probability distribution.
  - If `city_placement` is a string equal to 'grid_gaussian_kde', then the probability distribution is formed using gaussian_kde from the scipy package.
  Kernel density estimation is a way to estimate the probability density function (PDF) of a random variable in a non-parametric way. 
  gaussian_kde works for both uni-variate and multi-variate data. It includes automatic bandwidth determination.
  For our implementation of the gaussian_kde, the kernel is formed by giving the function of all the grid locations that are urban (>50% census blocks that are urban means the grid location is urban).
  In this way, the probability of landing on an urban census block is higher than that of landing on a rural census block. 
  Additionally, the probability distribution starts at a uniform value equal to the mean of the probabilities for urban and rural census blocks, and then the probability distribution is added to this, so as to increase the probability of choosing a random grid location that is rural. 
  - If `city_placement` is a string equal to 'grid_gaussian_kde', then a similar procedure as when `city_placement` was equal to 'grid_gaussian_kde' will take place to determine the probability distribution.
  The main difference is that the kernel function is constructed using the actual locations of all the census blocks in all grid locations. 
  This gives a more representative kernel distribution function, however, it will take longer to build the kernel. 

An example of a fully filled out command to run in the terminal would be the following:
```
run test_main.py ('mixed',3,4,5) diam intervals 50 '44' all (0,5) none 0.5 ('BVAP','HVAP','WVAP','VAP') (1,1,0) (1,1) 0.8 1 0 1000 random
```
Here is a description of each variable in the above command:
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

# Images 
The files in images are examples of instances run by text_main.py. The description of each is recorded based on the same variables and order as listed above. 
