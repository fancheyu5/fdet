# fdet
An R package to find differential expressed topologically associating domains (TADs) from RNA-seq result and plot


1.Install

Download fdep package by

    git clone https://github.com/fancheyu5/fdet.git

R package devtools and ggplot2 should be pre-installed. If not, use

    install.packages("devtools")
    install.packages("ggplot2")

Install the package by 

    devtools::install_local('fdet_0.1.1.tar.gz') #path to fdet_0.1.1.tar.gz

2.Prepare input data

This is a down-stream tool of Differential Gene Expression (DGE) analysis like DESeq2 or limma. 
The input data.frame should contain the following columns without NAs:
    log2FoldChange   chr   start   end   gene_name
    *chromosome number in “chr” column should not contain string “chr”
    *gene_name is the label plotted in figure.

3.Find differential expressed TADs

    library(fdet)
    res <- search_genome(DGE) #DGE is the input data.frame
    res
    
The res has the following columns:

    WindowNo       Window ID. Could be used in plot_window and plot_window_with_TADs functions.

    window_5pos    5’ posision of the window.

    window_3pos    3’ posision of the window.

    gene_number    Count of genes in this window.

    E_w            Expected count of | log2FoldChange |>2 genes in this window.

    O_w            Observed count of | log2FoldChange |>2 genes in this window.

    mean_log2FC    Mean of gene  log2FoldChange in this window.

    P.wilcox	   P-value of  median log2FoldChange  ≠ 0 conducted by wilcox.test funtion.

    Regulation	   Direction of differential expression of the window.


4.Plot

Plot potential differential expressed TADs in genome:

    options(scipen=200)# do not use scientific notation in xlab
    plot_window(DGE,res,c("WindowID")) 
    
Function  plot_window takes three prameters:

        DGE	– data.frame	       The input data,frame.
        res	– data.frame	       The data.frame generated by function search_genome
        windowID	– vector of strings  	WindowNo column in res. Just use the first and last windowID for continua windows.

Plot differential expressed windows with TADs:

    plot_window_with_TADs(DGE,res,TADs1,TADs2,"group_name1","group_name1",c("WindowID"))

Function plot_window_with_TADs could plot window with TADs information with following parameters:

        DGE	– data.frame  Same as   plot_window
        Res	– data.frame  Same as   plot_window
        TADs1	– data.frame  A data.frame with group1 TADs.bed.   Colnames should be chr start end
        TADs2	– data.frame  A data.frame with group2 TADs.bed.   Colnames should be chr start end
        group1	– string      name of group1
        group2	– string      name of group2
        windowID	– vector of strings   Same as   plot_window   
        
* chromosome name in tads.bed should not contain string 'chr'

5.Details and run a demo
   
   Please see doc.pdf
   
   Bin size is 40kb and window width is 0.5Mb.
