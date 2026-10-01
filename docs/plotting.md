# Visualizing Surfaces

Here we present various ways to visualize a DREEMS migration surface. All you need is a `surface` object returned from `dreems_infer`. Please see the tutorials for further information.

We will be using the fishes data.

## Interactive maps
Given a `surface` object, one can create an interactive map that we will save in a file called `fishes.html`.

```python
draw_interactive_map(surface, "./fishes.html")
```

<iframe
    src="_static/fishes.html"
    width="100%"
    height="700"
    style="border: 1px solid #ccc; border-radius: 6px;"
    loading="lazy">
</iframe>

Using this interactive map, you can scroll around and see street names, river systems, natural parks, and landscapes. Blue coloring on the grid represent higher relative effective migration and red is for lower relative effective migration. The sampled nodes are depcited with white circles and sizes are proportional to the number of samples on the node. You can also change the map by hovering your cursor over to the top right corner where you have the option for "OpenStreetMap" and "Colored terrain". The terrain option will depict the map in terms of elevation.

## Grid maps
One can also create a grid map on contextily maps to `fishes.pdf`. The object `pop_map` is just a dictionary that maps a sample to its population label. This parameter is completely optional and will assign colors to associated populations, the default is to plot every sample uniformly. 

```python
draw_projected_grid_map(
    surface,
    sample_to_pop=pop_map,
    save_figure=True,
    figure_path = "./fishes.pdf"
)
```

<iframe
    src="_static/fishes.pdf"
    width="100%"
    height="700px"
    style="border: none;">
</iframe>

## Contour maps
One can also plot contour maps to `fishes_contour.pdf`

```python
draw_projected_contour_map(
    surface,
    sample_to_pop=pop_map,
    smoothing_km=5.0,
    contour_fill_alpha = 0.30,
    contour_line_alpha=1.0,
    contour_levels=15,
    map_margin_km=75.0,
    posterior=False,
    save_figure=True,
    figure_path = "./fishes_contour.pdf"
)
```

<iframe
    src="_static/fishes_contour.pdf"
    width="100%"
    height="700px"
    style="border: none;">
</iframe>