# Datasheet: ADEPT

Author: Ali Emami (lead author) 

Other research team member(s): Ian Porada, Alexandra Olteanu, Kaheer Suleman, Adam Trischler, and Jackie Chi Kit Cheung

Organization(s): Mila and McGill University


## Motivation

*The questions in this section are primarily intended to encourage dataset creators to clearly articulate their reasons for creating the dataset and to promote transparency about funding interests.*

1. **For what purpose was the dataset created?** Was there a specific task in mind? Was there a specific gap that needed to be filled? Please provide a description.

	The ADEPT dataset was created to support explorations into how certain classes of adjectives might influence the plausibility of events depicted in natural language sentences. 
	
	For example, given an event depicted in a sentence like "_A monkey turns on a light switch_," the dataset tries capture how the plausibility of the event changes if the sentence is altered by adding an adjective in front of the noun like in "_A [dead] monkey turns on a light switch_." To operationalize changes in plausability, the dataset uses a 5-point bipolar scale including: impossible, less likely, equally likely, more likely, and necessarily true.  In our example, the addition of the adjective [_dead_] makes the event _impossible_ (in most scenarios). 

2. **Who created this dataset (e.g. which team, research group) and on behalf of which entity (e.g., company, institution, organization)**?

	The dataset was created as part of a research project at Mila and McGill University. The research team includes members of the Reasoning and Learning Lab at McGill University and collaborators from Microsoft Research Montreal.

3. **What support was needed to make this dataset?** (e.g., who funded the creation of the dataset? If there is an associated grant, provide the name of the grantor and the grant name and number, or if it was supported by a company or government agency, give those details.)

	This work was supported by the Natural Sciences and Engineering Research Council of Canada and Microsoft Research. The PI, Jackie Chi Kit Cheung, is also supported by the Canada CIFAR AI Chair program, and a consulting researcher for Microsoft Research.

4. **Any other comments?**
	
	We hope the ADEPT dataset will help others test language models more extensively by also checking how well these models capture the effects of adjectival modifiers, and that it will contribute to the ongoing efforts to better testing for context sensitivity and common-sense reasoning. We also hope that it will provide the foundation for others both to iterate and construct complementary datasets that employ different strategies to operationalize and capture the events plausability, or to offer critiques towards how to better build such datasets.  

## Composition

*Dataset creators should read through the questions in this section prior to any data collection and then provide answers once collection is complete. Most of these questions are intended to provide dataset consumers with the information they need to make informed decisions about using the dataset for specific tasks. The answers to some of these questions reveal information about compliance with the EU’s General Data Protection Regulation (GDPR) or comparable regulations in other jurisdictions.*

1. **What do the instances that comprise the dataset represent (e.g., documents, photos, people, countries)?** Are there multiple types of instances (e.g. movies, users, and ratings; people and interactions between them; nodes and edges)? Please provide a description.

	The dataset entries consist of sentence pairs (including an original sentence and its' modified version) generated to capture simple events, along categorical labels for each pair that characterize the change in plausability between the original sentence and the modified version. 

2. **How many instances are there in total (of each type, if appropriate)?**

	Overall, the dataset includes 16,115 instances of sentence pairs.

3. **Does the dataset contain all possible instances or is it a sample (not necessarily random) of instances from a larger set?** If the dataset is a sample, then what is the larger set? Is the sample representative of the larger set (e.g. geographic coverage)? If so, please describe how this representativeness was validated/verified. If it is not representative of the larger set, please describe why not (e.g. to cover a more diverse range of instances, because instances were withheld or unavailable).

	The dataset does not contain all possible instances from a larger set (i.e., all possible sentences along all their possible modified versions obtained by adding possible adjectives in front of the nouns). 
	
	The dataset is also not representative of this larger set, including due to non-subsective adjectives being oversampled and due to biases in the underlying datasets from which possible nouns were extracted (e.g., Wikipedia) and the knowledge base from which possible predicates were extracted (i.e., ConceptNet). Furthermore, the predicates that relate to the noun in each sentence were restricted to a number of pre-selected relations from ConceptNet, and thus also not representative of all possible predicates in ConceptNet. Finally, the nouns are extracted from English Wikipedia and the Common Crawl which are also not representative of either all people, all written text, or all types of text.

4. **What data does each instance consist of?** "Raw" data (e.g., unprocessed text or images) or features? In either case, please provide a description.

	Each data instance consists of generated sentences obtained by combining nouns extracted from e.g., Wikipedia and possible predicated about those nouns extracted from i.e., ConceptNet. The dataset also tracks which modifier was used to alter a give sentence, which noun the modifier is added to, and the plausability change assessment. An example instance is: 
	
	{<br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence1": "A secretary writes a letter.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence2": "A deputy secretary writes a letter.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "modifier": "deputy", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "noun": "secretary", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "label": 3, <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "idx": 10263 <br />
	}
	
	Specifically, this dataset was sampled from an intermediary dataset where each instance consisted of an "original" sentence and four modified sentences; all constructed automatically from a noun, a list of four adjectives, and a predicate, where the original sentence excludes the adjectives and the modified sentences are a copy of the original sentence but with the adjectives added in front of the noun. The adjectives and nouns pairs were extracted based on their co-occurance on Wikipedia and the Common-Crawl, keeping only the pairs with nouns that co-occured frequently with at least four different adjectival modifiers. For each noun ConceptNet was used to find predicates under the ConceptNet relations: IsCapableOf, HasProperty, ReceivesAction, HasA, and UsedFor. 
	
	For example, the original sentence in an instance would be: "A photo explains what happened", where the noun is "photo", the predicate is "explain what happened", and based on a list of four modifiers ["fake", "2nd", "historic", "autographed"], the modified sentences would then be: "A [fake] photo explained what happened", "A [2nd] photo explaine what happened", etc. A full example from this intermediate data format is: 

	{<br />
		&nbsp;&nbsp;&nbsp;&nbsp; "original_sentence": "A photo explains what happened.", <br />
			&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; "alternate1": "A [fake] photo explains what happened.", <br />
			&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; "alternate2": "A [2nd] photo explains what happened.", <br />
			&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; "alternate3": "A [historic] photo explains what happened.", <br />
			&nbsp;&nbsp;&nbsp;&nbsp;  &nbsp;&nbsp;&nbsp;&nbsp; "alternate4": "An [autographed] photo explains what happened.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "surfaceText": "A photo explains what happened.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "noun": "photo", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "predicate": "explain what happened", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "original_modifiers": ["fake", "2nd", "historic", "autographed"], <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "labelled_plausibilities": ["None", "None", "None", "None"] <br />
	} 

5. **Is there a label or target associated with each instance?** If so, please provide a description.

	Yes, each instance included in the dataset is labeled according to possible plausibility changes from the original sentence to a given modified version of that sentence. The labels correspond to a 5-point bipolar scale from impossible, less likely, equally likely, more likely, and necessarily true. 


6. **Is any information missing from individual instances?** If so, please provide a description, explaining why this information is missing (e.g., because it was unavailable). This does not include intentionally removed information, but might include, e.g. redacted text.

	No information is missing from individual instances, but during the automatic construction of the sentence some ConceptNet predicates where changed for e.g., tense compatibility and other grammer issues. For example, the predicate "explain what happened" was modified to "explains what happened" in order to fit with the singular noun, photo, in the resulting sentence.

7. **Are relationships between individual instances made explicit (e.g., users' movie ratings, social network links)?** If so, please describe how these relationships are made explicit.

	N/A.

8. **Are there recommended data splits (e.g., training, development/validation, testing)?** If so, please provide a description of these splits, explaining the rationale behind them.

	The dataset is provided with pre-determined random splits of 80%/10%/10% for training, development/validation, and testing. While the dataset was split at random to preserve label distribution, several measures were taken to diversify the examples themselves during preprocessing (e.g., see Section 4.1 in the accompanying ACL paper).

9. **Are there any errors, sources of noise, or redundancies in the dataset?** If so, please provide a description.

	Several measures were taken to remove errors, typos, and other such sources of noise, but some errors may remain. Many errors originate with the predicates extracted from ConceptNet or were introduced during preprocessing when the predicates and adjective-noun pairs were combined to produce sentences.

10. **Is the dataset self-contained, or does it link to or otherwise rely on external resources (e.g., websites, tweets, other datasets)?** If it links to or relies on external resources, a) are there guarantees that they will exist, and remain constant, over time; b) are there official archival versions of the complete dataset (i.e., including the external resources as they existed at the time the dataset was created); c) are there any restrictions (e.g., licenses, fees) associated with any of the external resources that might apply to a future user? Please provide descriptions of all external resources and any restrictions associated with them, as well as links or other access points, as appropriate.

	The dataset is self-contained.

11. **Does the dataset contain data that might be considered confidential (e.g., data that is protected by legal privilege or by doctor-patient confidentiality, data that includes the content of individuals' non-public communications)?** If so, please provide a description.

	N/A. The nouns, modifiers and predicates originate from public data sources like Wikipedia, Common Crawl, and ConceptNet5.

12. **Does the dataset contain data that, if viewed directly, might be offensive, insulting, threatening, or might otherwise cause anxiety?** If so, please describe why.

	During pre- and post- processing we filtered out adjectives and nouns that were, for instance, explicit or that might have offensive connotations using preset lists and automated tools (e.g., profanity filters). Despite best efforts, such instances might remain. 

13. **Does the dataset relate to people?** If not, you may skip the remaining questions in this section.

	The dataset is not specifically about people, but it may relate to people in two ways: 1) the labels are selected by crowd judges, and 2) people are sometimes references in the sentences, though the references are mostly generic (e.g., an old man, a president, a qualified photographer). 

14. **Does the dataset identify any subpopulations (e.g., by age, gender)?** If so, please describe how these subpopulations are identified and provide a description of their respective distributions within the dataset.

	Among the possible adjectives, nouns, and predicates, there are some that identify or allude to specific subpopulations (e.g., based on age or gender). Such examples include:
	
	{<br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence1": "A ribbon is for a female human's hair.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence2": "A favorite ribbon is for a female human's hair.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "modifier": "favorite", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "noun": "ribbon", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "label": 2, <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "idx": 13239 <br />
	}
	
	{<br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence1": "A smile warms an old man's heart.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence2": "A superior smile warms an old man's heart.", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "modifier": "superior", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "noun": "smile", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "label": 2, <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "idx": 12427 <br />
	} 

15. **Is it possible to identify individuals (i.e., one or more natural persons), either directly or indirectly (i.e., in combination with other data) from the dataset?** If so, please describe how.

	No, identifying individuals should not be possible. For instance, there should be no mentions of specific individuals (via names or aliases) in the dataset.

16. **Does the dataset contain data that might be considered sensitive in any way (e.g., data that reveals racial or ethnic origins, sexual orientations, religious beliefs, political opinions or union memberships, or locations; financial or health data; biometric or genetic data; forms of government identification, such as social security numbers; criminal history)?** If so, please provide a description.

	We tried to filter out many adjectives and nouns that might be construed as ethnic/cultural/explicit/genderized or that are explicit or that have offensive connotations using preset lists and automated tools (e.g., profanity filters). However not all sentences that include such terms are problematic, and thus some are still included in the dataset; like the following example:

	{ <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence1": "A christian goes astray.",  <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "sentence2": "A devoted christian goes astray.",  <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "modifier": "devoted",  <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "noun": "christian", <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "label": 1, <br />
		&nbsp;&nbsp;&nbsp;&nbsp; "idx": 1768 <br />
	} 


17. **Any other comments?**

	

## Collection

*As with the previous section, dataset creators should read through these questions prior to any data collection to flag potential issues and then provide answers once collection is complete. In addition to the goals of the prior section, the answers to questions here may provide information that allow others to reconstruct the dataset without access to it.*

1. **How was the data associated with each instance acquired?** Was the data directly observable (e.g., raw text, movie ratings), reported by subjects (e.g., survey responses), or indirectly inferred/derived from other data (e.g., part-of-speech tags, model-based guesses for age or language)? If data was reported by subjects or indirectly inferred/derived from other data, was the data validated/verified? If so, please describe how.

	The dataset was indirectly derived by using part-of-speech tagging to extract nouns and adjectives from Wikipedia and the Common Crawl, while the predicates these nouns and adjectives were combined with originate from ConceptNet. 

2. **What mechanisms or procedures were used to collect the data (e.g., hardware apparatus or sensor, manual human curation, software program, software API)?** How were these mechanisms or procedures validated?

	Automated scripts and existing libraries were used to extract the necessary components (nouns, adjectives, predicates) to create the data instances. These components were then manually curated (to label instances, to inspect and remove errors, as well as to assess quality; see Section 4.1. in the accompanying ACL paper).  In addition, adjectives and nouns that might be e.g., explicit or have offensive connotations using preset lists and automated tools (e.g., profanity filters). For crowdsourcing, several pilots were run before the final annotation to adjust instructions and construct tests to assess the annotation quality. More details about the quality assessment are available in Section 4.2 of the [accompanying ACL paper](https://add_paper_url_here 'ADAPT').  

3. **If the dataset is a sample from a larger set, what was the sampling strategy (e.g., deterministic, probabilistic with specific sampling probabilities)?**

	In addition to the constraints we put on the frequency of the nouns and (adjective, noun) pairs, described earlier, we also sampled adjectives to ensure a higher prevalence of non-subsective adjectives than it occurs in existing text corpora. 
	
	Specifically, we identify non-subsective adjectives using a set of 60 non-subsective adjectives identified by ([Nayak et al, 2014](https://cs.stanford.edu/~angeli/papers/2014-tr-adjectives.pdf 'Nayak et al, 2014')). Then for each noun, we selected four adjectives by first 1) randomly selecting up to two non-subsective modifiers if they co-occured with the noun, and then 2) randomly selecting the remaining adjectives from the list of subsective modifiers. The selection process for the four adjectives is as follows: 1) randomly select up to two non-subsective modifiers if they are within the list of co-occuring modifiers 2) for the remaining modifiers, randomly select from the list of subsective modifiers.  We over-sample non-subsective modifiers as they occur much more sparsely in the corpora and we want to evaluate their effects in comparison to other modifiers. This sampling strategy resulted in an about 1:4 non-subsective to subsective adjective ratio (as some entries have no non-subsective adjectives), allowing us to analyze the effect of non-subsective modifiers while maintaining an element of randomness.

4. **Who was involved in the data collection process (e.g., students, crowdworkers, contractors) and how were they compensated (e.g., how much were crowdworkers paid)?**
	
	Crowdworkers from Amazon Mechanical Turk annotated the dataset. The crowdworkers were paid $0.12 USD for each completed HIT (including 4 paired sentences). The median/mean completion time was 34.0/38.7 seconds, which amounts to an expected hourly wage of $11.16 USD based on mean completion time. We also restricted our tasks to Master workers only (who performed well on past tasks on AMT), but collected no demographic data. 


5. **Over what timeframe was the data collected?** Does this timeframe match the creation timeframe of the data associated with the instances (e.g., recent crawl of old news articles)? If not, please describe the timeframe in which the data associated with the instances was created. Finally, list when the dataset was first published.

	The dataset was created around December 2020-February 2021. In addition, the timeframe for the underlying datasets is:
	* A version of English Wikipedia created on 2020/04/01 (16.9 GB, xml.bz2); and represents a subset of the dataset decribed in ([Panchenko et al., 2018](https://arxiv.org/abs/1710.01779) 'Panchenko et al., 2018'). 
	* The Common Crawl corpus was created on September-October, 2017. See https://commoncrawl.org/ for details.
	* A version of ConceptNet5 available on March 2020. See https://conceptnet.io/ for details.  

6. **Were any ethical review processes conducted (e.g. by an institutional review board)?** If so, please provide a description of these review processes, including the outcomes, as well as a link or other access point to any supporting documentation.

	An IRB was obtained at McGill University, which reviewed study details related to the purpose of research, recruitment details, study methodology and procedures, potential harms and risks, privacy/confidentiality, and that informed consent was obtained from crowdsourcing judges. The IRB approval is accessible as a pdf in the current folder as IRB_Approval.pdf. 

7. **Does the dataset relate to people?** If not, you may skip the remainder of the questions in this section.

	Not directly. The dataset is constructed around nouns, adjectives and predicates extracted from existing text corpora (largely written by people). The dataset does not aim to describe people, but any lingustic phenomena is social phenomena. 

8. **Did you collect the data from the individuals in question directly, or obtain it via third parties or other sources (e.g., websites)?**
	
	No, we used existing text corpora from Wikipedia, the Common Crawl, and ConceptNet.  

9. **Were the individuals in question notified about the data collection?** If so, please describe (or show with screenshots or other information) how notice was provided, and provide a link or other access point to, or otherwise reproduce, the exact language of the notification itself.
	
	No. 

10. **Did the individuals in question consent to the collection and use of their data?** If so, please describe (or show with screenshots or other information) how consent was requested and provided, and provide a link or other access point to, or otherwise reproduce, the exact language to which the individuals consented.
	
	N/A

11. **If consent was obtained, were the consenting individuals provided with a mechanism to revoke their consent in the future or for certain uses?** If so, please provide a description, as well as a link or other access point to the mechanism (if appropriate).
	
	N/A

12. **Has an analysis of the potential impact of the dataset and its use on data subjects (e.g., a data protection impact analysis) been conducted?** If so, please provide a description of this analysis, including the outcomes, as well as a link or other access point to any supporting documentation.
	
	The accompanying ACL paper includes a reflection of the possible broader impact of the dataset. 

13. **Any other comments?**



## Preprocessing / Cleaning / Labeling

*Dataset creators should read through these questions prior to any pre-processing, cleaning, or labeling and then provide answers once these tasks are complete. The questions in this section are intended to provide dataset consumers with the information they need to determine whether the “raw” data has been processed in ways that are compatible with their chosen tasks. For example, text that has been converted into a “bag-of-words” is not suitable for tasks involving word order.*

1. **Was any preprocessing/cleaning/labeling of the data done (e.g., discretization or bucketing, tokenization, part-of-speech tagging, SIFT feature extraction, removal of instances, processing of missing values)?** If so, please provide a description. If not, you may skip the remainder of the questions in this section.

	Yes, there was a combination of all three: preprocessing, cleaning, and annotation (described in the previous section and in the accompanying ACL paper).

2. **Was the "raw" data saved in addition to the preprocessed/cleaned/labeled data (e.g., to support unanticipated future uses)?** If so, please provide a link or other access point to the "raw" data.

	Only the resulting dataset is provided in this GitHub Repo. The text corpora used to extract the nouns, adjectives and predicates include English Wikipedia, the Common Crawl corpus (see https://commoncrawl.org/ for details), and ConceptNet5 (see https://conceptnet.io/ for details).  

3. **Is the software used to preprocess/clean/label the instances available?** If so, please provide a link or other access point.

	Yes, the scripts used to preprocess/clean/generate the instances included in this dataset are also provided in this GitHub Repo.

4. **Any other comments?**



## Uses

*These questions are intended to encourage dataset creators to reflect on the tasks  for  which  the  dataset  should  and  should  not  be  used.  By  explicitly highlighting these tasks, dataset creators can help dataset consumers to make informed decisions, thereby avoiding potential risks or harms.*

1. **Has the dataset been used for any tasks already?** If so, please provide a description.

	The dataset has only been used for the event plausibility assessment task (described in the accompanying ACL dataset). We might provide pointers to future uses of our dataset by others at ________.

2. **Is there a repository that links to any or all papers or systems that use the dataset?** If so, please provide a link or other access point.

	Yes, we might provide pointers to future papers or systems that use the dataset at _______.

3. **What (other) tasks could the dataset be used for?**
	
	Examples of other tasks (although there are potentially many more) include information extraction and recognizing textual entailment.


4. **Is there anything about the composition of the dataset or the way it was collected and preprocessed/cleaned/labeled that might impact future uses?** For example, is there anything that a future user might need to know to avoid uses that could result in unfair treatment of individuals or groups (e.g. stereotyping, quality of service issues) or other undesirable harms (e.g. financial harms, legal risks) If so, please provide a description. Is there anything a future user could do to mitigate these undesirable harms?

	While the goal for our dataset is to enable others to explore the effects of modifiers and how these effect might impact various inference tasks, users of this dataset should acknowledge possible biases and should not use it to make deployment decisions or rule out failures. This dataset should be used only for research and exploratory evaluation and auditing. 

5. **Are there tasks for which the dataset should not be used?** If so, please provide a description.

	Given that the dataset is not representative and that current models do not perform well enoungh on it, this dataset should not, for instance, be used for in credibility assessment. It would be problematic if a system would reproduce highly harmful stereotypes by inferring that a [black] witness is less likely to be trustworthy than just a witness or that an [old] applicant is less likely to be a productive employee than just an applicant, so users should be wary of these potential effects. 

6. **Any other comments?**

	Another application of plausibility inferences is perhaps information extraction for which one should also be very careful about the instances that identify subpopulations like the ones exampled above. Such instances might for instance harmfully reinforce the belief that the prototypical human is male (Menegatti and Rubini, 2017), if female is deemed as more likely to change the plausibility of events about e.g., doctors, scientists, or other professionals; and thus deemed a relevant (or not) detail to surface based on it. 


## Distribution

*Dataset creators should provide answers to these questions prior to distributing the dataset either internally within the entity on behalf of which the dataset was created or externally to third parties.*

1. **Will the dataset be distributed to third parties outside of the entity (e.g., company, institution, organization) on behalf of which the dataset was created?** If so, please provide a description.

	Yes, via the current github repository. 

2. **How will the dataset will be distributed (e.g., tarball on website, API, GitHub)?** Does the dataset have a digital object identifier (DOI)?

	The dataset will be distributed via GitHub. The dataset does not have a DOI.

3. **When will the dataset be distributed?**

	As of August 01, 2021.

4. **Will the dataset be distributed under a copyright or other intellectual property (IP) license, and/or under applicable terms of use (ToU)?** If so, please describe this license and/or ToU, and provide a link or other access point to, or otherwise reproduce, any relevant licensing terms or ToU, as well as any fees associated with these restrictions.

	See license information in the github repository. 

5. **Have any third parties imposed IP-based or other restrictions on the data associated with the instances?** If so, please describe these restrictions, and provide a link or other access point to, or otherwise reproduce, any relevant licensing terms, as well as any fees associated with these restrictions.

	See license information in the github repository. 

6. **Do any export controls or other regulatory restrictions apply to the dataset or to individual instances?** If so, please describe these restrictions, and provide a link or other access point to, or otherwise reproduce, any supporting documentation.

	No.

7. **Any other comments?**


## Maintenance

*As with the previous section, dataset creators should provide answers to these questions prior to distributing the dataset. These questions are intended to encourage dataset creators to plan for dataset maintenance and communicate this plan with dataset consumers.*

1. **Who is supporting/hosting/maintaining the dataset?**

	The lead author, Ali Emami.

2. **How can the owner/curator/manager of the dataset be contacted (e.g., email address)?**

	For any questions, please email: aemami@brocku.ca

3. **Is there an erratum?** If so, please provide a link or other access point.

	There is no erratum available at the moment, but one might be added if any errors or issues are found in the future.  

4. **Will the dataset be updated (e.g., to correct labeling errors, add new instances, delete instances)?** If so, please describe how often, by whom, and how updates will be communicated to users (e.g. mailing list, GitHub)?

	There is no current plan to update the dataset (and thus no timeline can be provided for it). If there will any updates to the dataset in the future, they will be communicated and highlighted in the same GitHub repository.

5. **If the dataset relates to people, are there applicable limits on the retention of the data associated with the instances (e.g., were individuals in question told that their data would be retained for a fixed period of time and then deleted)?** If so, please describe these limits and explain how they will be enforced.

	No, but if we make corrections to the dataset, those corrections will be communicated in this github repository. 

6. **Will older versions of the dataset continue to be supported/hosted/maintained?** If so, please describe how. If not, please describe how its obsolescence will be communicated to users.

	If the dataset were to be updated and older versions become obsolute, this will be communicated in the dataset documentation in this github repository.

7. **If others want to extend/augment/build on/contribute to the dataset, is there a mechanism for them to do so?** If so, please provide a description. Will these contributions be validated/verified? If so, please describe how. If not, why not? Is there a process for communicating/distributing these contributions to other users? If so, please provide a description.

	While there is no plan to do so, extensions/augmentations/contributions to the dataset by others will be validated or verified only if we decide to add them to this original github repository. No other process for communicating these contributions are in place at this moment.  

8. **Any other comments?**

