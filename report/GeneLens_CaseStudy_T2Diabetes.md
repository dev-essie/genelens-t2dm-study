[GeneLens_CaseStudy_T2Diabetes.md](https://github.com/user-attachments/files/28998156/GeneLens_CaseStudy_T2Diabetes.md)
# GeneLens Case Study
## Transcriptomic Workflow Demonstration, Type 2 Diabetes Liver
### Differential Expression · GO Enrichment · KEGG Pathways · ML Classification · AI Interpretation

**Author:** Alabi Esther Oluwatimilehin
**GitHub:** [github.com/ESTIE-CREATOR](https://github.com/ESTIE-CREATOR)
**Tool:** [GeneLens](https://github.com/ESTIE-CREATOR/GeneLens) · [Live App](https://genelens.streamlit.app)
**Date:** June 2026

---

> ⚠️ **Important Note**
>
> This report demonstrates the GeneLens **workflow** on a synthetic dataset, a hand-curated panel of ~226 oxidative-stress and metabolic genes structured to resemble a Type 2 Diabetes liver study.
> The objective was to exercise the statistical, enrichment, machine-learning, and AI-interpretation pipeline end to end while developing GeneLens.
> **Everything here is a workflow illustration, not a biological finding.** Because the panel was curated around oxidative-stress/metabolic biology, enrichment returns glutathione, insulin, and glucose-homeostasis terms **by construction**, an enrichment p-value here measures gene selection, not discovery. Enrichment p-values have therefore been removed from this report. For real findings, the same workflow is run on real GEO data (COVID-19 GSE152418 in the GeneLens repo; a real GEO T2DM liver cohort is the recommended next step).
>
> **Primary real-data validation of GeneLens:** COVID-19 PBMC dataset (GSE152418, *Science* 2020), 44,242 genes (after gene-ID mapping) × 34 real patient samples. See [GeneLens](https://github.com/ESTIE-CREATOR/GeneLens).

---

## 1. Background & Motivation

Type 2 diabetes mellitus (T2DM) is characterised by chronic hyperglycaemia, insulin resistance, and impaired hepatic metabolism. The liver plays a central role in glucose homeostasis, and oxidative stress is a key mechanism of diabetic liver injury.

My final-year thesis investigated the **antioxidant effect of phytol on the liver of alloxan-induced diabetic rats**, quantifying a full oxidative stress biomarker panel (GSH, SOD, CAT, GPx, GST, MDA) across five treatment groups. That work demonstrated that phytol modulates the GSH antioxidant pathway and significantly reduces hepatic oxidative damage (P<0.001 for SOD and GPx activity).

This case study was designed to demonstrate the GeneLens pipeline using a synthetic dataset structured around T2DM biology, specifically to show how the tool connects wet-lab biochemistry findings to transcriptomic and pathway-level analysis.

---

## 2. Dataset

| Parameter | Detail |
|-----------|--------|
| Type | Synthetic demonstration dataset |
| Reference biology | Published T2DM transcriptomics literature |
| Structure | 226 curated metabolic and oxidative stress genes |
| Samples | 5 Normal conditions, 8 T2DM conditions |
| Gene directions | Derived from published T2DM fold-change literature |
| Download | [Diabetic_Liver_RNA-seq.csv](https://github.com/user-attachments/files/28788734/Diabetic_Liver_RNA-seq.csv) |

> This is not a direct download from NCBI GEO. Gene expression directions and magnitudes were constructed to reflect published T2DM biology for pipeline demonstration purposes.

---

## 3. Methods

All analysis performed using [GeneLens](https://github.com/ESTIE-CREATOR/GeneLens):

| Step | Method | Where Referenced |
|------|--------|-----------------|
| Differential Expression | Welch's t-test + Benjamini-Hochberg FDR correction | Standard statistical method, no citation needed |
| Thresholds | padj < 0.05, fold change ≥ 1.5 | Standard DE thresholds |
| GO Enrichment | GO_Biological_Process_2021 via Enrichr API | Ashburner et al. 2000 \[1\]; Kuleshov et al. 2016 \[2\] |
| KEGG Enrichment | KEGG_2021_Human via Enrichr API | Kanehisa & Goto 2000 \[3\]; Kuleshov et al. 2016 \[2\] |
| ML Classification | Random Forest (200 trees, 5-fold CV) | Standard scikit-learn implementation |
| AI Interpretation | Anthropic Claude API | n/a |

---

## 4. Differential Expression Results

### 4.1 Summary

| Category | Count |
|----------|-------|
| Total genes | 226 |
| Upregulated | 22 |
| Downregulated | 25 |
| Not significant | 179 |

> Reproducible with the committed `data/Diabetic_Liver_RNA-seq.csv` at padj < 0.05, FC ≥ 1.5 using the current pipeline. The tables below list a **representative subset** of those genes (the thesis-relevant ones), not the full list.

### 4.2 Representative Up-regulated Genes (illustrative subset)

| Gene | Function | log2FC | padj |
|------|----------|--------|------|
| CYP2E1 | ROS-generating cytochrome P450 | +2.59 | 0.042 |
| PCK1 | Gluconeogenesis, PEPCK | +1.97 | 0.042 |
| TGFB1 | Fibrogenic signalling | +1.77 | 0.042 |
| PTGS2 | Cyclooxygenase-2, inflammation | +1.72 | 0.041 |
| ATF3 | ER stress transcription factor | +1.64 | 0.042 |
| DDIT3 | ER stress, CHOP protein | +1.38 | 0.042 |

### 4.3 Representative Down-regulated Genes (illustrative subset)

| Gene | Function | log2FC | padj | Thesis connection |
|------|----------|--------|------|-------------------|
| GCLM | GSH synthesis, modifier subunit | −1.96 | 0.041 | ✅ GSH pathway |
| GCK | Glucokinase, glucose sensing | −2.02 | 0.042 | n/a |
| PYGL | Glycogen phosphorylase | −1.88 | 0.043 | n/a |
| GPX1 | Glutathione peroxidase | −1.69 | 0.041 | ✅ Measured in thesis |
| TFAM | Mitochondrial transcription factor | −1.69 | 0.041 | n/a |
| HK1 | Hexokinase | −1.59 | 0.043 | n/a |
| SIRT3 | Mitochondrial sirtuin, antioxidant | −1.45 | 0.041 | n/a |
| PPARA | Fatty acid oxidation regulator | −1.45 | 0.041 | n/a |
| HADHA | Mitochondrial fatty acid oxidation | −1.25 | 0.043 | n/a |
| GSR | Glutathione reductase | −1.23 | 0.041 | ✅ Measured in thesis |
| INSR | Insulin receptor | −1.21 | 0.041 | n/a |
| RPS6 | Ribosomal protein, mTOR target | −0.85 | 0.043 | n/a |

---

## 5. GO Enrichment Results

GO enrichment performed using GeneLens via Enrichr API (GO_Biological_Process_2021) \[1,2\].

On this curated panel, enrichment returns the following GO families: glucose homeostasis, carbohydrate homeostasis, glutathione metabolic process, cell redox homeostasis, pyruvate metabolic process, ROS metabolic process, regulation of protein kinase B signaling, and fatty-acid beta-oxidation.

> **P-values are intentionally omitted.** The panel was hand-curated around oxidative-stress and metabolic biology, so these families are returned *by construction*. On a curated panel tested against a genome-wide background, an enrichment p-value quantifies gene selection, not biology, reporting it as a finding would be circular. These families are shown only to illustrate the enrichment step. Directional enrichment (up- vs down-regulated genes analysed separately) is now supported in GeneLens.

---

## 6. KEGG Pathway Enrichment

KEGG enrichment performed using GeneLens via Enrichr API (KEGG_2021_Human) \[2,3\]:

On this curated panel, enrichment returns the following KEGG families: insulin signaling pathway, non-alcoholic fatty liver disease, glucagon signaling pathway, insulin resistance, type II diabetes mellitus, glutathione metabolism, arachidonic acid metabolism, and diabetic cardiomyopathy.

> **P-values are intentionally omitted**, for the same reason as the GO families above: the panel was curated around exactly this biology, so these pathways are returned by construction. This is a demonstration of the enrichment *capability*, not a biological result. Run on a real GEO T2DM cohort, the same step produces a genuine, defensible enrichment result.

---

## 7. The Wet Lab → Pipeline Bridge

This table shows how the GeneLens pipeline maps thesis biochemical findings to computational outputs, illustrating the workflow rather than presenting experimental conclusions:

| Thesis Measurement | Gene | Direction | GO Term | KEGG Pathway |
|-------------------|------|-----------|---------|--------------|
| GPx activity ↓ | GPX1 | ↓ | GO:0006749 Glutathione process | hsa00480 Glutathione metabolism |
| GSH synthesis ↓ | GCLM | ↓ | GO:0006749 Glutathione process | hsa00480 Glutathione metabolism |
| GSH levels ↓ | GSR | ↓ | GO:0006749 Glutathione process | hsa00480 Glutathione metabolism |
| SOD activity ↓ | SOD2 | ↓ trend | n/a | n/a |
| CAT activity ↓ | CAT | ↓ trend | n/a | n/a |
| MDA levels ↑ | CYP2E1 | ↑ | GO:0072593 ROS metabolic process | hsa04932 NAFLD |

---

## 8. ML Classification

| Metric | Value |
|--------|-------|
| Classifier | Random Forest (200 trees, 5-fold CV) |
| Top discriminating features | GPX1, GCLM, CYP2E1, GCK, GSR |
| AUC | See ROC curve in figures |

The ML component demonstrates GeneLens's ability to train a classifier on gene expression features and report cross-validated performance and feature importance, applicable to any real dataset uploaded to the tool.

---

## 9. Conclusion

This case study demonstrates the full GeneLens pipeline, from differential expression through GO and KEGG enrichment to ML classification and AI interpretation, using a synthetic dataset structured around T2DM biology.

The workflow successfully:
- Identifies differentially expressed genes and labels them with biological functions
- Maps gene sets to enriched GO biological processes and KEGG disease pathways
- Trains a cross-validated ML classifier and reports feature importance
- Generates AI-powered biological interpretation

The biological findings are illustrative and consistent with published T2DM literature by design, they demonstrate what the pipeline produces when applied to this type of disease context. For real biological conclusions, the same workflow should be applied to experimentally validated datasets such as GSE152418 (COVID-19, demonstrated in GeneLens) or other publicly available GEO datasets.

---

## 10. Reproducibility

- **Live app:** [genelens.streamlit.app](https://genelens.streamlit.app)
- **Tool:** [github.com/ESTIE-CREATOR/GeneLens](https://github.com/ESTIE-CREATOR/GeneLens)
- **Dataset:** [Diabetic_Liver_RNA-seq.csv](https://github.com/user-attachments/files/28788734/Diabetic_Liver_RNA-seq.csv)
- **Parameters:** padj < 0.05, FC ≥ 1.5, 200 RF trees, 5-fold CV

---

## References

Only references directly relevant to tools and databases used in this analysis:

1. Ashburner M, et al. (2000). Gene ontology: tool for the unification of biology. *Nat Genet*, 25(1):25–29., *Gene Ontology database used for GO enrichment*

2. Kuleshov MV, et al. (2016). Enrichr: a comprehensive gene set enrichment analysis web server. *Nucleic Acids Res*, 44(W1):W90–97., *Enrichr API used by GeneLens for GO and KEGG enrichment*

3. Kanehisa M, Goto S. (2000). KEGG: Kyoto Encyclopedia of Genes and Genomes. *Nucleic Acids Res*, 28(1):27–30., *KEGG database used for pathway enrichment*

---

*Alabi Esther Oluwatimilehin · github.com/ESTIE-CREATOR · June 2026*
