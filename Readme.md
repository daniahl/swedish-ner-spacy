# Swedish NER for spaCy

This repository contains scripts to train and evaluate a Swedish Named Entity Recognition (NER) model in spaCy based on the [Swedish NER corpus](https://github.com/klintan/swedish-ner-corpus) published by Andreas Klintberg.

The scores below were obtained after 100 iterations (evaluation on the test set):

    ents_p: 78.46743295019158, ents_r: 76.4179104477612, ents_f: 77.42911153119093
    ents_per_type: {'PER': {'p': 90.61488673139159, 'r': 92.56198347107438, 'f': 91.57808667211775}, 'MISC': {'p': 34.73684210526316, 'r': 26.82926829268293, 'f': 30.275229357798167}, 'LOC': {'p': 74.78260869565217, 'r': 82.42811501597444, 'f': 78.419452887538}, 'ORG': {'p': 70.04048582995951, 'r': 57.859531772575245, 'f': 63.369963369963365}}
Here, `p` = precision, `r` = recall, `f` = F1 score.

# Usage
## Getting the Data
Download training and test sets from here: https://github.com/klintan/swedish-ner-corpus

## Converting the Data
Since the downloaded data are in Stanford CoreNLP format, it needs to be converted to spaCy JSON format.

The convert.py script in this repository is based on https://gist.github.com/DataTurks/97ff613967e8139e57091f9299c3a104

Modifications were made to keep Swedish characters in the output file, and to work properly with the specific data sets mentioned above.

To convert the data, run:
* `python convert.py train_corpus.txt train.json 0`
* `python convert.py test_corpus.txt test.json 0`

where 0 is the label used for non-entity tokens.

## Training and Evaluating the Model (randomly initialised word vectors)
Use the `train.py` script, e.g.

* `python train.py -o sv_ner_100 -n 100` -- train a new model for 100 iterations, saving it with the name sv_ner_100
* `python train.py -m sv_ner_100 -o sv_ner_200 -n 100` -- continue training sv_ner_100 for 100 more iterations, saving it to sv_ner_200.

The script assumes training and test data in the files `train.json` and `test.json`, respectively, as generated in the previous step.

## Training and Evaluating the Model (using pretrained fastText word vectors)
Download the Swedish fastText vectors in text format from here: https://dl.fbaipublicfiles.com/fasttext/vectors-crawl/cc.sv.300.vec.gz. Unpack the file to the same directory as the scripts.

Use the `train_vec.py` script following the syntax described in the previous section. The script will populate the model vocabulary with the fastText vectors prior to training the recogniser.
