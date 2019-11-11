This page details evaluation of Nugget Detection (ND) and Dialogue Quality (DQ) subtasks, NTCIR14-STC3.

[The dataset](https://sakai-lab.github.io/stc3-dataset)



# Metrics

During the data annotaiton, we noticed that annotators' assessment on dialgoues are highly subjective and are hard to consolidate them into one gold label. Thus, we proposed to preserve the diverse views in the annotations “as is” and leverage them at the step of evaluation measure calculation.

Instead of juding whether the estimated label is equal to the gold label, we compare the difference between the estiamted distributions and the gold distributions calculaed by 19 anntators' annotations). Specifically, we employ these metrics for quality sub-task and nugget sub-task:

- Quality:
  - **NMD**: Normalised Match Distance. 
  - **RSNOD**: Root Symmetric Normalised Order-aware Divergence
- Nugget:
  - **RNSS**: Root Normalised Sum of Squared errors
  - **JSD**: Jensen-Shannon divergence


For the details about the metrics, please visit:

* [Slides for STC-3 Task (DQ and ND subtasks)](http://sakailab.com/wp-content/uploads/2018/06/STC3atNTCIR-14.pdf ) 

* [Comparing Two Binned Probability Distributions for Information Access Evaluation](https://waseda.app.box.com/v/SIGIR2018preprint).

To calculate these metrics for training set or your validation set, you may use the evaluation script [eval.py](https://github.com/sakai-lab/stc3-dataset/blob/master/data/eval.py). See details below (**Local Validation**).

#  Submission

You may submit your estimation to get official evaluation scores on the test set. For obvious reasons,  we do not release the annotations of the test set to the public. Instead, we require you to submit your prediction file for evaluation. 

To build a submission file, please put the estimated distributions and the corresponding IDs in a JSON dump file (please see [submission_example.json](https://github.com/sakai-lab/stc3-dataset/blob/master/data/submission_example.json)). For example: 

```json
{
    "id": "3636650070956277",
    "quality": {
      "A": {
        "2": 0.15,
        "1": 0.25,
        "0": 0.3,
        "-1": 0.2,
        "-2": 0.1
      },
      "S": {
        "2": 0.25,
        "1": 0.15,
        "0": 0.3,
        "-1": 0.2,
        "-2": 0.1
      },
      "E": {
        "2": 0.2,
        "1": 0.3,
        "0": 0.25,
        "-1": 0.1,
        "-2": 0.15
      }
    },
    "nugget": [
      {
        "CNUG0": 0.2,
        "CNUG": 0.5,
        "CNUG*": 0.2,
        "CNaN": 0.1
      },
      {
        "HNUG": 0.6,
        "HNUG*": 0.3,
        "HNaN": 0.1
      }
    ]
  },
```

If you are only interested in one subtask (either nugget detection  or dialogue quality), the submission JSON may include only `nugget` or `quality`. 

To avoid overfitting the test collection, the evaluation result obtained from the API is calcualted on a subset of test data (190 dialogues), and only the final evaluation step of STC3 will use the whole subset. Further more, each team can only evaluate their prediction for 50 times in total and 5 times per day.



To help participants submit their prediction results, we provide three **python3** scripts with the dataset:

* [create_team.py](https://github.com/sakai-lab/stc3-dataset/blob/master/data/create_team.py): To register your team on our test server.
* [submit.py](https://github.com/sakai-lab/stc3-dataset/blob/master/data/submit.py): To submit your prediction file and get evaluatoin scores.
* [eval.py](https://github.com/sakai-lab/stc3-dataset/blob/master/data/eval.py): To evaluate your training set locally.

To create a team using `create_team.py`:

```shell
python create_team.py --team_name "My Team" --team_info "ABC University XYZ Lab"
```

Here, `team_name` is a required field but `team_info` is optional. The `team_name`  must be unique and will be used to submit your prediction.  

To submit your prediction and get evaluation scores, run:

```shell
python submit.py --team_name "My Team" -s submission.json --language English
```

Note that please `language` is required to indicate which training data you used, either English or Chinese. If the submission succeed, the response  will look like:

```python
{   'nugget': {'jsd': 0.27933961311133393, 'rnss': 0.4151423073428835},
    'quality': {   'nmd': {   'A': 0.2022664001143675,
                              'E': 0.17681933392701513,
                              'S': 0.21453260324352053},
                   'rsnod': {   'A': 0.2823656602955875,
                                'E': 0.2540277798097729,
                                'S': 0.30573614235936863}}}
```

By default, the evaluation scores will be showed on our leaderboard to public. If you prefer not to show your scores, please use `--no_leaderboard` flag.

# Local Validation

To calculate metrics of the training set or the validtion set (a subset of the training set) locally, you may use the [evaluation script](https://github.com/sakai-lab/stc3-dataset/blob/master/data/eval.py):

```shell
python eval.py train_estimation.json train_data_en.json
```
