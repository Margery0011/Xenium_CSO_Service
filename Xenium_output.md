# Understanding Xenium Outputs

## Overview

| File type                       | File and description                                                                                           | Read in Xenium Explorer |
|---------------------------------|-----------------------------------------------------------------------------------------------------------------|-------------------------|
| **Experiment file**             | experiment.xenium: Experiment manifest file                                                                    | Required                |
| **Interactive summary**         | analysis_summary.html: Summary metrics, graphs, and images to QC your run data in HTML format                   |                         |
| **Image files**                 | morphology.ome.tif: The nuclei-stained (DAPI) morphology image in OME-TIFF format                               |                         |
|                                 | morphology_focus.ome.tif: Single best-focus Z-plane of the morphology image                                     |                         |
|                                 | morphology_mip.ome.tif: Maximum intensity projection (mip) of the morphology image                             |                         |
| **Cell summary**                | cells.csv.gz: Cell summary file                                                                                 |                         |
|                                 | cells.parquet: Cell summary file in Parquet format                                                              |                         |
| **Cell segmentation masks and polygons** | cells.zarr.zip: Cell summary file in zipped Zarr format, only file that contains the nuclei and cell segmentation masks and boundaries used for transcript assignment |                         |
| **Cell boundary polygons**      | cell_boundaries.csv.gz: Cell boundary file                                                                      |                         |
|                                 | cell_boundaries.parquet: Cell boundary file in Parquet format                                                   |                         |
| **Nucleus boundary polygons**   | nucleus_boundaries.csv.gz: Nucleus boundary file                                                                |                         |
|                                 | nucleus_boundaries.parquet: Nucleus boundary file in Parquet format                                             |                         |
| **Transcript data**             | transcripts.csv.gz: Transcripts data file                                                                       |                         |
|                                 | transcripts.parquet: Transcripts data in Parquet format                                                         |                         |
|                                 | transcripts.zarr.zip: Transcript data in zipped Zarr format                                                     |                         |
| **Cell-feature matrix**         | cell_feature_matrix/: Cell-feature matrix file in Market Exchange format                                       |                         |
|                                 | cell_feature_matrix.h5: Cell-feature matrix file in HDF5 format                                                |                         |
|                                 | cell_feature_matrix.zarr.zip: Cell-feature matrix file in zipped Zarr format                                   |                         |
| **Metric summary**              | metrics_summary.csv: Summary of key metrics                                                                    |                         |
| **Secondary analysis**          | analysis/: Directory of secondary analysis results                                                             |                         |
|                                 | analysis.zarr.zip: Secondary analysis outputs in zipped Zarr format                                            |                         |
| **Gene panel**                  | gene_panel.json: Copy of input gene panel file                                                                 |                         |
| **Auxiliary data (aux_outputs/)** | morphology_fov_locations.json: Field of view (FOV) name and position information (in microns)                   |                         |
|                                 | overview_scan_fov_locations.json: FOV name and position information (in pixels)                                |                         |
|                                 | per_cycle_channel_images/: A folder containing downsampled RNA image files in TIFF format from each cycle and channel |                         |
|                                 | overview_scan.png: Full resolution image of entire slide sample                                                |                         |

### Analysis summary

The Xenium onboard analysis pipeline outputs an interactive HTML file named `analysis_summary.html`. Open it in a web browser or Xenium Explorer. It contains summary metrics and automated secondary analysis results. Any alerts issued by the pipeline are displayed at the top of the page.

There are four clickable tabs that capture different information:

- The **Summary tab** contains summary metrics, images, and experiment information for a quick overview of the data.

- The **Decoding tab** contains more specific transcript decoding metrics.

- The **Cell Segmentation tab** shows the metrics for cell segmentation and partitioning transcripts into single cells.

- The **Analysis tab** captures the results from the pipeline's secondary analysis run on single cell data.


You could also click the `? `at the top of each dashboard for more information about each metric. For detailed descriptions and guidance on interpretation, see the Overview of the Xenium Analysis Summary documentation.

*Notes:* For more information about the Xenium Analysis Summary, check the [10x genomics website](https://www.10xgenomics.com/support/in-situ-gene-expression/documentation/steps/onboard-analysis/xenium-analysis-summary) 

### Summary tab

The **Key Metrics** panel is the starting point to QC your data and to spot outliers among multiple runs at a glance. These metrics are often helpful indicators if there is a problem with the data.

![Alt text](<Screenshot 2023-12-13 at 10.06.10 AM.png>)

There are no universal thresholds for these metrics as interpretation requires some understanding of the sample and gene panel used. Researchers should have some understanding of how many cells are expected given the tissue size and type. Here are some important considerations for interpreting key metrics:


- **Median transcripts per cell** and **decoded transcript density** will depend on tissue type and the selected gene panel (e.g., a 300-gene panel will have more transcripts than a 50-gene panel).

- **The number of cells** detected will depend on tissue type and the size of the region of interest (ROI) selected on the instrument in the analysis.

### Decoding tab

The Xenium codebook contains a collection of codewords that are assigned to genes in a **gene panel**. The pipeline uses the `gene_panel.json` to specify a given gene name to an indexed codeword. Each codeword is defined based on a pattern of fluorescent signal intensities recorded across channels and cycles. Some codewords are reserved for negative controls. 

![Alt text](<Screenshot 2023-12-13 at 10.13.56 AM.png>)

The Decoding Yield section provides QC metrics at a glance. Biological and experimental context may be needed for interpretation.

### Cell Segmentation tab


![Alt text](<Screenshot 2023-12-13 at 10.31.04 AM.png>)


The purpose of cell segmentation is to approximate cell boundaries so that transcripts can be assigned to cells. Downstream, these results will be used to produce a cell-feature matrix, similar to those output by existing single cell and spatial technologies. 

### Analysis tab

The **Transcripts Per Cell** view shows the spatial distribution (left) and UMAP projection (right) of cells colored by the total number of transcripts detected in each cell. 

![Alt text](<Screenshot 2023-12-13 at 10.33.44 AM.png>)

The **Clustering** view shows the spatial distribution (left) and UMAP projection (right) of cells colored by cluster assignment using the Onboard Analysis pipeline's automated clustering algorithm. The clusters should reflect groups of cells that have similar expression profiles. Only cells with a nucleus detected by the DAPI stain are used in the clustering algorithm.

![Alt text](<Screenshot 2023-12-13 at 10.33.50 AM.png>)

The **Top Features by Cluster table** displays results from the Onboard Analysis pipeline's automated differential expression analysis. For each cluster, the table shows features that are more highly expressed in that cluster relative to the rest of the sample.

![Alt text](<Screenshot 2023-12-13 at 10.33.56 AM.png>)