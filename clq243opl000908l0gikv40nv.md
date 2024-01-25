---
title: "Local Embeddings with LangChain."
datePublished: Tue Dec 12 2023 09:00:09 GMT+0000 (Coordinated Universal Time)
cuid: clq243opl000908l0gikv40nv
slug: local-embeddings-with-langchain
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702365617621/aadb0bf2-c730-48f1-945c-88a24a1993c6.png
tags: langchain, embeddings, ollama, local-embeddings

---

Update: Thursday 25<sup>th</sup> January 2024.

## TL;DR.

The post discusses generating local embeddings with LangChain. Embeddings address (some of the) memory limitations in Large Language Models (LLMs). I demonstrate an embedding implementation using various AI tools. Using embeddings for long-term memory management has potential applications in many fields, including business, science, and engineering.

> **Attributions:**
> 
> * [https://www.youtube.com/watch?v=CPgp8MhmGVY](https://www.youtube.com/watch?v=CPgp8MhmGVY)
>     

## An Introduction.

In the fascinating world of AI, Large Language Models (LLMs) hold a special place due to their impressive ability to understand and generate human-like responses. However, their static memory, limited to what they were trained on, poses certain challenges. To tackle these limitations, I turn to using LangChain to create local embeddings. In this post, I delve deep into this innovative solution, demonstrating how to implement embeddings using tools like Ollama, Llama2, bs4, GPT4All, Chroma, and LangChain itself.

> The purpose of this post is to present a way for LLMs to use locally generated embeddings.

## The Big Picture.

LLMs (large language models) have static knowledge, limited to what they learned up until the day they were trained and compiled. They cannot retain new information beyond their base knowledge, meaning they only know what they've been taught and nothing more.

Embeddings (partly) resolve one of the known LLM memory issues.

## Prerequisites.

* A Linux-based distro (I use Ubuntu),
    
* Python 3.9 or later, and
    
* [Anaconda](https://solodev.app/installing-anaconda).
    

## Updating the System.

* I update the system:
    

```bash
sudo apt clean && sudo apt update && sudo apt dist-upgrade -y && sudo apt autoremove -y
```

## Creating an Anaconda Environment.

* I create an Anaconda environment called (langchain):
    

```bash
conda create -n langchain
```

* I activate the (langchain) environment:
    

```bash
conda activate langchain
```

## Installing Ollama.

* I install Ollama:
    

```bash
curl https://ollama.ai/install.sh | sh
```

## Installing Llama2.

* I install Llama2:
    

```bash
ollama pull llama2
```

## Installing LangChain.

* I install LangChain:
    

```bash
pip install langchain
```

## Creating a Python Module.

* I create a Python module called `langchain-test.py`:
    

```bash
sudo nano ./langchain-test.py
```

* I add the following to the `langchain-test.py` module, save the file, and exit Nano:
    

```python
from langchain.llms import Ollama
ollama = Ollama(base_url='http://localhost:11434', model='llama2')
print(ollama('Why is the sky blue?'))
```

* I run the `langchain-test.py` module:
    

```bash
python3 langchain-test.py
```

## Installing More Requirements.

* I install bs4:
    

```bash
pip install bs4
```

* I install GPT4All:
    

```bash
pip install gpt4all
```

* I install Chroma:
    

```bash
pip install chromadb
```

## Creating Another Python Module.

* I create another Python module called `langchain-ollama.py`:
    

```bash
sudo nano langchain-ollama.py
```

* I add the following to the `langchain-ollama.py` module:
    

```python
# Import Ollama
from langchain.llms import Ollama
# Import the document loader
from langchain.document_loaders import WebBaseLoader
# Import the text splitter
from langchain.text_splitter import RecursiveCharacterTextSplitter
# Import the GPT4All embeddings tool
from langchain.embeddings import GPT4AllEmbeddings
# Import the vector store
from langchain.vectorstores import Chroma
# Import chains
from langchain.chains import RetrievalQA

ollama = Ollama(base_url='http://localhost:11434', model='llama2')

# Load the document from a URL
loader = WebBaseLoader('https://solodev.app/12s-my-2024-technology-stack')
# Load the document contents as 'data'
data = loader.load()
# Define the 'text_splitter' params
text_splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
# Split the 'data'
all_splits = text_splitter.split_documents(data)
# Instantiate the vector store
vectorstore = Chroma.from_documents(documents=all_splits, embedding=GPT4AllEmbeddings())
# Use chains to link tasks
qachain = RetrievalQA.from_chain_type(ollama, retriever=vectorstore.as_retriever())

question = "What technologies will be used in 2024?"
print(qachain({"query": question}))
```

* I run the `langchain-ollama.py` module:
    

```bash
python3 langchain-ollama.py
```

## The Results.

Local embeddings with LangChain offer one solution to the memory limitations of Large Language Models (LLMs). By using tools like Ollama, Llama2, bs4, GPT4All, Chroma, and of course, LangChain itself, I can enhance the static knowledge of an LLM, making it more adaptable and versatile. This process, although technical, is achievable with a Linux-based operating system and Python 3.9 or later. The potential applications of this technology are vast, promising specialist knowledge adaptations in many fields including business, science, and engineering.

## In Conclusion.

In the fascinating world of AI, Large Language Models (LLMs) have some memory limitations. They can only retain information up to the day they were trained and compiled. But guess what? There is a workaround.

Local embeddings with LangChain!

The embedding approach helps LLMs overcome their memory limitations, making them more flexible and useful. Imagine LLMs not being restricted by their initial knowledge. With modern AI tools, I can increase an LLM's knowledge base. Embeddings offer specialized knowledge adaptations in many specialist fields.

I found the concept of embeddings easy to understand and so I created a comprehensive guide for myself. With a Linux-based operating system and Python 3.9 or later (and a decent GPU), I can extend the knowledge base of my favourite LLMs.

What's *your* take on this? What do you think the impact of embeddings will be? I'm looking forward to your thoughts and insights!

Until next time: Be safe, be kind, be awesome.