# Comparing Linear Regression and Linear Mixed Models for GWAS on Chromosome 22

Cole Carter (A16927029) — CSE 284, Option 2

## Overview

I'm comparing standard GWAS (linear regression in PLINK) with LMM-based GWAS (GEMMA) on 1000 Genomes chromosome 22. The goal is to look at runtime, p-values, effect sizes, and how the two methods differ. Linear regression assumes samples are independent; the mixed model uses a relatedness matrix so population structure is accounted for.

The main analysis is in the Jupyter notebook: it converts the chr22 VCF to PLINK format, runs both GWAS methods, and makes QQ plots, Manhattan plots, and scatter plots comparing the results.

## Data

Put the chr22 VCF in a `data/` folder (or in the project root). Links:

- VCF: https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr22.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz (~196 MB)
- Tabix index: same URL + `.tbi`

```bash
mkdir -p data
cd data
wget https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr22.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz
wget https://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20130502/ALL.chr22.phase3_shapeit2_mvncall_integrated_v5b.20130502.genotypes.vcf.gz.tbi
cd ..
```

The notebook uses a simulated quantitative phenotype (1000 Genomes doesn’t have traits), so no extra phenotype file is needed.

## Running the analysis

You need PLINK (1.9 or 2), GEMMA, and Python with pandas, numpy, matplotlib, seaborn, scipy. For the “quick run” option you also need bcftools (`brew install bcftools`).

In the notebook, set `USE_QUICK_RUN = True` to work on a 5 Mb chunk of chr22 so everything finishes in a few minutes. Set it to `False` to run on full chr22 (takes a lot longer). If you use quick run, run the “Create 5 Mb subset” cell first so it builds the subset VCF; then run the rest in order.

Open the notebook and run cells top to bottom. It will convert VCF to PLINK binary, generate the phenotype, run PLINK and GEMMA, then load results and make the plots. Outputs go to `outputs/`.

## Analysis scripts and results so far

- **gwas_lr_vs_lmm.ipynb** — full pipeline: VCF → PLINK binary, phenotype, PLINK linear GWAS, GEMMA relatedness + LMM GWAS, then loading results and plotting (QQ, Manhattan, PLINK vs GEMMA scatter).
- Pipeline is set up and runs on the 5 Mb subset when quick run is on; full chr22 runs but takes a long time (see below). Once a run finishes, the notebook produces the comparison plots and can print runtimes.

## Known issues / setup

- **Runtime:** Full chr22 (VCF → PLINK conversion and then GWAS) can take 30–60+ minutes on a laptop. The conversion step in particular is slow. Using the 5 Mb quick run is a lot more practical for iterating; you can switch to full chr22 when you have time or run it on a server.
- PLINK isn’t on Homebrew; you have to download the macOS build from the PLINK site and point the notebook at it (set `PLINK_PATH` in the first cell).
- GEMMA also has to be installed separately (e.g. from source or a pre-built binary). The notebook looks for it on your PATH or you can set `GEMMA_PATH`.

## Remaining work

- Run full chr22 with both PLINK and GEMMA and record runtime/memory for the report.
- Finish comparing p-values and effect sizes (concordance, inflation).
- Clean up figures and write up methods/results for the final report.
- Add references and code availability to the report.

## Challenges I’d like to discuss

- With a simulated phenotype we don’t have true causal variants—what’s a good way to summarize “accuracy” or method comparison in that case?
- GEMMA on full chr22: runtime and memory, and what to do if resources are limited (e.g. smaller region or fewer samples).
- Whether to restrict to one population for a cleaner LR vs LMM comparison or keep the full sample.
