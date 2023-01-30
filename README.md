# NER_methods_evaluation
## Table of Contents : 
1. Used Dependencies 
2. Steps Done to prepreprocess the data 
3. Models used for the extracting the NER
4. Metrics I used for models evaluation
5. Models scores
## Used dependencies 
- BeautifulSoup for web scrapping
- re for matching regular expressions
- spacy to import statistical and transformer-based NER models
## Steps done to preprocess the data 
1. Getting clean text from every <p> tag without any further tags, hence we can pass it to the models to get the Named entities
2. Getting the sentences with their NER tags
3. We can get the entities for every sentence from the output of the last step
4. Now we have the ground truth entities and the text itself , then we have to pass this pure text into the 2 NER models we have
5. Use the output of the last steps along with the ground truth to evaluate Both models

## Metrics I used for models evaluation

According to this [link](https://stackoverflow.com/questions/1783653/computing-precision-and-recall-in-named-entity-recognition) , they stated that : In the CoNLL-2003 NER task, the evaluation was based on correctly marked entities, not tokens, as described in the paper 'Introduction to the CoNLL-2003 Shared Task: Language-Independent Named Entity Recognition'. An entity is correctly marked if the system identifies an entity of the correct type with the correct start and end point in the document, So the criteria I followed in calculating true positives , false positive and False negatives is the following : he way precision and recall is typically computed (this is what I use in my papers) is to measure entities against each other. Supposing the ground truth has the following (without any differentiaton as to what type of entities they are)
[Microsoft Corp.] CEO [Steve Ballmer] announced the release of [Windows 7] today
This has 3 entities.
Supposing your actual extraction has the following
[Microsoft Corp.] [CEO] [Steve] Ballmer announced the release of Windows 7 [today]
You have an exact match for Microsoft Corp, false positives for CEO and today, a false negative for Windows 7 and a substring match for Steve
We compute precision and recall by first defining matching criteria. For example, do they have to be an exact match? Is it a match if they overlap at all? Do entity types matter? Typically we want to provide precision and recall for several of these criteria.
Exact match: True Positives = 1 (Microsoft Corp., the only exact match), False Positives =3 (CEO, today, and Steve, which isn't an exact match), False Negatives = 2 (Steve Ballmer and Windows 7)
Precision = True Positives / (True Positives + False Positives) = 1/(1+3) = 0.25.

## Important note :
When I get the output from the 2 NER models, I considered only the entities that have labels of "ORG" , "PER" , "LOC" ,"GPE"  , and I treat  "LOC" or "GPE" as "LOC"

## Models scores
After passing our samples to the 2 models , we get the named entities for every model along with having the ground truth , so I iterated over every sample and copmared the annotations with the models predictions , then I got the total true positives , false positive and False negatives , then I calculated the scores which are the following :
- For the statistical model : Precision: 0.69, Recall: 0.63 ,f1_score: 0.662
- for the transformer-based model :Precision: 0.79, Recall: 0.72, f1_score: 0.75

