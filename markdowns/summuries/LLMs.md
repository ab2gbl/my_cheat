
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

# load token
checkpoint = "distilbert-base-uncased-finetuned-sst-2-english"
tokenizer = AutoTokenizer.from_pretrained(checkpoint)
# sentance to tokes
sequence = "I've been waiting for a HuggingFace course my whole life."
model_inputs = tokenizer(sequence)
# or model_inputs = tokenizer(sequences, padding="max_length", max_length=8)
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY2Mjg4Njk0MywtMTkxNjk2MTI4NSw4MD
I3MzkyNTUsMTAzNDI3NjMxMSwtMjczMjU2NTA5LC0xOTUxMTgy
ODQyLC0xNTgxNzgwOTc2LDE1MTE4ODg5NzEsMjkxMzYxNDM1LD
czMDk5ODExNl19
-->