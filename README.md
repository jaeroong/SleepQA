# DL4H Reproduction Project

This repo is a fork from https://github.com/IvaBojic/SleepQA.

The purpose of this repo is to reproduce the results in the paper:

SleepQA: A Health Coaching Dataset on Sleep for Extractive Question Answering
https://proceedings.mlr.press/v193/bojic22a/bojic22a.pdf

# Dataset collection

The data and code are in the https://github.com/IvaBojic/SleepQA.

This repo will help on how to setup an environment and outlining requirements.

## Environment

Python 3.10

It's recommended to setup a new and separate environment using a tool such as conda.

## Requirements

Please refer to the requirements.txt

Some special libraries:

jnius
pip install pyjnius

pyserini
https://github.com/castorini/pyserini/
The repo says that Pyserini is built on Python 3.10 and Java 21. So you need Java 21.

I installed OpenJDK@21 using HomeBrew.  
It also needed to set JAVA_HOME. I set:  
export JAVA_HOME="/opt/homebrew/Cellar/openjdk@21/21.0.7/libexec/openjdk.jdk/Contents/Home"
export PATH="$JAVA_HOME:$PATH"

Additionally, the JAVA needs a library called anserini
https://github.com/castorini/anserini
You can follow the install instructions in the repo. Basically, you need to build this library and export the jar file. For example, I exported CLASSPATH:
export CLASSPATH="/yourpath/SleepQA/eval/anserini-1.0.0-fatjar.jar"

## Some updates to the code

Some edits to the path. In the original repo, eval/**main**.py, code path was set to:

json_folder = "/4TB/guest1/github/SleepQA/data/bm25_json/"
index_folder = "/4TB/guest1/github/SleepQA/data/bm25_indexes/"

But this will give an error. If you clone the repo, the data is included in the repo. So you want to update to:

json_folder = "../data/bm25_json/"
index_folder = "../data/bm25_indexes/"

Questions and respective answers were generated manually for a total of 5,000 randomly chosen passages. For each label, the question formed must start with one of the following words: who, what, where, when, why or how (i.e., factoid question). The question ends with a question mark. A text span from the passage is selected as answer.

## DPR setup

In the directory DPR-main, run:

python setup.py install

Then if you try to run dense_retiever.py, it will give an error:
Can't find model 'en_core_web_sm'. So you want to the following command:

python -m spacy download en_core_web_sm

https://stackoverflow.com/questions/54334304/spacy-cant-find-model-en-core-web-sm-on-windows-10-and-python-3-5-3-anaco

So first you want to download data by running a command:  
python dpr/data/download_data.py --resource data.retriever
This is explained in README.md
It will download a lot of data and will take a long time.

I experience the below error but could not resolve it.  
Another error is:
AttributeError: 'NoneType' object has no attribute 'seek'
