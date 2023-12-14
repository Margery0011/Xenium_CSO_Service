# 10x Genomics Xenium In Situ CSO Service

### Where to find files

- To be decided

### Background: workflow overview

The Xenium workflow begins with sample preparation. Fresh frozen (FF) or formalin-fixed paraffin-embedded (FFPE) samples are mounted on Xenium slides. The samples are fixed and permeabilized (FF samples) or deparaffinized and decrosslinked (FFPE samples). Then, probe hybridization, ligation, and rolling circle amplification (RCA) are performed.

Once the sample has been prepared, imaging is performed in cycles on the **Xenium Analyzer**. During each cycle, fluorescently labeled probes for detecting RNA target sequences and other reagents (see [Controls](https://www.10xgenomics.com/support/in-situ-gene-expression/documentation/steps/onboard-analysis/xenium-algorithms-overview#qvs)), are automatically cycled in, imaged, and removed. The internal image sensor captures data across multiple Z-planes (with a 0.75 μm step size across the entire tissue thickness) for every field of view (FOV) in the user-selected region (see Region Selection Guidelines in the [Xenium Analyzer instrument user guide](https://www.10xgenomics.com/support/in-situ-gene-expression/documentation/instruments/xenium-analyzer/xenium-analyzer-user-guide)). Image data are captured for multiple fluorescence channels every cycle, and are processed and stitched to build a spatial map of the transcripts across the tissue section.


### Understand Xenium Outputs

[CSO Xenium Outputs Explanations](https://github.com/Margery0011/Xenium_CSO_Service/blob/main/Xenium_output.md)

## Downstream Analysis

### R

[**Seurat**: Analysis of Image-based Spatial Data in Seurat](https://satijalab.org/seurat/articles/spatial_vignette_2)

```
# Load the Xenium data
xenium.obj <- LoadXenium(path, fov = "fov")
```

### Python

[**Squidpy**: Analyze Xenium data](https://squidpy.readthedocs.io/en/stable/notebooks/tutorials/tutorial_xenium.html)

```
# Load Xenium data
adata = sc.read_10x_h5(
    filename="xenium_data/Xenium_FFPE_Human_Breast_Cancer_Rep1_cell_feature_matrix.h5"
)
```
[**stLearn**: Xenium data analysis with spatial trajectories inference](https://stlearn.readthedocs.io/en/latest/tutorials/Xenium_PSTS.html)

```
# Load Xenium data
adata = st.ReadXenium(feature_cell_matrix_file="Xenium_FFPE_Human_Breast_Cancer_Rep1_cell_feature_matrix.h5",
                     cell_summary_file="Xenium_FFPE_Human_Breast_Cancer_Rep1_cells.csv.gz",
                     library_id="Xenium_FFPE_Human_Breast_Cancer_Rep1",
                     image_path="CS1384_post-CS0_H&E_S1A_RGB-shlee-crop.png",
                     scale=1,
                     spot_diameter_fullres=15 # Recommend
                     )
```