# Transcriptomic Analysis of Type 2 Diabetes Liver — A Computational Case Study using GeneLens

This repository is a companion case study for [GeneLens](https://github.com/dev-essie/GeneLens) ([live app](https://genelens.streamlit.app/)): it walks through differential expression, GO and KEGG enrichment, machine-learning classification, and AI-assisted interpretation on a Type 2 diabetes (T2DM) liver–themed gene panel. The data are a **curated demonstration dataset** informed by published T2DM transcriptomics literature—not a direct download from NCBI GEO. Gene directions and magnitudes were constructed for pipeline illustration; see [Dataset](#dataset) below.

**Author:** [Alabi Esther Oluwatimilehin](https://github.com/dev-essie) · [Portfolio](https://devessie.xyz/)  
**Institution:** University of Medical Sciences, Nigeria — BSc Biochemistry, First Class Honours  
**Tool:** [GeneLens](https://github.com/dev-essie/GeneLens) · [Live app](https://genelens.streamlit.app/)  
**Date:** June 2026

---

> **Important note**
>
> This repository demonstrates the GeneLens analysis **workflow** using a **synthetic dataset**: a hand-curated panel of ~226 oxidative-stress and metabolic genes structured to resemble a Type 2 Diabetes liver study.
>
> The objective was to exercise the statistical, enrichment, machine-learning, and AI-interpretation pipeline end to end while developing GeneLens.
>
> **Everything here is a workflow illustration, not a biological finding.** Because the gene panel was curated around oxidative-stress and metabolic biology, enrichment necessarily returns glutathione, insulin, and glucose-homeostasis terms **by construction**—that reflects how the panel was selected, not a discovery. Enrichment p-values have been removed from this document; they would quantify gene selection, not biology. For real findings, run the same workflow on real GEO data (COVID-19 GSE152418 in the main [GeneLens](https://github.com/dev-essie/GeneLens) repo; a real GEO T2DM liver cohort is a recommended next step).
>
> **Primary real-data validation of GeneLens:** COVID-19 PBMC dataset (GSE152418, *Science* 2020), 44,242 genes (after gene-ID mapping) × 34 patient samples. See [GeneLens](https://github.com/dev-essie/GeneLens).

---

## Overview

The study was motivated by my final-year biochemistry thesis, which measured antioxidant and oxidative-stress markers (SOD, CAT, GPx, GST, GSH, MDA) in diabetic rat liver. This demonstration explores whether the same antioxidant genes studied in that work appear dysregulated in a T2DM-structured transcriptomics dataset, and showcases the full GeneLens pipeline from raw counts to pathway-level interpretation.

---

## The wet lab → computational bridge

| Thesis measurement | Gene | Direction | GO term | KEGG pathway |
|-------------------|------|-----------|---------|--------------|
| GPx activity ↓ | GPX1 | ↓ | GO:0006749 Glutathione process | hsa00480 Glutathione metabolism |
| GSH synthesis ↓ | GCLM | ↓ | GO:0006749 Glutathione process | hsa00480 Glutathione metabolism |
| GSH levels ↓ | GSR | ↓ | GO:0006749 Glutathione process | hsa00480 Glutathione metabolism |
| SOD activity ↓ | SOD2 | ↓ trend | n/a | n/a |
| CAT activity ↓ | CAT | ↓ trend | n/a | n/a |
| MDA levels ↑ | CYP2E1 | ↑ | GO:0072593 ROS metabolic process | hsa04932 NAFLD |

This table illustrates the conceptual connection between wet-lab biochemistry and transcriptomic pipeline outputs, not experimental validation.

---

## Dataset

| Parameter | Detail |
|-----------|--------|
| Type | Synthetic demonstration dataset |
| Reference biology | Published T2DM transcriptomics literature |
| Structure | 226 curated metabolic and oxidative-stress genes |
| Samples | 5 normal conditions, 8 T2DM conditions |
| Gene directions | Derived from published T2DM fold-change literature |
| File in repo | [`data/Diabetic_Liver_RNA-seq.csv`](data/Diabetic_Liver_RNA-seq.csv) |

This is **not** a direct download from NCBI GEO. Expression directions and magnitudes were constructed to reflect published T2DM biology for pipeline demonstration purposes.

---

## Results summary

| Category | Count |
|----------|-------|
| Total genes | 226 |
| Upregulated | 22 |
| Downregulated | 25 |
| Not significant | 179 |

Reproducible with the committed `data/Diabetic_Liver_RNA-seq.csv` at padj < 0.05 and fold change ≥ 1.5 using the current pipeline. Outputs on a synthetic panel are illustrative only.

---

## Figures

### Volcano plot

<img width="1918" height="877" alt="Volcano plot of differential expression" src="https://github.com/user-attachments/assets/d2528e19-c859-4b4f-a5ef-e8ab85319d89" />

*Red = upregulated (CYP2E1, PCK1). Blue = downregulated (GPX1, GCLM, GSR).*

### Heatmap

<img width="1919" height="879" alt="Expression heatmap" src="https://github.com/user-attachments/assets/e6317568-e7e0-48ac-91a5-61f0d985ffce" />

### PCA — sample clustering

<img width="1919" height="877" alt="PCA sample clustering" src="https://github.com/user-attachments/assets/c0fdf5ae-aacf-4589-bcd9-917c22df1d9d" />

### GO enrichment

<img width="1919" height="943" alt="GO enrichment bar chart" src="https://github.com/user-attachments/assets/1f415041-4918-4d2a-932d-954a9172ff04" />

*Glutathione metabolic process enriched among down-regulated genes (thesis-relevant term; illustrative—see note above).*

### KEGG pathways

<img width="1919" height="948" alt="KEGG pathway enrichment" src="https://github.com/user-attachments/assets/e79afb9c-c05f-4051-a77d-dbd09504edc8" />

*Insulin signalling, glutathione metabolism, and type II diabetes mellitus (illustrative; returned by construction from the curated panel; p-values omitted—see note above).*

### ML classification — ROC curve

<img width="1919" height="885" alt="Random forest ROC curve" src="https://github.com/user-attachments/assets/581f2724-8a13-476f-9411-43ffc6b91fc7" />

---

## Enrichment families returned (illustrative; no p-values by design)

On this curated panel, GO enrichment returns **glucose homeostasis, carbohydrate homeostasis, glutathione metabolic process, and cell redox homeostasis** families; KEGG returns **insulin signaling, non-alcoholic fatty liver disease, type II diabetes mellitus, and glutathione metabolism**.

These are the families the panel was curated around, so their appearance is **expected by construction, not a discovery**. P-values are intentionally omitted: on a hand-selected gene panel against a genome-wide background, an enrichment p-value measures panel selection, not biology. The families are listed to show what the enrichment step outputs; treat them as a workflow illustration. Directional enrichment (up vs. down separately) is supported in GeneLens, so on real data these families would carry a defensible direction.

---

## Methods

| Step | Method |
|------|--------|
| DE analysis | Welch's t-test + BH FDR (padj &lt; 0.05, FC ≥ 1.5) |
| GO enrichment | GO_Biological_Process_2021 via Enrichr API [1, 2] |
| KEGG enrichment | KEGG_2021_Human via Enrichr API [2, 3] |
| ML classification | Random Forest (200 trees, 5-fold CV) |
| AI interpretation | Anthropic Claude API |
| Tool | [GeneLens](https://github.com/dev-essie/GeneLens) |

---

## Full report

[`report/GeneLens_CaseStudy_T2Diabetes.md`](report/GeneLens_CaseStudy_T2Diabetes.md)

---

## How to reproduce

```bash
# Open GeneLens: https://genelens.streamlit.app/
# Upload: data/Diabetic_Liver_RNA-seq.csv (or the CSV from this repo)
# Control columns: Normal_1 to Normal_5
# Treated columns: T2Diabetes_1 to T2Diabetes_8
# Parameters: padj < 0.05, fold-change threshold 1.5
```

---

## References

1. Ashburner M, et al. (2000). Gene ontology: tool for the unification of biology. *Nat Genet*, 25(1):25–29. GO database used for GO enrichment.

2. Kuleshov MV, et al. (2016). Enrichr: a comprehensive gene set enrichment analysis web server. *Nucleic Acids Res*, 44(W1):W90–97. Enrichr API used by GeneLens.

3. Kanehisa M, Goto S. (2000). KEGG: Kyoto Encyclopedia of Genes and Genomes. *Nucleic Acids Res*, 28(1):27–30. KEGG database used for pathway enrichment.

---

## Related

- [GeneLens repository](https://github.com/dev-essie/GeneLens)
- [GeneLens live app](https://genelens.streamlit.app/)
- [LinkedIn](https://linkedin.com/in/alabi-esther-essie)

---

*Alabi Esther Oluwatimilehin · [github.com/dev-essie](https://github.com/dev-essie) · [devessie.xyz](https://devessie.xyz/) · June 2026*
