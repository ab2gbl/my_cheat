
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
from transformers import AutoTokenizer

/
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)

sequence = "I've been waiting for a HuggingFace course my whole life."
model_inputs = tokenizer(sequence)
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1NTc2MTg4NjYsLTE5MTY5NjEyODUsOD
AyNzM5MjU1LDEwMzQyNzYzMTEsLTI3MzI1NjUwOSwtMTk1MTE4
Mjg0MiwtMTU4MTc4MDk3NiwxNTExODg4OTcxLDI5MTM2MTQzNS
w3MzA5OTgxMTZdfQ==
-->