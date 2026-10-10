# Reading guide: explaining all three projects to a technical business audience

**For:** you, as presenter.
**Audience you are preparing for:** business stakeholders who understand the technology but have not followed these three projects in detail.
**What this gives you:** what to read and in what order, what to be able to say, which numbers to have ready, which questions to expect, and where each project is weak.
**Paths** are relative to `C:\demo\pro` unless stated.

The deck that goes with this guide is `Kunal_Tembhare_Full_Presentation_v2.pptx` (27 slides). Speaker notes are on every slide.

---

## 1. The three projects at a glance

| | Project 1: multi-omics survival model | Project 2: chromatin accessibility | Project 3: deconvolution |
|---|---|---|---|
| **One-line pitch** | Four omics layers predict whether a breast cancer patient survives beyond five years, and the model points to the genes behind it. | Open-chromatin mapping shows why a drought-tolerant rice cultivar switches on stress genes. | Learn cell-type structure from bulk RNA-seq by training on bulk data simulated from single-cell data. |
| **Your role** | First author: software, formal analysis, data curation, original draft | Co-author: sequencing data analysis, manuscript writing | All 37 commits are under your name (three spellings of it) |
| **Status** | Published (Informatics in Medicine Unlocked 47, 2024, 101507) | Published (The Plant Journal 119, 2024, 2063–2079) | Research prototype, March to July 2024, not validated |
| **Strongest evidence** | Test AUC 0.89 on 46 held-out patients; hub genes and miRNA regulators | Gained accessibility in tolerant N22 lines up with up-regulated drought genes; 13 genes confirmed by RT-qPCR | Simulation, model and experiment tracking are built; 202 tracked runs |
| **Main weakness** | One cohort, small test set, no experimental validation of biomarkers | Associative, not causal; few replicates | No accuracy check against known cell proportions; 9 of the 11 ROSMAP runs with a logged final loss ended in NaN |
| **Deck slides** | 3–12 | 13–18 | 19–26 |

Slides 1–2 frame the talk and slide 27 maps each role requirement to evidence.

---

## 2. Read first (about 25 minutes)

| Order | Read | What to take from it | Time |
|---|---|---|---|
| 1 | `description.md` | What the business cares about: multi-omic integration, biomarker models, ML and generative models, reproducible code, clear communication of uncertainty. | 5 min |
| 2 | Deck slides 1, 2 and 27 | The framing, the three-project story, and the requirement-to-evidence map. | 10 min |
| 3 | Section 1 of this guide | Status and weaknesses of each project, so you never oversell. | 10 min |

Then read one project at a time using sections 3 to 5.

---

## 3. Project 1: multi-omics survival model (ConnectOME)

**Source:** `1-s2.0-S2352914824000637-main.pdf`. The code is public at https://github.com/bioinformatics-cdac/ConnectOME and is not in this folder.
**Time to prepare:** about 90 minutes.

### What to read, in order

| # | Section of the paper | What to take from it |
|---|---|---|
| 1 | Abstract | The whole story in one paragraph: 302 TCGA patients, four omics layers, three ensemble pipelines, best model SVM + PLS-DA with AUC 0.89 and accuracy 0.83. |
| 2 | 1.1 *Machine learning using multi-omics data integration* | Why single-omics and deep learning fall short, and the gap this paper targets: biomarkers plus their background network. |
| 3 | 2.1 *Datasets* and 2.2 *Data preprocessing* | The cohort funnel (1,026 to 302), the 220 long and 82 short survivors, the 85/15 split, the data types (FPKM, RPKM, beta values, GISTIC2). |
| 4 | 2.3 *Feature selection methods* | t-test, χ², mutual information, mRMR; how continuous data were discretised. |
| 5 | 2.4 *Data integration and optimization* with **Fig. 1** | The architecture to be able to draw from memory: per-modality SVM or RF, per-modality PLS-DA, a meta-model on the stacked probabilities. |
| 6 | 3.1 *Model performance*, **Tables 1 and 2**, **Fig. 3, Fig. 4** | What was tried and why SVM + PLS-DA won. **Read Table 2 in the PDF itself**: it is a dense grid and does not copy cleanly as text. |
| 7 | **Table 3** | The comparison with Wang 2020, Malik 2021 and Mitchel 2019. |
| 8 | 2.5 *Biomarker analysis*, **Fig. 2**, then 3.2 *Biomarker analysis*, **Figs. 5 and 6** | From selected features to enriched pathways, a protein network, hub genes and miRNA targets. |
| 9 | 4 *Discussion* | The authors' own limitations: fixed survival cutoff, class imbalance, normality assumption, RF overfitting, and the nonsense-mediated decay finding. |

### Concepts you should be able to explain plainly

- **Stacking:** base models make predictions, and a second model learns from those predictions.
- **PLS-DA:** a supervised way to compress many correlated features into a few latent variables that track the class label.
- **Why limit the feature count:** with 302 patients and thousands of features, overfitting is the default. The feature count was capped where training and test curves diverged.
- **MCC versus accuracy:** accuracy is misleading with a 73/27 class split.
- **Hub genes:** genes that rank high on degree, betweenness and closeness in the protein interaction network.

### Numbers to have ready

| Number | Meaning | Where |
|---|---|---|
| 1,026 → 302 | Patients with survival status, then with all four omics layers | 2.1 |
| 220 / 82 | Long and short survival classes | 2.1 |
| 256 / 46 | Training and test patients | 2.2 |
| 0.89 / 0.83 | Test AUC and accuracy of the best model | Abstract, Table 3 |
| 15 | Reshuffles of the full dataset used as a stability check | 2.4 |
| 845 → 768 | Gene biomarkers selected, then mapped | 3.2.1 |
| 4,221 | Interactions in the 768-gene STRING network | 3.2.2 |
| 8 | Hub genes: HSP90AA1, HSPA8, IL18, MAPK8, NFKBIA, NOTCH1, PIK3CA, RELA | 3.2.2 |
| 148 / 82 / 195 | miRNA biomarkers; miRNAs that regulate 195 enriched targets | 3.2.3 |

### Questions to expect

- **Deep learning scores higher. Why not use it?** In Table 3 Malik 2021 reports accuracy 0.94 and AUC 0.98. The paper's argument is that deep learning gives no actionable biomarkers, while this model gives features, hub genes and network context. Say that the cohorts and splits differ, so it is not a head-to-head benchmark.
- **Is a 46-patient test set enough?** It is small: one patient is about 2 points of accuracy. The 15 reshuffles help with stability. There is no external cohort.
- **Random forest scored higher on some numbers. Why was it not chosen?** It overfit: the gap between train and test accuracy and AUC was large (Table 2, Fig. 3).
- **Were the biomarkers validated experimentally?** No. The paper says knockout studies are needed. Many selected genes match earlier prognostic reports.
- **Why a five-year binary outcome?** It is a stated limitation. The authors suggest several survival intervals. Time-to-event modelling would be the natural next step.

---

## 4. Project 2: chromatin accessibility in drought-stressed rice

**Source:** `The Plant Journal - 2024 - Rajkumar - Genome‐wide mapping of DNase I hypersensitive sites revealed differential chromatin.pdf`. Supplementary tables and figures are not in the folder. Reads are deposited at INSDC under PRJEB66718.
**Time to prepare:** about 60 minutes.

### What to read, in order

| # | Section of the paper | What to take from it |
|---|---|---|
| 1 | Summary and Introduction | The question: does open chromatin explain why N22 tolerates drought and IR64 does not. |
| 2 | Experimental procedures: *Plant material*, *DNase I treatment, library preparation and sequencing* | The design: two cultivars, control and drought (6 h in 20% PEG 6000), two replicates (one for IR64 under drought), 101–147 million read pairs per sample. |
| 3 | Experimental procedures: ***DNase I-seq data analysis*** | **The pipeline to know cold:** NGS QC Toolkit, BWA-MEM to Nipponbare v7, MACS3 against an input control, IDR for replicate consensus, csaw for differential sites, annotation with a 10% overlap rule. |
| 4 | Results: *Identification of DHSs in rice genome*, **Fig. 1** | Landscape: 21,549 to 35,965 consensus sites per sample; 14,959 shared; N22 has 3.3 times more sample-specific sites at control and 2.8 times under drought. |
| 5 | Results: *Influence of DHSs on gene expression*, **Fig. 2**; *Epigenetic modifications within DHSs*, **Fig. 3** | Open sites at the TSS and promoter go with high expression, low methylation and activating histone marks. |
| 6 | Results: *Correlation between changes in DHSs and differential gene expression*, **Fig. 4** | The core result: 272 of 276 drought-responsive sites gained accessibility in N22, against 41 of 54 in IR64. |
| 7 | Results: the candidate-gene sections, **Figs. 5 and 6**; *Identification of enriched CREs*, **Fig. 7** | 30 candidates, 13 with gained accessibility in N22, all up-regulated by RT-qPCR; ABRE/ABF and ARF motifs. |
| 8 | Author contributions | Your contribution as the authors state it. |

### Concepts you should be able to explain plainly

- **DNase-seq and DHS:** DNase I cuts DNA where chromatin is open. Sequencing the cut sites maps open regions, called DNase I hypersensitive sites.
- **Peak calling and IDR:** MACS3 finds enriched regions against an input control. IDR keeps peaks that reproduce across replicates.
- **csaw:** a Bioconductor method for differential windows between conditions.
- **CREs and motifs:** cis-regulatory elements are the DNA sequences transcription factors bind. ABRE responds to abscisic acid and ARF to auxin.
- **Why DNase-seq counts as an ATAC-seq-style analysis:** both give open-chromatin reads, and the workflow from QC to motifs is the same.

### Numbers to have ready

| Number | Meaning | Where |
|---|---|---|
| 10,076 vs 3,670 | N22 versus IR64 sample-specific sites under drought | Results, Fig. 1b |
| 98.6% vs 75.9% | Share of drought-responsive sites that gained accessibility (272/276, 41/54) | Results, Fig. 4 |
| 44 + 12 | N22 genes with gained promoter or TSS sites under drought; all up-regulated | Results, Fig. 4d,e |
| 60% vs 34% | Share of gained sites in promoters, N22 versus IR64 | Results, Fig. 4c |
| 13 | Candidate genes confirmed up-regulated by RT-qPCR | Results, Fig. 6, Fig. S7 |

### Questions to expect

- **Is this ATAC-seq?** No. The assay is DNase-seq. The analysis pattern is the same as for ATAC-seq. Say so plainly.
- **Is the link causal?** No. Gained accessibility goes with up-regulation. RT-qPCR confirms expression, not function. The motifs are candidates for editing or follow-up.
- **How many replicates?** Two per condition, and one for IR64 under drought. Be ready to say that this limits the IR64 comparison.
- **What exactly did you do?** The author statement says you analysed the data and contributed to writing. Describe your own steps in the pipeline. The Bash and Python detail comes from you, not from the paper.

---

## 5. Project 3: deconvolution with a variational autoencoder

**Source:** the `Deconvolution` folder, a Git repository with 37 commits from 18 March to 31 July 2024.
**Time to prepare:** about 90 minutes. This is the project where you most need to know the code, because there is no paper to lean on.

### What to read, in order

| # | File | What to take from it |
|---|---|---|
| 1 | `Deconvolution/README.md` | The one-paragraph purpose and the project layout. |
| 2 | `Deconvolution/references/1.0-Raw-data-Gathering.md` and `img/REadme_image_1.png` | The datasets: pancreas E-MTAB-5061, GSE50244 (bulk, 89 samples) and GSE81608; the ROSMAP brain data. |
| 3 | `Deconvolution/references/2.0-Convolution-of-Single-Cell.md` | The maths of the simulation: gene abundance θ, cell size S, proportions p. |
| 4 | `Deconvolution/src/data/DeConv_function_ROSMAP.py` | The `Deconv` class. Follow `Gene_abundance_matrix`, `cell_size`, `Sc_gene_exp`, `reference_matrix`, `cell_type_propertion_estimate` and the IQR outlier step. |
| 5 | `Deconvolution/src/data/make_dataset_rosmap.py` | How the class is run and what it writes: one `_single` and one `_pseudo` table per sample. |
| 6 | `Deconvolution/notebooks/rosmap/00` to `02` | Preprocessing: Scanpy filtering, 24 sample files, counts to CPM, gene filtering. Early pancreas work is in `notebooks/diabetes/`. |
| 7 | `Deconvolution/references/3.0-Model-building.md`, `src/models/model.py`, `src/models/AE_model.py` | The VAE and the autoencoder variant that also returns the latent value. |
| 8 | `Deconvolution/src/models/train_model_optuna_rosmap.py` | Data loading (`input_data`), the loss (`compute_kl_loss`, `loss_function`), tuning (`objective`) and the final run (`__main__`). |
| 9 | `Deconvolution/params.yaml`, `dvc.yaml`, `data.dvc`, `mlflow.yaml`, `requirements.txt`, `references/4.0-Documentation-on-using-DVC-and-Mlflow.md` | How runs are configured, versioned and tracked. |
| 10 | `Deconvolution/notebooks/rosmap/03` and `04`, then `img/loss_plot.png` and `git log` | The most recent experiments, the instability, and the order in which problems were fixed. |

### Concepts you should be able to explain plainly

- **Bulk versus single-cell RNA-seq:** bulk averages over all cells in a sample. Single-cell shows each cell type separately.
- **Deconvolution:** estimating cell-type composition or cell-type-specific expression from bulk data.
- **Pseudo-bulk:** adding single-cell data back up, weighted by cell-type proportions, to create bulk-like data with a known answer.
- **VAE:** an encoder compresses the input to a small latent variable, a decoder rebuilds it, and the loss balances reconstruction error against a KL term.
- **Why simulate:** real bulk samples have no ground truth for cell-type expression. Simulation gives matched training pairs.
- **The tooling:** DVC versions large data, MLflow records every run, Optuna searches hyperparameters, `params.yaml` holds the settings.

### Numbers to have ready

| Number | Meaning | Where |
|---|---|---|
| about 172,700 nuclei, 28,781 genes, 24 samples, 9 cell types (4 modelled) | The ROSMAP dataset | `notebooks/rosmap/00`, `02` |
| 1,600 cells, 18 donors, 33,889 genes | The GSE81608 pancreas dataset after filtering | `notebooks/diabetes/02` |
| 27 GB in 125 files | Data versioned with DVC | `data.dvc` |
| 202 runs in 9 experiments | Tracked in MLflow | `mlruns/` |
| 9 of 11 | ROSMAP runs with a logged final loss that ended in NaN | `mlruns/`, ROSMAP experiments |
| 10 trials, 10 epochs, batch 24, latent 1, input 4 | Current settings | `params.yaml` |

### Questions to expect

- **Does it work?** Not yet shown. The repository has no accuracy check against known cell proportions, and `predict_model.py` is empty. Say this first, then describe what is built.
- **Why simulate the training data?** To get matched cell-type and bulk pairs with a known answer. The formulation follows the MuSiC paper cited in `references/2.0`.
- **Why a latent dimension of one?** The pseudo-bulk signal is one value per gene. The 31 July commit moves toward tying the latent value to it.
- **How does it differ from MuSiC or CIBERSORT?** Those are established methods. This project reuses MuSiC's formulation to build data and explores a learned alternative. There is no benchmark against them yet.
- **Why so many failed runs?** Scale and numerical problems: raw counts, all-zero inputs, gradient spikes. The fixes are in the commit history and on slide 25.
- **Is it reproducible?** Partly. See the list below.

### Things in the repository that differ from the documentation

Know these before someone finds them.

1. **Loss.** The README and `references/3.0` describe a VAE with a KL term. `train_model_optuna_rosmap.py` still uses a KL term against a prior computed from the batch's own data. In batch mode the pseudo-bulk tensor is loaded but does not enter the loss. The newer idea, an MSE term tying the latent value to pseudo-bulk, is in `AE_model.py` and notebook `04`.
2. **DVC.** `dvc.yaml` declares commands and parameters but no dependencies or outputs, so DVC cannot track stage inputs. `dvc.lock` is stale: it records the older pancreas pipeline with 20 trials and different search ranges.
3. **Docs.** The first section of `references/2.0` is titled as `make_dataset_rosmap.py` but describes metadata extraction for the pancreas data (`create_metadata.py`).
4. **Paths and servers.** Notebooks and some scripts use absolute `/home/kunalt/...` paths, and the MLflow tracking URI is fixed at `http://0.0.0.0:8083`, so a local MLflow server must be running.
5. **Defaults.** `params.yaml` has `grad_clip: False` and `num_epochs: [10]`, so the current settings are small.

---

## 6. How the three projects connect (two-minute story)

1. **Project 1** shows supervised multi-omic modelling on clinical outcomes: careful validation, honest limits, and biology traced back from the model.
2. **Project 2** shows NGS and epigenomics: raw reads to differential regulation, with replicate statistics and experimental validation.
3. **Project 3** shows building and operating deep learning on transcriptomics: simulation, a generative model, and reproducible tooling, with the validation gap stated openly.

The common thread is end-to-end ownership and stating what is and is not proven.

---

## 7. Before you present

- [ ] **ATAC-seq wording.** The chromatin paper is DNase-seq. Keep the wording "same workflow pattern as ATAC-seq".
- [ ] **Bash and Python.** This detail comes from you. Be ready to describe or show the scripts.
- [ ] **Proposed next steps.** Slides 12 and 26 are proposals, not completed work.
- [ ] **Permission to share.** Confirm you may show the paper PDFs, repository details, MLflow numbers and the loss figure to this audience.
- [ ] **Role gaps.** The three projects do not cover clinical-trial data, generative AI or agentic systems, or causal inference. Say so if asked, rather than stretch.
- [ ] **Things not in the folder.** The ConnectOME code, the papers' supplementary files and the 27 GB of Deconvolution data are not here, so some numbers cannot be re-checked locally.
- [ ] **Open the deck from `_v2`.** The 26-slide file `Kunal_Tembhare_Full_Presentation.pptx` is the earlier version.

---

## 8. Glossary

| Term | Meaning |
|---|---|
| Omics | Genome-wide measurements such as gene expression, miRNA, DNA methylation, copy number. |
| TCGA | The Cancer Genome Atlas, a public cancer dataset. |
| AUC | Area under the ROC curve: how well a model separates two classes (0.5 is chance, 1.0 is perfect). |
| MCC | Matthews correlation coefficient: a balanced accuracy-style score for imbalanced classes. |
| Hub gene | A gene highly connected in a network. |
| DHS | DNase I hypersensitive site: an open-chromatin region. |
| CRE | Cis-regulatory element: DNA a transcription factor binds. |
| IDR | Irreproducible discovery rate: keeps peaks that reproduce between replicates. |
| ROSMAP | The Religious Orders Study and Memory and Aging Project, the source of the brain single-nucleus data. |
| CPM | Counts per million: a simple library-size normalisation. |
| Pseudo-bulk | Bulk-like data built by summing single-cell data. |
| VAE | Variational autoencoder: a generative neural network with a probabilistic latent variable. |
| DVC / MLflow / Optuna | Tools for data versioning, experiment tracking and hyperparameter search. |
