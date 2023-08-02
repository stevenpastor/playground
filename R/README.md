# R Scripts

## Quick Snippets

* For quick snippets of code I frequently use, see the snippets markdown 

## INSTALLATION

* Use Dockerfile for R version and package versions

* Example (can change r_scripts to whatever name you prefer):

```
cd /path/to/playground/R
docker build -t r_scripts images/R_4.2.2_base/
docker run -it --rm r_scripts
```

* This example will go into the R-4.2.2 prompt

