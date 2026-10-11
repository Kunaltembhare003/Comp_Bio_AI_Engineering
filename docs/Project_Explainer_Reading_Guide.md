# Reading guide: explaining all four projects to a technical business audience

**For:** you, as presenter.
**Audience you are preparing for:** business stakeholders who understand the technology but have not followed these projects in detail.
**What this gives you:** what to read and in what order, what to be able to say, which numbers to have ready, which questions to expect, and where each project is weak.
**Paths** are relative to `C:\demo\pro` unless stated.
**Time plan:** a 50-minute presentation, about 40 minutes of talk and 10 of discussion. Section 2A has the plan. The script, rehearsal passes and cut list are in the last section of `description.md`.

The deck that goes with this guide is `Full_Presentation_v4.pptx` (34 slides). Speaker notes are on every slide. `Full_Presentation_v3.pptx` and `Full_Presentation_v2.pptx` are earlier versions.

---

## 1. The four projects at a glance

| | Project 1: multi-omics survival model | Project 2: chromatin accessibility | Project 3: deconvolution | Project 4: generative AI and agents |
|---|---|---|---|---|
| **One-line pitch** | Four omics layers predict whether a breast cancer patient survives beyond five years, and the model points to the genes behind it. | Open-chromatin mapping shows why a drought-tolerant rice cultivar switches on stress genes. | Learn cell-type structure from bulk RNA-seq by training on bulk data simulated from single-cell data. | A LangGraph multi-agent system that gathers evidence and assesses compliance questions, using RAG, MCP tools and tracing. |
| **Your role** | First author: software, formal analysis, data curation, original draft | Co-author: sequencing data analysis, manuscript writing | All 37 commits are under your name (three spellings of it) | "Designed and developed" (your summary); say exactly which parts were yours |
| **Status** | Published (Informatics in Medicine Unlocked 47, 2024, 101507) | Published (The Plant Journal 119, 2024, 2063–2079) | Research prototype, not validated | Enterprise, production-oriented (your summary) |
| **Strongest evidence** | Test AUC 0.89 on 46 held-out patients; hub genes and miRNA regulators | Gained accessibility in tolerant N22 lines up with up-regulated drought genes; 13 genes confirmed by RT-qPCR | Simulation, model and experiment tracking are built; 202 tracked runs | A full architecture: routing, parallel specialist agents, scheduled Sentinel, MCP tools, MLflow tracing, AWS platform |
| **Main weakness** | One cohort, small test set, no experimental validation of biomarkers | Associative, not causal; few replicates | No accuracy check against known cell proportions; 9 of the 11 ROSMAP runs with a logged final loss ended in NaN | No outcome numbers or evaluation results in the material provided; domain is compliance, not biomedicine |
| **Deck slides** | 3–12 | 13–18 | 19–26 | 27–33 |

Slides 1–2 frame the talk and slide 34 maps each role requirement to evidence.

---

## 2. Read first (about 25 minutes)

| Order | Read | What to take from it | Time |
|---|---|---|---|
| 1 | `description.md` | What the business cares about: multi-omic integration, biomarker models, ML and generative models, agentic systems, reproducible code, clear communication of uncertainty. | 5 min |
| 2 | Deck slides 1, 2 and 34 | The framing, the four-project story, and the requirement-to-evidence map. | 10 min |
| 3 | Section 1 of this guide | Status and weaknesses of each project, so you never oversell. | 10 min |

Then read the 50-minute plan in section 2A, and one project at a time using sections 3 to 6.

---

## 2A. The 50-minute presentation at a glance

The full plan, with a script and a cut list, is at the end of `description.md`. This is the short version to keep beside the project sections.

**Shape:** about 40 minutes of talk and 10 minutes of discussion. Check the clock at the end of slides 12, 18, 26 and 33. You should be at 0:14, 0:20, 0:28 and 0:37.

| Part | Slides | Min | Clock | One message | Prepare with |
|---|---|---|---|---|---|
| Opening | 1–2 | 3 | 0:00–0:03 | Four projects, one pattern: build it, check it, state the limits. | Section 2 |
| 1: multi-omics survival model | 3–12 | 11 | 0:03–0:14 | A published model that predicts survival and explains itself. | Section 3 (about 90 min) |
| 2: chromatin accessibility | 13–18 | 6 | 0:14–0:20 | NGS and epigenomics, from raw reads to regulatory insight. | Section 4 (about 60 min) |
| 3: deconvolution | 19–26 | 8 | 0:20–0:28 | Built and tracked end to end; validation is the open step. | Section 5 (about 90 min) |
| 4: generative AI and agents | 27–33 | 9 | 0:28–0:37 | An agentic system with retrieval, tools and observability. | Section 6 (about 2 hours) |
| Close | 34 | 3 | 0:37–0:40 | Evidence against each requirement; name the gaps. | Section 7 |
| Discussion | 34 | 10 | 0:40–0:50 | Questions. | The question lists in sections 3 to 6 |

**Where to put your preparation.** Prepare Parts 1 and 4 deepest: Part 1 has the most talk time and the strongest evidence, and Part 4 answers the role's generative AI and agentic requirement. Prepare Part 3 for honesty about its status, and Part 2 for crispness.

**Minimum preparation (about 3 hours).** Sections 1 and 2 (25 minutes), the "Minimum reading" line in each project section (about 30 minutes each, 2 hours), and one timed run-through (40 minutes).

**Slides never to skip when time is short:** 8, 12, 15, 17, 23, 25, 26, 29, 31 and 34. The cut list is in `description.md`.

---

## 3. Project 1: multi-omics survival model (ConnectOME)

**Source:** `1-s2.0-S2352914824000637-main.pdf`. The code is public at https://github.com/bioinformatics-cdac/ConnectOME and is not in this folder.
**Time to prepare:** about 90 minutes.

### In the 50-minute talk

**Slides 3–12, 11 minutes (0:03–0:14).** One message: a published model that predicts survival and explains itself.

| Part of the talk | Slides | Time |
|---|---|---|
| Divider with the three numbers | 3 | 0.25 min |
| Problem and data | 4–5 | 2 min |
| Method; draw the stacked ensemble | 6–7 | 2.5 min |
| Result, and why SVM + PLS-DA won | 8–9 | 2.5 min |
| Biology: hub genes, miRNAs, nonsense-mediated decay | 10–11 | 2.25 min |
| Limits and next steps | 12 | 1.25 min |

**Say before anyone asks:** one cohort, a 46-patient test set, and biomarkers not validated experimentally. Say that deep learning scores higher on accuracy.
**Minimum reading (about 30 minutes):** the abstract, 2.4 with Fig. 1, Table 3 and the Discussion. Know 302, 220/82, 46, 0.89/0.83 and the eight hub genes.

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

### In the 50-minute talk

**Slides 13–18, 6 minutes (0:14–0:20).** One message: NGS and epigenomics, from raw reads to regulatory insight.

| Part of the talk | Slides | Time |
|---|---|---|
| Divider with the three numbers | 13 | 0.25 min |
| Design: two cultivars, two conditions | 14 | 1 min |
| The pipeline, QC to motifs | 15 | 1.5 min |
| Landscape of open sites | 16 | 1 min |
| Gained accessibility and expression | 17 | 1.25 min |
| Chromatin state, motifs, candidate genes | 18 | 1 min |

**Say before anyone asks:** the assay is DNase-seq, not ATAC-seq; the link is associative, not causal; IR64 under drought has one replicate.
**Minimum reading (about 30 minutes):** the methods section *DNase I-seq data analysis*, plus Figs. 1 and 4. Know 21–36 thousand sites, 272 of 276 against 41 of 54, and the 13 confirmed genes.

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

**Source:** the `Deconvolution` folder, a Git repository with 37 commits.
**Time to prepare:** about 90 minutes. This is the project where you most need to know the code, because there is no paper to lean on.

### In the 50-minute talk

**Slides 19–26, 8 minutes (0:20–0:28).** One message: built and tracked end to end; validation is the open step.

| Part of the talk | Slides | Time |
|---|---|---|
| Divider; say "research prototype" in the first sentence | 19 | 0.25 min |
| Idea and datasets | 20 | 1 min |
| End-to-end architecture | 21 | 1.25 min |
| Simulating training data | 22 | 1.25 min |
| The VAE, and the transformer attempt | 23 | 1.5 min |
| Reproducibility: DVC, params, MLflow | 24 | 0.75 min |
| Debugging the loss | 25 | 1 min |
| Status and next steps | 26 | 1 min |

**Say before anyone asks:** it is a prototype; there is no accuracy check against known proportions; 9 of 11 ROSMAP runs with a logged final loss ended in NaN; the transformer attempt is not in the repository.
**Minimum reading (about 30 minutes):** the README, `params.yaml`, the loss plot and a skim of `DeConv_function_ROSMAP.py`. Know about 172,700 nuclei, 24 samples, 4 cell types, 202 runs and 9 of 11.

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
- **Transformer encoder and self-attention:** a transformer encoder lets every input element attend to every other one. With genes as the elements, the model can learn how genes influence each other, which the one-gene-at-a-time VAE cannot.

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
- **Why a latent dimension of one?** The pseudo-bulk signal is one value per gene. The latest commit moves toward tying the latent value to it.
- **How does it differ from MuSiC or CIBERSORT?** Those are established methods. This project reuses MuSiC's formulation to build data and explores a learned alternative. There is no benchmark against them yet.
- **Why so many failed runs?** Scale and numerical problems: raw counts, all-zero inputs, gradient spikes. The fixes are in the commit history and on slide 25.
- **Is it reproducible?** Partly. See the list below.
- **Can it use how genes interact?** Not in the VAE, which sees one gene at a time. You also tried an encoder-only transformer so that attention across genes could capture gene–gene interactions. That attempt is not in the repository, so be ready to say how genes were encoded as inputs, how large the model was, and what happened.

### Things in the repository that differ from the documentation

Know these before someone finds them.

1. **Loss.** The README and `references/3.0` describe a VAE with a KL term. `train_model_optuna_rosmap.py` still uses a KL term against a prior computed from the batch's own data. In batch mode the pseudo-bulk tensor is loaded but does not enter the loss. The newer idea, an MSE term tying the latent value to pseudo-bulk, is in `AE_model.py` and notebook `04`.
2. **DVC.** `dvc.yaml` declares commands and parameters but no dependencies or outputs, so DVC cannot track stage inputs. `dvc.lock` is stale: it records the older pancreas pipeline with 20 trials and different search ranges.
3. **Docs.** The first section of `references/2.0` is titled as `make_dataset_rosmap.py` but describes metadata extraction for the pancreas data (`create_metadata.py`).
4. **Paths and servers.** Notebooks and some scripts use absolute `/home/kunalt/...` paths, and the MLflow tracking URI is fixed at `http://0.0.0.0:8083`, so a local MLflow server must be running.
5. **Defaults.** `params.yaml` has `grad_clip: False` and `num_epochs: [10]`, so the current settings are small.
6. **Transformer attempt.** No code, notebook or commit message in any branch mentions a transformer or attention model. Find your code or notes before presenting. The slides describe the idea only.

---

## 6. Project 4: generative AI and a multi-agent compliance system

**Source:** your own project summary (five bullets) and the architecture diagram, `IMG-20260821-WA0025.jpg`. The image is a photo of a draw.io screen. It also shows browser tabs, a taskbar and internal file names, so do not show or send the photo itself. The slides are a clean redraw.
**Not in this folder:** the code, design documents, RAG implementation, CI/CD configuration and any evaluation results.
**Time to prepare:** about 2 hours. The folder has the least material for this project, so the preparation is mostly reading your own repository.

### In the 50-minute talk

**Slides 27–33, 9 minutes (0:28–0:37).** One message: an agentic system with retrieval, tools and observability.

| Part of the talk | Slides | Time |
|---|---|---|
| Divider with the three counts | 27 | 0.25 min |
| Scope and approach | 28 | 1 min |
| Architecture; practise this one, it takes the longest | 29 | 2.5 min |
| The RAG pipeline | 30 | 1.25 min |
| Multi-agent orchestration | 31 | 2 min |
| MCP tools | 32 | 1 min |
| Observability and delivery | 33 | 1 min |

**Say before anyone asks:** you have no outcome numbers on the slides yet, the domain is compliance rather than biomedicine, and be exact about your own share of the work.
**Minimum reading (about 30 minutes):** your project summary and the diagram, the graph file, the concrete RAG choices, and three outcome numbers you can quote.

### What to read, in order (in your own repository)

Folder names below come from the diagram. Adjust them if the real layout differs.

| # | Read | What to take from it |
|---|---|---|
| 1 | Your project summary and the architecture diagram | The story in one page: the five bullets and how the boxes in the diagram map to them. |
| 2 | The LangGraph graph definition (`backend/app/graph.py`) and the node files (`nodes/`) | How a request flows: router, LLM-routing orchestrator, `Send()` fan-out to the risk, ECCN and test-strategy nodes, and the Sentinel node. |
| 3 | `super_agents/` and `utility_agents/` | What each specialist and utility agent does, its inputs and what it returns. |
| 4 | `mcp_server/` | The tools exposed (SharePoint, GitHub), the transports (stdio and HTTP) and how authentication works. |
| 5 | `a2a_clients/a2a_bridge.py` and `a2a_servers/` | Why the specialist agents are also served over A2A, and how the bridge calls them. |
| 6 | `configs/`, especially the `get_llm()` factory | How models are chosen and how Amazon Bedrock is configured. |
| 7 | **Your RAG code:** ingestion, document processing, chunking, embeddings, the ANN index, retrieval, re-ranking, the output schema | **The diagram does not show these stages; they come from your summary.** Collect the concrete choices: embedding model, vector store or ANN index, chunk size and overlap, re-ranker, schema library. |
| 8 | The MLflow tracking and tracing code | What is logged for each request and how traces are viewed. |
| 9 | CI/CD, security-scanning and code-quality configuration | The tool names, and what blocks a deployment. |
| 10 | Deployment and platform configuration | EKS manifests, Cognito and API Gateway setup, the EventBridge schedule for Sentinel. |

Outside reading to be fluent: the LangGraph concepts (state graph, nodes, edges, the Send API), the Model Context Protocol (tools and transports), the Agent2Agent (A2A) protocol, MLflow Tracing, and Amazon Bedrock.

### Concepts you should be able to explain plainly

- **RAG:** retrieve relevant passages first, then have the LLM answer from them, so answers rest on evidence rather than memory.
- **ANN search:** approximate nearest-neighbour search finds similar vectors quickly across a large collection.
- **Re-ranking:** a second model reorders the retrieved candidates so the best passages reach the LLM.
- **Structured output validation:** the LLM's answer must match a schema before anything downstream uses it.
- **LangGraph and `Send()`:** a graph of steps with shared state. `Send()` dispatches several nodes in parallel.
- **MCP:** the Model Context Protocol gives agents one standard way to call tools such as SharePoint and GitHub.
- **A2A:** a protocol for one agent to call another over HTTP.
- **Sentinel node:** a graph node triggered on a schedule (EventBridge) rather than by a user request.
- **ECCN:** Export Control Classification Number, the export-control category assigned to an item.

### Numbers to have ready

The material has no outcome numbers, so collect them before presenting.

| Needed | Why | Where it might come from |
|---|---|---|
| Documents indexed and assessments run | Scale of use | Your index and request logs |
| Time per assessment | Practicality | MLflow traces and CloudWatch |
| Retrieval quality and answer quality | Whether it works | Any evaluation set or expert comparison you ran |
| Calls and cost per agent and per model | Operating cost | The MLflow API call counter |

What the diagram does give you: 3 specialist assessment agents, 4 utility agents, 4 MLflow observability signals (experiments, prompts, API call counts, errors) and 5 AWS platform services (EKS, Cognito, Secrets Manager, CloudWatch, EventBridge).

### Questions to expect

- **How do you know the answers are right?** Say what you measured. If there is no formal evaluation, say what you did instead and what you would add, such as a labelled set, retrieval hit rate or agreement with expert reviewers.
- **Why several agents rather than one prompt?** Each specialist has one job and its own tools, assessments run in parallel, and failures are easier to trace.
- **How do you limit hallucination in a compliance setting?** Describe what the system does: retrieved evidence, structured and validated output, tracing. Do not claim guarantees.
- **How is it secured?** Cognito OAuth2 and JWT validation at the API, keys in Secrets Manager, workloads on EKS. Add the CI/CD and security-scanning tools you used.
- **What does Sentinel do?** It runs on a schedule rather than from a chat request. Be ready to say what it checks.
- **How do agents reach SharePoint?** See the first item below.
- **Which parts did you build?** Be exact about your share.

### Things to reconcile before presenting

1. **SharePoint.** The diagram marks the URL loader utility agent as "SharePoint not supported", while your summary says MCP tools give access to SharePoint. Be ready to explain that SharePoint access goes through the MCP tools.
2. **Agent names.** Your summary names agents for document retrieval, ECCN and cryptography checks, risk assessment and final compliance evaluation. The diagram shows document loading as a utility agent and risk, ECCN and test strategy as specialists. Decide where the final compliance evaluation sits.
3. **RAG details.** The diagram does not show the vector store, embeddings, chunking or re-ranker. The RAG slide describes the stages generically for that reason.
4. **Partial diagram.** The photo cuts off at the MCP server area, so check the real diagram for anything below it.
5. **Confidentiality.** The diagram contains internal file names, ports and context names. The slides leave them out. Confirm you may present the architecture at all.
6. **Domain.** This is enterprise compliance, not biomedicine. Say how the approach transfers to biomarker or clinical analytics rather than leaving it implied.

---

## 7. How the four projects connect (two-minute story)

1. **Project 1** shows supervised multi-omic modelling on clinical outcomes: careful validation, honest limits, and biology traced back from the model.
2. **Project 2** shows NGS and epigenomics: raw reads to differential regulation, with replicate statistics and experimental validation.
3. **Project 3** shows building and operating deep learning on transcriptomics: simulation, a generative model, and reproducible tooling, with the validation gap stated openly.
4. **Project 4** shows building agentic generative AI for knowledge work: retrieval, orchestration, tools and observability, on a cloud platform.

The common thread is end-to-end ownership and stating what is and is not proven.

---

## 8. Before you present

- [ ] **ATAC-seq wording.** The chromatin paper is DNase-seq. Keep the wording "same workflow pattern as ATAC-seq".
- [ ] **Bash and Python.** This detail comes from you. Be ready to describe or show the scripts.
- [ ] **Proposed next steps.** Slides 12 and 26 are proposals, not completed work.
- [ ] **Permission to share.** Confirm you may show the paper PDFs, the Deconvolution repository details and loss figure, and the Project 4 architecture to this audience.
- [ ] **Role gaps.** The projects do not cover clinical-trial data or causal inference. Generative AI and agentic systems are covered by Project 4, in a compliance domain rather than biomedicine. Say so if asked, rather than stretch.
- [ ] **Project 4 numbers.** Collect outcome and evaluation numbers (section 6) before the talk.
- [ ] **Transformer attempt (Project 3).** It is not in the repository. Collect the design and any result, or present it as an idea that was tried.
- [ ] **Things not in the folder.** The ConnectOME code, the papers' supplementary files, the 27 GB of Deconvolution data and the Project 4 code are not here, so some numbers cannot be re-checked locally.
- [ ] **Rehearse against the clock.** Give the talk once, timed, using section 2A. If you run over, use the cut list in `description.md`.
- [ ] **Know your fixed points.** Clock targets 0:14, 0:20, 0:28 and 0:37; slides never to skip: 8, 12, 15, 17, 23, 25, 26, 29, 31 and 34.
- [ ] **Open the right deck.** Use `Full_Presentation_v4.pptx`. `Full_Presentation_v3.pptx`, `Full_Presentation_v2.pptx` and `Kunal_Tembhare_Full_Presentation.pptx` are earlier versions.

---

## 9. Glossary

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
| Transformer encoder | A neural network that uses self-attention so every input element can weigh every other. |
| DVC / MLflow / Optuna | Tools for data versioning, experiment tracking and hyperparameter search. |
| RAG | Retrieval-augmented generation: retrieve evidence, then generate from it. |
| ANN | Approximate nearest neighbour: fast similarity search over vectors. |
| LangGraph | A library for building LLM applications as graphs of steps with shared state. |
| MCP | Model Context Protocol: a standard way for agents to call tools. |
| A2A | Agent2Agent protocol: agents calling other agents over HTTP. |
| ECCN | Export Control Classification Number. |
| Bedrock / EKS / Cognito | Amazon's managed LLM service, Kubernetes service, and identity service. |
| JWT | JSON Web Token: a signed token used to authenticate API requests. |
