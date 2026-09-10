# Blacklegged Ticks

In this example we will examine spatial SNP data for the blacklegged tick (Ixodes scapularis), to analyze population gene flow and connectivity across the Midwestern United States. For more details regarding the study please visit:

Dong, D.-y., S. M. Paskewitz, J. I. Tsao, and S. D. Schoville. 2025. “ Genetic and Landscape Connectivity of Blacklegged Ticks During Range Expansion in Select States of the Midwestern USA.” Ecology and Evolution 15, no. 10: e72360. [https://doi.org/10.1002/ece3.72360.](https://onlinelibrary.wiley.com/doi/10.1002/ece3.72360))

Please download three files listed under the name '03.pruned.vcf.gz', 'coord_03', and 'polygon_outer' using ['this link'](https://datadryad.org/dataset/doi:10.5061/dryad.c866t1gh7#readme). The files are a VCF that has pruned, linkage disequilibrium (LD)-controlled variants but has not yet been imputed for missing, a text file with sample coordinates, and a text file with coordinates of an outer polygon.

We start by opening the vcf and extracting the genotype matrix. The genotype matrix in this example is defined as the count of the minor allele. The blacklegged ticks are a diploid species, so the entries of the genotype matrix will take on values {0, 1, 2}

```python
import pysam
from sklearn.impute import SimpleImputer
import numpy as np
import pickle

# Load the VCF file
pysam.tabix_index("03.pruned.vcf.gz", preset="vcf")
vcf = pysam.VariantFile("03.pruned.vcf.gz")

# get genotype
sample_names = list(vcf.header.samples)

# Matrix shape: [num_snps][num_samples]
matrix = []

for record in vcf:
    row = []
    for sample in sample_names:
        gt = record.samples[sample]["GT"]

        if gt is None or None in gt:
            row.append(None)  # Missing genotype (./.)
        else:
            row.append(gt.count(1))  # Count of 1s

    matrix.append(row)

# make the dimensions [num_samples, num_snps]
tmp = np.array(matrix).T

# Call rate must be > 0.8
missing_fraction = np.mean(tmp == None, axis=0) 
keep = missing_fraction < 0.2
cleaned_arr = tmp[:, keep]

# imputing using the mean
imp = SimpleImputer(missing_values=np.nan, strategy="mean")
cleaned_arr = imp.fit_transform(cleaned_arr)

# MAF filtering using 0.05
prop_ones = (clean_arr == 1).mean(axis=0)
keep = (prop_ones >= 0.05) & (prop_ones <= 0.95)
genotypes = clean_arr[:, keep]

with open("ticks_genotype.pkl", "wb") as f:
    pickle.dump(genotypes, f)
```

Using the previous code, we have filtered for SNPs with a call rate of greater than 0.8, imputed the missing data using the mean, and applied a MAF filtering of 0.05. These preprocessing steps are crucial for migration surface inference. 




