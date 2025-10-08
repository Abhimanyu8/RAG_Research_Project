# The Tesla RAG Robustness Corpus (TRC-22)

![Dataset Version](https://img.shields.io/badge/version-1.0.0-blue)
![Documents](https://img.shields.io/badge/documents-22-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)
[![Paper](https://img.shields.io/badge/paper-coming_soon-purple)](https://#)

## Abstract

As Retrieval-Augmented Generation (RAG) models become more prevalent in answering complex, knowledge-based queries, their vulnerability to subtle misinformation poses a significant research challenge. A robust RAG system must not only retrieve relevant context but also verify its factual accuracy before synthesis. This repository contains the **Tesla RAG Robustness Corpus (TRC-22)**, a specialized dataset designed to evaluate the verifiability and adversarial resilience of RAG models. The corpus focuses on the well-documented domain of Tesla, Inc., and provides a balanced set of factually correct and sophisticatedly misleading documents.

## Dataset Overview

The TRC-22 corpus is a curated collection of English-language documents designed for evaluating RAG models. It is structured to test a model's ability to distinguish between factual ground truth and plausible, adversarially crafted falsehoods.

* **Topic Domain:** Tesla, Inc. (history, technology, products, manufacturing)
* **Total Documents:** 22
* **Corpus Split:**
    * **11 Benign Documents:** Factually correct, up-to-date, and verifiable.
    * **11 Adversarial Documents:** Plausible but contain specific, intentional falsehoods.

## Motivation & Research Goal

The primary goal of this dataset is to provide a standardized benchmark for RAG model evaluation beyond simple retrieval accuracy. Researchers can use TRC-22 to investigate questions such as:
1.  Can a RAG model's retriever distinguish between documents containing true and false information when they are semantically similar?
2.  Does the generator component blindly trust and propagate misinformation from a retrieved context?
3.  How do different adversarial attack vectors affect a model's final output?
4.  Can verifiability or fact-checking modules be effectively tested using this corpus?

## Directory Structure

The corpus is organized into two main directories, separating the ground truth from the adversarial examples.

```
.
└── corpus/
    ├── adversarial/
    │   ├── 006_sfm_model-s-plaid-speed.md
    │   ├── 007_sfm_roadster-battery.md
    │   ├── ... (and other adversarial files)
    │   └── 022_sp_robotaxi-network.md
    └── benign/
        ├── 001_benign_cybertruck-design.md
        ├── 002_benign_supercharger-network.md
        ├── ... (and other benign files)
        └── 019_benign_tesla-semi-truck.md
```

## Document & Filename Schema

Each document is a Markdown file containing YAML front matter for metadata, followed by the text content. This structure is designed to be easily machine-readable.

### YAML Front Matter

* `id`: A unique integer ID for the document.
* `type`: The document category (`benign` or `adversarial`).
* `attack_vector`: The specific manipulation technique used (`null` for benign, or one of the adversarial types).
* `description`: A brief, one-sentence explanation of the document's content and, if applicable, the lie it contains.

### Filename Convention

Filenames follow a consistent `[ID]_[TYPE]_[TOPIC].md` format for easy sorting and identification.

* **[ID]:** A three-digit, zero-padded number (`001`, `002`, ...).
* **[TYPE]:** An abbreviation for the document type (`benign`, `sfm`, `ci`, `sp`).
* **[TOPIC]:** A short, kebab-case summary of the content (e.g., `model-y-success`).

## Adversarial Attack Vectors

The adversarial documents employ three distinct and increasingly sophisticated attack vectors:

### 1. Subtle Fact Manipulation (SFM)
These documents contain minor, plausible errors in numbers, dates, or specifications that a non-expert would likely miss.
* **Example:** Changing a vehicle's 0-60 mph time from 1.99s to a still-fast but incorrect 2.5s.

### 2. Context Injection (CI)
These documents are mostly (80-90%) factually correct. However, a single, contradictory, and false sentence is embedded ("injected") within the truthful context to hide it.
* **Example:** A correct description of Tesla's over-the-air updates that includes a false sentence claiming they require a service center visit for calibration.

### 3. Semantic Paraphrasing (SP)
This is a sophisticated attack that rephrases a clear falsehood using technical or official-sounding language. The goal is to create a document that is semantically close to true concepts, designed to fool vector-based retrievers.
* **Example:** Instead of "Autopilot is self-driving," the document claims it has "achieved a regulatory certification for Level 5 autonomy" for "unattended operation."

## Usage Example

A typical evaluation workflow using this dataset would be:

1.  **Ingest Knowledge:** Use the documents in `corpus/benign/` as the trusted knowledge base for your RAG system's retriever.
2.  **Formulate Queries:** Create a set of questions whose answers can be found in the documents. For example, "What is the towing capacity of the Model X?"
3.  **Evaluate Robustness:** Query the RAG system.
    * For a query related to an adversarial document (e.g., `020_sfm_model-x-towing.md`), does the model correctly answer with information from the benign corpus, or does it retrieve and parrot the adversarial information (e.g., the incorrect towing capacity)?

## Citation

If you use this dataset in your research, please cite our work.

```bibtex
@inproceedings{avimanyu2025trc,
  title={{The Tesla RAG Robustness Corpus (TRC-22): A Dataset for Evaluating Verifiability in Generative Models}},
  author={Avimanyu Dutta},
  year={2025},
  address={Guwahati, Assam, India}
}
```

## License

This dataset is licensed under the [MIT License](LICENSE).
