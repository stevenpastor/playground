* Read from command line:

```
args <- commandArgs(trailingOnly = TRUE)
basename <- args[1]
```

* basename is now whatever had on command line; for example:

```
# bash:
Rscript name_of_script.R HELLO
```

* basename is now HELLO in R script named name_of_script.R

---

* Split vector by delimiter (period here) and take only first part of the split

* Note the \\ to escape the period but not needed for all chars

* Also, colnames(tmp@assays$RNA@counts) is the vector below

```
unlist(lapply(strsplit(as.character(colnames(tmp@assays$RNA@counts)), "\\."), '[[', 1))
```

---

* Specific RNA-seq example: reading separate counts files and column binding them

* Also, keeps one column as the genes column (Ensemble here, to ensure uniqueness of rownames)

* Assuming the counts value (FPKM is typically col7 from RSEM; 6 is TPM)

```
x <- readDGE(files, columns=c(1,7))

# ALTERNATIVE IF NOT USING DESEQ2 OR LIMMA (EDGER):
# load rsem counts:
files <- list.files('results/quants/', pattern='*.genes.results')
files <- paste0('results/quants/', files)

# get genes from one (rsem-based) counts file:
genes_tmp <- read.table(files[1], header=T)
genes_tmp <- genes_tmp$gene_id

# cbind all col7 only files:
df <- do.call(cbind,lapply(files,function(fn)read.table(fn,header=T, sep="\t")[,7]))
df <- cbind(genes_tmp,df)

# clean-up:
rownames(df) <- df[,1]
df <- df[,-1]
basenames <- gsub('-', '_', gsub('\\.genes\\.results', '', gsub('results/quants/', '', files)))
colnames(df) <- basenames
df <- as.data.frame(df)

# convert to numeric (all cols are char right now):
df2 <- sapply(df, as.numeric)
df2 <- as.data.frame(df2)
rownames(df2) <- rownames(df)
df <- df2
```

* placeholder
