# Data preprocessing

# Grid creation
The input for DREEMS requires a user-defined grid over the geographic region. We recommend using a triangular lattice grid. If the user already has a pre-defined grid, then they can pass in a `grid` variable that stores every node coordinate as (longitude, latitude) pairs. For users that do not have an already constructured grid, here's how you can construct one. Please click on [this link](https://www.birdtheme.org/useful/v3tool.html) that will open up a map.

On the map, you can scroll around and zoom in on your region of interest. For example, suppose you are interested in the region around the University of Michigan (Go Blue!). You navigate to Michigan, then click around the region of interest, and form an outer polygon around Ann Arbor. The coordinates of every point will be on the right hand side.

![google maps](./images/google_maps.png)

