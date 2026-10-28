# Ar-WELFake-LIAR: An Arabic Machine-Translated Headline Corpus for Fake News Detection

**74,559 instances · Binary labels (Fake / Real) · Arabic · UTF-8 CSV**

## Description

This dataset is an Arabic headline-level corpus for binary fake news detection. It was built by cross-lingual transfer of two established English benchmarks, **WELFake** and **LIAR**.

It is the translated resource described in the article:

> *Cross-Lingual Data Transfer for Arabic Headline Misinformation Detection: Translation, Multi-Source Evaluation, and Ensemble Learning* (Albtoush, Gan, et al.)

---

## Files

| File | Description |
|------|-------------|
| `Welfake_Ar_preprocessed.csv` | Arabic WELFake headlines (cleaned) — 61,855 rows |
| `Liar_Ar_preprocessed.csv` | Arabic LIAR statements (cleaned) — 12,704 rows |
| `Arabic_Translated_Welfak_and_Liar_Dataset.csv` | Merged corpus (WELFake + LIAR) — 74,559 rows |

**Label coding (all files):** `1` = Fake, `0` = Real

---

## 1. Source Datasets

### WELFake (Verma et al., 2021)
- 72,134 English news articles (35,028 real, 37,106 fake), merged from Kaggle, McIntire, Reuters and BuzzFeed Political.
- Only the **title** field was kept.
- Records with empty or non-textual titles, or that produced a translation error, were removed. This left **62,154 titles** (27,818 fake, 34,336 real).
- Most removals were fake items, because empty titles are concentrated in the BuzzFeed Political and McIntire subsets.

### LIAR (Wang, 2017)
- 12,745 short political statements from PolitiFact, labelled on a six-level scale.
- Labels were collapsed into two classes:
  - **Fake:** pants-fire, false, barely-true
  - **Real:** half-true, mostly-true, true
- Result: **5,641 fake** and **7,104 real** statements.

---

## 2. Translation

| Source | Engine | Procedure |
|--------|--------|-----------|
| WELFake | Google Translate (`googletrans` Python library) | 72 chunks of ~1,000 titles (due to rate limits), merged back in original order |
| LIAR | Helsinki-NLP/opus-mt-en-ar (MarianMT) | Run locally in batch |

### Translation Quality Evaluation
Evaluated on a stratified sample of **200 items** (100 WELFake + 100 LIAR; 50 fake and 50 real each). Both engines translated the same items, and the outputs were compared using LaBSE cosine similarity, chrF, BLEU (sacreBLEU), 1,000 bootstrap replicates, and a numeric-preservation check.

| Metric | Google | Helsinki |
|--------|--------|----------|
| Mean LaBSE similarity | 0.888 | 0.826 |
| Numbers preserved | 93.5% | 95.5% |

- Agreement between the two engines was lower for WELFake headlines (chrF 47.6, BLEU 19.3) than for LIAR statements (chrF 63.1, BLEU 36.1).
- LaBSE did not differ systematically between classes (fake 0.8845, real 0.8911).

---

## 3. Arabic Cleaning and Normalisation

Both translated sources went through the same pipeline:

1. Removal of Arabic diacritics and tatweel
2. Letter normalisation
3. Removal of URLs, Latin-script tokens and digits
4. Removal of Arabic stop-words
5. Collapsing of three or more repeated characters
6. Whitespace normalisation

Rows were then removed if they:
- carried the translation-error marker
- were one character or less
- were exact duplicates (first occurrence kept)
- appeared with both a fake and a real label

---

## 4. Final Composition

| Source | Fake | Real | Total | Mean words |
|--------|------:|------:|------:|-----------:|
| Ar-WELFake | 27,651 | 34,204 | 61,855 | 9.87 |
| Ar-LIAR | 5,617 | 7,087 | 12,704 | 12.48 |
| **Total** | **33,268** | **41,291** | **74,559** | — |

No rows were removed when the two sources were merged.

---

## Citation

If you use this dataset, please cite:

> Albtoush, A., Gan, K. H., et al. *Cross-Lingual Data Transfer for Arabic Headline Misinformation Detection: Translation, Multi-Source Evaluation, and Ensemble Learning.* [Journal], [Year].

Please also cite the original datasets:
- Verma, P. K., Agrawal, P., Amorim, I., & Prodan, R. (2021). WELFake: Word embedding over linguistic features for fake news detection. *IEEE Transactions on Computational Social Systems*, 8(4), 881–893.
- Wang, W. Y. (2017). "Liar, liar pants on fire": A new benchmark dataset for fake news detection. *Proceedings of ACL 2017*, 422–426.

## License

Released under the **Creative Commons Attribution 4.0 (CC BY 4.0)** license.

## Contact

Asma Albtoush — [your email]
