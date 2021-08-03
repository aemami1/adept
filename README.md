# ADEPT: An Adjective Dependent Plausibility Task
# AUTHORS: Ali Emami, Ian Porada, Alexandra Olteanu, Kaheer Suleman, Adam Trischler, and Jackie Chi Kit Cheung

# Abstract:

A false contract is more likely to be rejected than a contract is, yet a false key is less likely than a key to open doors. While correctly interpreting and assessing the effects of such adjective-noun pairs (e.g., false key) on the plausibility of given events (e.g., opening doors) underpins many natural language understanding tasks, doing so often requires a significant degree of world knowledge and common-sense reasoning. We introduce ADEPT -- a large-scale semantic plausibility task consisting of over 16 thousand sentences that are paired with slightly modified versions obtained by adding an adjective to a noun. Overall, we find that while the task appears easier for human judges (85% accuracy), it proves more difficult for transformer-based models like RoBERTa (71% accuracy). Our experiments also show that neither the adjective itself nor its taxonomic class suffice in determining the correct plausibility judgement, emphasizing the importance of endowing automatic natural language understanding systems with more context sensitivity and common-sense reasoning.

# Reproduce Results

**ADEPT_Dataset** contains the dataset itself, as well as a datasheet with many details as provided in Gebru et al, 2018.

**Reproduce_Model_Results** contains code to reproduce results of Bert, RoBERTa, and DeBERTa (or any model on the HuggingFace Hub--https://huggingface.co/models--) on ADEPT

The code for extracting (noun, adjective) bigrams from a depedency parsed corpus is located at [github.com/ianporada/conllu-amod-extract](https://github.com/ianporada/conllu-amod-extract)
