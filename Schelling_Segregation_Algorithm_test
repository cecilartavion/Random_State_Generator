Schelling invokes the modified Schelling segregation algorithm. In the original
formulation of the Schelling segregation algorithm, both groups were moved 
around the space. However, for this formulation, all rural census blocks are 
fixed as it would make little sense to move them from their current location.
The similarity ratio (r_{sim}) is the ratio of neighbors of the same group as 
the agent. Let the similarlity threshold be t. If an agent is satisfied (i.e.
r_{sim}>=t), then the agent will do nothing. Otherwise, the agent will randomly
move to an unoccupied location. 

The files in this folder allow one to test the various parameters of the Schelling Segregation Algorithm (SSA). The parameters of the algorithm are as follows.
- `st`: This is a string that represents the state on which we are running the SSA. 
- `ratio`: this is the similarity threshold for the similarity ratio.
This value will take on a value between 0 and 1. 
The variable ratio is a measure of how intolerant an agent is towards the other groups. 
If the similarity ratio is greater than ratio, then the specified grid location is kept.
- `iterations`: This is the number of iterations that the Schelling segregation algorithm is run. 
- `nbr_dist`: This is the distance used when calculating the similarity ratio for the agent.
- `no_isolates`: this is a True/False variable. 
If it is True, then all isolated (in the graph theoretical sense) grid locations that are designated as urban (with over 50% urban census blocks) will be removed from the urban_cb list which is the list of grid locations.
- `fixed_urban_gl`: this variable is the number of urban census blocks that are randomly generated on the countryside. 
