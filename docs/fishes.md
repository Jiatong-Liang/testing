# Blacklegged Ticks

In this example we will examine spatial SNP data for the Allohistium fishes, to analyze population gene flow and connectivity across river systems. For more details regarding the study please visit:

Daniel J MacGuigan, Adam Taylor, Ava Ghezelayagh, Julia E Wood, Jeffrey W Simmons, Jon M Mollish, Thomas J Near, Genomic and Phenotypic Delimitation of Species in a Temperate Aquatic Biodiversity Hotspot, Systematic Biology, Volume 75, Issue 5, September 2026, Pages 911–929, [https://doi.org/10.1093/sysbio/syaf083.](https://academic.oup.com/sysbio/article/75/5/911/8346017)

Please download files listed under the name 'VCFs.tar' and 'FEEMS.tar' using [this link](https://datadryad.org/dataset/doi:10.5061/dryad.r4xgxd2q4#readme). We will be using the files 'Allo.coord.txt', 'Allo.boundary.txt', and 'Allohistium.m95p.unlinked.vcf'. The files are a VCF that has pruned, linkage disequilibrium (LD)-controlled variants, a text file with sample coordinates, and a text file with coordinates of an outer polygon.


Using the previous code, we have filtered for SNPs with a call rate of greater than 0.8, imputed the missing data using the mean, and applied a MAF filtering of 0.05. These preprocessing steps are crucial for migration surface inference. 

## Convert a compressed VCF file into a Zarr file

```python
import pysam
from sgkit.io.vcf import vcf_to_zarr

vcf_gz = "03.pruned.vcf.gz"

# Index the bgzipped VCF file
pysam.tabix_index(vcf_gz, preset="vcf", force=True)

# Convert to zarr file
vcf_to_zarr(vcf_gz, "./ticks.zarr")
```

## Load in DREEMS input
There are three mandatory inputs to DREEMS, a Zarr file, the sample coordinates, and the grid. If you do not have a grid, please see the data preprocessing tutorial on how to construct a grid around your geographic region of interest. 

```python
import xarray as xr

# load in Zarr file
ds = xr.open_zarr("./ticks.zarr")
outer = np.loadtxt("polygon_outer.txt") # outer polygon for constructing grid
df = pd.read_excel("Metadata.xlsx")

# you need to create a dictionary mapping sample names to their coordinates.
# The authors of this dataset have conveniently ordered the coordinates
# to match all sample_names
sample_coordinates = {
    row["id"]: (row["lat"], row["long"])
    for _, row in df.iterrows()
}
```

## Running DREEMS
```python
from dreems.utility import dreems_infer

surface = dreems_infer(data=ds, sample_coordinates=sample_coordinates, nodes=nodes, edges=edges)
```

## Plotting surface
```python
from dreems.plotting import draw_projected_contour_map
draw_projected_contour_map(
    surface,
    sample_to_pop=pop_map,
    smoothing_sigma=5,
    contour_levels=15,
    contour_linewidth=2.0,
    contour_fill_alpha=0.20,
    save_figure=True,
    figure_path="./dreems_ticks_dense_relative_migration_map.pdf"
)
```
