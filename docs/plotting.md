# Visualizing Surfaces

Here we present various ways to visualize a DREEMS migration surface. All you need is a `surface` object returned from `dreems_infer`. Please see the tutorials for further information.

We will be using the fishes data.

## Interactive Maps
Given a `surface` object, one can create an interactive map that we will save in a file called fishes.html.

```python
draw_interactive_map(surface, "./fishes.html")
```

<iframe
    src="./images/fishes.html"
    width="100%"
    height="700"
    style="border: 1px solid #ccc; border-radius: 6px;"
    loading="lazy">
</iframe>