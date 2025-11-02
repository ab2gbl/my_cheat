
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
## Fine-tuning a model
###  With the Trainer API
-   The  `Trainer`  API provides a high-level interface that handles most training complexity
-   Use  `processing_class`  to specify your tokenizer for proper data handling
-   `TrainingArguments`  controls all aspects of training: learning rate, batch size, evaluation strategy, and optimizations
-   `compute_metrics`  enables custom evaluation metrics beyond just training loss
-   Modern features like mixed precision (`fp16=True`) and gradient accumulation can significantly improve training efficiency
```python
from transformers import Trainer
from transformers import AutoModelForSequenceClassification
# after doing preprocess like last section
# making the  model from checkpoint
model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)

# config the trainer
trainer = Trainer(
    model,
    training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator, # default is DataCollatorWithPadding
    processing_class=tokenizer,
)

# start fine-tuning
trainer.train()
```
#### evaluation 

```python
def compute_metrics(eval_preds):
    metric = evaluate.load("glue", "mrpc")
    logits, labels = eval_preds
    predictions = np.argmax(logits, axis=-1)
    return metric.compute(predictions=predictions, references=labels)

training_args = TrainingArguments("test-trainer", eval_strategy="epoch")
model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)

trainer = Trainer(
    model,
    training_args,
    train_dataset=tokenized_datasets["train"],
    eval_dataset=tokenized_datasets["validation"],
    data_collator=data_collator,
    processing_class=tokenizer,
    compute_metrics=compute_metrics,
)
```
#### Advanced features
```python
training_args = TrainingArguments(
	"test-trainer",
	eval_strategy="epoch",
	# Enable mixed precision ( faster training and reduced memory usage )
    fp16=True,
	# Effective batch size = 4 * 4 = 16
	per_device_train_batch_size=4,
	gradient_accumulation_steps=4,  
	# Try different schedulers  
	learning_rate=2e-5,
	lr_scheduler_type="cosine",  
)
```
### Manually with full training loop:
1. **pre process:** like last section
2. **post process:**
```python
# postProcessiong dataset to fit our training
tokenized_datasets = tokenized_datasets.remove_columns(["sentence1", "sentence2", "idx"]) # Remove the columns corresponding to values the model does not expect
tokenized_datasets = tokenized_datasets.rename_column("label", "labels") # The model expects the argument to be named `labels`
tokenized_datasets.set_format("torch") # return PyTorch tensors instead of lists
tokenized_datasets["train"].column_names
# ["attention_mask", "input_ids", "labels", "token_type_ids"]
```
3. **define our dataloaders:**
```python
from torch.utils.data import DataLoader

train_dataloader = DataLoader(
    tokenized_datasets["train"], shuffle=True, batch_size=8, collate_fn=data_collator
)
eval_dataloader = DataLoader(
    tokenized_datasets["validation"], batch_size=8, collate_fn=data_collator
)

# quick check 
for batch in train_dataloader:
    break
{k: v.shape for k, v in batch.items()}

'''
result: {'attention_mask': torch.Size([8, 65]),
 'input_ids': torch.Size([8, 65]),
 'labels': torch.Size([8]),
 'token_type_ids': torch.Size([8, 65])}
'''
```
3. **define model:**
```python
from transformers import AutoModelForSequenceClassification

model = AutoModelForSequenceClassification.from_pretrained(checkpoint, num_labels=2)

outputs = model(**batch)
print(outputs.loss, outputs.logits.shape)
# tensor(0.5441, grad_fn=<NllLossBackward>) torch.Size([8, 2])
```
4. define the **optimiser** and a **learning rate scheduler**: 
```python
from torch.optim import AdamW
from transformers import get_scheduler
# optimzer
optimizer = AdamW(model.parameters(), lr=5e-5)
'''
**Modern Optimization Tips**: For even better performance, you can try:
-   **AdamW with weight decay**:  `AdamW(model.parameters(), lr=5e-5, weight_decay=0.01)`
-   **8-bit Adam**: Use  `bitsandbytes`  for memory-efficient optimization
-   **Different learning rates**: Lower learning rates (1e-5 to 3e-5) often work better for large models
'''
# learning rate scheduler
num_epochs = 3
num_training_steps = num_epochs * len(train_dataloader)
lr_scheduler = get_scheduler(
    "linear",
    optimizer=optimizer,
    num_warmup_steps=0,
    num_training_steps=num_training_steps,
)

```
5. **Define training:**
```python
from tqdm.auto import tqdm

progress_bar = tqdm(range(num_training_steps))

model.train()
for epoch in range(num_epochs):
    for batch in train_dataloader:
        batch = {k: v.to(device) for k, v in batch.items()}
        outputs = model(**batch)
        loss = outputs.loss
        loss.backward()

        optimizer.step()
        lr_scheduler.step()
        optimizer.zero_grad()
        progress_bar.update(1)
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY3Njc1Mzg0MCwxMTkwNTU4Mzc4LDIwND
QyNDQ2MTUsMTg5MTMzMTgwNCwtNDUyMTM5MDIsLTIwODk0NzAy
MzUsLTE5NTIxMjA2OTIsLTc1MTE0NjcxMywyMjQ1NjU3NTEsMT
g4Nzk5MDEwNCwxNDU0NDI5OTU3LC0xOTE2OTYxMjg1LDgwMjcz
OTI1NSwxMDM0Mjc2MzExLC0yNzMyNTY1MDksLTE5NTExODI4ND
IsLTE1ODE3ODA5NzYsMTUxMTg4ODk3MSwyOTEzNjE0MzUsNzMw
OTk4MTE2XX0=
-->