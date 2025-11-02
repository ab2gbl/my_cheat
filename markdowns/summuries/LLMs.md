
# LLMs Architectures  


Model | Examples |Tasks
|--|--|--|
**Encoder-only** | BERT, DistilBERT, ModernBERT | Sentence classification, named entity recognition, extractive question answering
**Decoder-only** | GPT, LLaMA, Gemma, SmolLM | Text generation, conversational AI, creative writing 
**Encoder-decoder** | BART, T5, Marian, mBART | Summarization, translation, generative question answering


![transformers_architecture](./pics/LLMs/transformers_architecture.png)

# LLM pipeline
## Full LLM pipeline
![Full LLM pipeline](./pics/LLMs/full_nlp_pipeline-dark.svg)
> **Tokenizer types** : Word-based,  Character-based, Subword tokenization
## inside model pipe
![inside model pipeline](./pics/LLMs/transformer_and_head-dark.svg)

## transformers Library

```python
import torch
from transformers import AutoTokenizer, AutoModelForSequenceClassification

# load tokenizer and model
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
model = AutoModelForSequenceClassification.from_pretrained(checkpoint)

# sentance to tokes
sequence = "I've been waiting for a HuggingFace course my whole life."
model_inputs = tokenizer(sequence)
# or model_inputs = tokenizer(sequences, padding="max_length", max_length=8)
# or model_inputs = tokenizer(sequences, max_length=8, truncation=True)

# return as tensors
model_inputs = tokenizer(sequences, padding=True, return_tensors="pt") # pt: pytorch tensors 

# pass input to model 
output = model(**tokens)
```
# Fine-tuning 
## Processing the data
-   Use  `batched=True`  with  `Dataset.map()`  for significantly faster preprocessing
-   Dynamic padding with  `DataCollatorWithPadding`  is more efficient than fixed-length padding
-   Always preprocess your data to match what your model expects (numerical tensors, correct column names)
-   The 🤗 Datasets library provides powerful tools for efficient data processing at scale
```python
from datasets import load_dataset
from transformers import AutoTokenizer, DataCollatorWithPadding

# loading tokenizer and dataset
raw_datasets = load_dataset("glue", "mrpc")
checkpoint = "bert-base-uncased"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

# tokenizing func
def tokenize_function(example):
    return tokenizer(example["sentence1"], example["sentence2"], truncation=True)
    
# run with map and batched for processes multiple examples at once, making tokenization much faster. 
tokenized_datasets = raw_datasets.map(tokenize_function, batched=True)
# padding for max leght in batch
data_collator = DataCollatorWithPadding(tokenizer=tokenizer)
```
## Fine-tuning a model with the Trainer API
- after doing preprocess like last section
```python
from transformers import Trainer
from transformers import AutoModelForSequenceClassification

model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)

trainer = Trainer(
    model,
    training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator, # default is DataCollatorWithPadding
    processing_class=tokenizer,
)
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQ4ODg4MTE3MSwyMjQ1NjU3NTEsMTg4Nz
k5MDEwNCwxNDU0NDI5OTU3LC0xOTE2OTYxMjg1LDgwMjczOTI1
NSwxMDM0Mjc2MzExLC0yNzMyNTY1MDksLTE5NTExODI4NDIsLT
E1ODE3ODA5NzYsMTUxMTg4ODk3MSwyOTEzNjE0MzUsNzMwOTk4
MTE2XX0=
-->