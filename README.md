
# multisc

<!-- badges: start -->
<!-- badges: end -->

MultiSC is an R package for sample-level pseudotime construction based on multi-sample scRNA-seq data. Users need to provide the raw gene expression count matrix, sample-level metadata, cell-level metadata, as well as hierarchical tree structure of cell clusters (output from TreeCorTreat) as input to run the pipeline. 

Users have two options to construct sample pseudotime.

* semi-supervised approach based on canonical correlation analysis (CCA). MultiSC would generate the optimal direction where the sample projection has largest correlation with sample phenotype (e.g., severity level).

* unsupervised approach based on TSCAN. MultiSC would incorporate TSCAN to construct sample-level pseudotime without knowing sample phenotype. The optimal pseudotime is selected based on pseudo R-squared that we define to indicate the proportion of variation that could be explained by the pseudotime backbone. 

## Installation

You can install the development version of multisc like so:

``` r
devtools::install_github("stephlee3/multisc")
```



## Example

Here we show two examples to run MultiSC.

### Load input

``` r
library(multisc)
data("example_data")
raw_count = example_data$raw_count
sample_meta = example_data$sample_meta
cell_meta = example_data$cell_meta
leaves_info = example_data$leaves_info
batch = sample_meta$batch; names(batch) = sample_meta$sample
```

### Approach 1: Unsupervised TSCAN

The following code will create a new directory `result_tscan` in your current working directory to save the outputs. You should be able to find `pseudotime.csv` and `sample_embed_tscan` in the `result_tscan/pseudotime` folder. The pseudotime construction should be fast but the DEG analysis can be relatively slow if not paralleled. 


``` r
ptime_result_tscan = runPseudoAnalysis(raw_count = raw_count,
                                 cell_meta = cell_meta, 
                                 sample_meta = sample_meta,
                                 leaves_info = leaves_info,
                                 features = 'expr_and_prop', 
                                 batch = batch, 
                                 pb_filter_pct = 0.9, 
                                 pb_HVG = T, 
                                 pb_qnorm = T, 
                                 pb_batch_correction = F, 
                                 pb_parallel = F,
                                 num_cores = 10,
                                 ctp_clr = F, 
                                 ctp_adj_cov = NULL, 
                                 ctp_batch_correction = F, 
                                 sample_name = 'sample', 
                                 severity_name = 'severity', 
                                 severity_num_name = 'sev.level',
                                 ptime_method = 'tscan', 
                                 clustermethod = 'kmeans', 
                                 cluster_id = NULL, 
                                 reduce = F, 
                                 clusternum = 2, 
                                 startcluster = 1, 
                                 near_nbr_num = 3, 
                                 modelNames = 'VVV',
                                 pc_scale = T,
                                 eval_method = 'corr',
                                 pseudotime_name = 'pseudotime', 
                                 gene_mapping = F,
                                 deg_parallel = F,
                                 deg_num_cores = 10,
                                 save_deg = T, 
                                 sample_embed_plot = T, 
                                 result_path = './result_tscan/')
```



### Approach 2: Unsupervised TSCAN

Similar to Approach 1, the following code will create a new directory `result` in your current working directory to save the outputs. You should be able to find `pseudotime.csv` and `sample_embed_cca` in the `result/pseudotime` folder.

``` r
ptime_result = runPseudoAnalysis(raw_count = raw_count,
                                 cell_meta = cell_meta, 
                                 sample_meta = sample_meta,
                                 leaves_info = leaves_info,
                                 features = 'expr_and_prop', 
                                 batch = batch, 
                                 pb_filter_pct = 0.9, 
                                 pb_HVG = T, 
                                 pb_qnorm = T, 
                                 pb_batch_correction = F, 
                                 pb_parallel = F,
                                 num_cores = 10,
                                 ctp_clr = F, 
                                 ctp_adj_cov = NULL, 
                                 ctp_batch_correction = F, 
                                 sample_name = 'sample', 
                                 severity_name = 'severity', 
                                 severity_num_name = 'sev.level',
                                 ptime_method = 'cca', 
                                 clustermethod = 'kmeans', 
                                 cluster_id = NULL, 
                                 reduce = F, 
                                 clusternum = 2, 
                                 startcluster = 1, 
                                 near_nbr_num = 3, 
                                 modelNames = 'VVV',
                                 pc_scale = T,
                                 eval_method = 'corr',
                                 pseudotime_name = 'pseudotime', 
                                 gene_mapping = F,
                                 deg_parallel = F,
                                 deg_num_cores = 10,
                                 save_deg = T, 
                                 sample_embed_plot = T, 
                                 result_path = './result/')
```

