### Transcriptional AML subtype predictor
This is a SVM based model to predict transcriptional AML subtypes.

### Create input file
To use this R package, you need to use STAR (we used version: 2.7.5c--0) to allign and quantify your counts. As Index and GTF use the following files: [GDC.h38.d1.vd1 STAR2 Index Files (v36)](https://api.gdc.cancer.gov/data/c0008693-0583-4eac-bd5c-583070763893) & [GDC.h38 GENCODE v36 GTF](https://api.gdc.cancer.gov/data/be002a2c-3b27-43f3-9e0f-fd47db92a6b5). We also include an example snakemake file to see the options used.

Merge the _ReadsPerGene.out.tab_ files into a matrix with samples on the rows and the gene counts on the columns. Make sure you select the right count column according to strandness. For your R matrix, set sample-ids as rownames, and ensembl-ids (column 1 of _ReadsPerGene.out.tab_) as colnames.
### Use package
First install the package.
```R
library(devtools)
install_github("jeppeseverens/AMLmapR")
```

Then you can predict the transcriptional subtypes for your AML cases. __Important__: do not normalise or log transform counts. 
```R
library(AMLmapR)
predictions <- predict_AML_clusters(matrix)
```
### How to cite
If you used this work for a publication, please reference: "paper reference" 

### Contact
Jeppe Severens - PhD student at Leiden University Medical Center, the Netherlands
Mail: jeppe.severens@gmail.com
