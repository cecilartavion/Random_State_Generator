# Random State Generator
This repository holds both the code (the code that is allowed to be released due to limitations on completeness and publication rules) as well as the images produced by the code. 

## Goal
The purpose of this code is to produce randomly generated states built from the census blocks of 48 of the 50 U.S. states (Alaska and Hawaii are removed due to contiguity complications). 

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
- `sampling_method`: 
- `merge_method`: 
- `sample_num`: 
- `state`: 
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
