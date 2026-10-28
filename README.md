Ar-WELFake-LIAR: An Arabic Machine-Translated Headline Corpus for Fake News Detection (74,559 Instances)

Description
This dataset is an Arabic headline-level corpus for binary fake news detection. It was built by cross-lingual transfer of two established English benchmarks, WELFake and LIAR. It is the translated resource described in the article “Cross-Lingual Data Transfer for Arabic Headline Misinformation Detection: Translation, Multi-Source Evaluation, and Ensemble Learning” (Albtoush, Gan, et al.)
1. Source datasets
WELFake (Verma et al., 2021). 72,134 English news articles (35,028 real, 37,106 fake) merged from Kaggle, McIntire, Reuters and BuzzFeed Political. Only the title field was kept. Records with empty or non-textual titles, or that produced a translation error, were removed, leaving 62,154 titles (27,818 fake, 34,336 real). The removals fell mostly on fake items, because empty titles are concentrated in the BuzzFeed Political and McIntire subsets.
LIAR (Wang, 2017). 12,745 short political statements from PolitiFact, labelled on a six-level scale. The labels were collapsed into two classes: Fake (pants-fire, false, barely-true) and Real (half-true, mostly-true, true). This gave 5,641 fake and 7,104 real statements.
2. Translation

WELFake titles were translated into Arabic with Google Translate via the googletrans Python library. Because of rate limits, they were translated in 72 chunks of about 1,000 titles each and then merged back in the original order. LIAR statements were translated with the open-source Helsinki-NLP/opus-mt-en-ar model (MarianMT), run locally in batch.
Translation quality was evaluated on a stratified sample of 200 items: 100 from WELFake and 100 from LIAR, each with 50 fake and 50 real. Both engines translated the same items, and the outputs were compared with LaBSE cosine similarity, chrF, BLEU (sacreBLEU), 1,000 bootstrap replicates and a numeric-preservation check.
Mean LaBSE: 0.888 for Google and 0.826 for Helsinki.
Agreement between the two engines was lower for WELFake headlines (chrF 47.6, BLEU 19.3) than for LIAR statements (chrF 63.1, BLEU 36.1).
Numbers were preserved in 93.5% of Google outputs and 95.5% of Helsinki outputs.
LaBSE did not differ systematically between classes (fake 0.8845, real 0.8911).

3. Arabic cleaning and normalisation
Both translated sources went through the same pipeline:

removal of Arabic diacritics and tatweel
letter normalisation
removal of URLs, Latin-script tokens and digits
removal of Arabic stop-words
collapsing three or more repeated characters 
whitespace normalisation 

Rows were then removed if they:
carried the translation-error marker 
were one character or less
were exact duplicates (the first occurrence was kept)
appeared with both a fake and a real label

4. Final composition

Source	Fake	Real	Total	Mean words
Ar-WELFake	27,651	34,204	61,855	9.87
Ar-LIAR	5,617	7,087	12,704	12.48
Total	33,268	41,291	74,559	

no rows were removed when they were merged.


All CSV files are UTF-8 encoded, and every file uses the same label coding: 1 = Fake, 0 = Real.

