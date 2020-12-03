
# Named-Entity Recognition Application

This project is to recognize named-entities in Chinese language. There are three types of named entities in the dataset, including person, organization and location.

<img src="https://github.com/uabinf/nlp-group-project-fall-2020-cner/blob/main/image/cner_example.png" width="800"/>

# Model

Bi-LSTM Model for NER (Named-Entity Recognition):

<img src="https://github.com/uabinf/nlp-group-project-fall-2020-cner/blob/main/image/bi-lstm.png" width="500"/>

ID-CNNs(Iterated Dilated Convolutional Neural Networks) Model is a faster alternative to Bi-LSTMs for NER: 

<img src="https://github.com/uabinf/nlp-group-project-fall-2020-cner/blob/main/image/id-cnns.png" width="800"/>

# Execution
### Clone CNER
Copy/Paste this command in your local terminal to clone this project:
```
git@github.com:uabinf/nlp-group-project-fall-2020-cner.git
```

### Enviroment setup
These commands are only for the UAB Cheaha server (https://docs.uabgrid.uab.edu/wiki/cheaha). <br>
Please do not execute the following commands on the Login Node! You are supposed to request a resource from Cheaha, and 'ssh' to the resource nodes!

Copy/Paste this command in your local terminal to load Anaconda module:
```
module load Anaconda3/2020.07
```
Copy/Paste this command in your local terminal to create Environment:
```
conda env create -f environment.yml
```
Copy/Paste this command in your local terminal to check your environment list, after create environment sucessfully. The environment called "nlp" is the environment for this project.
```
conda env list
```

Submit a job on Cheaha server (https://rc.uab.edu/pun/sys/myjobs). <br>
You will get a log file `/jupyter-log-pascal-<log_id>.txt`. <br>
These commands can be found  in the log file to run jupyter notebook on Cheaha. <br>

Copy/Paste this in your local terminal to ssh tunnel with remote:
```
ssh -L <ip adddress> <your username>@cheaha.rc.uab.edu
```
Copy/Paste this in brower to open jupyter notebook:
```
[I 16:30:27.170 NotebookApp] The Jupyter Notebook is running at:
[I 16:30:27.170 NotebookApp] http://<ip address>/?token=<token_id>
```


# Dataset

Dataset includes 27818 news title, which is from Chinese People's Daily (中国人民日报).<br>
70% samples for training data, 13% samples for evaluation data, 17% samples for testing data.<br>
For each sample, one Chinese character is a token with IOB tagging.

Sample Instance: 
```
[['因', 'O'], ['此', 'O'], ['，', 'O'], ['这', 'O'], ['次', 'O'], ['政', 'O'], ['府', 'O'], ['危', 'O'], ['机', 'O'], ['终', 'O'], ['于', 'O'], ['得', 'O'], ['到', 'O'], ['化', 'O'], ['解', 'O'], ['，', 'O'], ['对', 'O'], ['俄', 'B-LOC'], ['罗', 'I-LOC'], ['斯', 'I-LOC'], ['来', 'O'], ['说', 'O'], ['是', 'O'], ['值', 'O'], ['得', 'O'], ['庆', 'O'], ['幸', 'O'], ['的', 'O'], ['。', 'O']]
```


# Predicted Results
The predicted results are stored in the file ```predict_result.txt``` under the floder ```./dataset/```. <br>
In this file, first column is the chinese characters, second column is the correct tags, and the third column is the predicted tags. <br>
There is an empty line between each sentence.

For example:
```
辽 B-LOC B-LOC
宁 I-LOC I-LOC
南 O O
部 O O
温 O O
泉 O O
众 O O
多 O O
。 O O

大 B-LOC O
庆 I-LOC O
这 O O
一 O O
步 O O
， O O
走 O O
得 O O
好 O O
！ O O

中 B-LOC O
法 B-LOC O
文 O O
化 O O
研 O O
讨 O O
会 O O
今 O O
天 O O
在 O O
京 B-LOC B-LOC
举 O O
行 O O
。 O O
```
The predict results are very good for the entities which contains more than one chinese characters, but not so good for the phrase which only has one character.

# Training Result
For each training epoch, there would generate a training report, which includes the overall accracy, precision, recall, fscore, and the precision, recall, fscore for each entity type (LOC, PRE, ORG). <br>

**F1>0.80: need 4 epochs (Cheaha,  NVIDIA Tesla P100 16GB), 130s.**
For example:

<img src="https://github.com/uabinf/nlp-group-project-fall-2020-cner/blob/main/image/training_result.png" width="600"/>

# The created Bi-LSTM Model 
## Model Structure
There are two inputs of the created Bi-LSTM model. The first input layer is designed for the encoded Chinese chars. The second input layer is for the encoded Chinese Phrases. This design enables Bi-LSTM model take advantage of the Chinese Phrases features. 

<img src="https://github.com/uabinf/nlp-group-project-fall-2020-cner/blob/main/image/bi-lstm%20model.png" width="400"/>

## Model Parameters

<img src="https://github.com/uabinf/nlp-group-project-fall-2020-cner/blob/main/image/Bi-lstm-hyperp.png" width="600"/>

## Training Result

**F1>0.80: need 101 epochs (Cheaha,  NVIDIA Tesla P100 16GB), 7272s.**

<img src="https://github.com/uabinf/nlp-group-project-fall-2020-cner/blob/main/image/bestF1_Bi_lstm.jpg" width="800"/>


# Paper Reference
```
[1] Strubell, Emma, et al. "Fast and accurate entity recognition with iterated dilated convolutions." arXiv preprint arXiv:1702.02098 (2017).
[2] Yu, Fisher, and Vladlen Koltun. "Multi-scale context aggregation by dilated convolutions." arXiv preprint arXiv:1511.07122 (2015).
[3] Yoon, Wonjin, et al. "Collabonet: collaboration of deep neural networks for biomedical named entity recognition." BMC bioinformatics 20.10 (2019): 249.
```
