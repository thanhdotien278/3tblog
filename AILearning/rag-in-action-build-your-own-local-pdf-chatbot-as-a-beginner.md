---
icon: brain-circuit
---

# RAG in Action: Build your Own Local PDF Chatbot as a Beginner

## RAG in Action: Build your Own Local PDF Chatbot as a Beginner

### Understanding chunking, embeddings and vector search better by building a PDF chatbot with LangChain, Ollama and Mistral.

[![Sarah Lea](https://miro.medium.com/v2/resize:fill:88:88/1*p_WtVuIhiaNIDi7CIAECaw.png)](https://medium.com/@schuerch_sarah)[Sarah Lea](https://medium.com/@schuerch_sarah)[Follow](https://medium.com/@schuerch_sarah)[![Data Science Collective](https://miro.medium.com/v2/resize:fill:48:48/1*0nV0Q-FBHj94Kggq00pG2Q.jpeg)Data Science Collective](https://medium.com/data-science-collective)a11y-light\~13 min read·April 12, 2025 (Updated: April 12, 2025)·Free: No

Tools such as Perplexity, ChatGPT, Claude or NotebookLM have completely changed the way we interact with PDFs, technical articles or even entire books. Instead of scrolling page by page, we receive summaries, answers and even explanations of specific sections in seconds.

But how does this actually work in the background?

In this article, I'll show you how you can build your own little PDF chatbot using Python, LangChain, FAISS and a local LLM like Mistral.

Of course, the tool built is not a competitor to existing solutions. Rather, it serves as a practical learning project to understand the underlying concepts — such as chunking, embeddings, vector search and RAG — step by step.

Let's dive straight into the practical example — the theory will follow along the way!

_**Table of Content**_ [_Tech Stack Overview for the Chatbot_](https://freedium.cfd/https://medium.com/data-science-collective/rag-in-action-build-your-own-local-pdf-chatbot-as-a-beginner-96c2833869ff#c1ca) [_Preparation — Mis en Place_](https://freedium.cfd/https://medium.com/data-science-collective/rag-in-action-build-your-own-local-pdf-chatbot-as-a-beginner-96c2833869ff#1945) [_Step-by-Step-Guide for the Chatbot_](https://freedium.cfd/https://medium.com/data-science-collective/rag-in-action-build-your-own-local-pdf-chatbot-as-a-beginner-96c2833869ff#ec76) [_Final Thoughts — How Could we Develop the App further?_](https://freedium.cfd/https://medium.com/data-science-collective/rag-in-action-build-your-own-local-pdf-chatbot-as-a-beginner-96c2833869ff#2904) [_Where to Continue Learning?_](https://freedium.cfd/https://medium.com/data-science-collective/rag-in-action-build-your-own-local-pdf-chatbot-as-a-beginner-96c2833869ff#7b20)

#### Tech Stack Overview for the Chatbot <a href="#c1ca" id="c1ca"></a>

**LangChain** was launched in 2022 by Harrison Chase as an open-source project. We use the framework as it simplifies the development of applications with language models.

We use **Python** as a programming language and **Anaconda** as a Python distribution that facilitates the management of environments and packages.

![The tech stack consists of Python with Anaconda, LangChain, Ollama with Mistral, FAISS as the vector database and Streamlit.](https://miro.medium.com/v2/resize:fit:700/1*x0_VZHu7qxPwFLjtK3diXA.png)Own visualization — Illustrations from [napkin.ai](https://www.napkin.ai/)

The **Facebook AI Similarity (FAISS) Search vector database** was developed specifically for fast similarity searches in large amounts of text embeddings. In this project, we use FAISS to store the text sections of the PDF and efficiently search for matching passages later.

**Ollama** is a local runtime server for LLMs. It makes it possible to run models such as Mistral, Llama, DeepSeek etc., directly on our own computer without a cloud connection. In this project, we use **Mistral** as the LLM.

And for the user interface, we use the open-source framework **Streamlit,** with which we can quickly create a simple web app with Python.

#### Preparation — Mis en Place <a href="#id-1945" id="id-1945"></a>

First, we set up our project environment, install all the required packages and install Ollama.

**1. Python and Anaconda for environment management** Of course, Python must be installed. _Hint for Newbies: Type 'python — version' in your terminal to check if you are using at least Python 3.7._

I work with Anaconda. So we open Anaconda prompt and create a new Conda environment for the project with _'conda create -n pdfchatbot python=3.10'_.

Now we activate the environment with _'conda activate pdfchatbot'_.

_Hint for Newbies: In this article '_[_Python Data Analysis Ecosystem — A Beginner's Roadmap_](https://medium.com/python-in-plain-english/python-data-analysis-ecosystem-a-beginners-roadmap-adf22ba20ed2)_' I have explained these steps in more detail._

**2. Project folder** Next we create a project folder with 'mkdir pdf-chatbot' and navigate to the project folder with 'cd pdf-chatbot'.

**3. Requirements.txt with all required packages** Now we create a 'requirements.txt' file, which we place in the same directory and add the following packages to the file:

_langchain langchain-community pypdf faiss-cpu sentence-transformers openllama streamlit_ _Hint for Newbies: We use a requirements.txt file to capture all the Python packages that our project requires. This makes it easy to recreate the exact same environment on another computer or share the project with others._

**4. Installing all packages** To install all the required packages, we can just run the following command at a time: _'pip install -r requirements.txt'._

![We install all packages with the requirements.txt as it makes it easy to recreate the exact same environment on another computer or share the project with others.](https://miro.medium.com/v2/resize:fit:700/1*bBv-GfL6iz6laFD75OLSbw.png)Screenshot taken by the author

**5. Installation of Ollama** Now we install Ollama to use LLMs locally. To do this, we visit the [official Ollama download page](https://ollama.com/download/windows) and install Ollama.

As soon as the installation is complete, we check in a terminal with _'ollama — version'_ whether everything has worked:

![We check if the ollama version was installed successfully.](https://miro.medium.com/v2/resize:fit:700/1*QOWh411ss10HwZKjMMoWCQ.png)Screenshot taken by the author

Now we open a second Anaconda Prompt Terminal, activate our environment and run Ollama. We do this in a second terminal, as the terminal then remains "blocked" because the model is waiting for our input. This will become clearer in steps 5 and 6 below.

![We run ollama with the LLM Mistral in the background.](https://miro.medium.com/v2/resize:fit:700/1*b8O92dz2XUv-PszcHmOprg.png)Screenshot taken by the author

#### Step-by-Step-Guide for the Chatbot <a href="#ec76" id="ec76"></a>

The aim of this app is to enable us to ask questions about a PDF document in natural language and have them answered (based on the PDF and not general knowledge). The app combines a language model with an intelligent search in the document to find relevant text passages and generate a suitable answer.

**1 — Creating 3 files**

We open our desired IDE, in which we create the various files with the code for the chatbot. I use Visual Studio Code for this.

We create 3 files to show the separation of logic and interface. This would not be necessary for this small app, but this way, we can see the best practice principle directly.

![We separate the logic and the interface of the app in three separate files.](https://miro.medium.com/v2/resize:fit:700/1*dfWvdJO0xwxmZAPRu0hRjg.png)Own visualization — Illustrations from [napkin.ai](https://www.napkin.ai/)

**2 — File 1: chatbot\_core.py**

In this file we create the so-called RAG pipeline consisting of PDF→Chunks→Embeddings→Vector database→Retriever→LLM with the following code:

```python
Copy# chatbot_core.py: This module defines the core logic of the PDFchatbot.
# It builds a RAG pipeline using LangChain.

from langchain_community.document_loaders import PyPDFLoader
from langchain.text_splitter import CharacterTextSplitter
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.vectorstores import FAISS
from langchain.chat_models import ChatOllama
from langchain.chains import ConversationalRetrievalChain

def build_qa_chain(pdf_path="example.pdf"):
    loader = PyPDFLoader(pdf_path) # Loads the PDF
    documents = loader.load()[1:]  # Skip page 1 (element 0)

    splitter = CharacterTextSplitter(chunk_size=500, chunk_overlap=100) # Generates chunks of the document
    docs = splitter.split_documents(documents)

    embeddings = HuggingFaceEmbeddings(model_name="sentence-transformers/all-MiniLM-L6-v2") # Generates vector embeddings for each chunk
    db = FAISS.from_documents(docs, embeddings) # Stores the chunks in a FAISS vector db for similarity search
    retriever = db.as_retriever() # Create a retriever to find relevant chunks based on a question

    llm = ChatOllama(model="mistral") # Combines the retriever with mistral
    qa_chain = ConversationalRetrievalChain.from_llm(
        llm=llm,
        retriever=retriever,
        return_source_documents=True
    )

    return qa_chain # The function 'qa_chain()' returns a ready-to-use question-answering chain
```

**What happens in the code?**

First we load the required packages:

![This image shows all the packages from the code to import.](https://miro.medium.com/v2/resize:fit:700/1*oyCsC4MrXtt7wR8uYTYyRw.png)Own visualization

After loading the packages, we create a function _'build\_qa\_chain()'_ in which we go through all the necessary steps:

**1. Loading the PDF** We load our desired PDF into the 'pdf-chatbot' directory that we created earlier. With 'PyPDFLoader(pdf\_path)' we read the PDF and ignore the title page, because this PDF only consists of an image — without text. Make sure that you choose a PDF that consists of actual text and not just image data. This loader provides us with LangChain so that we can read in a file and convert it into document objects so that LangChain can work with it. _Hint for newbies: In Python, lists start at 0._

**2. Chunking** Next, we split the text into small parts so that we can later save individual sections in the vector database. With 'CharacterTextSplitter()' we split the text of the entire PDF into overlapping chunks. Here I have chosen 500 characters, whereby 100 characters are overlapping. We then save the individual chunks in 'docs'. **Why is this necessary?** Language models like Mistral or GPT cannot handle huge texts at once — e.g. a whole 50-page PDF. We therefore divide the text into smaller chunks (e.g. 500 characters) and select an overlap (e.g. 100 characters) so that the transitions are not lost.

**3. Embeddings** In the next line, we create the embeddings with 'HuggingFaceEmbeddings(model\_name= )'.)'.

**What are embeddings?** An embedding is a mathematical representation of text in the form of a vector. As soon as this conversion has taken place, an AI model can compare which texts are similar — e.g. which chunks match the question asked.

With LangChain, we can use the HuggingFaceEmbeddings class for this and choose the 'all-MiniLM-l6-v2' model from HuggingFace, which is sufficient for this project.

**4. Vector Database** We then create a FAISS Vector database with 'FAISS.from\_documents(docs, embeddings)'.

The database automatically converts each chunk into a FAISS entry via an embedding. We extract a retriever from this.

**What is FAISS?** Facebook AI Similarity Search (FAISS) is Facebook's vector database. This saves the text chunks based on the embeddings (vectors from the previous step). This allows the system to quickly find similar text passages later when we ask a question.

The impressive thing is that with LangChaing we only need a single line of code for this.

**What is a retriever?** A retriever is the link between the user question and the vector data. When we ask a question, the system creates a vector of the question and then searches your FAISS database for the most similar chunks. And the most similar chunks are probably the most relevant text sections for our question.

However, the retriever alone does not create the complete answer — it only returns the relevant text chunks that are most likely to contain the answer. We can imagine the research department underneath, so to speak. Such a retriever also minimizes hallucinating from the LLM, as it selects a precise database from the PDF.

**5. Large Language Model** The LLM then provides a response in natural language and a summary of the chunks provided by the retriever. This is the basic principle of RAG, so to speak.

In the line 'llm' we specify that we want to use the LLM Mistral, which is currently being run locally by Ollama. This is why we entered the command 'ollama run mistral' in a new terminal in the preparatory steps.

**What is Ollama?** Ollama is a tool that makes it super easy for us to run LLMs locally on our own computer. It doesn't need a complex setup, API key or cloud provider. Ollama runs locally in the background and offers an API that can access the LangChain directly — so we don't need an API key.

In our projects, we use Ollama to run the LLM Mistral locally. This means that we do not need a connection to the cloud (e.g. OpenAI or Hugging Face), that we have no API costs and that the model even runs without a GPU — directly on the CPU. And with just a single line of code, we can integrate Ollama into LangChain. This makes it ideal for a small PDF RAG project like this one that runs directly on the laptop.

**6. Conversational Retrieval Chain** With 'ConversationalRetrievalChain' we now combine a language model (here Mistral) with a retriever (here from FAISS) and the process into a question-answer chain.

**7. Answer as the result** At the end we return the finished chain with 'return qa\_chain', which we can use in chatbot\_terminal.py or streamlit.py.

**What is RAG?** In the article '[How to Make Your LLM More Accurate with RAG & Fine-Tuning](https://towardsdatascience.com/how-to-make-your-llm-more-accurate-with-rag-fine-tuning/)' I have explained the basics of RAG in detail. But in essence, the point is that with RAG, we can improve the model by improving the input: The model remains the same, but it gets access to an external knowledge source (in this project only to a specific PDF), which in turn reduces the problem of hallucinating.

**3 — File 2: streamlit\_app.py**

With this file we create the graphical user interface (GUI) of the PDF chatbot. In order not to make the article even longer, we will keep it super simple:

```python
Copy# streamlit_app.py: Simple web-based user interface for the PDF chatbot using Streamlit
# The UI allows users to ask questions about a PDF document and get answers generated by a local LLM (via Ollama) - combined with RAG.

import streamlit as st
from chatbot_core import build_qa_chain #Imports the function that builds the RAG pipeline.

st.set_page_config(page_title="📄 PDF-Chatbot", layout="wide") #To set up the Sreamlit page with a title & wide layout
st.title("📄 Chat with your PDF")

qa_chain = build_qa_chain("example.pdf") #Builds the QA chain using the specified PDF file

#Initializes the chat history in Streamlit's session state
if "chat_history" not in st.session_state:
    st.session_state.chat_history = []  

# Creates a text input field for the user to ask a question
question = st.text_input("What would you like to know?", key="input")

#If a question is submitted, the question is sent to the QA chain & stores the result
if question:
    result = qa_chain({
        "question": question,
        "chat_history": st.session_state.chat_history
    })

    st.session_state.chat_history.append((question, result["answer"])) #Saves the question & the answer to the session history

    # Displays the chat history in reverse order (newest on top)
    for i, (q, a) in enumerate(st.session_state.chat_history[::-1]):
        st.markdown(f"**❓ Question {len(st.session_state.chat_history) - i}:** {q}")
        st.markdown(f"**🤖 Answer:** {a}")
```

**What happens in the code?**

1. First, we set the title and the layout for the Streamlit Page.
2. Next, we use 'build\_qa\_chain' to create the LLM + retriever chain from the PDF.
3. Then we use the if loop to check whether a chat history already exists — if not, we start an empty history.
4. With 'st.text\_input()' we define an input field so that we can enter the questions.
5. With the 'if question loop' we send the question to the QA chain and receive an answer from the LLM.
6. At the end, we save the question and answer in the session state and display the previous questions and answers.

**4. File 3: chatbot\_terminal.py**

We create this file so that we can test the PDF chatbot directly in the terminal without having to start the Streamlit interface. Since we can directly see how the bot responds to questions and what the source looks like, this step is good for learning and practical for development:

```python
Copy# main.py: This file contains the code to interact with the PDF chatbot
# The file is mainly intended for testing, debugging or if no web interface is needed
# The chatbot uses a RAG pipeline that is defined in chatbot_core.py

from chatbot_core import build_qa_chain # Imports the RAG pipeline builder from chatbot_core.py

qa_chain = build_qa_chain("example.pdf") #Builds the QA chain using a local PDF file
chat_history = [] #Initializes an empty list to store the chat history

print("🧠 PDF-Chatbot started! Enter 'exit' to quit.") # Prints the welcome message to the terminal

# Starts a loop to allow the user to ask questions continuously
while True:
    query = input("\n❓ Your questions: ")
    # Breaks the loop if the user types 'exit' or 'quit'
    if query.lower() in ["exit", "quit"]:
        print("👋 Chat finished.")
        break

    # Get the answer from the QA chain (LLM + Retriever) and prints the answer to the terminal
    result = qa_chain({"question": query, "chat_history": chat_history})
    print("\n💬 Answer:", result["answer"])
    chat_history.append((query, result["answer"])) #Saves the Q&A pair in the chat history
    print("\n🔍 Source – Document snippet:") #Shows a snippet from the source document that is used
    print(result["source_documents"][0].page_content[:300])
```

**What happens in the code?**

1. First we import the function 'build\_qa\_chain', which prepares the entire model, embeddings and retrievers.
2. With the line 'qa\_chain = build\_qa\_chain()' we start the RAG pipeline with our example PDF.
3. With 'chat\_history' we initialise an empty list to store questions & answers.
4. Then we display the greeting for the user and specify how to end it again.
5. With the while loop we define that questions can be asked indefinitely until the user enters 'exit' or 'quit'.

In the last part of the code, we pass the question to the QA chain. This chain combines the language model with the document search and provides a suitable answer. The answer is then displayed in the terminal (including a short excerpt from the text section that the model used to answer the question).

**5 — Let's run the Streamlit app**

Now we enter 'streamlit run streamlit\_app.py' in the terminal.

![With streamlit we can run our app in minutes.](https://miro.medium.com/v2/resize:fit:700/1*qrz5s3mI328NMFdgnrdzlA.png)Screenshot taken by the author

The Streamlit app then opens automatically and we can ask our question:

![In the UI we can ask then our questions about our PDF.](https://miro.medium.com/v2/resize:fit:700/1*7g2DulTFkxpJwkHOGYJRTA.png)Screenshot taken by the author

_On my_ [_Substack_](https://sarahleaschrch.substack.com/)_, I regularly write summaries about the published articles in the fields of Tech, Python, Data Science, Machine Learning and AI. If you're interested, take a look or subscribe._

#### Final Thoughts — How could we develop the app further now? <a href="#id-2904" id="id-2904"></a>

We have now built a simple chatbot that can answer questions about the PDF 'Practical Linear Algebra for Data Science'. With open source tools such as Python, LangChain, Ollama and FAISS, a first working version can be realised in a short time. It is particularly valuable to go through the individual steps yourself — because this gives you a much better understanding of how chunking, embeddings, vector databases and the basic logic behind RAG-based chatbots work.

**How could we further develop the app now to make it really usable?**

**Improving the performance** It currently takes around 2 minutes to receive a response. We could optimise this with a faster LLM or with more resources.

**Making the app publicly accessible** The app currently only runs on the local computer. With Streamlit Cloud, we could make the app publicly accessible.

**PDF upload by user** The PDF is currently specified. Next, we should add an upload button in the Streamlit app to upload and process any PDF.

**Better UI** The Streamlit app is extremely simple. Here we should use HTML & CSS to visually separate the question and answer. We could also display the PDF source for the answer.

#### Where to continue learning? <a href="#id-7b20" id="id-7b20"></a>

* [Towards Data Science Article: Understanding the Tech Stack Behind Generative AI](https://towardsdatascience.com/tech-stack-generative-ai/)
* [Towards Data Science Article: How to Make Your LLM More Accurate with RAG & Fine-Tuning](https://towardsdatascience.com/how-to-make-your-llm-more-accurate-with-rag-fine-tuning/)
* [Medium Article: Your first API integration with Streamlit and Plotly: How to Create Interactive Web Apps as a Data Scientist](https://python.plainenglish.io/your-first-api-integration-with-streamlit-and-plotly-how-to-create-interactive-web-apps-as-a-data-4fd60c32cd1a)
* [Streamlit Documentation: Basic Concepts of Streamlit](https://docs.streamlit.io/get-started/fundamentals/main-concepts)
* [IBM Blog: What is a Vector Database](https://www.ibm.com/think/topics/vector-database)
* [DataCamp Course: Vector Databases for Embeddings with Pinecone](https://www.datacamp.com/courses/vector-databases-for-embeddings-with-pinecone?utm_source=google\&utm_medium=paid_search\&utm_campaignid=18132061805\&utm_adgroupid=172588448570\&utm_device=c\&utm_keyword=what%20is%20a%20vector%20database\&utm_matchtype=e\&utm_network=g\&utm_adpostion=\&utm_creative=744592627145\&utm_targetid=kwd-1106179552989\&utm_loc_interest_ms=\&utm_loc_physical_ms=9186901\&utm_content=field~ml~pinecone\&accountid=9624585688\&utm_campaign=220808_1-sea~field~ml_2-b2c_3-emea_4-prc_5-na_6-na_7-le_8-pdsh-go_9-nb-e_10-na_11-na\&gad_source=1\&gclid=Cj0KCQjwnui_BhDlARIsAEo9GuuMSjsa_SO6lpJAJ_FIiZJdi8PBvpF8eBvD9FwEM8ondSWEcvyGvCkaAlZfEALw_wcB)
* [DataCamp Blog: What Is Faiss (Facebook AI Similarity Search)?](https://www.datacamp.com/blog/faiss-facebook-ai-similarity-search)
* [DataCamp Blog: How to Set Up and Run Gemma 3 Locally With Ollama](https://www.datacamp.com/tutorial/gemma-3-ollama)
* [LangChain: Tutorials Get Started](https://python.langchain.com/docs/tutorials/)
* [Elastic Blog: LangChain Tutorial](https://www.elastic.co/blog/langchain-tutorial)

![None](https://miro.medium.com/v2/resize:fit:700/1*xSFO7V2x3oQw14Z10dD2Ow.png)Own visualization — Illustrations from [unDraw.co](https://undraw.co/)
