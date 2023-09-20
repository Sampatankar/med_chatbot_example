# med_chatbot_example
The first deployable Generative AI example from this: https://www.youtube.com/watch?v=kXuHxI5ZcG0

the AI Anytime YouTube channel (https://www.youtube.com/@AIAnytime/videos) has published this deployable medical encyclopaedia-based generative AI chatbot.

The basis of this video was to use open-source as much as possible.  We use a quantized Llama2-based AI model which is going to run from CPU rather than GPU.  We use Chainlit, Faiss, and Langchain leveraging what HuggingFace makes available.

I'll add more details if required, but I do not take any more credit other than trying to deploy the example myself.  The full credit goes to AI Anytime (links above).  

# Where to dl the model used:
https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGML/blob/main/llama-2-7b-chat.ggmlv3.q8_0.bin
15:21 start of dl
15:51 finish of dl

# Method:
1. The Llama2 model has been quantized by 'The Bloke', and the model was hosted on Huggingface (link above) - This is the LLM
2. We want to run it on the CPU rather than GPU, so we use CTransformers which offers Python bindings
3. Sentence transformers
4. Vector store for embeddings - chroma DB, Faiss CPU, Qdrant


# Ongoing Notes:
19/9/23:
Chainlit - enables a UI for the GPT interface

do so by using a Llama-2 quantized model able to run on CPU alone
Use a quantization model to reduce the RAM requirements needed to get the use of the Llama2 LLM.

We use transformers to 'load' the model. However we can't use standard transformers as we want to use CPU rather than GPU. That's why we use CTransformers, which has C++ Python bindings.

The sentence transformer used will be miniLLM v6 to create the embeddings.

Then we need a vector database to store the embeddings in a lower dimnesional space.  
Store types:
1. ChromaDB
2. FAISS CPU
3. Qdrant
The above are open source. There are other closed sourced variants.

20/9/23:
Process in more detail:
1. docs/data are preprocessed by Langchain - loaders, splitters
2. The preprocessed text is passed to an embedding model - sentence transformer
3. The embeddings are then stored somewhere - vector database/store - faiss CPU
4. The user inputs a prompt, which via Faiss CPU, is passed through an LLM to give a response

Inside the vector store, we use inbuilt similarity algorithms to have next to no latency.  

# Once all dl:
Create a file called ingest.py
At this point, I am having issues with langchain module is installed in the environment (0.0.74), along with the the LLMS AND the ALL options, but on running ingest.py, we are not finding the method being called. This requires debugging.
The problem was due to langchain requiring upgrading - used pip --upgrade command.
I then changed the run command from 'python3 ingest.py' to 'python ingest.py'.  Multiple versions of Python perhaps?
embeddings created and stored in the data folder store

model.py will be the code for the bot, through which the LLM will be accessible. (25:56 onwards)
