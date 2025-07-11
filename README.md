# HomeMatch
## Project description
This project consist on different files:
- project.ipynb: This is the jupyter notebook that executes the script required for the exercise.
- listing.json: Premade list of properties generated in the script. This is a requirement for the exercise.
- requirements.txt: Python requirements for running the jupyter notebook. This has been tested in Python 3.12 in a OS X environment (arm64). You'll need to execute `pip install -r requirements.txt` to make it work.

You'll need to create a .env file containing something like this, but with a correct key:
>OPENAI_API_KEY=*******************************

## Notebook sections
### Libraries and setup
This section includes all the necessary imports, it loads the environment variables required for openai, and it instanciates the llm and embeddings objects.
### Generate listings
In this section I create the Pydantic models for the properties listings, and using structured output, I ask the LLM to provide me with a list of 30 properties. Then, I store that inside listing.json and reload it (in case you don't want to generate it with the LLM all the time).
Finally, it processess the properties list, generating a list of LangChain documents, and inserts those into a Chroma db collection (I reset the collection every time to clear it). I set the Chroma db to use cosine distance to measure similarity.
### Buyer preferences
In this section I instanciate a set of premade questions and answers on buyer preferences (there are some options in the answer to test the code works for both of them), and I create a in memory chat history, inserting those questions and answers in there.
Then, I have some functions for creating the different pieces neccessary for having a retrieval chain using premade chains from Langchain:
- I use a "create_retrieval_chain" that consist of a retriever and a qa_chain.
- For retrieval I use a "create_history_aware_retriever" that brings the chat history in place to produce a list of documents that takes it into consideration.
- As QA chain I use "create_stuff_documents_chain" that takes that list of documents to generate a response.
- Wrapping all of that I use a RunnableWithMessageHistory, that enables further conversation in case we'd like to offer more interactivity in the results.
### Instanciating everything
I instantiate the chain that I defined in the previous section. I set the Chroma db as a retriever, configuring it to perform similarity search and getting maximum 5 elements. 
Then I invoke it and I print the results.
## Considerations
I'm saving the notebook with the output of an execution to demonstrate how the output should look like.