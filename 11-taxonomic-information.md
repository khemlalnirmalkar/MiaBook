# Taxonomic information {#taxonomic-information}

<script>
document.addEventListener("click", function (event) {
    if (event.target.classList.contains("rebook-collapse")) {
        event.target.classList.toggle("active");
        var content = event.target.nextElementSibling;
        if (content.style.display === "block") {
            content.style.display = "none";
        } else {
            content.style.display = "block";
        }
    }
})
</script>

<style>
.rebook-collapse {
  background-color: #eee;
  color: #444;
  cursor: pointer;
  padding: 18px;
  width: 100%;
  border: none;
  text-align: left;
  outline: none;
  font-size: 15px;
}

.rebook-content {
  padding: 0 18px;
  display: none;
  overflow: hidden;
  background-color: #f1f1f1;
}
</style>


```r
library(mia)
data("GlobalPatterns")
se <- GlobalPatterns 
```

## Data agglomeration {#data-agglomeration}

Agglomerate at a specific taxonomic rank. If multiple assays (counts and relabundance) exists, both will be agglomerated.


```r
altExp(se, "Family") <- agglomerateByRank(se, rank = "Family",
                                          agglomerateTree = TRUE)

se
```

```
## class: TreeSummarizedExperiment 
## dim: 19216 26 
## metadata(0):
## assays(1): counts
## rownames(19216): 549322 522457 ... 200359 271582
## rowData names(7): Kingdom Phylum ... Genus Species
## colnames(26): CL3 CC1 ... Even2 Even3
## colData names(7): X.SampleID Primer ... SampleType Description
## reducedDimNames(0):
## altExpNames(1): Family
## rowLinks: a LinkDataFrame (19216 rows)
## rowTree: a phylo (19216 leaves)
## colLinks: NULL
## colTree: NULL
```

```r
altExp(se, "Family")
```

```
## class: TreeSummarizedExperiment 
## dim: 603 26 
## metadata(0):
## assays(1): counts
## rownames(603): Class::Thermoprotei Family:Sulfolobaceae ...
##   Family:Thermodesulfobiaceae Phylum::SR1
## rowData names(7): Kingdom Phylum ... Genus Species
## colnames(26): CL3 CC1 ... Even2 Even3
## colData names(7): X.SampleID Primer ... SampleType Description
## reducedDimNames(0):
## altExpNames(0):
## rowLinks: a LinkDataFrame (603 rows)
## rowTree: a phylo (603 leaves)
## colLinks: NULL
## colTree: NULL
```


```r
altExp(se, "Family") <- relAbundanceCounts(altExp(se, "Family"))
assay(altExp(se, "Family"), "relabundance")[1:5,1:7]
```

```
##                            CL3       CC1 SV1 M31Fcsw M11Fcsw M31Plmr   M11Plmr
## Class::Thermoprotei  0.0000000 0.000e+00   0       0       0       0 0.000e+00
## Family:Sulfolobaceae 0.0000000 0.000e+00   0       0       0       0 2.305e-06
## Class::Sd-NA         0.0000000 0.000e+00   0       0       0       0 0.000e+00
## Order::NRP-J         0.0001991 2.070e-04   0       0       0       0 6.914e-06
## Family:SAGMA-X       0.0000000 6.165e-06   0       0       0       0 0.000e+00
```
  

```r
assay(altExp(se, "Family"), "counts")[1:5,1:7]
```

```
##                      CL3 CC1 SV1 M31Fcsw M11Fcsw M31Plmr M11Plmr
## Class::Thermoprotei    0   0   0       0       0       0       0
## Family:Sulfolobaceae   0   0   0       0       0       0       1
## Class::Sd-NA           0   0   0       0       0       0       0
## Order::NRP-J         172 235   0       0       0       0       3
## Family:SAGMA-X         0   7   0       0       0       0       0
```

These newly created data can also be kept in `altExp` a feature provided by 
`SingleCellExperiment`.    

`altExpNames` now consists of `family` level data. This can be extended to use 
any level present in Kingdom, Phylum, Class, Order, Family, Genus, Species.   

## Get unique  

Get which Phyla are present.  

```r
head(unique(rowData(se)[,"Phylum"]))
```

```
## [1] "Crenarchaeota"  "Euryarchaeota"  "Actinobacteria" "Spirochaetes"  
## [5] "MVP-15"         "Proteobacteria"
```

## Pick specific  

Retrieving of specific elements are required for specific analysis. For
instance, extracting abundances for a specific taxa in all samples or all taxa 
in one sample.  

### Abundances of all taxa in specific sample 

```r
taxa.abund.cc1 <- getAbundanceSample(se, 
                                     sample_id = "CC1",
                                     abund_values = "counts")

taxa.abund.cc1[1:10]
```

```
## 549322 522457    951 244423 586076 246140 143239 244960 255340 144887 
##      0      0      0      0      0      0      1      0    194      5
```

### Abundances of specific taxa in all samples   


```r
taxa.abundances <- getAbundanceFeature(se, 
                                      feature_id = "255340",
                                       abund_values = "counts")
taxa.abundances[1:10]
```

```
##     CL3     CC1     SV1 M31Fcsw M11Fcsw M31Plmr M11Plmr F21Plmr M31Tong M11Tong 
##     153     194       0       0       0       0       0       0       0       0
```


## Session Info {-}

<button class="rebook-collapse">View session info</button>
<div class="rebook-content">
```
R version 4.0.3 (2020-10-10)
Platform: x86_64-pc-linux-gnu (64-bit)
Running under: Ubuntu 20.04 LTS

Matrix products: default
BLAS/LAPACK: /usr/lib/x86_64-linux-gnu/openblas-pthread/libopenblasp-r0.3.8.so

locale:
 [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
 [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
 [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=C             
 [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
 [9] LC_ADDRESS=C               LC_TELEPHONE=C            
[11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       

attached base packages:
[1] parallel  stats4    stats     graphics  grDevices utils     datasets 
[8] methods   base     

other attached packages:
 [1] mia_0.0.0.9009                   MicrobiomeExperiment_0.99.0.9014
 [3] Biostrings_2.58.0                XVector_0.30.0                  
 [5] TreeSummarizedExperiment_1.6.0   SingleCellExperiment_1.12.0     
 [7] SummarizedExperiment_1.20.0      Biobase_2.50.0                  
 [9] GenomicRanges_1.42.0             GenomeInfoDb_1.26.1             
[11] IRanges_2.24.0                   S4Vectors_0.28.0                
[13] BiocGenerics_0.36.0              MatrixGenerics_1.2.0            
[15] matrixStats_0.57.0               BiocStyle_2.18.1                
[17] rebook_1.0.0                     BiocManager_1.30.10             

loaded via a namespace (and not attached):
 [1] tidyselect_1.1.0          xfun_0.19                
 [3] beachmat_2.6.2            purrr_0.3.4              
 [5] lattice_0.20-41           vctrs_0.3.5              
 [7] generics_0.1.0            htmltools_0.5.0          
 [9] yaml_2.2.1                XML_3.99-0.5             
[11] rlang_0.4.9               pillar_1.4.7             
[13] scuttle_1.0.3             glue_1.4.2               
[15] BiocParallel_1.24.1       CodeDepends_0.6.5        
[17] GenomeInfoDbData_1.2.4    lifecycle_0.2.0          
[19] stringr_1.4.0             zlibbioc_1.36.0          
[21] codetools_0.2-18          evaluate_0.14            
[23] knitr_1.30                callr_3.5.1              
[25] ps_1.4.0                  Rcpp_1.0.5               
[27] DelayedArray_0.16.0       graph_1.68.0             
[29] digest_0.6.27             stringi_1.5.3            
[31] bookdown_0.21             processx_3.4.5           
[33] dplyr_1.0.2               grid_4.0.3               
[35] tools_4.0.3               bitops_1.0-6             
[37] magrittr_2.0.1            RCurl_1.98-1.2           
[39] tibble_3.0.4              tidyr_1.1.2              
[41] pkgconfig_2.0.3           crayon_1.3.4             
[43] ape_5.4-1                 ellipsis_0.3.1           
[45] Matrix_1.2-18             DelayedMatrixStats_1.12.1
[47] sparseMatrixStats_1.2.0   rmarkdown_2.5            
[49] R6_2.5.0                  nlme_3.1-150             
[51] compiler_4.0.3           
```
</div>