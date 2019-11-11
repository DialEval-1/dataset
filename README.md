
# Dataset Overview
The Chinese dataset contains 4,090 (3,700 for training + 390 for development)  customer-helpdesk dialgoues which are crawled from [Weibo](weibo.com). All of these dialogues are annotated by 19 annotators.

The English dataset contains 2062 dialogues (2, 251 for training + 390 for development)  are manually translated from a subset of the Chinese dataset. The English dataset shares the same annotations with the Chinese dataset.

- Training set
  -  train_cn.json (3,700 dialogues)
  -  train_en.json (2,251 dialogues)

- Development set
  - dev_cn.json (390 dialogues)
  - dev_en.json (390 dialogues)


### Annotators

We hired 19  Chinese students from the department of Computer Science, Waseda University to annotate this dataset.

# Format of the JSON file

Each file is in JSON format with UTF-8 encoding. 

Following are the top-level fields:

- **id**
- **turns**: array of turns from the customer and the helpdesk (see details below)
- **annotations**: a list of annotations provided by 19 annotators. Each annotation consists of two fields: **nugget** and **quality**

Each element of the turns field contains the following fields:

- **sender**: the speaker of this turn (either customer or helpdesk)
- **utterances**: the utterances (may be multiple) they sent in this turn. Note that some utterances are empty strings since we didn't crawl emoji and photos.

Each element of **annotations** contains the following fields:

- **nugget**: The list of nugget types for each turn (see details below).

- **quality**: A dictonary consists of the subjetive dialogue quality scores: A-score, S-score, and E-score (see details below).


### Nugget Types

- CNUG0: Customer trigger (problem stated)
- CNUG*: Customer goal (solution confirmed)
- HNUG*: Helpdesk goal (solution stated)
- CNUG: Customer regular nugget
- HNUG: Helpdesk regular nugget
- CNaN: Customer Not-a-Nugget
- HNaN: Helpdesk Not-a-Nugget

<img src="https://i.postimg.cc/TPqH4ttz/nugget-example.png" alt="drawing" style="width:600px;"/>



### Dialogue Quality

- A-score: Task **A**ccomplishment (Has the problem been solved? To what extent?) 

- S-score: Customer **S**atisfaction of the dialogue (not of the product/service or the company) 

- E-score: Dialogue **E**ffectiveness (Do the utterers interact effectively to solve the problem efficiently?) 

Scale: [2, 1, 0, -1, -2]




# Evaluation

### Metrics

During the data annotaiton, we noticed that annotators' assessment on dialgoues are highly subjective and are hard to consolidate them into one gold label. Thus, we proposed to preserve the diverse views in the annotations “as is” and leverage them at the step of evaluation measure calculation.

Instead of juding whether the estimated label is equal to the gold label, we compare the difference between the estiamted distributions and the gold distributions calculaed by 19 anntators' annotations). Specifically, we employ these metrics for quality sub-task and nugget sub-task:

- Quality:
  - **NMD**: Normalised Match Distance. 
  - **RSNOD**: Root Symmetric Normalised Order-aware Divergence
- Nugget:
  - **RNSS**: Root Normalised Sum of Squared errors
  - **JSD**: Jensen-Shannon divergence


For the details about the metrics, please vistit:

* [NTCIR-15 Dialogue Evaluation Task Definition ](http://sakailab.com/wp-content/uploads/2019/10/dialeval1taskdef.pdf ) 

* [Comparing Two Binned Probability Distributions for Information Access Evaluation](https://waseda.app.box.com/v/SIGIR2018preprint).

###  Test and Submission

 Will be released later.



# Conditions and Terms

See [terms]
