# Random State Generator
This repository holds both the code (the code that is allowed to be released due to limitations on completeness and publication rules) as well as the images produced by the code. 

## Goal
The purpose of this code is to produce randomly generated states built from the census blocks of 48 of the 50 U.S. states (Alaska and Hawaii are removed due to contiguity complications). 
The output of the code will be a dual graph where each vertex is a census block, edges represent adjacent census blocks and each census block contains the following columns of information:
- `BLOCKID10`:
- `geoid`:
- `BLOCKCE`:
- `block`:
- `STATEFP10`:
- `state`:
- `COUNTYFP10`:
- `county`:
- `TRACTCE10`:
- `tract`:
- `PARTFLG`:
- `HOUSING10`:
- `C_X`:
- `C_Y`:
- `NAME`:
- `NAME10`:
- `NAMELSAD10`:
- `UR10`:
- `area`:
- `boundary_perim`:
- `boundary_node`:
- `POP10`:
- `TOTPOP`:
- `NH_WHITE`:
- `NH_BLACK`:
- `NH_2MORE`:
- `NH_AMIN`:
- `NH_ASIAN`:
- `NH_NHPI`:
- `NH_OTHER`:
- `HISP`:
- `H_2MORE`:
- `H_AMIN`:
- `H_ASIAN`:
- `H_BLACK`:
- `H_NHPI`:
- `H_OTHER`:
- `H_WHITE`:
- `VAP`:
- `WVAP`:
- `BVAP`:
- `HVAP`:
- `AMINVAP`:
- `ASIANVAP`:
- `NHPIVAP`:
- `2MOREVAP`:
- `OTHERVAP`:

## How To Run Code

To run the code, change working directory to the file that contains "test_main.py". Then run the following command with the appropriate variables filled in (descriptions below):
```
run test_main.py grid_placement sampling_method merge_method sample_num state pop_category num_cities city_nums mod mod_prob demo_cols sampling_parameters
```
Here is a description of each variable in the above script:
- `grid_placement`: A tuple of length 2, 3, or 4. 
The grid corresponds to how each sample of `sample_num` census blocks is placed in the countryside.
The first element of each tuple describes the method used to build the countryside and possible the cities. 
The method options are 'random', 'fixed', and 'mixed' and must be given as strings. 
The remaining elements in each tuple are positive integers.  For the tuple lengths, the method corresponds to the length of the tuple. 
That is, the length 2, 3, and 4 tuples have the first element as 'random', 'fixed', and 'mixed' respectively. 
  - When the method is set to 'random', the first sample of census blocks is placed at grid location (0,0) (x,y coordinate locations). Then all of the subsequent samples are placed iteratively around what has been constructed so far for the grid. So for the second sample, the possible locations would be (0,1), (1,0), (-1,0), and (0,-1). 
  - When `grid_placement` is set to ('fixed',m,n), a rectangular m by n grid is made using mn samples. The first sample is placed in location (0,0) and the rectangle is constructed with corners (0,0), (a-1,0), (b-1,0), and (a-1,b-1). 
  - When `grid_placement` is set to ('mixed',m,n,p), a rectangular m by n grid is made using mn samples like when the method was 'fixed'. Then p additional samples of census blocks are added iteratively to the boundary of the rectangle using the 'random' method. In total, there will be mnp samples.
- `sampling_method`: A string which represents how each sample is built. The options for this string are 'rect' and 'diam'. 
  - For 'rect', a random census block is chosen (urban or rural depends which part of the code this variable is executed). 
  Then a window is drawn around one census block. 
  Then at each iteration, one side of the rectangular window is expanded by 0.1 degrees (between 6.8 and 7 miles in terms of distance in latitudinal and longitudinal directions). 
  The window will stop growing once it reaches at least `sample_num` census blocks in the window. 
  - For 'diam', a random census block, call it v, is chosen (urban or rural depends which part of the code this variable is executed).
  Then the vertices distance 1 (graph theoretic distance) from v are added to the sample. 
  If the number of vertices in this subgraph is less than `sample_num`, then add all of the vertices distance 2 from v to the sample. 
  Once there are at least `sample_num` vertices in the sample, vertices are deleted uniformly at random from the list of vertices furtherest away from v in our sample until only `sample_num` vertices remain. 
- `merge_method`: A string which represents how a sample is added to a grid of already merged samples. The options for this string are 'intervals' and 'intersect'. 
  - For 'intervals', the maximum and minimum values in the x- and y-directions are found. 
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
- `sample_num`: two digit string that represents the approximate number of census blocks in each sample taken. 
- `state`: two digit number that represents the U.S. from which data is sampled. 
The possible two-digit value and their corresponding states are 
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
- `pop_category`: 
- `num_cities`: 
- `city_nums`: 
- `mod`: 
- `mod_prob`: 
- `demo_cols`: 
- `sampling_parameters`: 

An example fo fully filled out command to run in the terminal would be the following:
```
run test_main.py ('mixed',3,4,5) diam intervals 50 '44' all 5 4 none 0.5 ('BVAP','HVAP','WVAP','VAP') (1,1,0)
```
Here is a description of each variable in the above command:
- `grid_placement` is set to 'mixed' which means that it is a mixture of the 'random' `grid_placement` method and 'fixed' `grid_placement` method when constructing the countryside (mostly rural census blocks). 
The '3' and '4' represent the length and width of the rectangle built using the 'fixed' method that will be constructed using samples of size `sample_num` respectively. The '5' represents the number of additional samples of census blocks added iteratively to the boundary of the rectangle using the 'random' method.
- `sampling_method` is set to 'diam' for diameter.
- `merge_method` is set to 'intervals'.
- `sample_num` is set to 50. That is, the number of census blocks chosen in each sample is approximately 50.
- `state` is set to '44' which is Rhode Island.
- `pop_category` is set to 'all'. This means that all possible population sizes are allowed when choosing census blocks to build the cities.
- `num_cities` is set to 5.
- `city_nums` is set to 4. That is, the code will build 4 cities.
- `mod` is set to none. That is, no modification will be made to the edges/vertices in each sample.
- `mod_prob` is set to 0.5. That is, there is a probability of 0.5 that a modification will take place for the edges/vertices according to the `mod` chosen. 
- `demo_cols` is set to ('BVAP','HVAP','WVAP','VAP'). That is, the only demographic columns that will be randomly modified (if at all) are Black, non-hispanic, voting age population in 2010 Census (BVAP), Hispanic voting age population in 2010 Census (HVA) White, non-hispanic, voting age population in 2010 Census (WVAP), and voting age population in 2010 Census (VAP). 
- `sampling_parameters` is set to (1,1,0). That is, the total population and the demographics will have noise added to the data in each census block randomly. 
