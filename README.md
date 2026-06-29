# MHPM-Mandarin-Chinese-Homonym-Polyseme-Monoseme-Database

The MHPM database is an open Mandarin Chinese lexical ambiguity resource covering homonym, polyseme, and monoseme candidate words extracted and validated from Xiandai Hanyu Cidian, 7th edition.

MPHP 数据库是一个开放型的汉语普通话词汇歧义数据集，涵盖了从《现代汉语词典》（第7版）中提取并经严格筛选和检验后的汉语普通话同音词、多义词及单义词三大类词语。

Lexical ambiguity in Mandarin Chinese has long been an important topic in linguistics, psycholinguistics, and cognitive neuroscience research. A single written sinogram can be associated with multiple meanings, and different forms of lexical ambiguity raise important questions about how native speakers access, select, and represent word meaning in context. In this preprocessing pipeline, the distinction was operationalized using the entry and sense structure of **Xiandai Hanyu Cidian, 7th edition** 《现代汉语词典 第7版》: homonyms refer to words that share the same written form but have distinct, generally unrelated meanings, whereas polysemes refer to words with the same written form multiple semantically related senses. This operational distinction follows the theoretical contrast between homonymy and polysemy, and the actual dataset construction relies on the dictionary's entry/sense organization rather than manual semantic-relatedness judgments alone. 

The related neuroscience work has examined how the brain represents different types of lexical ambiguity, including fMRI evidence that homonyms and polysemous words can involve different neural patterns and cognitive-control demands, for example as shown in [Brain representations of lexical ambiguity: Disentangling homonymy, polysemy, and their meanings](https://www.sciencedirect.com/science/article/pii/S0093934X2400049X). At the same time, inrecent years, lexical ambiguity has become a useful test baseline for evaluating whether large language models and humans rely on similar mechanisms when processing meaning in context. Recent studies have connected homonymy with pre-trained language models and large language models, including [Exploring Layer-wise Representations of English and Chinese Homonymy in Pre-trained Language Models](https://aclanthology.org/2025.findings-acl.1011/) and [Context and POS in Action: A Comparative Study of Chinese Homonym Disambiguation in Human and Language Models](https://aclanthology.org/2025.emnlp-main.1404/). These studies highlight the importance of context, part of speech, and word-level psycholinguistic variables for understanding both human lexical processing and language model representations.

Despite this developing interest and research direction, existing Chinese lexical datasets still provide limited systematic coverage of different types of Mandarin ambiguous words. In particular, there remains a practical need for carefully cleaned and feature-matched sets of Mandarin Chinese homonyms, polysemes, and monosemes. To address this research need, this repository documents the initial preprocessing pipeline used to build candidate word sets from **Xiandai Hanyu Cidian, 7th edition**. The workflow starts from two-character dictionary entries and systematically extracts, cleans, and screens two major types of Mandarin ambiguous words, **homonyms** and **polysemes**, together with **monosemes** as a control set.

This repository organizes the earliest preprocessing, cleaning, feature matching, sense-count matching, statistical validation, and final candidate selection steps used for the project **A database of meaning-specific age-of-acquisition ratings for 562 Mandarin Chinese disyllabic homonyms**. The corresponded manuscript is currently under review and revision for *Behavior Research Methods*. The files here can be understood as the initial lexical-data construction and candidate words cleaning stage that also supports the later meaning specific age-of-acquisition rating database.

The earliest public version of the preprocessing text/code was released at [Choir-IG/homonym-monoseme-polyseme_data_preprocess](https://github.com/Choir-IG/homonym-monoseme-polyseme_data_preprocess). This repository reorganizes the local working code, project intro, source dictionary files, and generated outputs into a cleaner GitHub repository structure, with each processing stage and saved result placed in a consistent order.

The contributors and contact links for this repository are:

- Mr. WANG Wenbo: [https://github.com/Choir-IG](https://github.com/Choir-IG)
- Dr. XIE Chenwei: [https://github.com/PatrickTse512](https://github.com/PatrickTse512)
- Dr. Matthew King-Hang Ma (https://neurothew.github.io/): [https://github.com/neurothew](https://github.com/neurothew)

## Repository Structure

```text
code/
  01_dictionary_entry_cleaning_and_organization.ipynb
  02_feature_matching_statistics_and_final_selection.Rmd
docs/
  project_intro.docx
data/
  raw_source/
  intermediate/01_notebook_step1_cleaning_results/
  processed/
```

## Raw Source Data

The original downloaded/cleaned Xiandai Hanyu 7 two-character dictionary sources are stored in `data/raw_source/`. The folder contains the homonym, polyseme, and monoseme source CSV files. These files are the starting point for extracting Mandarin Chinese disyllabic entries before feature matching and final candidate selection.

## Code Files

- `code/01_dictionary_entry_cleaning_and_organization.ipynb`  
  Reordered notebook code for the dictionary-entry cleaning and organization stage. This corresponds to the initial organization of homonym, polyseme, and monoseme entries. Its saved Step 01 outputs are copied from the original local `test/` folder into `data/intermediate/01_notebook_step1_cleaning_results/`.

- `code/02_feature_matching_statistics_and_final_selection.Rmd`  
  Reordered R Markdown code for Steps 02-08. It keeps the feature filtering, normalization, cosine-similarity matching, statistical checks, backup candidate selection, and final selected-word export in execution order.

Before rerunning the code, replace every `CHANGE_THIS_TO_YOUR_PATH` placeholder with the local path on the target machine.

## Processing and Selection Workflow

| Step | Folder | Code source | Purpose | Main outputs |
|---|---|---|---|---|
| 01 | `data/intermediate/01_notebook_step1_cleaning_results/` | `01_dictionary_entry_cleaning_and_organization.ipynb` | Copy the notebook Step 01 saved outputs directly from the original local `test/` folder. These are the files generated before the Rmd Step 02 workflow. | `homonym_cleaned_output.xlsx`, `polyseme_cleaned_output.xlsx`, `monoseme_cleaned_output.xlsx`, `*_data_preprocess.xlsx`, and `*_pos_sense_example.xlsx`. |
| 02 | `data/processed/02_Data_Cleaning_Procedure/` | `01_dictionary_entry_cleaning_and_organization.ipynb` | Keep the three cleaned dictionary-entry tables used as the starting point for the Rmd analysis workflow. | `homonym_pos_sense_example_cleaned.xlsx`, `polyseme_pos_sense_example_cleaned.xlsx`, `monoseme_pos_sense_example_cleaned.xlsx`. |
| 03 | `data/processed/03_Data_Normalization_Procedure/` | `02_feature_matching_statistics_and_final_selection.Rmd` | Normalize the selected feature columns with z-scoring so matching and comparisons are on a common scale. | `*_features_91saved_filtered_normalized_z.xlsx`. |
| 04 | `data/processed/04_Top-k_selection_based_on_Cosine_Similarity/` | same Rmd | Compute cosine similarity and select candidate polyseme/monoseme controls for homonym candidates. | `similarity_topk_cosine_unique_assign_latestNorm.xlsx`. |
| 05 | `data/processed/05_91_Features_level_validation_via_t-tests/` | same Rmd | Validate candidate balance at the 91-feature level using t-tests. | `feature_level_ttests_hom_vs_poly_mono.xlsx`. |
| 06 | `data/processed/06_The_number_of_Sense_matched/` | same Rmd | Keep the final sense-count matching summary with polyseme and monoseme backup candidates. | `sense_counts_summary_WITH_poly3_mono_backups.xlsx`. |
| 07 | `data/processed/07_The_final_t-test_comparison_for_homonym_selected_polyseme_and_monoseme_candidate_words/` | same Rmd | Run final Welch and Wilcoxon comparisons for selected homonym, polyseme, and monoseme candidate words. | `final_Welch_candidate_words_ttest.xlsx`, `final_Wilcoxon_candidate_words_ttest.xlsx`. |
| 08 | `data/processed/08_Final_result/` | same Rmd | Assemble the final selected word tables. | `final_all_words_selected.xlsx`. |

## Final Selected Candidate Tables

The final selected-word workbook is:

```text
data/processed/08_Final_result/final_all_words_selected.xlsx
```

It contains:

- `final_homo`: 125 unique homonym headwords, represented by 450 entry/sense/example rows.
- `final_poly`: 125 unique polyseme headwords, represented by 497 entry/sense/example rows.
- `final_mono`: 333 unique monoseme headwords, represented by 585 entry/sense/example rows.
- `settings`: provenance notes for the final assembly step.

These candidate sets are not the final AoA rating database itself. They document the initial lexical cleaning, candidate screening, and control-word selection work that supports the later meaning-specific AoA rating study.

## Result Notes

- The Step 01 files are produced by the notebook workflow.
- The original notebook Step 01 saved outputs from the local `test/` folder are stored in `data/intermediate/01_notebook_step1_cleaning_results/`.
- Steps 02-08 are produced or reconstructed from the R Markdown workflow and its saved outputs.
