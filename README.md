# Search and Rescue (SAR) Agent Framework - CSC 581

## Introduction

This is for the implementation of Medical Team Leader, one of the SAR agents for CSC 581. More can be found below:

https://docs.google.com/spreadsheets/d/1QZK5HAdDC-_XNui6S0JZTbJH5_PbYJTp8_gyhXmz8Ek/edit?usp=sharing
https://docs.google.com/spreadsheets/d/11rBV9CbKNeQbWbaks8TF6GO7WcSUDS_-hAoH75UEkgQ/edit?usp=sharing

To run, just install the depencies and set up your environment variables (see below for instructions). Then, run the cells for `medical_agent_lead.ipynb` in `src/sar_project/agents`. 
The query for the agent will be through the `answer_question` function. 

The agent is an attempt at implementing RAG. There are three PDF files that it draws from. One for help in triaging, one as a medical guide, and one with some sample data 
of people who are hurt. They are stored in vector databases. The text splitter is `RecursiveCharacterTextSplitter.from_tiktoken_encoder(chunk_size=300, chunk_overlap=50)` from `langchain`.

The first task of the agent is to ask it to create a suitable number of subquestions from the user query (the number of subquestions could be dynamically decided in a future update). 
Then for each subquestion, it is asked which knowledge base(s) would be best to use to answer the subquestion. The agent will select documents (which are ranked from most to least important) from the knowledge base(s) to use to help answer them. This is to have better context for the main query to use. Each following question/subquestion also has access to the previous ones as context. A chat history is implemented for previous queries to use as context. 

Testing for the medical agent mainly just included a few queries asking about triaging the hurt people in the sample data and checking that it pulls correct information from the knowledge bases. See the 
end of `medical_agent_lead.ipynb` for the tests and outputs. 

## Setup and Installation

1. Clone the repository:
```bash
git clone <repository-url>
cd sar-project
```

2. Set up Python environment:
```bash
# Using pyenv (recommended)
pyenv install 3.9.6  # or your preferred version
pyenv local 3.9.6

# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate  # On Unix/macOS
# or
.venv\Scripts\activate     # On Windows
```

3. Install dependencies:
```bash
pip install -r requirements.txt
pip install -e .
```

4. Configure environment variables:

#### OpenAI:
- Obtain required API keys:
  1. OpenAI API key: Sign up at https://platform.openai.com/signup
- Update your `.env` file with the following:
    ```
    OPENAI_API_KEY=your_openai_api_key_here
    ```

Make sure to keep your `.env` file private and never commit it to version control.

## A3 -- Insights
I was the only person who did not get feedback on my implementation. Thus, I came up with a few improvements on my own. I asked Professor Kurfess if the improvements would be okay, and he said they were fine. I wasn't sure if it would be enough, but Professor Kurfess said he didn't see if there would be much more I could realistically improve on. I am not technically sure I did learn anything. The improvements I implemented were ones I already thought of for A1, but felt that it wasn't super necessary to implement at the time. The main improvement I could think of was how to make it better at acquiring information and consolidating that information. Therefore, in A1, I asked it to create 3 subquestions to help answer the main question. I then removed duplicates. To improve upon that, I figured that perhaps ranking the documents retrieved on how helpful they would be could improve response quality. Then, based on the difficulty of the question, it may create more or less than just the constant 3 subquestions.

## A3 -- Modifications
Based on the insights, there were two modifications I made. Back when I was first creating the agent, I stumbled across something called RAG fusion that I thought might be helpful in ranking. I kept that in my notes. For A3, implementing it was actually relatively simple, as it turned out to be a simple scoring system where the first documents to appear got more points, and it was an additive system. Therefore, if documents appeared multiple times and appeared earlier in the vector search, they would rank higher. After using that to rank the documents, I rewrote some templates to the agents in order to take advantage of that as well as dynamically deciding the number of subquestions to create based on the main user query. 
